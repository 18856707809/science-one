<template>
    <div class="layout-footer">
        <img alt="Logo" :src="footerImage()" height="20" class="mr-2"/>
        by
        <span class="font-medium ml-2">科技ONE</span>
    </div>
</template>

<script>
export default {
    name: "AppFooter",
    data() {
        return {
            blackImg: require("@/assets/avatar/black.jpg"),
            whiteImg: require("@/assets/avatar/white.jpg"),
        }
    },
    methods: {
        footerImage() {
            return this.$appState.darkTheme ? this.whiteImg : this.blackImg;
        }
    },
    computed: {
        darkTheme() {
            return this.$appState.darkTheme;
        }
    }
}
</script>