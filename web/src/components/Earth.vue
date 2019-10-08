<template>
  <div id="earth">
    Earth
  </div>
</template>

<script>
import earth from '../assets/earth/earth.jpg'
// import bath from '../assets/earth/bathymetry_bw_composite_4k.jpg'
// import starfield from '../assets/earth/starfield.jpg'
import night from '../assets/earth/night.jpg'
import clouds from '../assets/earth/clouds.png'
export default {
  name: 'Earth',
  data () {
    return {
      msg: 'Welcome to Your Vue.js App',
      option: {
        backgroundColor: '#000',
        globe: {
          globeRadius: 20,
          globeOuterRadius: 250,
          baseTexture: earth,
          // heightTexture: bath,

          // displacementScale: 0.01,

          shading: 'lambert',

          // environment: starfield,

          light: {
            ambient: {
              intensity: 0.1
            },
            main: {
              intensity: 1.5
            }
          },

          layers: [{
            type: 'blend',
            blendTo: 'emission',
            texture: night
          }, {
            type: 'overlay',
            texture: clouds,
            shading: 'lambert',
            distance: 0
          }]
        },
        series: [
          {
            type: 'scatter3D',
            name: 'Marcus',
            coordinateSystem: 'globe',
            data: [
              {
                name: 'Wuhan',
                value: [114, 30, 20]
              }, {
                name: '武汉30',
                value: [114, 30, 30]
              }, {
                name: '武汉40',
                value: [114, 30, 40]
              }, {
                name: '武汉50',
                value: [114, 30, 50]
              }, {
                name: '武汉100',
                value: [114, 30, 100]
              }, {
                name: '武汉200',
                value: [114, 30, 200]
              }
            ]
          }
        ]
      }
    }
  },
  mounted () {
    this.drawLine()
  },
  methods: {
    drawLine () {
      // 基于准备好的dom，初始化echarts实例
      let myChart = this.$echarts.init(document.getElementById('earth'))
      // 绘制图表
      myChart.setOption(this.option)
    }
  }
}
</script>

<style scoped>
  #earth{
    position: absolute;
    top: 0;
    right: 0;
    bottom: 0;
    left: 0;
    z-index: -10000;
  }
</style>
