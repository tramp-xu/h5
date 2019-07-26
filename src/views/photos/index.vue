<template>
  <div class="photo-wrap">
    <canvas class="photo-canvas-cube" width="100%" height="100%">
      该浏览器不支持 canvas
    </canvas>
  </div>
</template>

<script>
export default {
  data() {
    return {
      photos: [],
      curCanvas: {}
    };
  },
  methods: {
    // 绘制立方体的函数
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
      ele.moveTo(startx + 2 * zxp, starty);
      ele.lineTo(startx + 2 * zxp, starty + zh);
      ele.lineTo(startx + zxp, starty + zh + zyp);
      ele.lineTo(startx + zxp, starty + zyp);
      ele.lineTo(startx + 2 * zxp, starty);
      ele.fillStyle = cmcolor;
      ele.fill();
      ele.closePath();
    },
    // 处理宽度的函数
    handlerWidth(width) {
      let initW = 750;
      let ratio = width / initW;
      return function autoFix(number) {
        return number * ratio;
      };
    },
    // 处理高度的函数
    handlerHeight(width) {
      let initW = 1334;
      let ratio = width / initW;
      return function autoFix(number) {
        return number * ratio;
      };
    },
    // 返回适配处理函数
    cbFn() {
      this.curCanvas = document.querySelector(".photo-canvas-cube");
      let wrap = document.querySelector(".photo-wrap");
      let width = wrap.clientWidth;
      let height = wrap.clientHeight;
      this.curCanvas.width = width;
      this.curCanvas.height = height;
      let handlerWidth = this.handlerWidth(width);
      let handlerHeight = this.handlerHeight(height);
      let context = this.curCanvas.getContext("2d");
      return { context, handlerWidth, handlerHeight };
    },
    // 绘制整个 canvas
    draw() {
      let { context, handlerWidth, handlerHeight } = this.cbFn();
      this.drawCube(context, handlerWidth, handlerHeight);
      this.drawPeople(context, handlerWidth, handlerHeight);
    },
    // 绘制人
    drawPeople(context, handlerWidth, handlerHeight) {
      let img = new Image();
      img.src = "/action/people.png";
      img.onload = () => {
        context.drawImage(
          img,
          handlerWidth(110),
          handlerHeight(700),
          handlerWidth(70),
          handlerHeight(200)
        );
      };
    },
    // 绘制立方体
    drawCube(context, handlerWidth, handlerHeight) {
      this.drawjx(
        context,
        handlerWidth(50),
        handlerHeight(900),
        handlerWidth(100),
        handlerWidth(20),
        handlerHeight(750),
        "#000",
        "#fff",
        "#ddd"
      );
    }
  },
  mounted() {
    this.draw();
    window.onresize = () => {
      return (() => {
        this.draw();
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
}
</style>
