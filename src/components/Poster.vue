<template>
  <section>
    <h2>Poster Image</h2>
    <div class="images_container">
      <!-- 海报html元素 -->
      <div id="posterHtml" :style="{backgroundImage: 'url('+posterHtmlBg+')'}" class="posterHtml">
        <div style="color: #f90; font-weight: 800; text-align: start; padding-left: 10px;">{{posterContent}}</div>
        <!-- 二维码 -->
        <div class="qrcode">
          <div id="qrcodeImg"></div>
        </div>
      </div>

      <div>
        <img :src="posterImg" alt class="finalImg" />
      </div>
    </div>
  </section>
</template>

<script>
import posterBgImg from '../assets/poster.jpeg'
// import qrcodeImg from '../assets/qrcode.png'

import QRCode from 'qrcodejs2'
import html2canvas from 'html2canvas'

export default {
  data() {
    return {
      posterContent: '众乐乐', // 文案内容
      posterHtmlBg: require('../assets/poster.jpeg'),
      posterImg: '' // 最终生成的海报图片
    }
  },
  mounted() {
    this.createQrcode('http://h5lk6.com?shareCode="1234')
    this.createPoster()
  },
  methods: {
    createQrcode(text) {
      // 生成二维码
      const qrcodeImgEl = document.getElementById('qrcodeImg')
      qrcodeImgEl.innerHTML = ''
      let qrcode = new QRCode(qrcodeImgEl, {
        width: 128,
        height: 128,
        colorDark: '#000000',
        colorLight: '#ffffff',
        correctLevel: QRCode.CorrectLevel.H
      })
      qrcode.makeCode(text)
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
      })
    }
  }
}
</script>

<style scoped>
svg {
  cursor: pointer;
}

.images_container {
  display: flex;
  align-items: center;
  justify-content: space-between;
}

.posterHtml {
  height: 400px;
  width: 400px;
  background-position: center;
  background-repeat: no-repeat;
  background-size: cover;
  position: relative;
}
.qrcode {
  position: absolute;
  right: 10px;
  bottom: 10px;
  width: 128px;
}
.finalImg {
  height: 400px;
  width: 400px;
  background-position: center;
  background-repeat: no-repeat;
  background-size: cover;
}
</style>
