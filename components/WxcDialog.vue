<!-- CopyRight (C) 2017-2022 Alibaba Group Holding Limited. -->
<!-- Created by Tw93 on 16/11/07. -->
<!--A dialog. -->
<template>
    <div class="dialog">
        <wxc-overlay :left='left' v-if="show" :show="true" :hasAnimation="false"></wxc-overlay>
        <div class="container" v-if="show">
            <div class="dialog-box">
                <div class="dialog-content">
                    <slot name="title">
                        <text class="content-title">{{title}}</text>
                    </slot>
                    <slot name="content">
                        <text class="content-subtext">{{content}}</text>
                    </slot>
                    <div class="no-prompt" v-if="showNoPrompt" @click="noPromptClicked">
                        <image :src="noPromptIcon" class="no-prompt-icon"></image>
                        <text class="no-prompt-text">{{noPromptText}}</text>
                    </div>
                </div>
                <div class="dialog-footer">
                    <div class="footer-btn cancel">
                        <text class="btn-text third-text" :style="{ color: thirdBtnColor }" v-if="thirdText" @click="thirdClicked">{{thirdText}}</text>
                    </div>
                    <div class="footer-btn confirm">
                        <text class="btn-text" :style="{ color: secondBtnColor }" v-if="!single" @click="secondaryClicked">{{cancelText}}</text>
                        <text class="btn-text confirm-btn" :style="{ color: mainBtnColor }" @click="primaryClicked">{{confirmText}}</text>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>
<style scoped>
.container {
    position: fixed;
    /*兼容H5异常*/
    z-index: 99999;
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
    justify-content: center;
    align-items: center;
}

.dialog-box {
    width: 648px;
    background-color: #FFFFFF;
    border-radius: 6px;
}

.dialog-content {
    padding-top: 36px;
    padding-bottom: 36px;
    padding-left: 36px;
    padding-right: 36px;
}

.content-title {
    color: #333333;
    font-size: 36px;
    text-align: left;
    margin-bottom: 24px;
}

.content-subtext {
    padding-top: 26px;
    padding-bottom: 26px;
    color: #434343;
    font-size: 26px;
    line-height: 36px;
    text-align: center;
}

.dialog-footer {
    flex-direction: row;
    align-items: center;
}

.footer-btn {
    flex-direction: row;
    align-items: center;
    justify-content: flex-end;
    flex: 1;
    height: 90px;
}

.cancel {
    justify-content: flex-start;
}

.btn-text {
    height: 90px;
    line-height: 90px;
    padding: 0 25px;
    font-size: 28px;
    color: #666666;
}
.confirm-btn {
  margin-right: 10px;
}
.third-text {
    margin-left: 10px;
}
.no-prompt {
    width: 486px;
    align-items: center;
    justify-content: center;
    flex-direction: row;
    margin-top: 24px;
}

.no-prompt-icon {
    width: 24px;
    height: 24px;
    margin-right: 12px;
}

.no-prompt-text {
    font-size: 24px;
    color: #A5A5A5;
}
</style>
<script>
import WxcOverlay from './WxcOverlay.vue'
const CHECKED = 'https://gw.alicdn.com/tfs/TB1UT3VpgMPMeJjy1XdXXasrXXa-42-42.png';
const UN_CHECKED = 'https://gw.alicdn.com/tfs/TB1hE3VpgMPMeJjy1XdXXasrXXa-42-42.png';

export default {
    components: { WxcOverlay },
    props: {
        show: {
            type: Boolean,
            default: false
        },
        single: {
            type: Boolean,
            default: false
        },
        title: {
            type: String,
            default: ''
        },
        content: {
            type: String,
            default: ''
        },
        top: {
            type: Number,
            default: 400
        },
        cancelText: {
            type: String,
            default: '取消'
        },
        confirmText: {
            type: String,
            default: '确定'
        },
        thirdText: {
            type: String,
            default: ''
        },
        thirdBtnColor: {
            type: String,
            default: '#119d90'
        },
        mainBtnColor: {
            type: String,
            default: '#119d90'
        },
        secondBtnColor: {
            type: String,
            default: '#119d90'
        },
        showNoPrompt: {
            type: Boolean,
            default: false
        },
        noPromptText: {
            type: String,
            default: '不再提示'
        },
        isChecked: {
            type: Boolean,
            default: false
        },
        left: {
            type: Number,
            default: 0
        }
    },
    data: () => ({
        noPromptIcon: UN_CHECKED
    }),
    created() {
        const { env: { deviceHeight, deviceWidth } } = weex.config;
    },
    methods: {
        secondaryClicked() {
            this.$emit('wxcDialogCancelBtnClicked', {
                type: 'cancel'
            });
        },
        primaryClicked(e) {
            this.$emit('wxcDialogConfirmBtnClicked', {
                type: 'confirm'
            });
        },
        noPromptClicked(e) {
            const isChecked = !this.isChecked;
            this.noPromptIcon = isChecked ? CHECKED : UN_CHECKED;
            this.$emit('wxcDialogNoPromptClicked', { isChecked });
        },
        thirdClicked() {
            this.$emit('wxcDialogThirdClicked', {
                type: 'thirdClicked'
            });
        },
    }
};
</script>