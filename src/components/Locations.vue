<template>
  <div>
    <div id="grid-container">
      <div id="intro">
        <h4>Calculated with the center of the pointers.</h4>
        <article>
          Drop a pin: Right click on the map, and enter a description when
          prompted
        </article>
      </div>
      <div id="buttons" class="coord-input">
        <input
          id="button"
          v-on:click="summarizeMarkers"
          type="button"
          value="Get All Markers and Refresh"
        />
        <input
          id="button"
          v-on:click="updateMarker"
          type="button"
          value="Update Marker"
        />
        <input
          id="button"
          v-on:click="removeMarker"
          type="button"
          value="Remove Marker"
        />
      </div>

      <div id="report"></div>
    </div>
  </div>
</template>

<script>
export default {
  name: "map-location",
  data() {
    return {
      map: null,
      mapCenter: { lat: 41.027836, lng: 28.973679 },
      locations: [
        { coord: { lat: 41.027836, lng: 28.973679 }, name: "İstanbul" },
        { coord: { lat: 38.42227, lng: 27.13058 }, name: "İzmir" },
        { coord: { lat: 39.933365, lng: 32.859741 }, name: "Ankara" },
      ],
    };
  },
  methods: {
    initMap() {
      this.calculateCenter();
      this.map = new window.google.maps.Map(document.getElementById("map"), {
        center: this.mapCenter,
        zoom: 14,
        maxZoom: 20,
        minZoom: 3,
        streetViewControl: true,
        mapTypeControl: false,
        fullscreenControl: true,
        zoomControl: true,
      });
      //adding new location mark with right click
      let noPOIStyle = [
        {
          featureType: "poi",
          elementType: "labels",
          stylers: [{ visibility: "off" }],
        },
      ];
      this.map.setOptions({ styles: noPOIStyle });
      window.google.maps.event.addListener(this.map, "rightclick", (event) => {
        this.addPinViaClick(event);
      });
    },
    calculateCenter() {
      let latTotal = 0,
        lngTotal = 0;
      for (let i = 0; i < this.locations.length; i++) {
        latTotal += parseFloat(this.locations[i].coord.lat);
        lngTotal += parseFloat(this.locations[i].coord.lng);
      }
      const lat = latTotal / this.locations.length;
      const lng = lngTotal / this.locations.length;
      this.mapCenter = { lat: lat, lng: lng };
    },
    makeMarkerObj(latLng, name) {
      const markerObj = { coord: latLng, name: name };
      return markerObj;
    },
    addPinViaClick(event) {
      let description = window.prompt("Enter a description");
      const markerObj = this.makeMarkerObj(event.latLng.toJSON(), description);
      this.locations.push(markerObj);
      this.dropPin(markerObj);
    },
    dropPins() {
      this.locations.forEach((x) => this.dropPin(x));
    },
    dropPin(markerObj) {
      new window.google.maps.Marker({
        position: markerObj.coord,
        map: this.map,
        label: {
          text: markerObj.name,
          color: "blue",
        },
      });
    },
    summarizeMarkers() {
      const dataSection = document.getElementById("report");
      let text = "<ol>";
      this.locations.forEach(
        (x) =>
          (text =
            text +
            `<li>Latitude: ${x.coord.lat} Longitude: ${x.coord.lng} Description: ${x.name}` +
            "</li>")
      );
      text += "</ol>";
      dataSection.innerHTML = text;
      localStorage.setItem(
        "locations",
        JSON.stringify(
          this.locations.map((location) => ({
            name: location.name,
            loc: location.coord,
          }))
        )
      );
    },
    updateMarker() {
      const index = parseInt(window.prompt("Enter index of marker to update"));
      if (index >= 0 && index < this.locations.length) {
        const dataSection = document.getElementById("report");
        const latValue = parseFloat(window.prompt("Enter new Latitude Value:"));
        const lngValue = parseFloat(
          window.prompt("Enter new Longitude Value:")
        );
        const description = window.prompt("Enter new description");
        this.locations[index].name = description;
        this.locations[index].coord.lat = latValue;
        this.locations[index].coord.lng = lngValue;

        dataSection.innerHTML = "Marker updated";
      } else {
        alert("Invalid index");
      }
    },
    removeMarker() {
      const index = parseInt(window.prompt("Enter index of marker to remove"));
      if (index >= 0 && index < this.locations.length) {
        this.locations.splice(index, 1);
        const dataSection = document.getElementById("report");
        dataSection.innerHTML = "Marker removed";
      } else {
        alert("Invalid index");
      }
    },
  },
  mounted() {
    this.initMap();
    this.dropPins();
  },
};
</script>

<style>
#grid-container {
  position: fixed;
  top: 0;
  left: 0;
  width: 300px;
  height: 100%;
  background-color: #f8f9fa;
  border-right: 1px groove #dee2e6;
  border-top-right-radius: 60px;
  border-bottom-right-radius:60px ;
  padding: 10px;
  overflow-y: auto;
}
#intro {
  font-family:system-ui, -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, 'Open Sans', 'Helvetica Neue', sans-serif;
}
#button{
  box-shadow:inset 0 -3em 3em rgba(0, 0, 0, 0.1),
    0 0 0 2px rgb(255, 255, 255),
    0.3em 0.3em 1em rgba(0, 0, 0, 0.3);
  border-radius: 5px;
  padding: 5px;
  margin: 5px;
  font-weight: 500;
  font-family:system-ui, -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, 'Open Sans', 'Helvetica Neue', sans-serif;
}

#report {
  width: 300px;
  height: 200px;
  border: 2px ridge black;
  border-radius: 20px;
  font-family:system-ui, -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, 'Open Sans', 'Helvetica Neue', sans-serif;
}

.coord-input {
  border: 2px ridge black;
  border-radius: 20px;
  padding: 25px;
  margin: 25px;
}
.input-cell {
  vertical-align: top;
}
</style>
