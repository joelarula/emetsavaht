<template>
  <v-app>
    <v-navigation-drawer app fixed  v-model="drawer" :clipped="$vuetify.breakpoint.lgAndUp">
     kihid
    </v-navigation-drawer>
    <v-app-bar app dense dark flat clipped-left>
      <v-app-bar-nav-icon @click.stop="drawer = !drawer" />
       <v-app-bar-title> E-metsavaht</v-app-bar-title>   
       <v-spacer></v-spacer>
       about |  login | register
    </v-app-bar> 
   
      <v-content>
          <l-map ref="map" :zoom="zoom" :center="center"  >
          
            <l-control-layers />
             <l-wms-tile-layer
              v-for="layer in layers"
              :key="layer.name"
              :base-url="baseUrl"
              :format="layer.format"
              :version="layer.version"
              :layers="layer.layers"
              :crs="layer.crs"
              :visible="layer.visible"
              :transparent="layer.transparent"
              :name="layer.name"
              layer-type="base"
          />
          </l-map>
   
      </v-content>
     
   
  </v-app>
</template>

<script>

import 'leaflet/dist/leaflet.css';
import { LMap ,LControlLayers ,LWMSTileLayer} from 'vue2-leaflet';
import L from "leaflet";
import "proj4leaflet";
//import proj4 from "proj4";

//proj4.defs("EPSG:3301" ,"+proj=lcc +lat_1=59.33333333333334 +lat_2=58 +lat_0=57.51755393055556 +lon_0=24 +x_0=500000 +y_0=6375000 +ellps=GRS80 +towgs84=0,0,0,0,0,0,0 +units=m +no_defs"); 


const est = new L.Proj.CRS(
  'EPSG:3301',
  '+proj=lcc +lat_1=59.33333333333334 +lat_2=58 +lat_0=57.51755393055556 +lon_0=24 +x_0=500000 +y_0=6375000 +ellps=GRS80 +towgs84=0,0,0,0,0,0,0 +units=m /+no_defs',
  {
    resolutions: [
      4000, 3750, 3500, 3250, 3000, 2750, 2500, 2250, 2000, 1750, 1500, 1250, 1000, 750, 650, 500, 250, 100, 50, 20, 10, 5, 2.5, 2, 1.5, 1, 0.5
    ],
    origin: [2420000, 1350000]
 })

export default {
  name: 'App',
  components: { LMap,LControlLayers,"l-wms-tile-layer": LWMSTileLayer},
  data: () => ({
    drawer: false,
    map:null,
    zoom: 8,
    center: [58.566791, 24.989689],
    url: 'https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png',
    baseUrl: 'http://kaart.maaamet.ee/wms/alus',
    layers: [
        {
          name: 'TOPOYKSUS_6569',
          visible: true,
          crs: est,
          version: '1.1.1',
          format: 'image/png',
          layers: 'MA-ALUS',
          transparent: true,
        }
    ]
  }),
  mounted(){
      this.$nextTick(() => {     
       this.map = this.$refs.map.mapObject
      });
  },
};
</script>
<style>
    #map {
      
    } 
</style>