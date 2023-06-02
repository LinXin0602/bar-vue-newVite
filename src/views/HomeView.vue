<template>
  <div ref="mapRef" style="height: 91vh" class="mt-5"></div>
  <nav class="navbar navbar-light bg-primary fixed-top">
    <div class="container-fluid">
      <h1>從捷運開始跑吧</h1>
      <button
        class="navbar-toggler"
        type="button"
        data-bs-toggle="offcanvas"
        data-bs-target="#offcanvasNavbar"
        aria-controls="offcanvasNavbar"
      >
        <span class="navbar-toggler-icon"></span>
      </button>
      <div
        class="offcanvas offcanvas-end"
        tabindex="-1"
        id="offcanvasNavbar"
        aria-labelledby="offcanvasNavbarLabel"
      >
        <div class="offcanvas-header">
          <h5 class="offcanvas-title" id="offcanvasNavbarLabel">
            從捷運開始跑吧
          </h5>
          <h2 v-if="stationSelect">
            {{ `${stationSelect[0]}捷運站 ` }}
          </h2>
          <button
            type="button"
            class="btn-close text-reset"
            data-bs-dismiss="offcanvas"
            aria-label="Close"
          ></button>
        </div>
        <div class="offcanvas-body">
          <div class="d-flex w-100 mb-4">
            <select v-model="station" class="form-select" name="" id="">
              <option value="文湖線(BR)">文湖線(BR)</option>
              <option value="淡水信義線(R)">淡水信義線(R)</option>
              <option value="松山新店線(G)">松山新店線(G)</option>
              <option value="中和新蘆線(O)">中和新蘆線(O)</option>
              <option value="板南線(BL)">板南線(BL)</option>
              <option value="環狀線(Y)">環狀線(Y)</option>
            </select>

            <select
              class="form-select"
              v-if="station === '文湖線(BR)'"
              v-model="stationSelect"
              name="BL_station"
            >
              <option
                name="BL_station"
                v-for="BL_itme in BL_station"
                :key="BL_itme.StationID"
                :value="[
                  BL_itme.StationName.Zh_tw,
                  BL_itme.StationAddress,
                  BL_itme.StationPosition.PositionLat,
                  BL_itme.StationPosition.PositionLon,
                ]"
              >
                {{ BL_itme.StationName.Zh_tw }}
              </option>
            </select>
            <select
              class="form-select"
              v-else-if="station === '淡水信義線(R)'"
              v-model="stationSelect"
              name="BR_station"
            >
              <option
                name="BR_station"
                v-for="BR_itme in BR_station"
                :key="BR_itme.StationID"
                :value="[
                  BR_itme.StationName.Zh_tw,
                  BR_itme.StationAddress,
                  BR_itme.StationPosition.PositionLat,
                  BR_itme.StationPosition.PositionLon,
                ]"
              >
                {{ BR_itme.StationName.Zh_tw }}
              </option>
            </select>
            <select
              class="form-select"
              v-else-if="station === '松山新店線(G)'"
              v-model="stationSelect"
              name="G_station"
            >
              <option
                name="G_station"
                v-for="G_itme in G_station"
                :key="G_itme.StationID"
                :value="[
                  G_itme.StationName.Zh_tw,
                  G_itme.StationAddress,
                  G_itme.StationPosition.PositionLat,
                  G_itme.StationPosition.PositionLon,
                ]"
              >
                {{ G_itme.StationName.Zh_tw }}
              </option>
            </select>
            <select
              class="form-select"
              v-else-if="station === '中和新蘆線(O)'"
              v-model="stationSelect"
              name="O_station"
            >
              <option
                name="O_station"
                v-for="O_itme in O_station"
                :key="O_itme.StationID"
                :value="[
                  O_itme.StationName.Zh_tw,
                  O_itme.StationAddress,
                  O_itme.StationPosition.PositionLat,
                  O_itme.StationPosition.PositionLon,
                ]"
              >
                {{ O_itme.StationName.Zh_tw }}
              </option>
            </select>
            <select
              class="form-select"
              v-else-if="station === '板南線(BL)'"
              v-model="stationSelect"
              name="R_station"
            >
              <option
                name="R_station"
                v-for="R_itme in R_station"
                :key="R_itme.StationID"
                :value="[
                  R_itme.StationName.Zh_tw,
                  R_itme.StationAddress,
                  R_itme.StationPosition.PositionLat,
                  R_itme.StationPosition.PositionLon,
                ]"
              >
                {{ R_itme.StationName.Zh_tw }}
              </option>
            </select>
            <select
              class="form-select"
              v-else-if="station === '環狀線(Y)'"
              v-model="stationSelect"
              name="Y_station"
            >
              <option
                name="Y_station"
                v-for="Y_itme in Y_station"
                :key="Y_itme.StationID"
                :value="[
                  Y_itme.StationName.Zh_tw,
                  Y_itme.StationAddress,
                  Y_itme.StationPosition.PositionLat,
                  Y_itme.StationPosition.PositionLon,
                ]"
              >
                {{ Y_itme.StationName.Zh_tw }}
              </option>
            </select>
            <select v-else class="form-select">
              <option value="請先選擇捷運線路">請先選擇捷運線路</option>
            </select>
          </div>
          <div v-if="dataResult.length > 0">
            <div class="d-flex align-items-center justify-content-between mb-3">
              <button @click="distanceSort" class="btn btn-info">
                距離排序
              </button>
              <button @click="ratingSort" class="btn btn-info">評分排序</button>
              <button @click="ratingsTotalSort" class="btn btn-info">
                評論數排序
              </button>
            </div>
            <table class="table table-hover" v-if="barList.length !== 0">
              <thead>
                <tr>
                  <th scope="col"></th>
                  <th scope="col" class="w-25">店名</th>
                  <th scope="col">評分</th>
                  <th scope="col">評論數</th>
                  <th scope="col">距離</th>
                  <th scope="col">步行時間</th>
                </tr>
              </thead>
              <tbody>
                <tr v-for="(item, index) in dataResult" :key="item.place_id">
                  <th scope="row">{{ index + 1 }}</th>
                  <td>{{ item.name }}</td>
                  <td>{{ item.rating }}</td>
                  <td>{{ item.user_ratings_total }}</td>
                  <td>{{ item.location.distance.text }}</td>
                  <td>約{{ item.location.duration.text }}</td>
                </tr>
              </tbody>
            </table>
          </div>
        </div>
      </div>
    </div>
  </nav>
</template>

<script setup>
// eslint-disable-next-line no-unused-vars
import { offcanvas } from 'bootstrap/js/dist/offcanvas';
import axios from 'axios';
import { ref, watch, reactive } from 'vue';
import { Loader } from '@googlemaps/js-api-loader';
const stationList = ref([]); //api接資料
const BL_station = ref([]);
const BR_station = ref([]);
const G_station = ref([]);
const O_station = ref([]);
const R_station = ref([]);
const Y_station = ref([]);
const stationSelect = ref('');
const station = ref('');
const getStation = () => {
  const api = `https://tdx.transportdata.tw/api/basic/v2/Rail/Metro/Station/TRTC?%24top=200&%24format=JSON
`;
  axios.get(api).then((res) => {
    stationList.value = res.data;

    stationList.value.forEach((item) => {
      if (item.StationID.includes('BL')) {
        BL_station.value.push(item);
      } else if (item.StationID.includes('BR')) {
        BR_station.value.push(item);
      } else if (item.StationID.includes('G')) {
        G_station.value.push(item);
      } else if (item.StationID.includes('O')) {
        O_station.value.push(item);
      } else if (item.StationID.includes('R')) {
        R_station.value.push(item);
      } else if (item.StationID.includes('Y')) {
        Y_station.value.push(item);
      }
    });
  });
};
getStation();

//獲取地圖資料,apikey等,界接api
const mapRef = ref(null);
const apiKey = 'AIzaSyBPM5EIbPP8qu7O1sp1EpHVNMKZoisvZQ4';
const loader = new Loader({
  apiKey,
  version: 'weekly',
  libraries: ['places'],
});
const mapOptions = reactive({
  center: {
    lat: 25.0307431,
    lng: 121.5611969,
  },
  zoom: 16,
});
const getMap = () => {
  loader.load().then((google) => {
    new google.maps.Map(mapRef.value, mapOptions);
  });
};
getMap();

const barList = ref([]); //20間酒吧初始資料
const barLocation = ref([]); //酒吧經緯度資訊
const barDistance = ref([]); //酒吧與目標地距離資訊
const dataResult = ref([]); //20間酒吧資料總和
const getShopList = () => {
  loader
    .load()
    .then((google) => {
      let map = new google.maps.Map(mapRef.value, mapOptions);
      let location = mapOptions.center;
      //Places搜尋資料
      let request = {
        location,
        radius: '300',
        query: '酒吧',
      };
      let Places = new google.maps.places.PlacesService(map);
      Places.textSearch(request, (res, status) => {
        barList.value = res;
        if (status === google.maps.places.PlacesServiceStatus.OK) {
          const promises = barList.value.map(async (item) => {
            let api = `https://maps.googleapis.com/maps/api/geocode/json?place_id=${item.place_id}
&key=${apiKey}`;
            const res = await axios.get(api);
            return res.data.results[0].geometry.location;
          });
          const DistanceMatrix = new google.maps.DistanceMatrixService();
          Promise.all(promises).then((locations) => {
            barLocation.value = locations;
            const DistanceRequest = {
              origins: [location],
              destinations: barLocation.value,
              travelMode: 'WALKING',
            };
            DistanceMatrix.getDistanceMatrix(DistanceRequest).then((res) => {
              barDistance.value = res.rows[0].elements;
              dataResult.value = barList.value.map((item, index) => ({
                ...item,
                location: barDistance.value[index],
                latLng: barLocation.value[index],
              }));
              console.log(dataResult.value);
              //製作地圖標記
              const infoWindow = new google.maps.InfoWindow();
              dataResult.value.forEach((item) => {
                const marker = new google.maps.Marker({
                  position: item.latLng,
                  map,
                  title: `${item.name}`,
                  optimized: false,
                });
                marker.addListener('click', () => {
                  infoWindow.close();
                  infoWindow.setContent(marker.getTitle());
                  infoWindow.open(marker.getMap(), marker);
                });
              });
            });
          });
        }
      });
    })
    .catch((e) => {
      console.log(e); // do something
    });
};

watch(stationSelect, () => {
  mapOptions.center.lat = stationSelect.value[2];
  mapOptions.center.lng = stationSelect.value[3];
  getShopList();
});
//距離sort
const distanceSort = () => {
  dataResult.value.sort((a, b) => {
    const distanceA = parseFloat(a.location.distance.text);
    const distanceB = parseFloat(b.location.distance.text);
    return distanceA - distanceB;
  });
};
//評分sort
const ratingSort = () => {
  dataResult.value.sort((a, b) => {
    const ratingA = a.rating;
    const ratingB = b.rating;
    return ratingB - ratingA;
  });
};
//評論數sort
const ratingsTotalSort = () => {
  dataResult.value.sort((a, b) => {
    const ratingA = a.user_ratings_total;
    const ratingB = b.user_ratings_total;
    return ratingB - ratingA;
  });
};
</script>
<style></style>
