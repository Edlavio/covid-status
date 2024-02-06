<script lang="ts">
import Chart from 'chart.js/auto'

interface CovidData {
  cases: number | null
  confirmed: number
  country: string
  deaths: number
  recovered: number | null
  updated_at: string
}

export default {
  name: 'CovidChart',
  data() {
    return {
      covidData: [] as CovidData[]
    }
  },
  mounted() {
    this.fetchCovidData()
  },
  methods: {
    fetchCovidData() {
      // Supondo que você tenha uma função fetch ou uma biblioteca como Axios para obter os dados da API
      fetch('https://covid19-brazil-api.vercel.app/api/report/v1/countries', { cache: 'default' })
        .then((response) => response.json())
        .then((data) => {
          this.covidData = data.data // Supondo que a resposta da API seja um array de objetos
          this.createChart()
        })
        .catch((error) => {
          console.error('Erro ao obter os dados da API:', error)
        })
    },
    createChart() {
      const ctx = document.getElementById('myChart')
      new Chart(ctx as HTMLCanvasElement, {
        type: 'bar',
        data: {
          labels: this.covidData.map((entry) => entry.country),
          datasets: [
            {
              label: 'Casos Confirmados',
              data: this.covidData.map((entry) => entry.confirmed),
              backgroundColor: '#11ff00',
              borderColor: '#11ff00',
              borderWidth: 1,
              barThickness: 6
            },
            {
              label: 'Mortes',
              data: this.covidData.map((entry) => entry.deaths),
              backgroundColor: '#f54242',
              borderColor: '#f54242',
              borderWidth: 1,
              barThickness: 6
            }
          ]
        },
        options: {
          scales: {
            y: {
              ticks: { color: '#ffffff' },
              grid: { color: '#49494a' }
            },
            x: {
              ticks: { color: '#ffffff' },
              grid: { color: '#49494a' }
            }
          },
          color: '#ffffff',
          plugins: {
            legend: {
              labels: {
                font: {
                  size: 17
                }
              }
            },
            tooltip: {
              bodyFont: {
                size: 14
              },
              titleFont: {
                size: 14
              }
            }
          }
        }
      })
    }
  }
}
</script>

<template>
  <canvas id="myChart"></canvas>
</template>
