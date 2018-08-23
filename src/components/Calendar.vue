<template>
  <div class="cal-container">
    <div class="cal-header">
      <h2>This is the calendar component</h2>
    </div>
    <div class="cal-calendar">
      <h3>{{ month }} - {{ year }}</h3>
      <table class="cal-table">
            <thead>
            <tr>
                <th>Sun</th>
                <th>Mon</th>
                <th>Tue</th>
                <th>Wed</th>
                <th>Thu</th>
                <th>Fri</th>
                <th>Sat</th>
            </tr>
            </thead>

            <tbody ref="calendarbody">

            </tbody>
      </table>
    </div>
  </div>

</template>
<script>
import Vue from "vue"
import * as moment from 'moment'

export default Vue.extend({
  data() {
    return {
      startDay: 15,
      month: 8,
      year: 2018
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
    }
  },
  computed: {
    renderCalendar() {
      //debugger;
      const currentDate = moment([this.year, this.month - 1])
      const firstDay = currentDate.day();
      const daysInMonth = moment([this.year,this.month - 1]).daysInMonth()

      let tbl = this.$refs['calendarbody'] // body of the calendar

      // clearing all previous cells
      tbl.innerHTML = "";

      // creating all cells
      let date = 1;
      for (let i = 0; i < 6; i++) {
        // creates a table row
        let row = document.createElement("tr");
        //creating individual cells, filing them up with data.
        for (let j = 0; j < 7; j++) {
          if (i === 0 && j < firstDay) {
            row.appendChild(this.createCell("", "cell-empty"));
          } else if (date > daysInMonth) {
            break;
          } else {
            if (date < this.startDay) {
              //add blank cell
              row.appendChild(this.createCell("", "cell-empty"));
            } else {
              // let cell = document.createElement("td");
              // let cellText = document.createTextNode(date);
              if (date === currentDate.day() && this.year === currentDate.year() && this.month === currentDate.month() - 1) {
                cell.classList.add("bg-info");
              } // color today's date
              // cell.appendChild(cellText);
              let cell = null;
              if (j === 0 || j === 6) {
                cell = this.createCell(date, "cell-weekend");
              } else {
                cell = this.createCell(date, "cell-weekday");
              }
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
    this.renderCalendar();
  }
});
</script>
<style>
.cell {
  height: 30px;
  width: 30px;
  border: 1px;
}
.cell-weekday {
  background-color: green;
}
.cell-weekend {
  background-color: yellow;
}
.cell-empty {
  background-color: gray;
}
</style>

