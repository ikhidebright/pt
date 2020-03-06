/* eslint-disable */
<template>
<div>
  <div class="wave-container">
  <div class="container">
      <div class="hero">
      <img class='logo' alt="TIIDELAB logo" src="../assets/TIIDELogo1.png">
      <h1>PTracker</h1>
</div>
</div>
</div>
<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 1440 260"><path fill="#ffb7c5" fill-opacity="1" d="M0,160L11.4,170.7C22.9,181,46,203,69,202.7C91.4,203,114,181,137,165.3C160,149,183,139,206,144C228.6,149,251,171,274,154.7C297.1,139,320,85,343,80C365.7,75,389,117,411,128C434.3,139,457,117,480,112C502.9,107,526,117,549,128C571.4,139,594,149,617,138.7C640,128,663,96,686,96C708.6,96,731,128,754,128C777.1,128,800,96,823,117.3C845.7,139,869,213,891,240C914.3,267,937,245,960,202.7C982.9,160,1006,96,1029,96C1051.4,96,1074,160,1097,176C1120,192,1143,160,1166,165.3C1188.6,171,1211,213,1234,224C1257.1,235,1280,213,1303,170.7C1325.7,128,1349,64,1371,37.3C1394.3,11,1417,21,1429,26.7L1440,32L1440,0L1428.6,0C1417.1,0,1394,0,1371,0C1348.6,0,1326,0,1303,0C1280,0,1257,0,1234,0C1211.4,0,1189,0,1166,0C1142.9,0,1120,0,1097,0C1074.3,0,1051,0,1029,0C1005.7,0,983,0,960,0C937.1,0,914,0,891,0C868.6,0,846,0,823,0C800,0,777,0,754,0C731.4,0,709,0,686,0C662.9,0,640,0,617,0C594.3,0,571,0,549,0C525.7,0,503,0,480,0C457.1,0,434,0,411,0C388.6,0,366,0,343,0C320,0,297,0,274,0C251.4,0,229,0,206,0C182.9,0,160,0,137,0C114.3,0,91,0,69,0C45.7,0,23,0,11,0L0,0Z"></path></svg>
    <div class="container mai">
 <b-row>
        <b-col>
      <div class="input">
      <p>When did your last period start</p>
    <b-form-datepicker id="example-datepicker" v-model="startDate" class="mb-2"></b-form-datepicker>
    <br>
      <p>Period length</p>
    <b-form-input
      type="number"
      min = 1
      max = 10
      v-model="lastPeriodDuration"
      placeholder=""
    ></b-form-input>
    <br>
      <p>What is your cycle length?</p>
    <b-form-input
      type="number"
      v-model="mensCycle"
      placeholder=""
    ></b-form-input>
    <br>
     <b-button :disabled="this.lastPeriodDuration.length < 1 || this.lastPeriodDuration.length > 1 || this.mensCycle.length < 2 || !this.startDate.length" @click="show" pill variant="" class="btnn" size="md">Calculate</b-button>
    </div>
    </b-col>
    <div v-if="this.count > 9">
      <b-col>
        <h6>Ovulating days</h6>
        <b-calendar v-model="value"
        :date-info-fn="dateClass"
        disabled = true
        readonly= true
        label-nav="hide"
        hide-header = true
        selected-variant="removeSelectedCss"
        label-help='Calender showing ovulating days in a selected month'
        locale="en">
        </b-calendar>
        </b-col>
    </div>
        <b-col>
          <img v-if='this.count < 10 && this.count != 0' class='girl' src="https://media.tenor.com/images/b403f6c06bcb9f96d1bad27d7f52f84c/tenor.gif" alt="" srcset="">
          <div class="details" v-if="this.count > 9">
            <h6>Period started</h6>
        {{ showvalue }}
        <br>
        <br>
        <h6>Period ends</h6>
        {{ stopDay }}
        <br>
        <br>
        <h6>Intercourse Window for Pregnancy</h6>
        {{ showvalue2 }} - {{ showvalue3 }}
        <br>
        <br>
        <h6>Next period start</h6>
        {{ nextperiod }}
          </div>
      </b-col>
    </b-row>
  </div>
</div>
</template>

<script>
/* eslint-disable */ 

export default {
  data () {
    return {
      nextperiod: 0,
      count: 0,
      showvalue: '',
      showvalue2: '',
      showvalue3: '',
      value: '',
      startDate: '',
      stopDay: '',
      lastPeriodDuration: '',
      mensCycle: null,
      context: null,
      months: ['January', 'February', 'March', 'April', 'May', 'June', 'July', 'August', 'September', 'October', 'November', 'December'],
      days: ['Sunday', 'Monday', 'Tuesday', 'Wednesday', 'Thursday', 'Friday', 'Saturday']
    }
  },
  methods: {
    nextDate(selectedDateString, selectedPeriod){
    // TODO: adjust code to detect leap year

    const calendar = {
        '1': 31,
        '2': 28,
        '3': 31,
        '4': 30,
        '5': 31,
        '6': 30,
        '7': 31,
        '8': 31,
        '9': 30,
        '10': 31,
        '11': 30,
        '12': 31
    }
    selectedPeriod = Number(selectedPeriod)
    let selectedDateObj =  new Date(selectedDateString)
    let selectedMonth =  selectedDateObj.getMonth() + 1;
    let selectedDay = selectedDateObj.getDate();
    let selectedYear = selectedDateObj.getFullYear();
    let mainDay = selectedDay + selectedPeriod;
    // console.log(Number(Object.keys(calendar)[10]))

    // check if month is from January to November
    if(selectedMonth <= Number(Object.keys(calendar)[10])){
        if( mainDay <= calendar[selectedMonth]){

            return `${selectedYear}-${selectedMonth}-${mainDay}`;

        } else if(mainDay > calendar[selectedMonth]){

            let numberOfDaysInNextMonth = mainDay - calendar[selectedMonth];
            return `${selectedYear}-${selectedMonth + 1}-${numberOfDaysInNextMonth}`;
              
        }
    }else{
        // if month is december
        if(mainDay <= 31){
            return `${selectedYear}-${selectedMonth}-${mainDay}`;
        }else{
            let numberOfDaysInNextMonth = mainDay - calendar[selectedMonth];
            return `${selectedYear + 1}-${1}-${numberOfDaysInNextMonth}`;
        }

    }
},
    dateClass (ymd, date) {
      const pickedDate = new Date(this.startDate).getDate()
      this.value = new Date(this.startDate)
      this.showvalue = this.days[new Date(this.startDate).getDay()] + ', ' + this.months[new Date(this.startDate).getMonth()] + ' ' + new Date(this.startDate).getDate() + ', ' + new Date(this.startDate).getFullYear()
      this.showvalue2 = this.months[new Date(this.startDate).getMonth()] + ' ' + (new Date(this.startDate).getDate() + 9) + ', ' + new Date(this.startDate).getFullYear()
      this.showvalue3 = this.months[new Date(this.startDate).getMonth()] + ' ' + (new Date(this.startDate).getDate() + 16) + ', ' + new Date(this.startDate).getFullYear()
      const day = date.getDate()
      const day2 = this.value.getDate()
      const nxtDate = day2 + Number(this.lastPeriodDuration) - 1
      const dateSet = new Date(this.startDate).setDate(nxtDate)
      this.stopDay = this.days[new Date(dateSet).getDay()] + ' ' + new Date(dateSet).getDate() + ' ' + this.months[new Date(dateSet).getMonth()] + ' ' + new Date(dateSet).getFullYear()
      return day >= pickedDate + 12 && day <= pickedDate + 16 ? 'table-info' : ''
    },
    show () {
      const date = String(this.nextDate(this.startDate, this.mensCycle))
      this.nextperiod = this.days[new Date(date).getDay()] + ', ' + this.months[new Date(date).getMonth()] + ' ' + new Date(date).getDate() + ', ' + new Date(date).getFullYear()
      const loading = setInterval(() => {
        this.count++
      }, 1000)
      if (this.count == 12) {
        clearInterval(loading)
      }
    }
  }
}
</script>

<style>
.btn {
  color: white;
  background-color: color;
}

.logo {
width: 27%;
height: 17vh;
position: relative;
top: 4vh;
right: 3vw;
margin: 0;
}

.details {
  position: relative;
  top: 8vh;
  left: 10vmin;
  transition-duration: 400ms;
}

.input {
  width: 80%
}

h6 {
  font-family: Fredoka One;
}

p {
  font-size: 1rem;
  font-family: Noto Sans SC;
}

.btnn {
  background-color: #ffb7c5;
  border: none
}

.btnn:hover {
  background-color: #641664;
}

i {
  background-color: #ffb7c5;
  padding: 20px 0 0 22px;
  color: white;
  width: 65px;
  height: 65px;
  border-radius: 50%;
  font-size: 4vmin;
  margin-bottom: 2vmin;
}

.mai {
  height: 60vh;
}

.moveInUp-enter-active {
  opacity: 0;
  transition: opacity 1s ease-in;
}

.bv-example-row {
  width: 75%;
  margin: 0 auto;
}

body {
  margin: 0;
  font-family: system-ui, sans-serif;
}
.wave-container {
  position: relative;
  /* top: 5vmin;
  margin: 0;
  box-sizing: border-box; */
  overflow: hidden;
}

/* h1 {
  font-size: 5rem;
  margin: 7rem 1.25rem 2.5rem 1.25rem;
}
p {
  font-size: 1.5rem;
  margin: 0 1.25rem 5rem 1.25rem;
} */

.girl {
  width: 40vmin;
  height: 40vmin;
  position: relative;
  left: 16vh;
  top: 5vh;
  border-radius: 50%;
}

html {
  box-sizing: border-box;
}
*,*::before,*::after {
  box-sizing: inherit;
}
body {
  margin: 0;
}
.wave-container {
  background: #ffb7c5;
}
.wave-container > svg {
  display: block;
}

.hero {
  background: #ffb7c5;
  width: 90%;
  margin: 0 auto;
  color: white;
}

.hero h1 {
  font-size: 4rem;
  font-family: Righteous;
}

.hero p {
  font-size: 1rem;
  font-family: Noto Sans SC;
}
</style>
