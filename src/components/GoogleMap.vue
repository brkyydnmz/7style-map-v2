<template>
  <div>
    <h1 style="text-align: center">Google Maps Rotation</h1>

    <nav class="nav">
      <select  id="select" v-model="fromLocation">
        <option value="">From</option>
        <option
          v-for="(eachLocation, index) in locations"
          v-bind:key="index"
          v-bind:value="eachLocation.loc"
        >
          {{ eachLocation.name }}
        </option>
      </select>

      <select  id="select" v-model="toLocation">
        <option value="">To</option>
        <option
          v-for="(eachLocation, index) in locations"
          v-bind:key="index"
          v-bind:value="eachLocation.loc"
        >
          {{ eachLocation.name }}
        </option>
      </select>

      <button id="button" v-on:click="calculateRoute(fromLocation, toLocation)">
        Get Route!
      </button>

      <button id="button" v-on:click="clearRoutes()">Clear!</button>
    </nav>

    <div id="map"></div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      map: null,
      directionsDisplay: null,
      service: null,
      locations: [
        { name: "İstanbul", loc: { lat: 41.027836, lng: 28.973679 } },
        { name: "İzmir", loc: { lat: 38.42227, lng: 27.13058 } },
        { name: "Ankara", loc: { lat: 39.933365, lng: 32.859741 } },
      ],
      fromLocation: "",
      toLocation: "",
    };
  },
  methods: {
    /*
     * Initialize map
     * https://developers.google.com/maps/documentation/javascript/overview
     */
    initMap() {
      const mapElement = document.getElementById("map");
      const mapOptions = {
        center: this.locations[0].loc, // This line changed { lat: 41.027836, lng: 28.973679 } like this.. I can't add a new marker with right click
        zoom: 13,
        mapTypeId: window.google.maps.MapTypeId.ROADMAP,
        panControl: true,
        zoomControl: true,
        mapTypeControl: false,
        scaleControl: true,
        streetViewControl: false,
        overviewMapControl: true,
        rotateControl: true,
      };

      this.map = new window.google.maps.Map(mapElement, mapOptions);
      this.addMarker();
    },
    addMarker() {
      const marker = new window.google.maps.Marker({
        position: this.locations[0].loc,
        animation: window.google.maps.Animation.DROP,
        draggable: true,
      });
      marker.setMap(this.map);
    },
     //https://stackoverflow.com/questions/27341214/how-to-draw-a-route-between-two-markers-in-google-maps
    calculateRoute(start, end) {
      const request = {
        origin: start,
        destination: end,
        // https://developers.google.com/maps/documentation/transportation-logistics/on-demand-rides-deliveries-solution/pickup-and-dropoff-selection/location-selection/reference/rest/v1beta/TravelMode
        travelMode: window.google.maps.TravelMode.TWO_WHEELER,
      };

      this.directionsDisplay.setMap(this.map);

      this.service.route(request, (response, status) => {
        if (status == window.google.maps.DirectionsStatus.OK) {
          this.directionsDisplay.setDirections(response);
          this.directionsDisplay.setMap(this.map);
        } else {
          alert(
            "Directions Request from " +
              start.toUrlValue(6) +
              " to " +
              end.toUrlValue(6) +
              " failed: " +
              status
          );
        }
      });
    },
    clearRoutes() {
      this.directionsDisplay.setMap(null);
    },
  },
  mounted() {
    // Initialize map after DOM is mounted
    this.initMap();
    this.directionsDisplay = new window.google.maps.DirectionsRenderer();
    this.service = new window.google.maps.DirectionsService();
  },
};
</script>

<style >
#map {
  width: 70%;
  height: 580px;
  padding: 20px;
  margin-left: 25%;
  border-radius: 10px;
  box-shadow:inset 0 -3em 3em rgba(0, 0, 0, 0.1),
    0 0 0 2px rgb(255, 255, 255),
    0.3em 0.3em 1em rgba(0, 0, 0, 0.3);
}

#select{
  background-color:#ffffff;
  box-shadow:inset 0 -3em 3em rgba(0, 0, 0, 0.1),
    0 0 0 2px rgb(255, 255, 255),
    0.3em 0.3em 1em rgba(0, 0, 0, 0.3);
  border-radius: 5px;
  padding: 5px;
  margin: 5px;
  font-weight: 400;
  font-family:system-ui, -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, 'Open Sans', 'Helvetica Neue', sans-serif;
}

#button{
  background-color:#ffffff;
  box-shadow:inset 0 -3em 3em rgba(0, 0, 0, 0.1),
    0 0 0 2px rgb(255, 255, 255),
    0.3em 0.3em 1em rgba(0, 0, 0, 0.3);
  border-radius: 5px;
  padding: 5px;
  margin: 5px;
  font-weight: 400;
  font-family:system-ui, -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, 'Open Sans', 'Helvetica Neue', sans-serif;
}
.nav {
  text-align: center;
  margin-bottom: 20px;
}
</style>