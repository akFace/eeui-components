<template>
    <div :class="[changeDirection,'flex-fluid']">
        <div class="radio-box flex-row" :class="[isOpacity]" @click="_click(index)" :accessible="true" :aria-label="`${item.title},${checked?'已选中':'未选中'}`" v-for="(item, index) in newItems" :style="Object.assign({}, cWrapperStyle, containerStyle)" :key="index">
            <icon class="icon" v-if="textDirection === 'right'" @click="_click(index)" :eeui="{content: iconName(item.selected), fontSize:iconSize, color:lasetColor(item.selected)}"></icon>
            <text class="radio-label" :class="[fullCell]" :style="Object.assign({}, cTitleStyle(item.selected), textStyles)">{{item.title || item.value}}</text>
            <!--<text class="radio-label" :class="['cb-flex-9']" :style="">jjjjjdsfs sfsdf s</text>-->
            <icon class="icon" v-if="textDirection === 'left'" @click="_click(index)" :eeui="{content: iconName(item.selected), fontSize:iconSize, color:lasetColor(item.selected)}"></icon>
        </div>
    </div>
</template>
<script>
module.exports = {
    data() {
        return {
            newItems: this.initList()
        }
    },
    props: {
        "textDirection": {
            type: String,
            default: "right"
        },
        "leftColumn": {
            type: Boolean,
            default: false
        },
        // 是否可选
        "disabled": {
            type: Boolean,
            default: false
        },
        // 是否选中
        "selected": {
            type: Boolean,
            default: false
        },
        // 标题
        "title": {
            type: String,
            default: ''
        },
        "fontSize": {
            type: String,
            default: '28px'
        },
        "iconSize": {
            type: String,
            default: '40px'
        },
        // 正常状态文字色值
        "color": {
            type: String,
            default: '#9ea7b4'
        },
        // 选中状态文字色值
        "selectedColor": {
            type: String,
            default: '#119d90'
        },
        "selectIcon": {
            type: String,
            default: "md-radio-button-on"
        },
        "unSelectedIcon": {
            type: String,
            default: "md-radio-button-off"
        },
        "textStyles": {
            type: Object
        },

        "items": {
            type: Array
        },
        "value": {
            type: Array
        },
        "direction": {
            type: String,
            default: 'horizontal' // horizontal | vertical
        },
        "containerStyle": {
            type: Object
        }
    },
    watch: {
        value() {
            this.newList = this.initList();
        }
    },
    computed: {
        //多格筛选项外框样式
        isOpacity() {
            const { disabled } = this;
            return disabled ? 'disabled' : 'undisabled'
        },
        fullCell() {
            const { leftColumn } = this;
            return leftColumn ? 'cb-flex-9' : ''
        },
        customStyles() {
            return {
                "textDirection": this.textDirection,
                "direction": this.direction,
                "fontSize": this.fontSize,
                "iconSize": this.iconSize,
                "color": this.color,
                "selectedColor": this.selectedColor,
                "selectIcon": this.selectIcon,
                "unSelectedIcon": this.unSelectedIcon,
                "textStyles": this.textStyles,
                "leftColumn": this.leftColumn,
            }
        },
        cWrapperStyle() {
            const { disabled } = this;
            return {
                opacity: disabled ? 0.5 : 1,
                paddingTop: '15px',
                paddingBottom: '15px',
            }
        },
        changeDirection() {
            return this.direction == "horizontal" ? "flex-row" : "flex-column";
        }
    },
    methods: {
        _click(index) {
            if (this.disabled) return;
            this.updateList(index);
            this.$emit('selected', this.value, this.newItems.filter(item => item.selected));
            this.$emit('input', this.value, this.newItems.filter(item => item.selected));
            this.$emit("change", this.value, this.newItems.filter(item => item.selected));
        },
        //数据赋值
        initList() {
            const newItems = this.items.map((item, i) => {
                let { disabled } = item;
                disabled = !!disabled;
                // disabled为true时认为selected无效，同时单选模式下只认为第一个selected为true的为有效值
                // selected = !disabled && !!selected && (!single || selectedCount === 0);
                if (this.value == item.value) {
                    Vue.set(item, 'selected', true);
                } else {
                    Vue.set(item, 'selected', false);
                }
                return item;
            });
            return newItems;
        },
        //点击筛选操作
        select(index) {
            if (this.disabled) return;
            this.updateList(index);
            this.$emit('selected', this.value, this.newItems.filter(item => item.selected));
            this.$emit('input', this.value, this.newItems.filter(item => item.selected));
            this.$emit("change", this.value, this.newItems.filter(item => item.selected));
        },
        //筛选时数据渲染
        updateList(index) {
            this.newItems = this.newItems.map((item, i) => {
                item.selected = index === i;
                this.value = this.newItems[index].value;
                return item;
            });
        },
        //多格筛选项文字样式
        cTitleStyle(selected) {
            const { color, selectedColor, fontSize } = this;
            return {
                "font-size": fontSize,
                "color": selected ? selectedColor : color,
                "marginLeft": '5px'
            }
        },
        lasetColor(selected) {
            const { color, selectedColor } = this;
            return selected ? selectedColor : color
        },
        iconName(selected) {
            const { selectIcon, unSelectedIcon } = this;
            return selected ? selectIcon : unSelectedIcon
        },
    }
}
</script>
<style scoped>
.radio-box {
    align-items: center;
}

.checkbox {
    border-bottom-width: 1px;
    border-bottom-color: #e2e2e2;
}

.checkbox:active {
    background-color: #f5f5f5;
}

.radio-label {
    font-size: 30px;
}

.disabled {
    opacity: 0.5;
}

.undisabled {
    opacity: 1;
}

.switch-box {
    height: 80px;
    flex-direction: row;
    align-items: center;
    background-color: #ffffff;
}

.switch {
    position: absolute;
    right: 0px;
    top: 15px;
}

.switch-label {
    font-size: $nb30;
}

.flex-column {
    /*flex-direction: column;*/
}

.flex-row {
    flex-direction: row;
}

.flex-fluid {
    flex-wrap: wrap;
}

.cb-flex-9 {
    flex: 9;
}

.cb-flex-1 {
    flex: 1;
    flex-direction: row;
    justify-content: flex-end;
}

.icon {
    width: 45px;
    height: 45px;
}
</style>