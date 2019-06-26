<template>
    <div :style="realStyle">
    </div>
</template>
<script>
const eeui = weex.requireModule('eeui');

export default {
    name: 'StatusBar',
    data() {
        return {
            history: [],
            meitu: [],
            page: 1,
            StatusBarHeight: eeui.getStatusBarHeightPx()
        }
    },
    props: {
        customStyle: {
            type: Object,
            default: {},
        },
    },
    methods: {},
    computed: {
        realStyle() {
            let style = {
                backgroundColor: '#3EB4FF',
                height: this.StatusBarHeight,
                ...this.customStyle
            }
            return style;
        }
    }
}
</script>
<style scoped>
</style>