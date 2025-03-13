<template>
  <div class="container">
    <div class="box" id="borderBox">
      <div class="commentBox" id="titleBox">百灶饭卡生成器</div>
      <div class="imgBox" id="imageBox">
        <img src="@/assets/card_base.png" alt="展示图片" id="cardBase" class="image" />
        <div v-for="(value, key) in inputs" :key="key">
          <input class="inputField" type="text" v-model="inputs[key]" :id="key" :placeholder="'请输入' + key"
            @input="saveInputs" @blur="saveInputs" />
        </div>
      </div>
      <div class="commentBox" id="creditBox">排版设计@石油 网页制作@NSLC</div>
    </div>

    <div class="floating-buttons">
      <button id="make-img" @click="generateAndDownloadImage">保存⤓</button>
      <button id="clear" @click="clearContent">清除内容⭯</button>
    </div>

    <!-- Canvas for drawing the image -->
    <canvas ref="canvas" style="display: none;"></canvas>
  </div>
</template>

<script>
export default {
  data() {
    return {
      inputs: {
        codeName: '',
        subjectCode: '',
        position: '',
        department: ''
      },
      inputFontStyle: {
        "codeName": { fontSize: 100, top: 92 },
        "subjectCode": { fontSize: 50, top: 269 },
        "position": { fontSize: 50, top: 378 },
        "department": { fontSize: 50, top: 482 }
      },
      paintFontStyle: {
        "codeName": { fontSize: 100, top: 106 },
        "subjectCode": { fontSize: 50, top: 275 },
        "position": { fontSize: 50, top: 385 },
        "department": { fontSize: 50, top: 489 }
      },
      imgStyle: {
        "imageBox": { width: 1075, height: 706 },
        "cardBase": { width: 1075, height: 706 },
      },
      BoxStyle: {
        "borderBox": { width: 1200, height: 900 }
      },
      textLeft: 80,
      commonValues: {
        baseWidth: 1075,
        baseHeight: 706,
        inputLeft: 80,
        inputWidth: 910,
        letterSpacing: -2.5,
        inputHeights: {
          codeName: 130,
          subjectCode: 65,
          position: 65,
          department: 65
        },
        commentBox: {
          baseHeight: 60,
          baseSize: 48,
          boost: 5.73336,
          finalScale: 5 / 3
        }
      }
    };
  },
  mounted() {
    this.loadInputs();
    this.resizeInputs();
    this.resizeBox();
    window.addEventListener("resize", this.resizeInputs);
    window.addEventListener("resize", this.resizeBox);
  },
  beforeUnmount() {
    window.removeEventListener("resize", this.resizeInputs);
    window.removeEventListener("resize", this.resizeBox);
  },
  methods: {
    loadInputs() {
      const savedInputs = localStorage.getItem('inputs');
      if (savedInputs) {
        this.inputs = JSON.parse(savedInputs);
      }
    },
    saveInputs() {
      localStorage.setItem('inputs', JSON.stringify(this.inputs));
    },
    resizeInputs() {
      const scale = window.innerWidth / this.commonValues.baseWidth;
      Object.entries(this.inputFontStyle).forEach(([id, size]) => {
        const input = document.getElementById(id);
        if (input) {
          if (window.innerWidth < this.commonValues.baseWidth) {
            // 统一缩放
            input.style.fontSize = `${size.fontSize * scale}px`;
            input.style.top = `${size.top * scale}px`;
            input.style.left = `${this.commonValues.inputLeft * scale}px`;
            input.style.width = `${this.commonValues.inputWidth * scale}px`;
            input.style.height = `${this.commonValues.inputHeights[id] * scale}px`;
            input.style.letterSpacing = `${this.commonValues.letterSpacing * scale}px`;
          } else {
            input.style.fontSize = `${size.fontSize}px`;
            input.style.top = `${size.top}px`;
            input.style.left = `${this.commonValues.inputLeft}px`;
            input.style.width = `${this.commonValues.inputWidth}px`;
            input.style.height = `${this.commonValues.inputHeights[id]}px`;
            input.style.letterSpacing = `${this.commonValues.letterSpacing}px`;
          }
        }
      });
    },
    resizeBox() {
      Object.entries(this.imgStyle).forEach(([id, size]) => {
        const imgBox = document.getElementById(id);
        if (imgBox) {
          const scale = window.innerWidth / 1075;
          if (window.innerWidth < 1075) {
            imgBox.style.width = `${size.width * scale}px`;
            imgBox.style.height = `${size.height * scale}px`;
          } else {
            imgBox.style.width = `${size.width}px`;
            imgBox.style.height = `${size.height}px`;
          }
        }
      });

      const borderBox = document.getElementById("borderBox");
      if (borderBox) {
        const scale = window.innerWidth / 1200;
        if (window.innerWidth < 1200) {
          borderBox.style.height = `${this.BoxStyle.borderBox.height * scale}px`;
          borderBox.style.width = `${this.BoxStyle.borderBox.width * scale}px`;
        } else {
          borderBox.style.height = `${this.BoxStyle.borderBox.height}px`;
          borderBox.style.width = `${this.BoxStyle.borderBox.width}px`;
        }
      }

      const commentBox = document.getElementsByClassName("commentBox");
      // console.log(titleBox)
      Object.entries(commentBox).forEach(([, item]) => {
        if (item) {
          var scaleBias = 0;
          if (item.id == "creditBox") {
            scaleBias = 0.3;
          } else {
            scaleBias = 1;
          }
          if (window.innerWidth < 1075) {
            const scale = window.innerWidth / 1075;
            item.style.height = `${scaleBias * this.commonValues.commentBox.baseHeight * scale}px`;
            item.style.fontSize = `${scaleBias * this.commonValues.commentBox.baseSize * scale}px`;
          } else if (window.innerWidth < 1200) {
            const scale = (1075 + (window.innerWidth - 1075) * this.commonValues.commentBox.boost) / 1075;
            item.style.height = `${scaleBias * this.commonValues.commentBox.baseHeight * scale}px`;
            item.style.fontSize = `${scaleBias * this.commonValues.commentBox.baseSize * scale}px`;
          } else {
            item.style.height = `${scaleBias * this.commonValues.commentBox.baseHeight * this.commonValues.commentBox.finalScale}px`;
            item.style.fontSize = `${scaleBias * this.commonValues.commentBox.baseSize * this.commonValues.commentBox.finalScale}px`;
          }
        }
      })
    },
    generateAndDownloadImage() {
      const img = new Image();
      img.src = require('@/assets/card_base.png');

      img.onload = () => {
        const canvas = this.$refs.canvas;
        const ctx = canvas.getContext('2d');
        canvas.width = img.width;
        canvas.height = img.height;

        // card-base
        ctx.drawImage(img, 0, 0);

        // Common Text Settings
        ctx.fillStyle = '#2a2a2a';
        ctx.textBaseline = "top";
        ctx.letterSpacing = "-2px";

        Object.entries(this.inputs).forEach(([key, value]) => {
          ctx.font = `${this.paintFontStyle[key].fontSize}px MiSans-Bold`;
          ctx.beginPath();
          const textMetrics = ctx.measureText(value);
          if (textMetrics.width >= 910) {
            alert(`输入的${key}内容过长`)
          } else {
            ctx.fillText(value, this.textLeft, this.paintFontStyle[key].top);
          }
          ctx.save();
        });

        this.downloadImage(canvas);
      };
    },

    downloadImage(canvas) {
      const link = document.createElement('a');
      link.href = canvas.toDataURL('image/png');
      link.download = `百灶饭卡-${this.inputs.codeName}.png`;
      link.click();
    },
    clearContent() {
      this.inputs = {
        codeName: '',
        subjectCode: '',
        position: '',
        department: ''
      };
      localStorage.setItem('inputs', JSON.stringify(this.inputs));
    }
  },
};
</script>

<style scoped>
@font-face {
  font-family: 'MiSans-Semi';
  src: url('@/assets/MiSans-Semibold.woff2') format('woff2');
  font-weight: 800;
  font-style: normal;
}

@font-face {
  font-family: 'MiSans-Bold';
  src: url('@/assets/MiSans-Bold.woff2') format('woff2');
  font-weight: 800;
  font-style: normal;
}

html,
body {
  margin: 0;
  padding: 0;
  overflow: hidden;
  background: #212121;
  height: 100vh;
  display: flex;
  align-items: center;
  justify-content: center;
}

#app {
  width: 100vw;
  height: 100vh;
  overflow: hidden;
}


.container {
  background: #FFF0;
  display: flex;
  align-items: center;
  justify-content: center;
  width: 100vw;
  height: 100vh;
}

.box {
  background: #843e28;
  max-width: 1200px;
  max-height: 900px;
  display: flex;
  align-items: center;
  justify-content: center;
  position: relative;
  flex: auto;
}

@media (max-width: 1075px) {
  .box {
    justify-content: flex-start;
  }
}

.imgBox {
  background: #0000;
  display: flex;
  align-items: center;
  justify-content: center;
  position: relative;
}

.image {
  object-fit: contain;
}

.inputField {
  position: absolute;
  background: transparent;
  border: none;
  outline: none;
  color: #2a2a2a;
  text-align: left;
  font-family: 'MiSans-Bold', sans-serif;
  text-align: left;
  vertical-align: text-top
}

.floating-buttons {
  position: fixed;
  bottom: 30px;
  left: 50%;
  transform: translateX(-50%);
  display: flex;
  gap: 10px;
  z-index: 999;
}

.floating-buttons button {
  padding: 10px 20px;
  font-size: 16px;
  border: none;
  border-radius: 5px;
  background-color: #007bff;
  color: white;
  cursor: pointer;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
}

.floating-buttons button:hover {
  background-color: #0056b3;
}

#titleBox {
  position: absolute;
  top: 0;
  left: 50%;
  transform: translateX(-50%);
  width: 100%;
  background: transparent;
  border: none;
  color: white;
  font-family: 'MiSans-Bold', sans-serif;
  text-align: center;
  vertical-align: text-bottom;
  display: flex;
  align-items: flex-end;
  justify-content: center;
}

#creditBox {
  position: absolute;
  bottom: 0;
  left: 50%;
  transform: translateX(-50%);
  width: 100%;
  background: transparent;
  border: none;
  color: white;
  font-family: 'MiSans-Bold', sans-serif;
  text-align: center;
  vertical-align: text-bottom;
  display: flex;
  align-items: flex-end;
  justify-content: center;
}
</style>