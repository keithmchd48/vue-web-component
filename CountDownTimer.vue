<template>
  <transition-group name="slide" tag="div">
    <div key="1" class="timer-container" v-if="show">
      <div class="timer-container-flex">
        <div class="timer-flex">
          <div class="timer-limited">LIMITED TIME OFFER!</div>
          <div v-html="timer"></div>
        </div>
      </div>
    </div>
    <div key="2" class="timer-dummy" v-if="show">
      <div class="timer-container-flex">
        <div class="timer-flex">
          <div class="timer-limited">LIMITED TIME OFFER!</div>
          <div v-html="timer"></div>
        </div>
      </div>
    </div>
  </transition-group>
</template>

<script>
  import moment from 'moment/moment'
  export default {
    name: 'CountDownTimer',
    props: {
      // date format should be in 'YYYY-MM-DD, h:mm:ss a' (eg. 2020-10-02, 10:45:00 pm)
      dateUntil: {
        type: String,
        default: ''
      }
    },
    computed: {
      countdown () {
        // if "this.dateUntil" is not a valid date, then consider the default date as today's midnight
        return moment(this.dateUntil, 'YYYY-MM-DD, h:mm:ss a').valueOf() || moment().endOf('day').valueOf()
      }
    },
    data () {
      return {
        // the time left to be displayed in template in days, hours, mins, seconds
        timer: 0,
        // setInterval() object will be stored in this variable
        timerInterval: null,
        // countdown: moment().endOf('day').valueOf(),
        // countdown: moment('2020-10-02, 2:11:00 pm', 'YYYY-MM-DD, h:mm:ss a').valueOf(),
        show: false
      }
    },
    mounted() {
      window.addEventListener('scroll', this.scrollListener)
      // trigger setInterval()
      this.startTimer()
    },
    methods: {
      scrollListener () {
        if (document.body.scrollTop > 60 || document.documentElement.scrollTop > 60) this.show = true
      },
      // run this method every 1 seconds through setInterval()
      runTimer() {
        let now = new Date().getTime()
        let distance = this.countdown - now
        let days = Math.floor(distance / (1000 * 60 * 60 * 24));
        let hours = Math.floor((distance % (1000 * 60 * 60 * 24)) / (1000 * 60 * 60));
        let minutes = Math.floor((distance % (1000 * 60 * 60)) / (1000 * 60));
        let seconds = Math.floor((distance % (1000 * 60)) / 1000);
        if (distance <= 0) {
          this.onTimesUp()
          return
        }
        function pad(n) {
          return (n < 10 ? "0" + n : n);
        }
        this.timer = `<div class="timer-expire"><span>Expires in: <span class="timer-num">${days}</span> days <span class="timer-num">${pad(hours)}</span> hours <span class="timer-num">${pad(minutes)}</span> min <span class="timer-num">${pad(seconds)}</span> sec</span></div>`
      },
      // when the time left is 0 seconds or less, call this method
      onTimesUp() {
        this.timer = `<div class="timer-expire"><span>Expires in: <span class="timer-num">00</span> days <span class="timer-num">00</span> hours <span class="timer-num">00</span> min <span class="timer-num">00</span> sec</span></div>`
        clearInterval(this.timerInterval);
      },
      startTimer() {
        this.timerInterval = setInterval(() => {
          this.runTimer()
        }, 1000);
      }
    }
  }
</script>

<style>
/* Timer bar slides in from top */
  .slide-enter {
    transform: translateY(-100%);
  }
  .slide-enter-active {
    transition: transform 0.5s;
  }
  .timer-dummy, .timer-container {
    top: 0;
    width: 100%;
    font-family: SansSerif, sans-serif;
    text-align: center;
    background-color: #ffeedf;
    padding-top: 1em;
    padding-bottom: 1em;
  }
  .timer-dummy {
    visibility: hidden;
  }
  .timer-container {
    position: fixed;
    z-index: 8;
  }
  .timer-limited {
    color: red;
    font-weight: bold;
    margin-right: 6px;
  }
  .timer-flex {
    display: flex;
    justify-content: center;
    flex-wrap: wrap;
    width: 70%;
  }
  .timer-container-flex {
    display: flex;
    justify-content: center;
  }
  .timer-expire {
    font-size: 14px;
    font-weight: lighter;
  }
  .timer-num {
    color: red;
    font-weight: bold;
  }
</style>
