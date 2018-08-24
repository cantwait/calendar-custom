<template>
  <div>
    <b-form @submit="onSubmit" @reset="onReset" v-if="show">
      <b-form-group id="exampleInputGroup1"
                    label="Start Date"
                    label-for="exampleInput1">
        <b-form-input id="exampleInput1"
                      type="date"
                      v-model="form.startDate"
                      required
                      placeholder="Select The starting date...">
        </b-form-input>
      </b-form-group>
      <b-form-group id="exampleInputGroup2"
                    label="Number of days"
                    label-for="exampleInput2">
        <b-form-input id="exampleInput2"
                      type="number"
                      v-model.number="form.days"
                      required
                      placeholder="Type the number of days">
        </b-form-input>
      </b-form-group>
      <b-form-group id="exampleInputGroup3"
                    label="Country Code:"
                    label-for="exampleInput3">
        <b-form-input id="exampleInput3"
                      type="string"
                      v-model="form.countryCode"

                      placeholder="Country Code e.g: US">
        </b-form-input>
      </b-form-group>
      <b-button type="submit" v-if="!isSubmitted" variant="primary">Calculate</b-button>
      <b-button type="reset" variant="danger">Reset</b-button>
    </b-form>
  </div>
</template>
<script>
import Vue from 'vue'
import * as moment from 'moment';

export default Vue.extend({
  data () {
    return {
      form: {
        startDate: null,
        days: 0,
        countryCode: '',
      },
      isSubmitted: false,
      show: true
    }
  },
  computed: {
    isValidData() {
      return this.validData();
    }
  },
  methods: {
    validData() {
      return this.form.startDate != null && this.form.days > 0;
    },
    calculateEndDate() {
      return this.validData() ? moment(this.form.startDate).add(this.form.days,'days') : null
    },
    onSubmit (evt) {
      evt.preventDefault();
      this.isSubmitted = true;
      moment(this.calculateEndDate(),'yyyy-mm-dd');

      const data = {
        startDate: this.form.startDate,
        endDate: this.calculateEndDate(),
        countryCode: this.form.countryCode
      }

      this.$emit('on-submit-values', data);

    },
    onReset (evt) {
      evt.preventDefault();
      /* Reset our form values */
      this.form.startDate = null;
      this.form.days = 0;
      this.form.countryCode = '';
      /* Trick to reset/clear native browser form validation state */
      this.show = false;
      this.$nextTick(() => { this.show = true });
      this.isSubmitted  = false
      this.$emit('on-reset-form');
    }
  }
});
</script>

<style>

</style>
