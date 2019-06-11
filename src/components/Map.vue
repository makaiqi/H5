<template>
  <div class="amap-page-container">
      <el-amap ref="map" 
            vid="amapDemo" 
            :amap-manager="amapManager" 
            :center="center" 
            :zoom="zoom" 
            :plugin="plugin" 
            :events="events"
            class="amap-demo">
        <!-- <el-amap-marker 
            v-for="(marker, index) in markers" 
            :key="index"
            :position="marker.position" 
            :events="marker.events" 
            :visible="marker.visible" 
            :draggable="marker.draggable" 
            :vid="index">
        </el-amap-marker> -->
      </el-amap>
    </div>
</template>

<script>
import Vue from 'vue'
import VueAMap from 'vue-amap'
let amapManager = new VueAMap.AMapManager();
export default {
  name: 'Map',
  props: {
     mapCenter: Array
    },
   data () {
     let that =this;
      return {
        scenicId:13,
        search:'',
        amapManager,
        zoom: 13,
        center: [121.59996,31.197646],
        events: {
            init: (o) => {
                o.getCity(result => {
                console.log(result,'s')
                })
            },
            'moveend': (e) => {},
            'zoomchange': () => {},
            'click': (e) => {
                console.log(e,'click=>e')
            }
        },
        plugin: ['ToolBar'],
        markers: [
            {
            position: [121.59996,31.197646],
            events: {
            click: (e) => {
                console.log(e,"click marker");
            },
            dragend: (e) => {
                console.log('---event---: dragend')
                this.markers[0].position = [e.lnglat.lng, e.lnglat.lat];
            }
            },
            visible: true,
            draggable: true,
            template:'<span>6666666666666666</span>',
            }
        ],
          
      }
    },
    watch:{
        'mapCenter'(n,o){
            this.center = n;
        },  
    },
    mounted(){
        console.log(this.mapCenter)
        this.findList(this.scenicId);
    },
    methods: {
        findList(id) {
        let that = this;
        that.markers = [];
        that.list = [];
        that.tableName = "";
        that.$axios.get("/ma/Home/menu2point?scenicId="+that.scenicId+"&menuId=" + 137 +'&search='+that.search).then(rs => {
            if (rs.data.code == 0) {
                let data = rs.data.data;
                console.log(data,'data');
                for (let i = 0; i < data.length; i++) {
                //标点
                let marker = {
                    // iconPath: "/static/images/dingwei.png" 
                };
                marker.id = data[i].pointId;
                marker.latitude = data[i].lat;
                marker.longitude = data[i].lng;
                that.markers.push(marker);
                //列表
                // let pointList = {};
                // pointList.href = "";
                // pointList.assetUrl = data[i].assetUrl;
                // pointList.bannerUrl = data[i].bannerUrl;
                // pointList.name = data[i].itemName;
                // pointList.id = data[i].pointId;
                // pointList.tableName = data[i].tableName;
                // pointList.tableId = data[i].tableId;
                // this.list.push(pointList);
                }
                //  console.log(this.markers)
            }
            });
        },
    }
}
</script>

<style>
.amap-page-container{
    height:520px;
    width: 100%;
}
</style>
