<template>
    <div id="canvasContainer">
    </div>
</template>
<script>
    import {
        Chart,
        ArcElement,
        LineElement,
        BarElement,
        PointElement,
        BarController,
        BubbleController,
        DoughnutController,
        LineController,
        PieController,
        PolarAreaController,
        RadarController,
        ScatterController,
        CategoryScale,
        LinearScale,
        LogarithmicScale,
        RadialLinearScale,
        TimeScale,
        TimeSeriesScale,
        Decimation,
        Filler,
        Legend,
        Title,
        Tooltip,
        SubTitle
    } from 'chart.js';

    Chart.register(
        ArcElement,
        LineElement,
        BarElement,
        PointElement,
        BarController,
        BubbleController,
        DoughnutController,
        LineController,
        PieController,
        PolarAreaController,
        RadarController,
        ScatterController,
        CategoryScale,
        LinearScale,
        LogarithmicScale,
        RadialLinearScale,
        TimeScale,
        TimeSeriesScale,
        Decimation,
        Filler,
        Legend,
        Title,
        Tooltip,
        SubTitle
    );
    
    export default {
        name: 'Chart',
        props: {
            data: Array
        },
        watch: {
            data: {
                handler: 'renderChart',
                deep: true
            }
        },
        data() {
            return {
                barChart: null
            }
        },
        methods: {
            renderChart () {
                const canvasContainer = document.querySelector('#canvasContainer');
                canvasContainer.innerHTML = "";
                let canvas = document.createElement('canvas');
                canvas.id = "bars";
                canvasContainer.appendChild(canvas);
                const labels = [
                    'January',
                    'February',
                    'March',
                    'April',
                ];
                const data = {
                    labels: labels,
                    datasets: [{
                        label: 'Ventas',
                        backgroundColor: 'rgb(255, 99, 132)',
                        borderColor: 'rgb(255, 99, 132)',
                        data: this.data,
                    }]
                };
                const config = {
                    type: 'bar',
                    data: data,
                    options: {}
                }
                this.barChart = new Chart(document.querySelector("#bars"), config);
            }
        },
        mounted () {
            this.renderChart();
        }   
    }
</script>