<template>
  <div class="container">
    <div class="box" id="borderBox">
      <div class="imgBox" id="imageBox">
        <img src="@/assets/card_base.png" alt="展示图片" id="cardBase" class="image" />
        <input class="inputField" type="text" id="codeName" placeholder="请输入文字" />
        <input class="inputField" type="text" id="subjectCode" placeholder="请输入文字" />
        <input class="inputField" type="text" id="position" placeholder="请输入文字" />
        <input class="inputField" type="text" id="department" placeholder="请输入文字" />
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      // baseScale: 1,
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
      }
    };
  },
  mounted() {
    // this.calculateBaseScale();
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
    // calculateBaseScale() {
    //   this.baseScale = window.innerWidth / 1200;
    // },
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
    }
  }
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

.container {
  background: #FFF0;
  display: flex;
  align-items: center;
  justify-content: left;
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
}

#codeName {
  top: 106px;
  height: 130px;
  font-family: 'MiSans-Semi', sans-serif;
  font-size: 100px;
}

#subjectCode {
  top: 275px;
  height: 65px;
  font-family: 'MiSans-Semi', sans-serif;
  font-size: 50px;
}

#position {
  top: 385px;
  height: 65px;
  font-family: 'MiSans-Bold', sans-serif;
  font-size: 50px;
}

#department {
  top: 489px;
  height: 65px;
  font-family: 'MiSans-Bold', sans-serif;
  font-size: 50px;
}
</style>