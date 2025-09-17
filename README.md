import React, { useEffect, useRef, useState } from 'react';
import { createChart, ColorType, IChartApi, ISeriesApi } from 'lightweight-charts';
import { CandleData, IndicatorData } from '../types/trading';

interface TradingChartProps {
  data: CandleData[];
  smaData: IndicatorData[];
  symbol: string;
}

export const TradingChart: React.FC<TradingChartProps> = ({ data, smaData, symbol }) => {
  const chartContainerRef = useRef<HTMLDivElement>(null);
  const chartRef = useRef<IChartApi | null>(null);
  const candlestickSeriesRef = useRef<ISeriesApi<'Candlestick'> | null>(null);
  const smaSeriesRef = useRef<ISeriesApi<'Line'> | null>(null);
  const volumeSeriesRef = useRef<ISeriesApi<'Histogram'> | null>(null);

  useEffect(() => {
    if (!chartContainerRef.current) return;

    // Clean up existing chart
    if (chartRef.current) {
      chartRef.current.remove();
    }

    // Create chart
    const chart = createChart(chartContainerRef.current, {
      width: chartContainerRef.current.clientWidth,
      height: 600,
      layout: {
        background: { type: ColorType.Solid, color: '#1f2937' },
        textColor: '#d1d5db',
      },
      grid: {
        vertLines: { color: '#374151' },
        horzLines: { color: '#374151' },
      },
      crosshair: {
        mode: 1,
      },
      rightPriceScale: {
        borderColor: '#4b5563',
      },
      timeScale: {
        borderColor: '#4b5563',
        timeVisible: true,
        secondsVisible: false,
      },
    });

    // Assign chart reference immediately
    chartRef.current = chart;

    if (!chart || typeof chart.addCandlestickSeries !== 'function') {
      console.error('Failed to create chart or chart methods not available:', chart);
      return;
    }

    // Create candlestick series
    const candlestickSeries = chart.addCandlestickSeries({
      upColor: '#22c55e',
      downColor: '#ef4444',
      borderDownColor: '#ef4444',
      borderUpColor: '#22c55e',
      wickDownColor: '#ef4444',
      wickUpColor: '#22c55e',
    });

    // Create SMA series
    const smaSeries = chart.addLineSeries({
      color: '#3b82f6',
      lineWidth: 2,
      title: 'SMA 20',
    });

    // Create volume series
    const volumeSeries = chart.addHistogramSeries({
      color: '#6b7280',
      priceFormat: {
        type: 'volume',
      },
      priceScaleId: '',
      scaleMargins: {
        top: 0.8,
        bottom: 0,
      },
    });

    candlestickSeriesRef.current = candlestickSeries;
    smaSeriesRef.current = smaSeries;
    volumeSeriesRef.current = volumeSeries;

    // Handle resize
    const handleResize = () => {
      if (chartContainerRef.current && chart) {
        chart.applyOptions({
          width: chartContainerRef.current.clientWidth,
        });
      }
    };

    window.addEventListener('resize', handleResize);

    return () => {
      window.removeEventListener('resize', handleResize);
      if (chartRef.current) {
        chartRef.current.remove();
        chartRef.current = null;
      }
    };
  }, []);

  useEffect(() => {
    if (!candlestickSeriesRef.current || !smaSeriesRef.current || !volumeSeriesRef.current) return;

    // Update data
    const chartData = data.map(item => ({
      time: item.time,
      open: item.open,
      high: item.high,
      low: item.low,
      close: item.close,
    }));

    const volumeData = data.map(item => ({
      time: item.time,
      value: item.volume,
      color: item.close >= item.open ? '#22c55e40' : '#ef444440',
    }));

    candlestickSeriesRef.current.setData(chartData);
    volumeSeriesRef.current.setData(volumeData);
    
    if (smaData.length > 0) {
      smaSeriesRef.current.setData(smaData.map(item => ({
        time: item.time,
        value: item.value,
      })));
    }

    // Fit content
    if (chartRef.current) {
      chartRef.current.timeScale().fitContent();
    }
  }, [data, smaData]);

  return (
    <div className="bg-gray-900 rounded-lg">
      <div className="p-4 border-b border-gray-700">
        <div className="flex items-center justify-between">
          <div>
            <h3 className="text-xl font-bold text-white">{symbol}</h3>
            <div className="text-sm text-gray-400">
              Price: ${data[data.length - 1]?.close.toLocaleString() || 'N/A'}
            </div>
          </div>
          <div className="flex space-x-2">
            {['1m', '5m', '15m', '1h', '4h', '1d', '1w'].map((timeframe) => (
              <button
                key={timeframe}
                className={`px-3 py-1 text-xs rounded ${
                  timeframe === '1d'
                    ? 'bg-blue-600 text-white'
                    : 'bg-gray-700 text-gray-300 hover:bg-gray-600'
                }`}
              >
                {timeframe}
              </button>
            ))}
          </div>
        </div>
      </div>
      <div ref={chartContainerRef} className="relative h-[600px]" />
    </div>
  );
};
