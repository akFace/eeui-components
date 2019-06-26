<template>
    <div :style="getContainerStyle()">
        <div :key="index" v-for="(item,index) in tabItems" :style="getItemStyle(item, index)" @click="itemClick($event,item,index)">
            <bui-icon v-if="item.icon" :name="item.icon" :color="index === value ? selectedColor : normalColor" @click="itemClick($event,item,index)" :size="iconSize"></bui-icon>
            <div class="text-box">
                <text v-if="item.title" :style="getTitleStyle(item, index)" @click="itemClick($event,item,index)">{{ item.title }}</text>
                <text class="text-line" :style="getTitleLine(item, index)"></text>
            </div>
        </div>
    </div>
</template>
<script>
module.exports = {
    name: 'Tabbar',
    props: {
        tabItems: { default: [] },
        value: { type: Number, default: 0 },
        height: { default: "100px" },
        iconSize: { default: "45px" },
        titleSize: { default: '32px' },
        background: { default: '#f7f7f7' },
        selectedBackground: { default: '#f7f7f7' },
        normalColor: { default: '#818181' },
        selectedColor: { default: '#4ca4fe' },
        borderBottomColor: { default: '#4ca4fe' },
        borderBottomWidth: {default: '30px'},
        borderBottomHeight: {default: '3px'},
        containerStyle: { default: function() { return {} } },
        itemStyle: { default: function() { return {} } },
        showSelectedLine: { default: false }
    },
    methods: {
        "getContainerStyle": function() {
            //合并样式
            var style = { 'flex-direction': 'row', 'height': this.height };
            style = Object.assign(style, this.containerStyle);
            return style;
        },
        "getItemStyle": function(item, index) {
            const selected = index === this.value;

            const backgroundColor = selected ? this.selectedBackground : this.background;
            const style = {
                'flex': 1,
                'align-items': 'center',
                'justify-content': 'center',
                'backgroundColor': backgroundColor,
            };
            return Object.assign(style, this.itemStyle);
            return style;
        },
        "getTitleStyle": function(item, index) {
            const selected = index === this.value;
            const backgroundColor = selected ? this.selectedBackground : this.background;
            const borderBottomColor = (selected && this.showSelectedLine) ? this.borderBottomColor : backgroundColor;

            var style = {
                'color': selected ? this.selectedColor : this.normalColor,
                'font-size': this.titleSize,
                'flex': 1,
                'line-height': this.height,
            };
            return style;
        },
        "getTitleLine": function(item, index) {
            const selected = index === this.value;
            const backgroundColor = selected ? this.selectedBackground : this.background;
            const borderBottomColor = (selected && this.showSelectedLine) ? this.borderBottomColor : backgroundColor;

            var style = {
                'background-color': borderBottomColor,
                'height': this.borderBottomHeight,
                'width': this.borderBottomWidth
            };
            return style;
        },
        "itemClick": function(e, item, index) {
            this.value = index;
            this.$emit('input', index);
            this.$emit('change', index);
        }
    }
}
</script>
<style scoped>
    .text-box {
        position: relative;
        justify-content: center;
        align-items: center;
    }
    .text-line {
        bottom: 15px;
        margin: auto;
    }
</style>