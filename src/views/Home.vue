<template>
  <div id="home" style="width: 100%;height:100vh;">
      <div @click="remove" class="remove">
        画折线
      </div>
  </div>
</template>

<script>
import AMapLoader from '@amap/amap-jsapi-loader'
export default {
  name: 'Home',
  components: {
  },
  data() {
    return {
      map: null,
      polyline: [],//坐标组
      AMap: null,
      num: 0,
    }
  },
  mounted() {
    this.initMap()
  },
  methods: {
    initMap(){
      AMapLoader.load({
        key: '12a2f404c5ab23ff49804af3ebf248ee',
        version: '2.0',
        plugins: ['AMap.ElasticMarker', 'Pixel'],
        Loca: {
          // 是否加载 Loca， 缺省不加载
          version: '2.0.0' // Loca 版本，缺省 1.3.2
        }
      }).then((AMap) => {
        this.AMap = AMap
        this.map = new AMap.Map('home', {
          // 设置地图容器id
          rotateEnable: true,
          pitchEnable: true,
          terrain: true,
          pitch: 85,
          mapStyle: 'amap://styles/normal',
          viewMode: '2D',
          showLabel: false,
          layers: [new AMap.TileLayer.Satellite()],
          zoom: 17,
          center: [103.448735,23.29681] // 初始化地图中心点位置
        })
        // 天地图栅格图层加载
        // const googleLayer = new AMap.TileLayer({
        //   getTileUrl: 'http://t0.tianditu.gov.cn/DataServer?T=img_w&tk=b12501d12f71425ea6b0f2a4ed9eb63a&x=[x]&y=[y]&l=[z]',
        //   zIndex: 2
        // })
        // googleLayer.setMap(this.map)
        // 地图加载完成后执行
        this.map.on('click',this.AddSome)
      })
    },
    AddMarker(e){
      console.log(e)
      this.num++
      const marker = new this.AMap.Marker({
        position: [e.lnglat.lng, e.lnglat.lat],
        map: this.map,
        label: {
          content: '<div>' + '第' + this.num + '个标点' + '</div>',
          offset: new this.AMap.Pixel(0, 0),
          direction: 'top',
          zIndex: 100
        },
        icon: new this.AMap.Icon({
          size: new this.AMap.Size(19, 31),  //图标大小
          image: 'https://webapi.amap.com/theme/v1.3/markers/n/mark_b.png',
          imageOffset: new this.AMap.Pixel(0, 0),
          imageSize: new this.AMap.Size(19, 31),
        }),
        offset: new this.AMap.Pixel(-10, -31), //相对于基点的偏移位置
      })
      marker.setMap(this.map)
      this.polyline.push(e.lnglat.lng+','+e.lnglat.lat)
      // this.map.add(marker)
      console.log(this.polyline)
    },
    //生成折线
    remove(){

    },
    AddSome(value){
      this.AddMarker(value)
    },

    LineDraw(){
      //

    }
  },
}
</script>


<style>
.remove{
  z-index: 999999;
  background-color: #fff;
}
</style>
