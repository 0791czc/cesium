<template>
	<div id="cesiumContainer"></div>
</template>

<script setup>
import * as Cesium from 'cesium';
import { onMounted } from 'vue'

	onMounted(()=>{
		// const viewer = new Cesium.Viewer('cesiumContainer');
		var custom = new Cesium.ArcGisMapServerImageryProvider({
			url:'https://services.arcgisonline.com/ArcGIS/rest/services/World_Street_Map/MapServer'
		});
		var viewer = new Cesium.Viewer('cesiumContainer',{
			baseLayerPicker:false,
			imageryProvider:custom,
			terrainProvider:Cesium.createWorldTerrain({
				requestWaterMask:true,
				requestVertexNormals:true,
			})
		});
		// 设置相机位置
		// viewer.camera.setView({ 
		// 	destination:Cesium.Cartesian3.fromDegrees(113.318977,23.114155,20000),
		// 	orientation:{
		// 		heading:Cesium.Math.toRadians(90),//方向
		// 		pitch:Cesium.Math.toRadians(-90),//仰角
		// 	}
		// }) 
		//设置相机位置
		viewer.camera.setView({
			destination:new Cesium.Cartesian3(1332761,-4662399,4137888),
			orientation:{
				heading:0.60,//方向
				pitch:-0.66,//仰角
			}
		})
		//引入cesium自带三维
		var city = viewer.scene.primitives.add(new Cesium.Cesium3DTileset({url:Cesium.IonResource.fromAssetId(75343)}))
		//定义3d建筑样式
		var heightStyle = new Cesium.Cesium3DTileStyle({
			color:{
				//条件判断具体颜色
				conditions:[
					['${Height} >= 300','rgba(245,34,45,0.8)'],
					['${Height} >= 200','rgba(0, 197, 183,0.8)'],
					['${Height} >= 100','rgba(255, 226, 17,0.8)'],
					['${Height} >= 50','rgba(250,84,28,0.8)'],
					['${Height} >= 20','rgba(102,204,255,0.8)'],
					['${Height} >= 10','rgba(19,194,194,0.8)'],
					['${Height} >= 5','rgba(114,46,209,0.8)'],
					['true','rgba(102,204,255,0.5)'],
				]
			}
		})
		city.style = heightStyle
		// geojson文件加载
		
	})
</script>

<style scoped>
</style>
