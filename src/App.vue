<template>
    <div id="app">
        <img src="./assets/logo.png">
        <insight-scheduler :events="events"></insight-scheduler>
    </div>
</template>

<script>
    import InsightScheduler from './components/InsightScheduler'
    import moment from 'moment';

    export default {
        name: 'app',
        components: {
            InsightScheduler
        },
        data() {
            return {
                events: [
                    { id: 1, start_time: moment('2017-03-16 08:00'), end_time: moment('2017-03-16 19:00') },
                    { id: 2, start_time: moment('2017-03-15 08:00'), end_time: moment('2017-03-16 19:00') }
                ]
            }
        }
    }
</script>

<style>
    #app {
        font-family: 'Avenir', Helvetica, Arial, sans-serif;
        -webkit-font-smoothing: antialiased;
        -moz-osx-font-smoothing: grayscale;
        text-align: center;
        color: #2c3e50;
        margin-top: 60px;
    }
</style>
