# qui-dialog

> 对话框组件，支持自定义标题、正文内容、按钮。依赖qui-overlay，qui-button组件

## 预览
<img src="https://qapp-ui.github.io/qapp-ui/docs/assets/qui-dialog.gif" width="300"/>

## 依赖接口

无依赖

## 使用方法

```ux
<import name="qui-dialog" src="@qapp-ui/qui-dialog/index"></import>
<import name="qui-toast" src="@qapp-ui/qui-toast/index"></import>

<template>
  <div class="page">
    <input type="button" value="常规对话框" @click="normal">
    <input type="button" value="无标题的对话框" @click="noTitle">
    <input type="button" value="只有确定按钮的对话框" @click="noCancel">
    <input type="button" value="无按钮的对话框" @click="noBtn">
    <qui-dialog option="{{dialog}}" @qui-btn-click="btnClick">
      <!--自定义正文内容-->
      <text>这是内容这是内容这是内容这是内容这是内容这是内容这是内容这是内容这是内容这是内容这是内容这是内容这是内容这是内容</text>
    </qui-dialog>
    <qui-toast id="qui-toast"></qui-toast>
  </div>
</template>

<style>
  .page {
    flex-direction: column;
    align-items: center;
    justify-content: center;
  }

  input {
    width: 800px;
    height: 100px;
    margin: 50px;
    padding: 20px;
    color: #ffffff;
    background-color: #0F8DE8;
  }

  text {
    color: #666666;
    font-size: 45px;
    line-height: 60px;
  }
</style>

<script>
  export default {
    private: {
      option: {
        show: true,
        title: '对话框',
        btns: [{
          text: '确定',
          color: '#0090ff'
        }, {
          text: '取消',
          color: '#999999'
        }]
      },
      dialog: {}
    },
    onReady() {
      this.toast = this.$child('qui-toast')
    },
    onBackPress() {
      if (this.dialog.show) {
        //要延迟才有过渡动画
        setTimeout(() => {
          this.dialog.show = false
        })

        return true
      }
    },
    normal() {
      this.dialog = Object.assign({}, this.option)
    },
    noTitle() {
      this.dialog = Object.assign({}, this.option, {
        title: ''
      })
    },
    noCancel() {
      this.dialog = Object.assign({}, this.option, {
        autoClose: true,
        btns: [{
          text: '确定'
        }]
      })
    },
    noBtn() {
      this.dialog = Object.assign({}, this.option, {
        autoClose: true,
        btns: []
      })
    },
    btnClick(evt) {
      this.toast.show({
        text: `点击的第${evt.detail.index + 1}个按钮`
      })

      switch (evt.detail.index) {
        case 1:
          this.dialog.show = false
      }
    }
  }
</script>
```

更详细代码可以参考[qui-dialog demo](https://github.com/qapp-ui/qapp-ui/blob/master/src/Dialog/index.ux)

## 参数

qui-dialog只接受属性option，option为对象，各属性如下

| 名称 | 类型 | 必填 | 默认值 | 描述 |
|------------|------------|--------|-----|-----|
| show | `Boolean` | `N` | `false` |显示或隐藏对话框 |
| background | `String` | `N` | `rgba(0,0,0,0.4)` |蒙层背景色，支持所有合法background |
| autoClose | `Boolean` | `N` | `false` | 点击蒙层是否自动关闭 |
| title | `String` | `N` | `''` | 对话框标题，默认为空字符串 |
| btns | `Array` | `N` | `[{text: '确定',color: '#0090ff'}, {text: '取消',color: '#999999'}]` | 按钮，默认有确定和取消按钮 |
| btns[{text}] | `String` | `Y` | `-` | 按钮文本 |
| btns[{color}] | `String` | `N` | `#000000` | 按钮文本颜色和边框颜色 |


## 事件

| 名称 | 参数 | 描述 |
|----------|------------|--------|
| qui-overlay-click | `-` | 点击蒙层时触发 |
| qui-btn-click| `{index: evt.detail.index}` | 点击按钮时触发，index表示第几个按钮，从0开始，在事件处理函数中通过event.detail.index获取 |
