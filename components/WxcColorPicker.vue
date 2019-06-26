<!-- CopyRight (C) 2019-2022 Alibaba Group Holding Limited. -->
<!-- Created by kang on 19/6/16. -->
<!--A gray overlay mask-->
<template>
    <div class="colorPick" v-if="show">
        <Dialog 
            :title="title" 
            :show="true" 
            :thirdText="thirdText" 
            :thirdBtnColor="thirdBtnColor" 
            :cancelText="cancelText" 
            :confirmText="confirmText" 
            :mainBtnColor="mainBtnColor" 
            :secondBtnColor="secondBtnColor"
            @wxcDialogCancelBtnClicked="wxcDialogCancelBtnClicked" 
            @wxcDialogConfirmBtnClicked="wxcDialogConfirmBtnClicked" 
            @wxcDialogThirdClicked="wxcDialogThirdClicked">
            <div slot="content" class="dialog-content">
                <div class="color-box" :style="realColorBoxStyle"></div>
                <div class="range-item">
                    <text class="range-label">R</text>
                    <WxcSliderBar v-bind="sliderBarCfgR" @updateValue="updateValueR"></WxcSliderBar>
                </div>
                <div class="range-item">
                    <text class="range-label">G</text>
                    <WxcSliderBar v-bind="sliderBarCfgG" @updateValue="updateValueG"></WxcSliderBar>
                </div>
                <div class="range-item">
                    <text class="range-label">B</text>
                    <WxcSliderBar v-bind="sliderBarCfgB" @updateValue="updateValueB"></WxcSliderBar>
                </div>
                <div class="range-item" v-if="hasOpacity">
                    <text class="range-label">A</text>
                    <WxcSliderBar v-bind="sliderBarCfgA" @updateValue="updateValueA"></WxcSliderBar>
                </div>
            </div>
        </Dialog>
    </div>
</template>
<style scoped>
.dialog-content {
    justify-content: center;
    align-items: center;
}

.color-box {
    width: 150px;
    height: 150px;
    border-radius: 100%;
    margin-bottom: 25px;
}

.range-item {
    margin-top: 30px;
    flex-direction: row;
    justify-content: center;
    align-items: center;
}

.range-label {
    padding-right: 10px;
    font-size: 32px;
}
</style>
<script>
import WxcSliderBar from "./WxcSliderBar.vue";
import Dialog from "./WxcDialog.vue";
export default {
    components: { WxcSliderBar, Dialog },
    data() {
        return {
            sliderBarCfgR: {
                length: 430,
                range: false,
                min: 0,
                max: 255,
                value: 17,
                defaultValue: 17,
                disabled: false
            },
            sliderBarCfgG: {
                length: 430,
                range: false,
                min: 0,
                max: 255,
                value: 157,
                defaultValue: 157,
                disabled: false
            },
            sliderBarCfgB: {
                length: 430,
                range: false,
                min: 0,
                max: 255,
                value: 144,
                defaultValue: 144,
                disabled: false
            },
            sliderBarCfgA: {
                length: 430,
                range: false,
                min: 0,
                max: 100,
                value: 100,
                minDiff: 1,
                defaultValue: 100,
                disabled: false
            },
            defaultConfig: {
                valueR: 14,
                valueG: 157,
                valueB: 144,
                valueA: 100,
                validColorR: '#119d90',
                validColorG: '#119d90',
                validColorB: '#119d90',
                validColorA: '#119d90',
                invalidColorR: '#E0E0E0',
                invalidColorG: '#E0E0E0',
                invalidColorB: '#E0E0E0',
                invalidColorA: '#E0E0E0',
            }
        }
    },
    props: {
        show: {
            type: Boolean,
            default: false
        },
        config: {
            default: Object,
            default: {}
        },
        colorBoxStyle: {
            default: Object,
            default: {}
        },
        hasOpacity: {
            type: Boolean,
            default: false
        },
        title: {
            type: String,
            default: '颜色选择器'
        },
        cancelText: {
            type: String,
            default: '取消'
        },
        confirmText: {
            type: String,
            default: '确定'
        },
        mainBtnColor: {
            type: String,
            default: '#119d90'
        },
        secondBtnColor: {
            type: String,
            default: '#119d90'
        },
        thirdText: {
            type: String,
            default: '自定义'
        },
        thirdBtnColor: {
            type: String,
            default: '#119d90'
        },
    },
    created() {
      this.defaultConfig = {
          ...this.defaultConfig,
          ...this.config
      }
      this.init();
    },
    computed: {
        overlayStyle() {

        },
        shouldShow() {

        },
        realColorBoxStyle() {
            let style = {
                ...this.colorBoxStyle,
                backgroundColor: `rgba(${this.defaultConfig.valueR}, ${this.defaultConfig.valueG}, ${this.defaultConfig.valueB}, ${this.defaultConfig.valueA/100})`
            }
            return style;
        },
    },
    methods: {
        init() {
          this.sliderBarCfgR.value = this.defaultConfig.valueR;
          this.sliderBarCfgG.value = this.defaultConfig.valueG;
          this.sliderBarCfgB.value = this.defaultConfig.valueB;
          this.sliderBarCfgA.validColorR = this.defaultConfig.validColorR;
          this.sliderBarCfgR.validColorG = this.defaultConfig.validColorG;
          this.sliderBarCfgG.validColorB = this.defaultConfig.validColorB;
          this.sliderBarCfgB.validColorA = this.defaultConfig.validColorA;
          this.sliderBarCfgA.invalidColorR = this.defaultConfig.invalidColorR;
          this.sliderBarCfgR.invalidColorG = this.defaultConfig.invalidColorG;
          this.sliderBarCfgG.invalidColorB = this.defaultConfig.invalidColorB;
          this.sliderBarCfgB.invalidColorA = this.defaultConfig.invalidColorA;
        },
        updateValueR(value) {
            this.defaultConfig.valueR = value;
        },
        updateValueG(value) {
            this.defaultConfig.valueG = value;
        },
        updateValueB(value) {
            this.defaultConfig.valueB = value;
        },
        updateValueA(value) {
            this.defaultConfig.valueA = value;
        },
        colorRgba(sHex, alpha = 1) {
            let rgb = this.colorRgbValue(sHex);
            return `rgba(${rgb},${alpha}`
        },
        colorRgbValue(sHex) {
            let reg = /^#([0-9a-fA-f]{3}|[0-9a-fA-f]{6})$/
            /* 16进制颜色转为RGB格式 */
            let sColor = sHex.toLowerCase()
            if (sColor && reg.test(sColor)) {
                if (sColor.length === 4) {
                    let sColorNew = '#'
                    for (let i = 1; i < 4; i += 1) {
                        sColorNew += sColor.slice(i, i + 1).concat(sColor.slice(i, i + 1))
                    }
                    sColor = sColorNew
                }
                //  处理六位的颜色值
                let sColorChange = []
                for (let i = 1; i < 7; i += 2) {
                    sColorChange.push(parseInt('0x' + sColor.slice(i, i + 2)))
                }
                // return sColorChange.join(',')
                // 或
                return sColorChange.join(',')
            } else {
                return sColor
            }
        },
        hexify(color) {
            let values = color
                .replace(/rgba?\(/, '')
                .replace(/\)/, '')
                .replace(/[\s+]/g, '')
                .split(',');
            let a = parseFloat(values[3] || 1),
                r = Math.floor(a * parseInt(values[0]) + (1 - a) * 255),
                g = Math.floor(a * parseInt(values[1]) + (1 - a) * 255),
                b = Math.floor(a * parseInt(values[2]) + (1 - a) * 255);
            return "#" +
                ("0" + r.toString(16)).slice(-2) +
                ("0" + g.toString(16)).slice(-2) +
                ("0" + b.toString(16)).slice(-2);
        },
        wxcDialogConfirmBtnClicked() {
            let rgbaColor = `rgba(${this.defaultConfig.valueR}, ${this.defaultConfig.valueG}, ${this.defaultConfig.valueB}, ${this.defaultConfig.valueA/100})`;
            let hexColor = this.hexify(rgbaColor);
            this.$emit('confirm', {
                R: this.defaultConfig.valueR,
                G: this.defaultConfig.valueG,
                B: this.defaultConfig.valueB,
                A: this.defaultConfig.valueA,
                rgbaColor: rgbaColor,
                hexColor: hexColor
            });
        },
        wxcDialogCancelBtnClicked() {
            this.$emit('cancel', {});
        },
        wxcDialogThirdClicked() {
            const modal = weex.requireModule('modal')
            modal.prompt({
                message: this.thirdText,
                okTitle: '确定',
                cancelTitle: '取消'
            }, (value) => {
              if (value.result === '确定') {
                let reg = /^#([0-9a-fA-F]{6}|[0-9a-fA-F]{3})$/
                if (!reg.test(value.data)) {
                  modal.toast({
                      message: '请输入正确的16进制颜色值',
                      duration: 0.3
                  })
                  return;
                }
                let rgb = this.colorRgbValue(value.data);
                let rgbarr = rgb.split(',');
                this.defaultConfig.valueR = rgbarr[0];
                this.defaultConfig.valueG = rgbarr[1];
                this.defaultConfig.valueB = rgbarr[2];
                this.defaultConfig.valueA = 100;
                this.sliderBarCfgR.value = this.defaultConfig.valueR;
                this.sliderBarCfgG.value = this.defaultConfig.valueG;
                this.sliderBarCfgB.value = this.defaultConfig.valueB;
                this.sliderBarCfgA.value = this.defaultConfig.valueA;
              }
            })
        },
    }
};
</script>