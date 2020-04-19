<template>
  <div class="specific">
    <div class="header"></div>
    <h1 class="name">
      {{customer.firstName + ' ' + customer.lastName + ', ' + getAge(customer.dateOfBirth)}}
    </h1>
    <Chart msg="Glucose" axisY="Value" axisX="Customer" :min=3.9 :max=8.1 :svg_width=300 :svg_height=250 :margin=60 className="glucose" :values="getRawValue(customer.glucose)"/>
  </div>
</template>

<script>
import * as moment from 'moment/moment'
import Chart from './Chart'
export default {
  name: 'Specific',
  props: {
    customer: Object
  },
  components: {
    Chart
  },
  methods: {
    getRawValue (rawValues) {
      return rawValues.map(val => val.value)
    },
    evalValue (rawValue, min, max) {
      if (rawValue <= min) {
        return 'low'
      } else if (rawValue >= max) {
        return 'high'
      } else {
        return 'normal'
      }
    },
    getAge (birthDate) {
      const today = moment()
      const birth = moment(birthDate)
      return Math.floor(moment.duration(today.diff(birth)).asYears())
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
.specific{
 font-family: Avenir, Helvetica, Arial, sans-serif;
}
.header{
  background-color: #3257a8;
  height: 10px;
}

.name{
  text-align: center;
  margin-top: 10px;
}
</style>
