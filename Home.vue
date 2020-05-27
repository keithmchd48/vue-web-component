<template>
  <div class="container">
    <meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no, user-scalable=no">
    <CountDownTimer></CountDownTimer>
    <div style="height: 100vh; background-color: aqua">
      TariApp
    </div>
    <div style="height: 100vh; background-color: aqua">
      TariApp
    </div>
  </div>
</template>
<script>
  import CountDownTimer from "./CountDownTimer";
  export default {
    name: 'Home',
    components: {
      CountDownTimer
    }
  }
</script>
<style>
  body {
    margin: 0 auto;
  }
</style>
