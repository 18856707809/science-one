<template>
    <div class="grid">
        <div class="col-12 xl:col-12">
            <div class="caption text-900 font-medium">2023手机CPU综合性能排行</div>
            <div class="notice text-800">CPU权重70% ,GPU权重30%。不包含功耗、AI、ISP基带性能。以骁龙865为基准</div>
        </div>
        <div class="col-10" style="flex: 0 0 auto;padding: 0.5rem;width: 20%;"
             v-for="({colorText, companyLogo, companyName, newChip, newPhone}) in brandList"
             :key="companyName">
            <div class="card mb-0">
                <div class="flex justify-content-between mb-3">
                    <div>
                        <span class="block text-500 font-medium mb-3">{{ companyName }}</span>
                        <div class="text-900 font-medium">代表作：<br>{{ newChip }}</div>
                    </div>
                    <div class="flex align-items-center justify-content-center bg-blue-100 border-round">
                        <!--                        <ImagePreview src="@/assets/.png" alt="Image Text" />-->
                        <img :src="companyLogo" alt="" style="height:5rem">
                        <!--                        <i class="pi pi-shopping-cart text-blue-500 text-xl"></i>-->
                    </div>
                </div>
                <span class="text-500">代表机型：</span>
                <span :class="colorText">{{ newPhone }}</span>
            </div>
        </div>
        <div class="col-12 xl:col-12 ">
            <div class="card">
                <CpuBar></CpuBar>
            </div>
        </div>
    </div>
</template>

<script>
import EventBus from '@/AppEventBus';
import ProductService from '@/service/ProductService';
import CpuBar from "@/components/cpu/CpuBar.vue";

export default {
    components: {
        CpuBar
    },
    data() {
        return {
            stockList: [],
            brandList: [],
            products: null,
            items: [
                {label: 'Add New', icon: 'pi pi-fw pi-plus'},
                {label: 'Remove', icon: 'pi pi-fw pi-minus'}
            ],
        }
    },
    productService: null,
    themeChangeListener: null,
    mounted() {
        this.productService.getProductsSmall().then(data => this.products = data);

        this.themeChangeListener = (event) => {
            if (event.dark)
                this.applyDarkTheme();
            else
                this.applyLightTheme();
        };
        EventBus.on('change-theme', this.themeChangeListener);

        if (this.isDarkTheme()) {
            this.applyDarkTheme();
        } else {
            this.applyLightTheme();
        }
    },
    beforeUnmount() {
        EventBus.off('change-theme', this.themeChangeListener);
    },
    created() {
        this.request.get("/company/list").then(res => {
            this.brandList = res
            console.log(res)
        })

        //每当有人打开这个页面就new出一个当前日期：xxxx/ss/mm
        let timestamp = Date.parse(new Date().toString());
        let date = timeFormat(new Date(timestamp));

        function timeFormat(date) {
            let year = date.getFullYear()
            let month = (date.getMonth() + 1).toString().length < 2 ? '0' + (date.getMonth() + 1) : (date.getMonth() + 1);
            let day = date.getDate().toString().length < 2 ? '0' + date.getDate() : date.getDate();
            return year + '-' + month + '-' + day;
        }

        console.log(date)


        this.productService = new ProductService();
    },
    methods: {
        formatCurrency(value) {
            return value.toLocaleString('en-US', {style: 'currency', currency: 'USD'});
        },
        isDarkTheme() {
            return this.$appState.darkTheme === true;
        },
        applyLightTheme() {
            this.lineOptions = {
                plugins: {
                    legend: {
                        labels: {
                            color: '#495057'
                        }
                    }
                },
                scales: {
                    x: {
                        ticks: {
                            color: '#495057'
                        },
                        grid: {
                            color: '#ebedef',
                        }
                    },
                    y: {
                        ticks: {
                            color: '#495057'
                        },
                        grid: {
                            color: '#ebedef',
                        }
                    },
                }
            };
        },
        applyDarkTheme() {
            this.lineOptions = {
                plugins: {
                    legend: {
                        labels: {
                            color: '#ebedef'
                        }
                    }
                },
                scales: {
                    x: {
                        ticks: {
                            color: '#ebedef'
                        },
                        grid: {
                            color: 'rgba(160, 167, 181, .3)',
                        }
                    },
                    y: {
                        ticks: {
                            color: '#ebedef'
                        },
                        grid: {
                            color: 'rgba(160, 167, 181, .3)',
                        }
                    },
                }
            };
        }
    }
}
</script>
<style scoped>

.caption {
    text-align: center;
    letter-spacing: 3px;
    font-size: 60px;
    margin: 0;
}

.notice {
    text-align: center;
    margin: 20px 0 20px 0;
}
</style>