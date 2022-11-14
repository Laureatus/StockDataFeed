<template>
  <div>

  
    
    {{ close }}
  </div>
  </template>
  
  <script>
  import { Bar } from 'vue-chartjs'
  import { Chart as ChartJS, Title, Tooltip, Legend, BarElement, CategoryScale, LinearScale } from 'chart.js'
  
  ChartJS.register(Title, Tooltip, Legend, BarElement, CategoryScale, LinearScale)
  
  export default {
    name: 'BarChart',
    components: { Bar },
    props: {
      chartId: {
        type: String,
        default: 'bar-chart'
      },
      datasetIdKey: {
        type: String,
        default: 'label'
      },
      width: {
        type: Number,
        default: 400
      },
      height: {
        type: Number,
        default: 400
      },
      cssClasses: {
        default: '',
        type: String
      },
      styles: {
        type: Object,
        default: () => {}
      },
      plugins: {
        type: Object,
        default: () => {}
      }
    },
    data() {
      return {
        close: [],
        chartData: {
          labels: [ 'January'],
          datasets: [ { data: ['140.0400'] } ]
        },
        chartOptions: {
          responsive: true
        },
        async fetch() {
        this.close = await fetch(
        `https://www.alphavantage.co/query?function=TIME_SERIES_MONTHLY&symbol=${this.searchInput}&apikey=0ZN6UO9D3R6K3R77`
        ).then(res => res.json())
    },
      }
      
    }
  }
  </script>