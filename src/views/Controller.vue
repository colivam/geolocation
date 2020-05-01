<template>
  <b-container>
    <b-row>
      <b-col>
        <h1>Controller View</h1>
        <b-button :disabled="isWorking" @click="iniciar">Iniciamos el control</b-button>
        <b-button :disabled="!isWorking" @click="stop">Detenemos el control</b-button>
        <pre>{{ $data }}</pre>
        <div>
          <b-badge pill variant="primary">Lat : {{ getPosicionInicial.latitude }}</b-badge>
          <b-badge pill variant="primary">Lng : {{ getPosicionInicial.longitude }}</b-badge>
        </div>
        <div>
          <b-badge pill variant="success">Lat : {{ getPosicionActual.latitude }}</b-badge>
          <b-badge pill variant="success">Lng : {{ getPosicionActual.longitude }}</b-badge>
        </div>
      </b-col>
    </b-row>
  </b-container>
</template>

<script>
export default {
  name: "Controller",
  data: () => ({
    posicionInicial: {},
    posicionActual: {},
    watchId: 0,
    cfg: {
      enableHighAccuracy: false,
      timeout: 5000,
      maximumAge: 0
    },
    isWorking: false
  }),
  mounted() {
    console.log("mounted");
  },
  created() {
    console.log("created");
  },
  computed: {
    getPosicionInicial() {
      return this.posicionInicial
        ? {
            latitude: this.posicionInicial.latitude,
            longitude: this.posicionInicial.longitude
          }
        : { latitude: 0, longitude: 0 };
    },
    getPosicionActual() {
      return this.posicionActual
        ? {
            latitude: this.posicionActual.latitude,
            longitude: this.posicionActual.longitude
          }
        : { latitude: 0, longitude: 0 };
    }
  },
  methods: {
    iniciar() {
      this.isWorking = true;
      const ko = err => {
        console.warn("ERROR(" + err.code + "): " + err.message);
      };
      navigator.geolocation.getCurrentPosition(
        pos => {
          this.posicionInicial = pos.coords;
          console.log("coords", pos.coords);
          console.log("ltd,lng", pos.coords.latitude, pos.coords.longitude);
        },
        ko,
        this.cfg
      );

      this.watchId = navigator.geolocation.watchPosition(
        pos => {
          this.posicionActual = pos.coords;
          console.log("coords", pos.coords);
          console.log("ltd,lng", pos.coords.latitude, pos.coords.longitude);
        },
        ko,
        this.cfg
      );
    },
    stop() {
      navigator.geolocation.clearWatch(this.watchId);
      this.posicionInicial = {};
      this.posicionActual = {};
      this.isWorking = false;
    }
  }
};
</script>

<style>
</style>