<template>
  <button @click="title = 'Changed Popup Title'">Change Title</button>
  <div id="map" />
</template>

<script>
import mapboxgl from "mapbox-gl";
import "mapbox-gl/dist/mapbox-gl.css";
import { onMounted } from "vue";
import { createApp, defineComponent, ref, nextTick } from "vue";
import MyPopup from "@/components/MyPopup.vue";
export default {
  setup() {
    const title = ref(location);
    onMounted(() => {
      mapboxgl.accessToken =
        "pk.eyJ1IjoieG5lb3giLCJhIjoiY2xhbnRsMTAwMDE2aTNuczRlcXp6b2t5ZCJ9.fApJifN-JTf1JDvXBH4FdA";
      const map = new mapboxgl.Map({
        container: "map",
        style: "mapbox://styles/mapbox/streets-v12",
        center: [-74.5, 40],
        zoom: 9, // starting zoom
      });
      

      // Here we want to setup the popup and marker
      map.on("click", function (e) {
        new mapboxgl.Marker()
        new mapboxgl.Popup()
          .setLngLat(e.lngLat)
          .setHTML('<div id="popup-content"></div>')
          
          .addTo(map);
         
        const MyNewPopup = defineComponent({
          extends: MyPopup,
          setup() {
            
            return { title };
          },
        });
        nextTick(() => {
          createApp(MyNewPopup).mount("#popup-content");
        });
      });
    });

    return { title };
  },
};
</script>

<style>
#map {
  height: 100vh;
}
</style>
