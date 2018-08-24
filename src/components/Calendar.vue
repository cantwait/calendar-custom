<template>
  <div >
    <b-card :title="monthTitle">
      <p class="card-text">
        <table class="calendar-table">
            <thead>
            <tr>
                <th>S</th>
                <th>M</th>
                <th>T</th>
                <th>W</th>
                <th>T</th>
                <th>F</th>
                <th>S</th>
            </tr>
            </thead>
            <tbody ref="calendarbody">
            </tbody>
      </table>
      </p>
    </b-card>
  </div>

</template>
<script>
import Vue from "vue"
import * as moment from 'moment'

export default Vue.extend({
  props: ['inputs'],
  data() {
    return {
      startDay: 0,
      month: 0,
      year: 0,
      endDay: 0
    };
  },
  methods: {
    createCell(text, ...className) {
      const cell = document.createElement("td")

      //cell.classList.add(className)
      this.addClassesToElement(cell, className);
      const cellText = document.createTextNode(text)
      cell.appendChild(cellText)
      return cell
    },
    addClassesToElement(el, ...classNames) {
      el.classList.add("cell"); //default
      for (let i = 0; i < classNames.length; i++) {
        el.classList.add(classNames[i]);
      }
    },
    toMonthString() {
      return moment([this.year, this.month]).format('MMMM')
    }
  },
  computed: {
    monthTitle: function() {
      return this.toMonthString() + ' ' + this.year
    },
    renderCalendar() {
      const currentDate = moment([this.year, this.month])
      const firstDay = currentDate.day();
      const daysInMonth = moment([this.year,this.month]).daysInMonth()
      //const endDayOfMonth = moment([this.year, this.month,this.endDay]).day()

      let tbl = this.$refs['calendarbody'] // body of the calendar

      // clearing all previous cells
      tbl.innerHTML = "";
      let days = 0;
      // creating all cellsd
      let date = 1;
      for (let i = 0; i < 6; i++) {
        // creates a table row
        let row = document.createElement("tr");
        //creating individual cells, filing them up with data.
        for (let j = 0; j < 7; j++) {
          if ((i === 0 && j < firstDay) || days >= this.endDay) {
            row.appendChild(this.createCell("", "cell-empty"));
          } else if (date > daysInMonth) {
            row.appendChild(this.createCell("", "cell-empty"));
          } else {
            if (date < this.startDay) {
              //add blank cell
              row.appendChild(this.createCell("", "cell-empty"));
              days++
            } else {
              // let cell = document.createElement("td");
              // let cellText = document.createTextNode(date);
              // cell.appendChild(cellText);
              let cell = null;
              if (j === 0 || j === 6) {
                cell = this.createCell(date, "cell-weekend");
              } else {
                cell = this.createCell(date, "cell-weekday");
              }
              days++;
              row.appendChild(cell);
            }

            date++;
          }

        }

        tbl.appendChild(row); // appending each row into calendar body.
      }
    }
  },
  mounted() {
    this.startDay = this.inputs.startDay
    this.endDay = this.inputs.endDay
    this.year = this.inputs.year
    this.month = this.inputs.month
    console.log(JSON.stringify(this.inputs));
    this.renderCalendar();
  }
});
</script>
<style>
.cell {
  height: 30px;
  width: 30px;
}
.cell-weekday {
  background-color: lightgreen;
}
.cell-weekend {
  background-color: yellow;
}
.cell-empty {
  background-color: lightgray;
}
table, th, td, tr {
  border: 1px solid white
}
</style>

