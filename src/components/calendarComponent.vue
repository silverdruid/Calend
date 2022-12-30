<template>
  <div id="calendar">
    <table>
      <thead id="thead-month">
        <tr>
          <th v-for="day in days" :key="day">{{ day }}</th>
        </tr>
      </thead>
      <tbody id="calendar-body">
        <tr v-for="(week, index) in calendar" :key="index">
          <td v-for="day in week" :key="day.date">{{ day.date }}</td>
        </tr>
      </tbody>
    </table>

    <div class=".monthAndYear">
      <button @click="previous" class="previous">Previous</button>

      <span id="monthAndYear">{{ monthAndYear }}</span>

      <button @click="next" class="next">Next</button>
    </div>

    <div>
      <label for="month">Month:</label>
      <select id="month" v-model="selectedMonth">
        <option v-for="(month, index) in months" :value="index" :key="month">{{ month }}</option>
      </select>

      <label for="year">Year:</label>
      <select id="year" v-model="selectedYear">
        <option v-for="year in yearOptions" :key="year">{{ year }}</option>
      </select>

      <button @click="jump">Jump</button>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      currentMonth: (new Date()).getMonth(),
      currentYear: (new Date()).getFullYear(),
      selectedMonth: null,
      selectedYear: null,
      yearOptions: [],
      months: [],
      days: [],
      calendar: []
    }
  },

  created() {
    this.selectedMonth = this.currentMonth
    this.selectedYear = this.currentYear
    this.yearOptions = this.generateYearRange(1970, 2050)
    // or this.yearOptions = this.generateYearRange(1970, this.currentYear)
    this.months = this.getMonthNames()
    this.days = this.getDayNames()
    this.calendar = this.getCalendar(this.currentMonth, this.currentYear)
  },

  methods: {
    generateYearRange(start, end) {
      const yearOptions = []
      for (let year = start; year <= end; year++) {
        yearOptions.push(year)
      }
      return yearOptions
    },

    getMonthNames() {
      const monthDefault = ["January", "February", "March", "April", "May", "June", "July", "August", "September", "October", "November", "December"]
        return monthDefault
     
     },
    getDayNames() {
      const dayDefault = ["Sun", "Mon", "Tue", "Wed", "Thu", "Fri", "Sat"]
      return dayDefault;
   },

    getCalendar(month, year) {
      const firstDay = (new Date(year, month)).getDay()
      const calendar = []
      let date = 1
      for (let i = 0; i < 6; i++) {
        const week = []
        for (let j = 0; j < 7; j++) {
          if (i === 0 && j < firstDay) {
            week.push({ date: '' })
          } else if (date > this.getDaysInMonth(month, year)) {
            week.push({ date: '' })
          } else {
            week.push({ date })
            date++
          }
        }
        calendar.push(week)
      }
      return calendar
    },

    getDaysInMonth(month, year) {
      return new Date(year, month + 1, 0).getDate()
    },

    next() {
      this.currentYear = (this.currentMonth === 11) ? this.currentYear + 1 : this.currentYear
      this.currentMonth = (this.currentMonth + 1) % 12
      this.calendar = this.getCalendar(this.currentMonth, this.currentYear)
      this.updateMonthAndYear()
    },

    previous() {
      this.currentYear = (this.currentMonth === 0) ? this.currentYear - 1 : this.currentYear
      this.currentMonth = (this.currentMonth === 0) ? 11 : this.currentMonth - 1
      this.calendar = this.getCalendar(this.currentMonth, this.currentYear)
      this.updateMonthAndYear()
    },

    jump() {
      this.currentYear = this.selectedYear
      this.currentMonth = this.selectedMonth
      this.calendar = this.getCalendar(this.currentMonth, this.currentYear)
      this.updateMonthAndYear()
    },

    updateMonthAndYear() {
      this.monthAndYear = `${this.months[this.currentMonth]} ${this.currentYear}`
    },
  }
}
</script>


<style>
table {
      font-family: Arial, sans-serif;
      font-size: 14px;
      border-collapse: collapse;
      width: 100%;
    }
  
    th {
      background-color: #ddd;
      border: 1px solid #ccc;
      text-align: center;
      padding: 8px;
    }
  
    td {
      border: 1px solid #ccc;
      text-align: center;
      padding: 8px;
    }
  
    tr:nth-child(even) {
      background-color: #f2f2f2;
    }
    button {
      font-family: Arial, sans-serif;
      font-size: 14px;
      background-color: #ddd;
      border: none;
      color: black;
      padding: 8px 16px;
      cursor: pointer;
      outline: none;
      }
      
      button:hover {
        background-color: #ccc;
      }
      
      select {
        font-family: Arial, sans-serif;
        font-size: 14px;
        padding: 8px 16px;
        border: 1px solid #ccc;
        appearance: none;
        background-color: white;
        background-image: url('data:image/svg+xml;charset=US-ASCII,%3Csvg%20xmlns%3D%22http%3A%2F%2Fwww.w3.org%2F2000%2Fsvg%22%20width%3D%22292.4%22%20height%3D%22292.4%22%3E%3Cpath%20fill%3D%22%23007CB2%22%20d%3D%22M287%2069.4a17.6%2017.6%200%200%200-13-5.4H18.4c-5%200-9.3%201.8-12.9%205.4A17.6%2017.6%200%200%200%200%2082.2c0%205%201.8%209.3%205.4%2012.9l128%20127.9c3.6%203.6%207.8%205.4%2012.8%205.4s9.2-1.8%2012.8-5.4L287%2095c3.5-3.5%205.4-7.8%205.4-12.8%200-5-1.9-9.2-5.5-12.8z%22%2F%3E%3C%2Fsvg%3E'),
          linear-gradient(to bottom, #ffffff 0%, #e5e5e5 100%);
        background-repeat: no-repeat, repeat;
        background-position: right 0.7em top 50%, 0 0;
        background-size: 0.65em auto, 100%;
      }
      
      label {
        font-family: Arial, sans-serif;
        font-size: 14px;
        margin-right: 8px;
      }
      .previous{
        margin-right: 100px;
      }

      .next {
        margin-left: 100px;
      }

      .monthAndYear{
        margin-top: 40px;
      }
</style>