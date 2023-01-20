<template>
  <div class="container">
    <div class="uploader">
      <h1>Upload a GeoJSON file</h1>
      <input class="input" type="file" @change="onFileChange" />
      <button class= 'btn' @click="submit">Submit</button>
    </div>
    <div class="map" >
      <l-map ref="map" :center="[78.44722884470269, 17.35146324412854]" v-model:zoom="zoom">
        <l-tile-layer url="https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png" layer-type="base" name="OpenStreetMap"
          :center="center"></l-tile-layer>
        <l-geo-json v-if="isData" :geojson="geojson" />
        <l-polygon :lat-lngs="polygon.latlngs" :color="polygon.color" />
      </l-map>
    </div>
  </div>


</template>

<script>
import "leaflet/dist/leaflet.css";
import { isProxy, toRaw } from "vue";
import {
  LMap,
  LTileLayer,
  LPolygon,
  LPolyline,
  LGeoJson,
} from "@vue-leaflet/vue-leaflet";

export default {
  components: {
    LMap,
    LTileLayer,
    LPolyline,
    LPolygon,
    LGeoJson,
  },
  data() {
    return {
      isData: false,
      zoom: 12,
      center: [17.35146324412854, 78.44722884470269],
      polygon: {
        latlngs: [
          [
            78.43695402145386,
            17.352440721296134
          ],
          [
            78.43751192092896,
            17.351478107288564
          ],
          [
            78.43815565109253,
            17.351887730888155
          ],
          [
            78.43862771987915,
            17.351437144878282
          ],
          [
            78.43873500823975,
            17.350556450843957
          ],
          [
            78.4407091140747,
            17.348180603982684
          ],
          [
            78.44276905059814,
            17.346869778746957
          ],
          [
            78.44538688659668,
            17.346542070974312
          ],
          [
            78.446524143219,
            17.3457330399038
          ],
          [
            78.44825148582458,
            17.345651112507827
          ],
          [
            78.4507405757904,
            17.345333643502823
          ],
          [
            78.45109462738037,
            17.346296289760616
          ],
          [
            78.45216751098633,
            17.35063837604883
          ],
          [
            78.45388412475586,
            17.355144205949756
          ],
          [
            78.45012903213501,
            17.35522612910484
          ],
          [
            78.45094442367552,
            17.35700794867062
          ],
          [
            78.44924926757812,
            17.357499482054763
          ],
          [
            78.44882011413574,
            17.355717667266557
          ],
          [
            78.44806909561157,
            17.35590199373743
          ],
          [
            78.44463586807251,
            17.35489843626487
          ],
          [
            78.44382047653198,
            17.352973229001694
          ],
          [
            78.44311237335205,
            17.35442737678127
          ],
          [
            78.44259738922119,
            17.354550261980627
          ],
          [
            78.44161033630371,
            17.354140644329117
          ],
          [
            78.44064474105835,
            17.354529781119787
          ],
          [
            78.44100952148438,
            17.355164686741958
          ],
          [
            78.43899250030518,
            17.35590199373743
          ],
          [
            78.43817710876465,
            17.353669582898544
          ],
          [
            78.43695402145386,
            17.352440721296134
          ]
        ],
        color: "green",
      },
      geojson: {},
    };
  },
  methods: {
    onFileChange(event) {
      this.isData = false;
      var file = event.target.files[0];
      var reader = new FileReader();

      reader.onload = (e) => {
        var content = reader.result;
        let mapData1 = JSON.parse(content);
        let mapData2 = JSON.parse(content);
        this.geojson = mapData2;
        this.polygon.latlngs = mapData1.features[0].geometry.coordinates[0];
        this.center.latlngs = mapData1.features[0].geojson.coordinates[0][0]
       
      };

      reader.readAsText(file);
    },
    submit() {
      this.isData = true;
    },
    getData(data) {
      let rawData = data;

      if (isProxy(data)) {
        rawData = toRaw(data);
      }
      return rawData;
    },
    getCenter(data) {
      let rawData = data;

      if (isProxy(data)) {
        rawData = toRaw(data);
      }
      return rawData;
    },
  },
};
</script>

<style scoped>
.container{
  display: flex;
  justify-content: space-between;
  margin: auto;
}
.uploader{
  box-shadow: rgba(100, 100, 111, 0.2) 0px 7px 29px 0px;
  margin: auto;
  background-color: white;
  border-radius: 2px;
}
.map{
  height: 90vh; 
  width: 80%;
}
.btn{
  padding :5px;
  margin: 10px 0px;
}
</style>
