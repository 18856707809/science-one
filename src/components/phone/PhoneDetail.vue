<template>
    <div class="grid p-fluid">
        <div class="col-12">
                <div class="product-item">
                    <div class="product-item-content" style="border: 0">
                        <div class="mb-3">
                            <img :src="'images/phone/' + product.image" :alt="product.name" class="product-image"/>
                        </div>
                        <div>
                            <h4 class="mb-1">
                                {{ product.name }}
                            </h4>
                            <span class="product-badge">¥{{ product.price }}</span>
                            <div class="car-buttons mt-5">
                                <Button type="button" class="p-button p-button-rounded mr-2"
                                        icon="pi pi-search"></Button>
                                <Button type="button" class="p-button-success p-button-rounded mr-2"
                                        icon="pi pi-star-fill"></Button>
                                <Button type="button" class="p-button-help p-button-rounded" icon="pi pi-cog"></Button>
                            </div>
                        </div>
                    </div>
                </div>
            <!--                1111111111-->
            <Timeline :value="customEvents" align="alternate" class="customized-timeline">
                <template #marker="slotProps">
                <span class="flex w-2rem h-2rem align-items-center justify-content-center text-white border-circle z-1 shadow-2"
                      :style="{backgroundColor: slotProps.item.color}">
                    <i :class="slotProps.item.icon"></i>
                </span>
                </template>
                <template #content="slotProps">
                    <h3 class="mb-2">{{slotProps.item.status}}</h3>
                    <span>
                        <span class="mb-2">{{slotProps.item.date}}:</span>
                        <span class="mb-2">{{slotProps.item.date}}:</span>
                    </span>
                </template>
            </Timeline>
        </div>
    </div>
</template>

<script>
export default {
    data() {
        return {
            customEvents: [
                {
                    status: '处理器',
                    date: '骁龙8Gen3',
                    icon: 'pi pi-shopping-cart',
                    color: '#9C27B0',
                },
                {
                    status: '相机',
                    date: '15/10/2020 14:00',
                    icon: 'pi pi-cog',
                    color: '#673AB7'
                },
                {
                    status: '电池',
                    date: '15/10/2020 16:15',
                    icon: 'pi pi-envelope',
                    color: '#FF9800'
                }
            ],
            product: {
                "id": "1000",
                "code": "f230fh0g3",
                "name": "iPhone 15 Pro Max",
                "description": "Product Description",
                "image": "iphone14promax.png",
                "price": 9899,
                "category": "Accessories",
                "inventoryStatus": "在售",
            },
            images: [],
        };
    },
    created() {

    },
    mounted() {

    },
};
</script>

<style lang="scss" scoped>
@import '@/assets/demo/badges.scss';
@import '@/assets/demo/items.scss';

@media screen and (max-width: 960px) {
  ::v-deep(.customized-timeline) {
    .p-timeline-event:nth-child(even) {
      flex-direction: row !important;

      .p-timeline-event-content {
        text-align: left !important;
      }
    }

    .p-timeline-event-opposite {
      flex: 0;
    }

    .p-card {
      margin-top: 1rem;
    }
  }
}
</style> 
