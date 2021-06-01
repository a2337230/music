<template>
  <div class="hello">
    <canvas id="canvas" style="border:0px solid #f00"/>
    <audio id="audio" controls :src="src"/>
    <button @click="run">播放</button>
  </div>
</template>

<script>
export default {
  name: "HelloWorld",
  data() {
    return {
      src: require("./../msc/hdl.mp3"),
      color: "",
      colort: "",
      cssn: 2
    };
  },
  mounted() {
    // this.play();
  },
  methods: {
    run() {
      let audio = document.getElementById("audio");
      audio.play();
      this.play()
    },
    play() {
      let _this = this
      let $ = function (id) {
        return document.getElementById(id);
      };
      let voicec1=["#EEEEE0","#EEEEE0","#EEEEE0"];
      let canvas = $("canvas");
      let audio = $("audio");
      window.AudioContext =
      window.AudioContext ||
      window.webkitAudioContext ||
      window.mozAudioContext;
      let ctx = canvas.getContext("2d");
      let actx = new AudioContext();
      this.color = ctx.createLinearGradient(
        canvas.width * 0.5,
        0,
        canvas.width * 0.5,
        300
      );
      this.color.addColorStop(0, voicec1[0])
      this.color.addColorStop(0.5, voicec1[1])
      this.color.addColorStop(1, voicec1[2])
      this.colort = ctx.createLinearGradient(canvas.width * 0.5,0,canvas.width * 0.5,300)
      this.colort.addColorStop(0, voicec1[2])
      this.colort.addColorStop(0.5, voicec1[1])
      this.colort.addColorStop(1, voicec1[2])
      canvas.width = window.innerWidth;
      canvas.height = 600;
      let analyser = actx.createAnalyser();
      let audioSrc = actx.createMediaElementSource(audio);
      audioSrc.connect(analyser);
      analyser.connect(actx.destination)
      let num = 100
      function draw() {
        let voicehigh = new Uint8Array(analyser.frequencyBinCount);
        analyser.getByteFrequencyData(voicehigh);
        let step = Math.round(voicehigh.length / num);
        ctx.clearRect(0, 0, canvas.width, canvas.height);
        ctx.beginPath();
        for (let i = 1; i < num; i++) {
          let value = voicehigh[step * i];
          ctx.fillStyle = _this.color;
          ctx.fillRect(i * 10 + canvas.width * 0.5, 250, 7, -value + 1);
          ctx.fillRect(canvas.width * 0.5 - (i - 1) * 10, 250, 7, -value + 1);
          ctx.fill();
          ctx.fillStyle = _this.colort;
          ctx.fillRect(i * 10 + canvas.width * 0.5, 250, 7, value + 1);
          ctx.fillRect(canvas.width * 0.5 - (i - 1) * 10, 250, 7, value + 1);
        }
        requestAnimationFrame(draw);
      }
      draw();
    },
  },
};
</script>
<style lang="css">
.hello {
  background: url('https://img0.baidu.com/it/u=4182936096,4047363586&fm=26&fmt=auto&gp=0.jpg');
  background-size: contain;
}
#canvas {
  /* opacity: 0.3; */
}
</style>
