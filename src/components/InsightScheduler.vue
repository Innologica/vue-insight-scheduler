/**
* Created by Nikola nb on 18.03.2017.
*/

<template>
    <div>
        <div class="schedule-container">
            <div v-for="day in timerange" class="day header">
                {{ format(day) }}
            </div>
        </div>
        <div class="schedule-container">
            <div v-for="day in timerange" class="day content">
                <template v-for="item in listEvents(day)" >
                    <div class="item" :style="getStyle(item, day)">{{item.start_time}}</div>
                </template>
            </div>
        </div>
    </div>
</template>

<script>
    import Moment from 'moment';
    import {extendMoment} from 'moment-range';

    const moment = extendMoment(Moment);

    export default {
        props: {
            views: {
                default() {
                    return {
                        TimelineWeekView: {
                            component: 'TimelineWeekView'
                        }
                    }
                }
            },
            events: {},
            resources: {
                default() {
                    return ['AllDayEvent']
                }
            }
        },
        data () {
            const start = moment('2017-03-13', 'YYYY-MM-DD')
            const end = moment('2017-03-19', 'YYYY-MM-DD')
            let range1 = moment.range(start, end)

            return {
                config: {
                    firstDayOfWeek: 1
                },
                timerange: Array.from(range1.by('day', {step: 1}))
            }
        },
        methods: {
            format(date) {
                return date.format("ddd MMM D")
            },
            listEvents(date) {
                return this.events.filter((item) => {
                    var date_to = date.clone();
                    //console.log(item.start_time)
                    return item.start_time.isBetween(date, date_to.add(1, 'day'))
                })
            },
            getStyle(event, date) {
                //console.log(event)
                var diff = event.start_time.diff(date)
                var total = 24*1000*60*60
                var perc = parseFloat(diff / total ) * 100
                console.log('diff:', diff, ', total:', total, ' perc:', perc)
                var width = event.end_time.diff(event.start_time)
                var perc_width = parseFloat(width / total) * 100
                return {
                    left: perc + '%',
                    width: perc_width + '%'
                }
            }
        }
    }
</script>

<style>
    .schedule-container {
        display: inline-flex;
        flex-wrap: wrap;
        width: 100%;
        /*height: 100px;*/
        align-items: stretch;
    }

    .schedule-container .day {
        width: calc(100% * (1 / 7) - 12px);
        border: 1px solid #777;
        padding: 5px;
        position: relative;
    }

    .schedule-container .day.content {
        min-height: 20px;
    }

    .schedule-container .day.header {
        background-color: #eee;
    }

    .schedule-container .item {
        position: relative;
        background-color: #eee;
        padding: 5px;
        z-index: 10;
        border: 1px solid #ddd;
        white-space: nowrap;
        overflow: hidden;
        text-overflow: ellipsis;
    }
</style>