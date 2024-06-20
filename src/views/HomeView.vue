<template>
  <div>
    <canvas ref="canvas" @mousedown="startDrawing" @mousemove="draw" @mouseup="stopDrawing"></canvas>
    <div>
      <button @click="clearCanvas">清除</button>
      <button @click="saveSignature">保存</button>
    </div>
    <div class="resword">
      生成的文件base64编码：{{ res }}
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      isDrawing: false, // 是否正在绘制
      context: null, // Canvas上下文
      res:''
    };
  },
  mounted() {
    this.context = this.$refs.canvas.getContext("2d"); // 获取Canvas上下文
    this.$refs.canvas.width = 500; // 设置Canvas的宽度
    this.$refs.canvas.height = 300; // 设置Canvas的高
  },
  methods: {
    // 鼠标按下时触发
    startDrawing(event) {
      this.isDrawing = true; // 开始绘制
      const { offsetX, offsetY } = event; // 获取鼠标相对于Canvas的偏移量
      this.context.beginPath(); // 开始新的路径
      this.context.moveTo(offsetX, offsetY); // 将路径移动到鼠标位置
    },
    // 当鼠标在 Canvas 上移动时触发
    draw(event) {
      if (!this.isDrawing) return; // 如果没有在绘制中，则返回
      const { offsetX, offsetY } = event; // 获取鼠标相对于Canvas的偏移量
      this.context.lineTo(offsetX, offsetY); // 绘制路径
      this.context.stroke(); // 绘制路径的边框
    },
    // 当鼠标松开时触发，用于停止绘制签名
    stopDrawing() {
      this.isDrawing = false; // 停止绘制
    },
    // 清除
    clearCanvas() {
      this.context.clearRect(
        0,
        0,
        this.$refs.canvas.width,
        this.$refs.canvas.height
      ); // 清除Canvas上的内容
    },
    // 保存
    saveSignature() {
      const dataURL = this.$refs.canvas.toDataURL(); // 获取签名图片的Base64编码
      // 在这里可以将dataURL发送到服务器保存，或者进行其他操作
      console.log(dataURL); // 输出签名的Base64编码到控制台
      this.res=dataURL
    },
  },
};
</script>
<style scoped>
canvas {
  border: 1px solid red;
}
.resword{
  word-break: break-all;
}
</style>
