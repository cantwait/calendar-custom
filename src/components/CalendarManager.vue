<template>
  <div>
    <calendar-form @on-submit-values="onSubmitValues" @on-reset-form="onResetForm"/>
    <br>
    <br>
    <template v-if="monthsLengthGreaterOrEqualToOne">
      <div v-for="(m, i) in dateArr" :key="i" >
        <calendar  v-bind:inputs="m" />
      </div>
    </template>
  </div>
</template>
<script>
import Vue from 'vue';
import CalendarForm from './CalendarForm'
import Calendar from './Calendar'
import * as moment from 'moment'

export default Vue.extend({
  data: function(){
    return {
      show: false,
      dateArr: []
    }
  },
  computed: {
    monthsLengthGreaterOrEqualToOne() {
      const isGreaterThanOne = this.dateArr.length >= 1
      console.log('is greater than one? ' + isGreaterThanOne)
      return isGreaterThanOne
    }
  },
  components: {
    'calendar-form': CalendarForm,
    'calendar': Calendar
  },
  methods: {
    onResetForm() {
      console.log('onResetForm')
      this.dateArr.splice(0,this.dateArr.length)
    },
    onSubmitValues(data) {
      console.log('onSubmitValues')
      this.buildArrayOfMonths(data.startDate, data.endDate)
    },
    getTimeSeries(startDate, endDate) {
      let dateArray = [];
      let currentDate = moment(startDate);
      let eDate = moment([endDate.year(), endDate.month(), endDate.daysInMonth()])
      while (currentDate <= eDate) {
          dateArray.push( moment(currentDate).format('YYYY-MM-DD') )
          currentDate = moment(currentDate).add(1, 'months');
      }
      return dateArray;
    },
    buildArrayOfMonths(startDate, endDate) {
      const timeSeries = this.getTimeSeries(startDate, endDate);

      for (let i = 0; i < timeSeries.length; i++) {
        let startDay = 1;
        if (i === 0) {
          startDay = moment(startDate).date();
        }

        let endDay = endDay = moment(timeSeries[i]).daysInMonth()
        if (i === timeSeries.length - 1 && timeSeries.length > 1) {
          endDay = moment(endDate).date()
        }
        this.dateArr.push({
          startDay,
          year: moment(timeSeries[i]).year(),
          month: moment(timeSeries[i]).month(),
          endDay: endDay !== null ? endDay : null
        })
      }

      console.log(JSON.stringify(this.dateArr));
    }
  }
})
</script>
<style scoped>

</style>
