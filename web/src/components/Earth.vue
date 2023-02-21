<template>
  <div id="earth">
    Earth
  </div>
</template>

<script>
import earth from '../assets/earth/earth.jpg'
import night from '../assets/earth/night.jpg'
import clouds from '../assets/earth/clouds.png'
import * as echarts from 'echarts'
import 'echarts-gl'
import * as satellite from "satellite.js"
export default {
  name: 'MEarth',
  mounted () {
    this.option.series[0].data.push({
      name: '空间站',
      value: this.twoline2satrec(
        '1 48274U 21035A   23051.00000000  .00013583  00000-0  17005-3 0  9993',
        '2 48274  41.4742 150.5704 0004665 278.9765 139.4829 15.61237738103515'
      )
    })
    this.drawLine()
  },
  data(){
    return {
      option: {
        backgroundColor: '#000000',
        globe: {
          globeRadius: 20,
          globeOuterRadius: 40,
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
              // {
              //   name: 'Wuhan',
              //   value: [114, 30, 20]
              // }, {
              //   name: '武汉30',
              //   value: [114, 30, 30]
              // }, {
              //   name: '武汉40',
              //   value: [114, 30, 40]
              // }, {
              //   name: '武汉50',
              //   value: [114, 30, 50]
              // }, {
              //   name: '武汉100',
              //   value: [114, 30, 100]
              // }, {
              //   name: '武汉200',
              //   value: [114, 30, 200]
              // }
            ]
          }
        ]
      }
    }
  },
  methods: {
    twoline2satrec(tleLine1, tleLine2){
      const satrec = satellite.twoline2satrec(
        tleLine1,
        tleLine2
      );
      // 计算当前时间
      const now = new Date();

      // 计算卫星的位置和速度
      const positionAndVelocity = satellite.propagate(satrec, now);

      // 获取卫星的地心坐标系位置向量
      const positionEci = positionAndVelocity.position;

      // 将地心坐标系位置向量转换为经纬度和海拔高度
      const positionGd = satellite.eciToGeodetic(positionEci, satellite.gstime(now));
      const longitude = satellite.degreesLong(positionGd.longitude);
      const latitude = satellite.degreesLat(positionGd.latitude);
      const altitude = positionGd.height;

      // 输出卫星的经纬度和海拔高度
      console.log("Longitude:", longitude, "Latitude:", latitude, "Altitude:", altitude, altitude / 637.8 * 20 + 20);
      return [longitude, latitude, altitude / 637.8 * 20 + 20]
      // return [114, 30, 25]
    },
    drawLine () {
      // 基于准备好的dom，初始化echarts实例
      const myChart = echarts.init(document.getElementById('earth'))
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
}
</style>
