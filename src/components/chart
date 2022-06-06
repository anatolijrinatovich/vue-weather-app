<template>
  <div class="chart">
    <!--  canvas component for drawing a chart -->
    <canvas id="chart" width="3000px" height="400px"></canvas>
  </div>
</template>

<script>
// import chart.js library (plugin)
import Chart from 'chart.js'
// moment library (plugin), for working with dates and times
import moment from 'moment'

export default {
  name: "chart",
  props: {
    chartData: {},
    chartType: null
  },
  data() {
    // settings for chart js
    return {
      gradient: null,
      data: {
        labels: [],
        datasets: [],
      },
      // setting options chart  js
      options: {
        tooltips: {
          mode: 'index',
          intersect: false,
          yAlign: 'center'
        },
        scales: {
          yAxes: [],
        },
        responsive: false,
        maintainAspectRatio: false,
        legend: {
          align: 'start',
        },
        plugins: {
          colorschemes: {
            scheme: 'brewer.Paired12'          }
        }
      }
    }
  },
  mounted() {
    // creating a chart when component has been mounted
    this.createChart()
  },
  watch: {
    // watching for data changes then updating a chart
    changeData: {
      // will look at her value
      handler(value) {
        // if undefined then resurrect
        if (value.chartData == undefined) return;
        // if hourly or daily undefined then returns
        if (value.chartData.hourly == undefined || value.chartData.daily == undefined) return;
        // cleans datasets
        this.data.datasets = [];
        // Checking chartType
        if (value.chartType) {
          // it will give us the value of ['daily_units'] using the chartDataGenerator function
          this.chartDataGenerator(value.chartData.daily, value.chartData ['daily_units']);
          // it generates elements for us vertically and horizontally
          this.chartDataGenerator(value.chartData ['daily_units'])
          // and put options responsive
          this.options.responsive = true;
        } else {
          // it will give us the value of ['hourly_units'] using the chartDataGenerator function
          this.chartDataGenerator(value.chartData.hourly, value.chartData ['hourly_units'])
          // it generates elements for us vertically and horizontally
          this.chartDataGenerator(value.chartData ['hourly_units'])
          // and put options responsive
          this.options.responsive = false;
        }
        this.createChart()
      }
    },
    deep: true
  },

  methods: {
    // create a chart and draw it
    createChart() {
      // create a variable(ctx) to which we assign the value taken by the ID(chart) from the document. canvas
      const ctx = document.getElementById('chart');
      // check if chartData is undefined, return empty
      if (this.chartData == undefined) return;
      // clear the previous value
      this.data.datasets = [];
      // and do a check chartType
      if (this.chartType) {
        // it will give us the value of ['daily_units'] using the chartDataGenerator function
        this.chartDataGenerator(this.chartData.daily, this.chartData ['daily_units']);
        // it generates elements for us vertically and horizontally
        this.chartUnitsGenerator(this.chartData['daily_units'])
        // and put options responsive
        this.options.responsive = true;
      } else {
        // it will give us the value of ['hourly_units'] using the chartDataGenerator function
        this.chartDataGenerator(this.chartData.hourly, this.chartData['hourly_units'])
        // it generates elements for us vertically and horizontally
        this.chartUnitsGenerator(this.chartData['hourly_units'])
        // and put options responsive
        this.options.responsive = false;
      }
      // create a new chart object, pass canvas (ctx) context there
      new Chart(ctx, {
        // specify the type
        type: 'line',
        // indicate the data, the value of the points
        data: this.data,
        // specify options
        options: this.options
      });
    },
    // generates value of chart
    // function chartDataGenerator takes value and units
    chartDataGenerator(value, units) {
      // if there is a value then
      if (value) {
        // then our label is set by the dateFormating function (forms a date)
        this.data.labels = this.dateFormating(value.time);
        // iterate over each value
        for (const key in value) {
          //   if it is not equal to time
          if (key != 'time') {
            // draw points on the graph
            // then emit datasets push value(key) and return object
            this.data.datasets.push({
                  // we give the label object from our value element ( key )
                  // and clean it by removing the undercut and put a space
                  label: key.replaceAll('_', ' '),
                  // return its value[key]
                  data: value[key],
                  // return units
                  yAxisID: units[key],
                  fill: false
                }
            )
          }
        }
      }
    },
    // generates units of values of chart
    //  the function clears the previous value and sets the new value
    chartUnitsGenerator(value) {
      // clears the previous value
      this.options.scales.yAxes = [];
      // iterate over each value
      for (const key in value) {
        //   if it is not equal to time
        if (key != 'time') {
          // if check then push value [key]
          this.options.scales.yAxes.push({
            id: value[key],
            type: 'linear',
            position: 'left',
            scaleLabel: {
              display: true,
              labelString: value[key],
            },
            ticks: {
              stepSize: 1,
              maxTicksLimit: 8,
            }
          });
        }
      }
    },
    // date and time format function for moment.js
    dateFormating(date) {
      // takes time
      let time = [];
      //iterates over the time from the array that came as an element(key)
      for (const key in date) {
        // if chartType is day then it forms the date as month day year
        if (this.chartType) time[key] = moment(date[key]).format('MMM Do, YYYY');
        // if hourly date then forms as month, day, year and time
        else time[key] = moment(date[key]).format('MMMM Do, YYYY - hh:mm A');
      }
      // calculates the time array
      return time;
    }
  }
}
</script>

<style scoped>
.chart {
  width: 100%;
  overflow-x: scroll;
  overflow-y: hidden;
  height: 417px;
  margin-top: 48px;
  margin-bottom: 48px;
}

canvas {
  display: block;
  width: 3000px;
  height: 400px;
  aspect-ratio: auto 3000 / 400;
}

canvas[Attributes Style] {
  aspect-ratio: auto 3000 / 400;
}

/*canvas [Attributes Style]*/
{
  aspect-ratio: auto 3000 / 400
;
}
</style>
