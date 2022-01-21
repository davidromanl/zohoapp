<template>
  <v-app>
    <v-container fluid>
      <v-row>
        <v-col>
          <h2>Logo Datos</h2>
        </v-col>
        <v-col>
          <v-btn
            href="https://accounts.zoho.com/oauth/v2/auth?response_type=token&client_id=1000.92J0MQUOZOG3A90SD80OAT0RVUS4UN&scope=ZohoCreator.report.READ&redirect_uri=https://zohoapp.vercel.app"
            color="success"
            >Login</v-btn
          >
        </v-col>
      </v-row>
    </v-container>
    <v-divider></v-divider>
    <v-container>
      <v-tabs v-model="tab" color="secondary" slider-color="primary">
        <v-tab to="/" ripple> Inicio </v-tab>
        <v-tab to="/about" ripple> About </v-tab>
      </v-tabs>
      <v-divider class="mb-5"></v-divider>
      <div>{{ token }}</div>
      <div>{{ datos }}</div>
      <v-btn @click="getDatos">Traer Datos</v-btn>
      <router-view />
    </v-container>
  </v-app>
</template>

<style lang="scss"></style>

<script>
import axios from "axios";

axios.defaults.baseURL = "https://creator.zoho.com/api/v2/vt.cel/colombia/report";

axios.defaults.headers["content-type"] = "application/json";

export default {
  name: "App",

  data: () => ({
    tab: null,
    token: "",
    datos: [],
  }),

  computed: {},

  created() {
    const urlParams = new URLSearchParams(this.$route.path);
    if (urlParams.has("access_token"))
      axios.defaults.headers.common.authorization = `Bearer ` + urlParams.get("access_token");
    this.token = urlParams.get("access_token");
  },

  methods: {
    async getDatos() {
      const { data } = await axios.get("/Municipio_Leads_Report");
      this.datos = data;
    },
  },
};
</script>
