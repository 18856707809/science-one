<template>
    <div id="chart-container"></div>
</template>

<script>
import * as echarts from 'echarts';
import huawei from "@/assets/logo/huawei.png"
import mediatek from "@/assets/logo/mediatek.png"
import apple from "@/assets/logo/apple.png"
import samsung from "@/assets/logo/samsung.png"
import snapdragon from "@/assets/logo/snapdragon.png"

export default {
    name: "BarData",
    data() {
        return {
            //CPU厂商信息
            logos: {
                huawei: huawei,
                apple: apple,
                snapdragon: snapdragon,
                mediatek: mediatek,
                samsung: samsung,
            },
            cpus: [],
            CpuBar: [],
            //添加一个自定义规则对象
            richData: [],
        }
    },
    created() {

    },
    methods: {},
    mounted() {
        this.request.get("/cpu/list").then(res => {
            for (let i = 0; i < 20; i++) {
                this.cpus[i] = res[i]
            }
            this.cpus = this.cpus.sort((a, b) => a.score - b.score)
            // this.cpus = res.sort((a, b) => a.score - b.score)
            for (let i = 0; i < this.cpus.length; i++) {
                let brand = this.cpus[i].title.slice(0, 2)
                this.richData[this.cpus[i].id] = {
                    backgroundColor: {
                        image: brand === "苹果" ? this.logos.apple :
                            (brand === "麒麟" ? this.logos.huawei :
                                (brand === "骁龙" ? this.logos.snapdragon :
                                    (brand === "天玑" ? this.logos.mediatek :
                                        (brand === "三星" ? this.logos.samsung : null))))
                    },
                    height:30,
                }
                this.CpuBar[i] = {
                    value: this.cpus[i].score,
                    itemStyle: {
                        color: brand === "苹果" ? '#5470C6' :
                            (brand === "麒麟" ? "#ea6666" :
                                (brand === "骁龙" ? "#E91422" :
                                    (brand === "天玑" ? "#FAC858" :
                                        (brand === "三星" ? "#72C0DE" : "#3F424C"))))
                    },
                    backgroundStyle: {
                        color: "red",
                    },
                    showBackground: true,
                    barMaxWidth: 80,
                }
            }
            {
                //提取出CPU的名称
                let Title = this.cpus.map(item => {
                    return item.title
                })
                let _this = this
                const dom = document.getElementById('chart-container');
                const myChart = echarts.init(dom, "light", {
                    renderer: 'canvas',
                    useDirtyRect: false
                });

                function fetchData(cb) {
                    // 通过 setTimeout 模拟异步加载
                    setTimeout(function () {
                        cb({
                            data: _this.CpuBar
                        });
                    }, 500);
                }

                let option = {

                    tooltip: {
                        trigger: 'axis',
                        axisPointer: {
                            type: 'shadow'
                        }
                    },
                    legend: {
                        textStyle: {
                            fontSize: "25px",
                        }
                    },
                    grid: {
                        left: '0%',
                        right: '4%',
                        bottom: '0%',
                        containLabel: true
                    },
                    xAxis: {
                        type: 'value',
                        position: 'top',
                        axisLabel: {
                            fontSize:30,
                            color:"black"
                        },
                    },
                    yAxis: {
                        type: 'category',
                        axisLabel: {
                            formatter: function (value, index) {
                                // return '{' + value + '|} {s|' + Title[index] + '}'
                                return Title[index] + '{' + value + '|} '
                            },
                            rich: this.richData,
                            fontSize:30,
                            color:"black"
                        },
                        data: _this.cpus.map(item => item.id),
                    },
                    series: [
                        {
                            // name: 'GeekBench6综合性能排行',
                            type: 'bar',
                            stack: 'total',
                            label: {
                                show: true,
                                fontSize: 30,
                                color:'black'
                            },
                            data: [],
                            itemStyle: {
                                borderRadius: [0, 50, 50, 0],
                            }
                        }
                    ]
                }
                myChart.showLoading();

                fetchData(function (data) {
                    myChart.hideLoading();
                    myChart.setOption(
                        {
                            series: [
                                {
                                    data: data.data,
                                }
                            ],
                        }
                    );
                });

                if (option && typeof option === 'object') {
                    myChart.setOption(option);
                }
                window.addEventListener('resize', () => {
                    myChart.resize()
                });
            }
        })

    },
}
</script>

<style scoped>
* {
    margin: 0;
    padding: 0;
}

#chart-container {
    position: relative;
    height: 2000px;
    overflow: hidden;
}


</style>
