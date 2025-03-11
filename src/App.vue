<template>
  <div class="container">
    <div class="box">
      <div class="imgBox">
        <img src="@/assets/card_base.png" alt="展示图片" class="image" />
        <input type="text" class="text-inputA" placeholder="请输入文字" />
        <input type="text" class="text-inputB" placeholder="请输入文字" />
        <input type="text" class="text-inputC" placeholder="请输入文字" />
        <input type="text" class="text-inputD" placeholder="请输入文字" />
      </div>
    </div>
  </div>
</template>

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
  background: #FFF;
  display: flex;
  align-items: center;
  justify-content: center;
  width: 99vw;
  height: 98vh;
}

.box {
  background: #843e28;
  border-radius: calc(100vw * (20 / 1200));
  max-width: 1200px;
  max-height: 900px;
  width: 100vw;
  height: calc(100vw * (900 / 1200));
  display: flex;
  align-items: center;
  justify-content: center;
  position: relative;
}

.imgBox {
  background: #0000;
  max-width: 1075px;
  max-height: 706px;
  width: 100vw;
  height: calc(100vw * (706 / 1075));
  display: flex;
  align-items: center;
  justify-content: center;
  position: relative;
}

.image {
  width: 100%;
  max-width: 1075px;
  height: auto;
  max-height: 706px;
  object-fit: contain;
}

.text-inputA {
  position: absolute;
  top: 106px;
  left: 80px;
  width: 910px;
  height: 130px;
  background: transparent;
  border: none;
  outline: none;
  font-family: 'MiSans-Semi', sans-serif;
  font-size: 100px;
  color: #2a2a2a;
  text-align: left;
  letter-spacing: -2.5px;
}

.text-inputB {
  position: absolute;
  top: 275px;
  left: 80px;
  width: 910px;
  height: 50px;
  background: transparent;
  border: none;
  outline: none;
  font-family: 'MiSans-Semi', sans-serif;
  font-size: 50px;
  color: #2a2a2a;
  text-align: left;
  letter-spacing: -2.5px;
}

.text-inputC {
  position: absolute;
  top: 385px;
  left: 80px;
  width: 910px;
  height: 50px;
  background: transparent;
  border: none;
  outline: none;
  font-family: 'MiSans-Bold', sans-serif;
  font-size: 50px;
  color: #2a2a2a;
  text-align: left;
  letter-spacing: -2.5px;
}

.text-inputD {
  position: absolute;
  top: 489px;
  left: 80px;
  width: 910px;
  height: 50px;
  background: transparent;
  border: none;
  outline: none;
  font-family: 'MiSans-Bold', sans-serif;
  font-size: 50px;
  color: #2a2a2a;
  text-align: left;
  letter-spacing: -2.5px;

}
</style>