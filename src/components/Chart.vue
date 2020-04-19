<template>
  <div :class="className">
    <svg></svg>
  </div>
</template>

<script>
import * as d3 from 'd3'
// import * as moment from 'moment/moment'

export default {
  name: 'Chart',
  props: {
    msg: String,
    svg_width: Number,
    svg_height: Number,
    margin: Number,
    values: Array,
    className: String,
    min: Number,
    max: Number
  },
  data () {
    return {
      chart: null
    }
  },
  mounted: function () {
    console.log(this.values)
    if (this.chart == null && this.values) this.renderChart(this.values)
  },
  methods: {
    renderChart (data) {
      // const minAcceptableValue = this.min
      // const maxAcceptableValue = this.max
      const max = Math.max(...data)
      const chartWidth = this.svg_width - (2 * this.margin)
      const chartHeight = this.svg_height - (2 * this.margin)

      const x = d3.scaleLinear()
        .range([0, chartWidth])
        .domain([0, data.length - 1])

      const y = d3.scaleLinear()
        .range([chartHeight, 0])
        .domain([0, max])

      const xAxis = d3.axisBottom(x).tickFormat(function (d) { return d.x })

      const yAxis = d3.axisLeft(y)

      const line = d3.area()
        .x(function (d, i) { return x(i) })
        .y1(function (d) { return y(d) })
        .y0(chartHeight)
        .curve(d3.curveLinear)

      const svg = d3.select('svg')
        .attr('width', chartWidth + 60)
        .attr('height', chartHeight + 50)
        .append('g')
        .attr('transform', 'translate(50, 10)')

      svg.append('g')
        .attr('class', 'x axis')
        .attr('transform', 'translate(0,' + chartHeight + ')')
        .call(xAxis)

      svg.append('text').attr('x', (chartWidth / 2))
        .attr('y', chartHeight + this.margin)
        .attr('text-anchor', 'middle')
        .style('font-size', '16px')
        .style('text-decoration', 'underline')
        .style('text-decoration', 'underline')
        .text('Blood sugar')

      svg.append('g')
        .attr('class', 'y axis')
        .call(yAxis)

      svg.append('path')
        .datum(data)
        .attr('class', 'line')
        .attr('d', line)
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
</style>
