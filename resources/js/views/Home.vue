<template>
  <div>
    <hero-bar :has-right-visible="false">
      Pagrindinis
    </hero-bar>
    <section class="section is-main-section">
      <tiles>
        <card-widget class="tile is-child" type="is-primary" icon="thermometer" :number="20.1" label="Lauko Temp."/>
		<card-widget class="tile is-child" type="is-danger" icon="thermometer" :number="25.8" label="Vidaus Temp."/>
        <card-widget class="tile is-child" type="is-info" icon="water" :number="75" label="Drėgnumas"/>
        <card-widget class="tile is-child" type="is-success" icon="umbrella" :number="256" label="Laistymas"/>
		<card-widget class="tile is-child" type="is-success" icon="bolt" :number="128" label="ON / OFF"/>
      </tiles>
	  
	  <card-component title="GreenHouse" icon="finance">
		  <div class="columns">
			  <div class="column">
			  </div>
			  <div class="column box has-text-centered m-2 p-2">
          <div class="has-background-success m-1 p-1">KAIRĖ</div>
            <div class="has-background-warning p-2 m-2">LANGAS</div>
            <div class="has-background-warning-light p-2 m-2">
            <b-field grouped>
              <b-checkbox v-model="UpLeftWindowsCheck" type="is-warning"> </b-checkbox>
              <b-switch v-if="UpLeftWindowsCheck"
                v-model="UpLeftWindows"
                passive-type='is-dark'
                type='is-warning'>
                {{ UpLeftWindows ? "ATIDARYTA" : "UZDARYTA" }}
              </b-switch>
              <b-switch v-else disabled>ISJUNGTA</b-switch>
            </b-field>
            </div>				
            <div class="has-background-danger  p-2 m-2">DURYS</div>
            <div class="has-background-danger-light p-2 m-2">
            <b-field grouped>
              <b-checkbox v-model="UpLeftDoorCheck" type="is-danger"> </b-checkbox>
              <b-switch v-if="UpLeftDoorCheck"
                v-model="UpLeftDoor"
                passive-type='is-dark'
                type='is-danger'>
                {{ UpRightDoor ? "ATIDARYTA" : "UZDARYTA" }}
              </b-switch>
              <b-switch v-else disabled>ISJUNGTA</b-switch>
            </b-field>
          </div>
			  </div>
			  <div class="column box has-text-centered m-2 p-2">			  
				  <div class="has-background-success m-1 p-1">DEŠINĖ</div>
				    <div class="has-background-warning p-2 m-2">LANGAS</div>
            <div class="has-background-warning-light p-2 m-2">
            <b-field grouped>
              <b-checkbox v-model="UpRightWindowsCheck" type="is-warning"> </b-checkbox>
              <b-switch v-if="UpRightWindowsCheck"
                v-model="UpRightWindows"
                passive-type='is-dark'
                type='is-warning'>
                {{ UpRightWindows ? "ATIDARYTA" : "UZDARYTA" }}
              </b-switch>
              <b-switch v-else disabled>ISJUNGTA</b-switch>
            </b-field>
            </div>			
            <div class="has-background-danger  m-2 p-2">DURYS</div>
            <div class="has-background-danger-light p-2 m-2">
            <b-field grouped>
            <b-checkbox v-model="UpRightDoorCheck" type="is-danger">	</b-checkbox>
              <b-switch v-if="UpRightDoorCheck"
                v-model="UpRightDoor"
                passive-type='is-dark'
                type='is-danger'>
                {{ UpRightDoor ? "ATIDARYTA" : "UZDARYTA" }}
              </b-switch>
              <b-switch v-else disabled>ISJUNGTA</b-switch>
            </b-field>
          </div>
				</div>
			  <div class="column">
			  </div>
		  </div>
      <!-- -->
      <div class="columns">
			  <div class="column box has-text-centered m-2 p-2">
          <div class="has-background-success m-1 p-1">KAIRĖ</div>
			  </div>
        <div class="column is-6 box has-text-centered m-2 p-2">
          <div class="has-background-success m-1 p-1">STOGLANGIAI</div>
          <div class="columns">
            <div class="column box has-text-centered m-2 p-2">
            <div class="has-background-success m-1 p-1">KAIRĖ</div>
            </div>
            <div class="column box has-text-centered m-2 p-2">
            <div class="has-background-success m-1 p-1">DEŠINĖ</div>
            </div>
          </div>
			  </div>
        <div class="column box has-text-centered m-2 p-2">
          <div class="has-background-success m-1 p-1">DEŠINĖ</div>
			  </div>
      </div>
      </card-component>
	  
      <card-component title="Performance" @header-icon-click="fillChartData" icon="finance" header-icon="reload">
        <div v-if="defaultChart.chartData" class="chart-area">
          <line-chart style="height: 100%"
                      ref="bigChart"
                      chart-id="big-line-chart"
                      :chart-data="defaultChart.chartData"
                      :extra-options="defaultChart.extraOptions">
          </line-chart>
        </div>
      </card-component>
    </section>
  </div>
</template>

<script>
// @ is an alias to /src
import * as chartConfig from '@/components/Charts/chart.config'
import HeroBar from '@/components/HeroBar'
import Tiles from '@/components/Tiles'
import CardWidget from '@/components/CardWidget'
import CardComponent from '@/components/CardComponent'
import LineChart from '@/components/Charts/LineChart'

export default {
  name: 'home',
  components: {
    LineChart,
    CardComponent,
    CardWidget,
    Tiles,
    HeroBar,
  },
  data () {
    return {
      defaultChart: {
        chartData: null,
        extraOptions: chartConfig.chartOptionsMain
      },
	  //left right
    //UP WINDOWS
    UpLeftWindowsCheck: false,
    UpLeftWindows: false,
    UpRightWindowsCheck: false,
    UpRightWindows: false,
    //UP DOOR
    UpLeftDoorCheck: false,
    UpLeftDoor: false,
    UpRightDoorCheck: false,
    UpRightDoor: false,
    }
  },
  computed: {
  },
  mounted () {
    this.fillChartData()

    this.$buefy.snackbar.open({
      message: 'Welcome back',
      queue: false
    })
  },
  methods: {
    randomChartData (n) {
      let data = []

      for (let i = 0; i < n; i++) {
        data.push(Math.round(Math.random() * 200))
      }

      return data
    },
    fillChartData () {
      this.defaultChart.chartData = {
        datasets: [
          {
            fill: false,
            borderColor: chartConfig.chartColors.default.primary,
            borderWidth: 2,
            borderDash: [],
            borderDashOffset: 0.0,
            pointBackgroundColor: chartConfig.chartColors.default.primary,
            pointBorderColor: 'rgba(255,255,255,0)',
            pointHoverBackgroundColor: chartConfig.chartColors.default.primary,
            pointBorderWidth: 20,
            pointHoverRadius: 4,
            pointHoverBorderWidth: 15,
            pointRadius: 4,
            data: this.randomChartData(9)
          },
          {
            fill: false,
            borderColor: chartConfig.chartColors.default.info,
            borderWidth: 2,
            borderDash: [],
            borderDashOffset: 0.0,
            pointBackgroundColor: chartConfig.chartColors.default.info,
            pointBorderColor: 'rgba(255,255,255,0)',
            pointHoverBackgroundColor: chartConfig.chartColors.default.info,
            pointBorderWidth: 20,
            pointHoverRadius: 4,
            pointHoverBorderWidth: 15,
            pointRadius: 4,
            data: this.randomChartData(9)
          },
          {
            fill: false,
            borderColor: chartConfig.chartColors.default.danger,
            borderWidth: 2,
            borderDash: [],
            borderDashOffset: 0.0,
            pointBackgroundColor: chartConfig.chartColors.default.danger,
            pointBorderColor: 'rgba(255,255,255,0)',
            pointHoverBackgroundColor: chartConfig.chartColors.default.danger,
            pointBorderWidth: 20,
            pointHoverRadius: 4,
            pointHoverBorderWidth: 15,
            pointRadius: 4,
            data: this.randomChartData(9)
          }
        ],
        labels: ['01', '02', '03', '04', '05', '06', '07', '08', '09']
      }
    }
  }
}
</script>
