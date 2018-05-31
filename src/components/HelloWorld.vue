<template>
  <div>
    <div>
      <!-- <label>
        <gmap-autocomplete
          @place_changed="setPlace">
        </gmap-autocomplete>
        <button @click="addMarker">Add</button>
      </label> -->
      <br/>

    </div>
    <div class="search-panel">
      <label > Origin:</label>
      <gmap-autocomplete @place_changed="setPlace" placeholder="Enter an origin location" > </gmap-autocomplete>
<br>
<label >Destination:</label>
      <gmap-autocomplete @place_changed="setPlace" placeholder="Enter a destination location" > </gmap-autocomplete>

    <div id="mode-selector" class="controls">
      <input type="radio" name="type" id="changemode-walking" checked="checked">
      <label for="changemode-walking">Walking</label>

      <input type="radio" name="type" id="changemode-transit">
      <label for="changemode-transit">Transit</label>

      <input type="radio" name="type" id="changemode-driving">
      <label for="changemode-driving">Driving</label>
    </div>

    <div id="map"></div>
    </div>
    <br>
    <gmap-map
      :center="center"
      :zoom="12"
      style="width:100%;  height: 600px;" id="map-canvas"
    >
      <gmap-marker
        :key="index"
        v-for="(m, index) in markers"
        :position="m.position"
        @click="center=m.position"
      ></gmap-marker>
    </gmap-map>
  </div>
</template>

<script>
export default {
  name: "GoogleMap",
  data() {
     
    return {
      // default to montreal to keep it simple
      // change this to whatever makes sense
      center: { lat: 45.508, lng: -73.587 },
      markers: [],
      places: [],
      currentPlace: null,
    };
   
  },

  mounted() {
    console.log("jgdch");
    this.initMap();
    // console.log(document.getElementById('start').id);
    
   
  },

  methods: {
    initMap : function() {
    var pointA = new google.maps.LatLng(42.508,-73.587),
        pointB = new google.maps.LatLng(44.508,-73.587),
        
        myOptions = {
            zoom: 7,
            center: pointA,
        },
        map = new google.maps.Map(document.getElementById('map-canvas'), myOptions),
        // Instantiate a directions service.
        directionsService = new google.maps.DirectionsService,
        directionsDisplay = new google.maps.DirectionsRenderer({
            map: map
        }),
        markerA = new google.maps.Marker({
            position: pointA,
            title: "point A",
            label: "A",
            map: map
        }),
        markerB = new google.maps.Marker({
            position: pointB,
            title: "point B",
            label: "B",
            map: map
        });

    // get route from A to B
    this.calculateAndDisplayRoute(directionsService, directionsDisplay, pointA, pointB);

    },

    calculateAndDisplayRoute : function( directionsService, directionsDisplay, pointA, pointB) {
      directionsService.route({
        origin: pointA,
        destination: pointB,
        avoidTolls: true,
        avoidHighways: false,
        travelMode: google.maps.TravelMode.DRIVING,
    }, 
    function (response, status) {
        if (status === google.maps.DirectionsStatus.OK) {
          directionsDisplay.setDirections(response);
        } else {
        window.alert("Directions request failed due to " + status);
        }
      });
    },
  },
};
</script>
<style scoped>

</style>
