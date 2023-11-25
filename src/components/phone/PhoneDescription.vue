<template>
    <div class="grid">
        <div class="col-12 xl:col-12">
            <div class="card ">
                <h5>Recent Sales</h5>
                <div class="flex justify-content-space flex-row flex-wrap" >
                    <div v-for="item in 8" :key="item" class="col-12 lg:col-6 xl:col-2">
                        <div class="card mb-0">
                            <div class="flex justify-content-between mb-3">
                                <div>
                                    <span class="block text-500 font-medium mb-3">{{ item }}</span>
                                    <div class="text-900 font-medium text-xl">152</div>
                                </div>
                                <div class="flex align-items-center justify-content-center bg-blue-100 border-round"
                                     style="width:2.5rem;height:2.5rem">
                                    <i class="pi pi-shopping-cart text-blue-500 text-xl"></i>
                                </div>
                            </div>
                            <span class="text-green-500 font-medium">24 new </span>
                            <span class="text-500">since last visit</span>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
import EventBus from '@/AppEventBus';
import ProductService from '@/service/ProductService';

export default {
    data() {
        return {
            id: 1,
            menuDescription: {},
            subtitleDescription: {},
            products: null,
            lineData: {
                labels: ['January', 'February', 'March', 'April', 'May', 'June', 'July'],
                datasets: [
                    {
                        label: 'Revenue',
                        data: [65, 59, 80, 81, 56, 55, 40],
                        fill: false,
                        backgroundColor: '#2f4860',
                        borderColor: '#2f4860',
                        tension: 0.4
                    },
                    {
                        label: 'Sales',
                        data: [28, 48, 40, 19, 86, 27, 90],
                        fill: false,
                        backgroundColor: '#00bb7e',
                        borderColor: '#00bb7e',
                        tension: 0.4
                    }
                ]
            },
            items: [
                {label: 'Add New', icon: 'pi pi-fw pi-plus'},
                {label: 'Remove', icon: 'pi pi-fw pi-minus'}
            ],
            lineOptions: null,
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
        this.request.get("/detailDescription/phone", {params: {id: 1}}).then(res => {
            this.subtitleDescription = res
        })
        this.request.get("/menuDescription/menu", {params: {id: 1}}).then(res => {
            this.menuDescription = res
        })
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