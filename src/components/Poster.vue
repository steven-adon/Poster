<template>
  <section>
    <h2>Poster Image</h2>
    <div class="images_container">
      <!-- 海报html元素 -->
      <div id="posterHtml" :style="{backgroundImage: 'url('+posterHtmlBg+')'}" class="posterHtml">
        <div style="color: #db3939; font-size: 31px; font-weight: 800; text-align: start; padding-left: 10px; font-family: 'Nunito', sans-serif;">{{posterContent}}</div>
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
      posterHtmlBg: require('../assets/bmr1o-st73u.png'),
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
      qrcodeImgEl.innerHTML = 'vue'
      let qrcode = new QRCode(qrcodeImgEl, {
        width: 150,
        height: 150,
        colorDark: '#e6954e',
        colorLight: '#fbf7e1',
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

/* .qrcodeImg {
  width:
} */
.finalImg {
  height: 685px;
  width: 500px;
  background-position: center;
  background-repeat: no-repeat;
  background-size: cover;
}
</style>
