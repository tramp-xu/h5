<template>
  <div class="photo-wrap">
    <img class="photo-people" src="/action/people.png" alt="" />
    <div class="photo-canvas-wrapper">
      <canvas class="photo-canvas-cube" width="100%" height="100%"
        >该浏览器不支持 canvas</canvas
      >
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      photos: [],
      curCanvas: null
    };
  },
  methods: {
    drawjx(ele, startx, starty, zxp, zyp, zh, zcolor, upcolor, cmcolor) {
      // 正面
      ele.beginPath();
      ele.lineWidth = 1;
      ele.strokeStyle = "transparent";
      ele.moveTo(startx, starty);
      ele.lineTo(startx + zxp, starty + zyp);
      ele.lineTo(startx + zxp, starty + zyp + zh);
      ele.lineTo(startx, starty + zh);
      ele.lineTo(startx, starty);
      ele.fillStyle = zcolor;
      ele.fill();

      ele.closePath();

      // 上面
      ele.beginPath();
      ele.strokeStyle = "transparent";
      ele.moveTo(startx, starty);
      ele.lineTo(startx + zxp, starty - zyp);
      ele.lineTo(startx + 2 * zxp, starty);
      ele.lineTo(startx + zxp, starty + zyp);
      ele.lineTo(startx, starty);

      ele.fillStyle = upcolor;
      ele.fill();
      ele.closePath();

      // 侧面
      ele.beginPath();
      ele.strokeStyle = "transparent";
      ele.moveTo(startx + 2 * zxp, starty); //正面右上
      ele.lineTo(startx + 2 * zxp, starty + zh); //正面右下
      ele.lineTo(startx + zxp, starty + zh + zyp); //侧面的点
      ele.lineTo(startx + zxp, starty + zyp); //上面右点
      ele.lineTo(startx + 2 * zxp, starty); //正面右上
      ele.fillStyle = cmcolor;
      ele.fill();
      ele.closePath();
    },
    handlerNum(width) {
      let initW = 200;
      let ratio = width / initW;
      return function autoFix(number) {
        return number * ratio;
      };
    },
    drawCube() {
      let myCav = (this.curCanvas = document.querySelector(
        ".photo-canvas-cube"
      ));
      let wrap = document.querySelector(".photo-canvas-wrapper");
      let width = wrap.clientWidth;
      let height = wrap.clientHeight;
      myCav.width = width;
      myCav.height = height;
      let handler = this.handlerNum(width);
      let draw = myCav.getContext("2d");
      this.drawjx(
        draw,
        handler(10),
        handler(60),
        handler(100),
        handler(20),
        handler(350),
        "#000",
        "#fff",
        "#ddd"
      );
    }
  },
  mounted() {
    this.drawCube();
    window.onresize = () => {
      return (() => {
        this.drawCube();
      })();
    };
  }
};
</script>
<style lang="less" scoped>
.photo {
  &-wrap {
    position: relative;
    width: 750px;
    height: 100vh;
    background-color: #66c6f2;
  }
  &-people {
    width: 80px;
    position: absolute;
    bottom: 400px;
    left: 100px;
    z-index: 100;
  }
  &-canvas-wrapper {
    width: 200px;
    height: 400px;
    position: absolute;
    bottom: 60px;
    left: 30px;
  }
}
</style>
