<template>
  <section>
    <h4>获取您的专属二维码</h4>
    <div class="images_container">
      <!-- 海报html元素 -->
      <div id="posterHtml" :style="{backgroundImage: 'url('+posterHtmlBg+')'}" class="posterHtml" v-show="originShow">
        <div
          style="color: #db3939; font-size: 31px; font-weight: 800; text-align: start; padding-left: 10px; font-family: 'Nunito', sans-serif;"
        >{{posterContent}}</div>
        <div style="color: #db3939; font-size: 11px; font-weight: 300; text-align: start; padding-left: 10px; font-family: 'Nunito', sans-serif;">
          微信内查看
        </div>
        <!-- 二维码 -->
        <div id="output" class="qrcode"></div>
      </div>
      <div>
        <img :src="posterImg" alt class="finalImg" />
      </div>
    </div>
  </section>
</template>

<script>
import ICON from '../assets/ICON.png'
import posterBgImg from '../assets/poster.jpeg'

import html2canvas from 'html2canvas'

export default {
  data() {
    return {
      originShow: true,
      posterContent: '众乐乐', // 文案内容
      posterHtmlBg: require('../assets/bmr1o-st73u.png'),
      posterImg: '' // 最终生成的海报图片
    }
  },
  mounted() {
    let shareCode = this.getUrlKey('code')

    console.clear()
    console.log(shareCode, 'has the code')

    var that = this;
    jQuery(function() {
      jQuery('#output').qrcode({
        render: 'canvas', //设置渲染方式，有table和canvas，使用canvas方式渲染性能相对来说比较好
        // text: `https://dwz.cn/8lUbgAcW?code=${shareCode}`, //扫描二维码后显示的内容,可以直接填一个网址，扫描二维码后自动跳向该链接
        text: `${shareCode}`, //扫描二维码后显示的内容,可以直接填一个网址，扫描二维码后自动跳向该链接
        width: 150, //二维码的宽度
        height: 150,
        background: '#fbf7e1', //二维码的后景色
        foreground: '#e6954e', //二维码的前景色
        src: ICON,
        imgWidth: 50,
        imgHeight: 50
      })
      setTimeout(() => {
        that.createPoster()
      }, 1000)
    })
  },
  methods: {
    getUrlKey(name) {
        return decodeURIComponent((new RegExp('[?|&]' + name + '=' + '([^&;]+?)(&|#|;|$)').exec(location.href) || [, ""])[1].replace(/\+/g, '%20')) || null
    },
    createPoster() {
      // 生成海报
      const vm = this
      const domObj = document.getElementById('posterHtml')
      html2canvas(domObj, {
        useCORS: true,
        allowTaint: false,
        logging: false,
        letterRendering: true,
        onclone(doc) {
          let e = doc.querySelector('#posterHtml')
          e.style.display = 'block'
        }
      }).then(function(canvas) {
        // 在微信里,可长按保存或转发
        vm.posterImg = canvas.toDataURL('image/png')
        vm.originShow = false
      })
    }
  }
}
</script>

<style scoped>
.images_container {
  display: flex;
  align-items: center;
  justify-content: center;
}

.posterHtml {
  height: 685px;
  width: 500px;
  background-position: center;
  background-repeat: no-repeat;
  background-size: cover;
  position: relative;
}
.qrcode {
  position: absolute;
  right: 6px;
  top: 4px;
  width: 150px;
}

.finalImg {
  height: 685px;
  width: 500px;
  background-position: center;
  background-repeat: no-repeat;
  background-size: cover;
}
</style>
