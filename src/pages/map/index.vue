<template>
  <view class="map-container">
    <map 
      class="map" 
      id="map" 
      :longitude="longitude" 
      :latitude="latitude" 
      scale="14" 
      show-location="true" 
      :markers="markers" 
      @markertap="makertap"></map>
    <view class="map_text">
      <text class="h1">{{textData.name}}</text>
      <text>{{textData.desc}}</text>
    </view>
  </view>
</template>

<script>
import { MAP_KEYS } from "./config"
var amapFile = require('@/lib/amap-wx.130.js')

export default {
  name: 'Map',
  data() {
    return {
      longitude: "",
      latitude: "",
      markers: null,
      textData: {},
      markersData: null
    }
  },
  onLoad: function() {
    var that = this;
    var myAmapFun = new amapFile.AMapWX({ key: MAP_KEYS.WEIXIN });
    myAmapFun.getPoiAround({
      iconPathSelected: '/static/mapIcon/icon.png', //如：..­/..­/img/marker_checked.png
      iconPath: '/static/mapIcon/icon.png', //如：..­/..­/img/marker.png
      success: function(data){
        that.markersData = data.markers;
        that.marker = data.markers
        that.latitude = that.markersData[0].latitude
        that.longitude = that.markersData[0].longitude
        that.showMarkerInfo(that.markersData,0);
      },
      fail: function(info){
        uni.showModal({title:info.errMsg})
      }
    })
  },
  methods: {
    makertap(e) {
      var id = e.markerId;
      var that = this;
      that.showMarkerInfo(this.markersData,id);
      that.changeMarkerColor(this.markersData,id);
    },
    showMarkerInfo(data,i){
      this.textData = {
        name: data[i].name,
        desc: data[i].address
      }
    },
    changeMarkerColor(data,i){
      var markers = [];
      for(var j = 0; j < data.length; j++){
        if(j==i){
          data[j].iconPath = "/static/mapIcon/icon.png"; //如：..­/..­/img/marker_checked.png
        }else{
          data[j].iconPath = "/static/mapIcon/icon.png"; //如：..­/..­/img/marker.png
        }
        markers.push(data[j]);
      }
      this.markers = markers
    }
  }
}
</script>

<style scoped>
.map-container{
  position: absolute;
  top: 0;
  bottom: 80px;
  left: 0;
  right: 0;
}
.map{
  width: 100%;
  height: 100%;
}
.map_text{
  position: absolute;
  left: 0;
  right: 0;
  bottom: 0px;
  height: 80px;
  background: #fff;
  padding: 0 15px;
}
text{
  margin: 5px 0;
  display: block;
  font-size:12px;
}
.h1{
  margin: 15px 0;
  font-size:15px;
}
</style>