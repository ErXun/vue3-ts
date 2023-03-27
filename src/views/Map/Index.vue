<template>
  <div class="map" ref="mapEle"></div>
  <button class="btn" @click="handleChange">切换</button>
  <button class="btn" @click="handleRemove">remove</button>
  <button class="btn" @click="handleAddImg">add img</button>
</template>

<script lang="ts" setup>
import maplibregl from "maplibre-gl";
import "maplibre-gl/dist/maplibre-gl.css";
import { defineComponent, onMounted, reactive, toRefs, ref } from "vue";

const mapEle = ref(null);
let map: any = reactive({});
const MY_KEY = "b53e756d10fd4737237d035f9eec858f";
const mapSource = [
  {
    key: "vector",
    baseMapUrl: `http://t0.tianditu.gov.cn/vec_w/wmts?SERVICE=WMTS&REQUEST=GetTile&VERSION=1.0.0&LAYER=vec&STYLE=default&TILEMATRIXSET=w&FORMAT=tiles&TILECOL={x}&TILEROW={y}&TILEMATRIX={z}&tk=${MY_KEY}`,
    markUrl: `http://t0.tianditu.gov.cn/cva_w/wmts?SERVICE=WMTS&REQUEST=GetTile&VERSION=1.0.0&LAYER=cva&STYLE=default&TILEMATRIXSET=w&FORMAT=tiles&TILECOL={x}&TILEROW={y}&TILEMATRIX={z}&tk=${MY_KEY}`,
  },
  {
    key: "image",
    baseMapUrl: `http://t0.tianditu.gov.cn/img_w/wmts?SERVICE=WMTS&REQUEST=GetTile&VERSION=1.0.0&LAYER=img&STYLE=default&TILEMATRIXSET=w&FORMAT=tiles&TILECOL={x}&TILEROW={y}&TILEMATRIX={z}&tk=${MY_KEY}`,
    markUrl: `http://t0.tianditu.gov.cn/cia_w/wmts?SERVICE=WMTS&REQUEST=GetTile&VERSION=1.0.0&LAYER=cia&STYLE=default&TILEMATRIXSET=w&FORMAT=tiles&TILECOL={x}&TILEROW={y}&TILEMATRIX={z}&tk=${MY_KEY}`,
  },
];
const initOption = {
  style: {
    version: 8,
    sources: {
      "tdt-vec-source": {
        type: "raster",
        tiles: [mapSource[0].baseMapUrl],
        tileSize: 256,
      },
      "tdt-cva": {
        type: "raster",
        tiles: [mapSource[0].markUrl],
        tileSize: 256,
      },
    },
    layers: [
      {
        id: "tdt-tiles-layer",
        type: "raster",
        source: "tdt-vec-source",
        paint: {},
      },
      {
        id: "tdt-cva-layer",
        type: "raster",
        source: "tdt-cva",
      },
    ],
  },
  center: [118.778, 32.043],
  zoom: 13,
  pitch: 38,
  // maxZoom: 20, //最大缩放级别
  // minZoom: 8, //最小缩放级别
};

onMounted(() => {
  map = new maplibregl.Map({
    container: mapEle.value,
    ...initOption,
  });
  map.on("click", (e: any) => {
    const { lng, lat } = e.lngLat;
    console.log("e", e);
    console.log(lng, lat);
  });
});

// 切换图层
const handleChange = () => {
  // 添加底图
  map.addSource("tdt-source-imgBase", {
    type: "raster",
    tiles: [mapSource[1].baseMapUrl],
    tileSize: 256,
  });
  map.addLayer({
    id: "tsib-layer",
    type: "raster",
    source: "tdt-source-imgBase",
  });
  // 添加标注
  map.addSource("tdt-source-imgMark", {
    type: "raster",
    tiles: [mapSource[1].markUrl],
    tileSize: 256,
  });
  map.addLayer({
    id: "tsim-layer",
    type: "raster",
    source: "tdt-source-imgMark",
  });
};
// 移除图层
const handleRemove = () => {
  if (map.getLayer("tsib-layer")) {
    map.removeLayer("tsib-layer");
    map.removeSource("tdt-source-imgBase");
  }
  if (map.getLayer("tsim-layer")) {
    map.removeLayer("tsim-layer");
    map.removeSource("tdt-source-imgMark");
  }
};
// add img
const handleAddImg = () => {
  const markOne = new maplibregl.Marker()
    .setLngLat([118.7537691547708, 32.038529098345904])
    .addTo(map);

  const marker2 = new maplibregl.Marker({ color: "black", rotation: 45 })
    .setLngLat([118.75443842743528, 32.03030570710669])
    .addTo(map);
};
</script>

<style scoped lang="less">
.map {
  height: 800px;
  width: 100%;
}
.btn {
  margin: 20px;
  // position: fixed;
  // top: 5%;
  // right: 5%;
}
</style>
