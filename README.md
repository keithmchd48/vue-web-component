# vue-web-component
An independent Vue web component that can be plugged into any website. It is a count-down timer that shows the time remaining until the datetime provided as props.

## Steps to get started

1. npm install -g @vue/cli-service-global
2. npm install --save moment
3. vue serve Home.vue (Make sure you are in the right directory)

## To build the dist directory
1. vue build CountDownTimer.vue --target wc --name countdown-timer
