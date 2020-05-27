<template>
  <div>
    <div class="timer-container" v-show="show">
      <div class="timer-container-flex">
        <div class="timer-flex">
          <div class="timer-limited">LIMITED TIME OFFER!</div>
          <div v-html="timer"></div>
        </div>
      </div>
    </div>
    <div class="timer-dummy" v-show="show">
      <div class="timer-container-flex">
        <div class="timer-flex">
          <div class="timer-limited">LIMITED TIME OFFER!</div>
          <div v-html="timer"></div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
  import moment from 'moment/moment'
  export default {
    name: 'CountDownTimer',
    data () {
      return {
        timer: 0,
        timerInterval: null,
        countdown: moment().endOf('day').valueOf(),
        show: false
      }
    },
    mounted() {
      window.addEventListener('scroll', this.scrollListener)
      this.startTimer()
    },
    methods: {
      scrollListener () {
        if (document.body.scrollTop > 60 || document.documentElement.scrollTop > 60) this.show = true
      },
      runTimer() {
        let now = new Date().getTime()
        let distance = this.countdown - now
        let days = Math.floor(distance / (1000 * 60 * 60 * 24));
        let hours = Math.floor((distance % (1000 * 60 * 60 * 24)) / (1000 * 60 * 60));
        let minutes = Math.floor((distance % (1000 * 60 * 60)) / (1000 * 60));
        let seconds = Math.floor((distance % (1000 * 60)) / 1000);
        function pad(n) {
          return (n < 10 ? "0" + n : n);
        }
        this.timer = `<div class="timer-expire"><span>Expires in: <span class="timer-num">${days}</span> days <span class="timer-num">${pad(hours)}</span> hours <span class="timer-num">${pad(minutes)}</span> min <span class="timer-num">${pad(seconds)}</span> sec</span></div>`

        if (this.distance < 0) {
          this.onTimesUp()
        }
      },
      onTimesUp() {
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
