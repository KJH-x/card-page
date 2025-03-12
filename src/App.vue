<template>
  <div class="container">
    <div class="box" id="borderBox">
      <div class="imgBox" id="imageBox">
        <img src="@/assets/card_base.png" alt="展示图片" id="cardBase" class="image" />
        <div v-for="(value, key) in inputs" :key="key">
          <input class="inputField" type="text" v-model="inputs[key]" :id="key" :placeholder="'请输入' + key"
            @input="saveInputs" @blur="saveInputs" />
        </div>
      </div>
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
      // baseScale: 1,
      inputs: {
        codeName: '',
        subjectCode: '',
        position: '',
        department: ''
      },
      inputFontStyle: {
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
      textLeft: 80
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
      const scale = window.innerWidth / 1075;
      Object.entries(this.inputFontStyle).forEach(([id, size]) => {
        const input = document.getElementById(id);
        if (input) {
          if (window.innerWidth < 1075) {
            input.style.fontSize = `${size.fontSize * scale}px`;
            input.style.top = `${size.top * scale}px`;
          } else {
            input.style.fontSize = `${size.fontSize}px`;
            input.style.top = `${size.top}px`;
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
    },
    generateAndDownloadImage() {
      // Load the base image
      const img = new Image();
      img.src = require('@/assets/card_base.png');  // Replace with the correct path if needed

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
          ctx.font = `${this.inputFontStyle[key].fontSize}px MiSans-Bold`;
          ctx.beginPath();
          const textMetrics = ctx.measureText(value);
          if (textMetrics.width >= 910) {
            alert(`输入的${key}内容过长`)
          } else {
            ctx.fillText(value, this.textLeft, this.inputFontStyle[key].top);
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
  /* border-radius: calc(100vw * (20 / 1200)); */
  /* max-width: 1200px; */
  /* max-height: 900px; */
  /* width: 100vw; */
  /* height: calc(100vw * (900 / 1200)); */
  display: flex;
  align-items: center;
  justify-content: center;
  position: relative;
}

.imgBox {
  background: #0000;
  /* max-width: 1075px; */
  /* max-height: 706px; */
  /* width: 100vw; */
  /* height: calc(100vw * (706 / 1075)); */
  display: flex;
  align-items: center;
  justify-content: center;
  position: relative;
}

.image {
  /* width: 100%; */
  /* max-width: 1075px; */
  /* height: auto; */
  /* max-height: 706px; */
  object-fit: contain;
}

.inputField {
  position: absolute;
  left: 80px;
  width: 910px;
  background: transparent;
  border: none;
  outline: none;
  color: #2a2a2a;
  letter-spacing: -2.5px;
  text-align: left;
  font-family: 'MiSans-Bold', sans-serif;
}

#codeName {
  top: 106px;
  height: 130px;
  font-size: 100px;
}

#subjectCode {
  top: 275px;
  height: 65px;
  font-size: 50px;
}

#position {
  top: 385px;
  height: 65px;
  font-size: 50px;
}

#department {
  top: 489px;
  height: 65px;
  font-size: 50px;
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

</style>