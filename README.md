#### 常用组件封装,部分迁移[weex-ui](https://github.com/alibaba/weex-ui)和[bui](https://github.com/bingo-oss/bui-weex)组件，并做了适配用于eeui，以及视觉优化调整

### 1、颜色选择器

#### gif演示

<figure class="center">
    <img src="https://raw.githubusercontent.com/kang558/eeui-components/master/screenshot/colorpicker.gif" width="400">
</figure>

``` vue
// 使用
<template>
    <div class="app">
        <ColorPicker :show="show_color_picker" @confirm="confirm" @cancel="cancel"></ColorPicker>
    </div>
</template>
<script>
import ColorPicker from "../components/ColorPicker.vue"
export default {
    components: { ColorPicker },
    data() {
        return {
            show_color_picker: true,
        }
    },
    mounted() {},
    created() {
    },
    methods: {
        confirm(result) {
            this.show_color_picker = false;
        },
        cancel() {
            this.show_color_picker = false;
        },
    },
};
</script>

```

### API
```
// 初始化颜色
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
```
| Prop      | Type   |Required  | Default   | Description  |
|-------------|------------|--------|--------|-----|
| show | `Bool` | `N`|  `false` |显示与隐藏|
| config | `Object` | `N`|  `{}` | 会替换掉默认的defaultConfig |
| colorBoxStyle  | `Object` | `N`| `{}` | 大圆形的样式 |
| hasOpacity  | `Bool` | `N`| `false`| 是否需要透明度 |
| title | `String` | `N`|`颜色选择器` |  标题 |
| cancelText | `String` |`N`| `取消` |  取消按钮 |
| confirmText | `String` | `N`| `确定` |  确定按钮 |
| mainBtnColor | `String` | `N`| `#119d90` | 确定按钮颜色 |
| secondBtnColor | `String` | `N`| `#119d90` | 取消按钮颜色|
| thirdText | `String` |`N`| `自定义` | 自定义按钮文本 |
| thirdBtnColor | `String` |`N`| `#119d90` | 自定义按钮颜色 |

### Event

```
//`@confirm="confirm"`
//`@cancel="cancel"`
```

### 2、弹框组件

### dialog 
[使用说明](https://github.com/alibaba/weex-ui/tree/master/packages/wxc-dialog)

### 3、popup 

#### gif演示
<figure class="center">
    <img src="https://raw.githubusercontent.com/kang558/eeui-components/master/screenshot/WxcPopup.gif" width="400">
</figure>

[使用说明](https://github.com/alibaba/weex-ui/tree/master/packages/wxc-popup)

### 4、Popover 

#### gif演示
<figure class="center">
    <img src="https://raw.githubusercontent.com/kang558/eeui-components/master/screenshot/WxcPopover.gif" width="400">
</figure>

[使用说明](https://github.com/alibaba/weex-ui/tree/master/packages/wxc-popover)


### 5、 sliderbar 进度条
[使用说明](https://github.com/alibaba/weex-ui/tree/master/packages/wxc-slider-bar)


### 6、 Overlay 蒙层
[使用说明](https://github.com/alibaba/weex-ui/tree/master/packages/wxc-overlay)


### 7、 Radio 单选
[使用说明](http://dev.bingocc.com/buiweex/docs/reference/bui-radio.html)


### 8、 StatusBar 状态栏

使用说明：
| Prop      | Type   |Required  | Default   | Description  |
|-------------|------------|--------|--------|-----|
| customStyle | `Object` | `N`|  `{}` | 状态栏颜色样式 |

### 9、 Tabbar
[使用说明](http://dev.bingocc.com/buiweex/docs/reference/bui-tabbar.html)

### 9、 Cellitem
[使用说明](http://dev.bingocc.com/buiweex/docs/reference/bui-cell.html)