<template>
  <main class="main">
    <section class="left">
      <div ref="can" class="canvas" :style="imgStyle">
        <div class="can">
          <img v-if="!img" class="bg" src="@/assets/img/bg.jpg" alt />
          <img v-else class="bg" :src="img" alt />
          <div class="title" :style="textStyle">{{text}}</div>
          <vue-qr
            v-if="url"
            class="qr"
            :autoColor="true"
            :text="url"
            :logoMargin="2"
            :margin="10"
            :logoScale="0.15"
            :style="qrStyle"
          ></vue-qr>
        </div>
      </div>
    </section>
    <section class="right p-3 pt-5">
      <div class="input-group mb-3">
        <div class="input-group-prepend">
          <span class="input-group-text" id="inputGroupFileAddon01">背景图片上传</span>
        </div>
        <div class="custom-file">
          <input type="file" class="custom-file-input" id="inputGroupFile01" @change="handleFile" />
          <label class="custom-file-label" for="inputGroupFile01">背景图片上传</label>
        </div>
      </div>
      <div class="form-group">
        <label for="formControlRange">图片宽度 {{imgWidth}}</label>
        <input
          v-model="imgWidth"
          type="range"
          min="200"
          max="1000"
          class="form-control-range"
          id="formControlRange"
        />
      </div>
      <div class="form-group">
        <label for="formControlRange">图片高度 {{imgHeight}}</label>
        <input
          v-model="imgHeight"
          type="range"
          min="200"
          max="1000"
          class="form-control-range"
          id="formControlRange"
        />
      </div>
      <div class="input-group mb-3">
        <div class="input-group-prepend">
          <span class="input-group-text" id="basic-addon1">二维码链接</span>
        </div>
        <input type="text" class="form-control" placeholder="二维码链接" v-model="url" />
      </div>
      <div class="form-group">
        <label for="formControlRange">二维码宽度 {{qrWidth}}</label>
        <input
          v-model="qrWidth"
          type="range"
          min="50"
          max="500"
          class="form-control-range"
          id="formControlRange"
        />
      </div>
      <div class="form-group">
        <label for="formControlRange">二维码高度 {{qrHeight}}</label>
        <input
          v-model="qrHeight"
          type="range"
          min="50"
          max="500"
          class="form-control-range"
          id="formControlRange"
        />
      </div>
      <div class="form-group">
        <label for="formControlRange">二维码偏移X {{qrTransX}}</label>
        <input
          v-model="qrTransX"
          type="range"
          min="0"
          max="100"
          class="form-control-range"
          id="formControlRange"
        />
      </div>
      <div class="form-group">
        <label for="formControlRange">二维码偏移Y {{qrTransY}}</label>
        <input
          v-model="qrTransY"
          type="range"
          min="0"
          max="100"
          class="form-control-range"
          id="formControlRange"
        />
      </div>
      <div class="input-group mb-3">
        <div class="input-group-prepend">
          <span class="input-group-text" id="basic-addon1">文字</span>
        </div>
        <input type="text" class="form-control" placeholder="文字" v-model="text" />
      </div>
      <div class="input-group mb-3">
        <div class="input-group-prepend">
          <label class="input-group-text" for="inputGroupSelect01">Options</label>
        </div>
        <select v-model="textColor" class="custom-select" id="inputGroupSelect01">
          <option selected value="#ff0000">红</option>
          <option value="#ffffff">白</option>
          <option value="#000000">黑</option>
        </select>
      </div>
      <div class="form-group">
        <label for="formControlRange">文字大小 {{textFontSize}}</label>
        <input
          v-model="textFontSize"
          type="range"
          min="7"
          max="50"
          class="form-control-range"
          id="formControlRange"
        />
      </div>
      <div class="form-group">
        <label for="formControlRange">文字偏移X {{textTransX}}</label>
        <input
          v-model="textTransX"
          type="range"
          min="-100"
          max="100"
          class="form-control-range"
          id="formControlRange"
        />
      </div>
      <div class="form-group">
        <label for="formControlRange">文字偏移Y {{textTransY}}</label>
        <input
          v-model="textTransY"
          type="range"
          min="-100"
          max="100"
          class="form-control-range"
          id="formControlRange"
        />
      </div>

      <div class="text-right pr-3">
        <button @click="genImage" type="button" class="btn btn-secondary">生成文件</button>
      </div>
      <a ref="link" v-show="false" :href="resImg" download="1">faffa</a>
    </section>
  </main>
</template>

<script>
import html2canvas from "html2canvas";
export default {
  data() {
    return {
      text: "文字",
      url: "http://baidu.com",
      img: null,
      resImg: null,
      imgHeight: 300,
      imgWidth: 300,
      textColor: "#ff0000",
      textFontSize: 14,
      textTransX: 0,
      textTransY: 0,
      qrWidth: 100,
      qrHeight: 100,
      qrTransY: 0,
      qrTransX: 0,
    };
  },
  computed: {
    textStyle() {
      return `color:${this.textColor};font-size:${
        this.textFontSize
      }px;transform:translate(${this.textTransX}px,${-this.textTransY}px)`;
    },
    imgStyle() {
      return `width:${this.imgWidth}px;height:${this.imgHeight}px;`;
    },
    qrStyle() {
      return `width:${this.qrWidth}px;height:${
        this.qrHeight
      }px;transform:translate(${-this.qrTransX}px,${-this.qrTransY}px)`;
    },
  },
  methods: {
    handleFile(e) {
      let file = e.target.files[0];
      if (!file) return;
      const url = URL.createObjectURL(file);
      this.img = url;
    },
    genImage() {
      html2canvas(this.$refs.can, {
        useCORS: true,
        width: this.imgWidth,
        height: this.imgHeight,
        x: 0,
        y: 0,
        scrollX: 0,
        scrollY: 0,
      })
        .then((canvas) => {
          let dataURL = canvas.toDataURL("image/png");
          this.resImg = dataURL;
          if (this.resImg) {
            this.$nextTick(() => this.$refs.link.click());
          }
        })
        .catch((err) => {
          console.log(err);
        });
    },
  },
};
</script>

<style lang='scss'>
.main {
  display: flex;
  width: 100vw;
  height: 100vh;
  .left {
    width: 50vw;
    display: flex;
    align-items: flex-start;
    justify-content: flex-start;
    .canvas {
      width: 50vw;
      height: 50vw;
      .can {
        width: 100%;
        height: 100%;
        position: relative;
        .bg {
          position: absolute;
          width: 100%;
          height: 100%;
          z-index: 0;
          opacity: 0.7;
        }
        .qr {
          position: absolute;
          right: 0;
          bottom: 0;
          width: 30%;
          height: 30%;
        }
        .title {
          position: absolute;
          left: 0;
          bottom: 0;
          color: black;
          font-size: 15px;
          font-weight: 600;
        }
      }
    }
  }
  .right {
    flex: 1;
  }
}
.link {
  display: block;
}
</style>