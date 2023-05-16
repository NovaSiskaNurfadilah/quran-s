<script>
import { RouterView } from "vue-router";
import { ref } from "vue";
import axios from "axios";

export default {
  data() {
    return {
      error: false,
      loading: true,
      juzs: ref([]), //ref untuk simpan data
      surahs: ref([]),
    };
  },

  components: {
    RouterView,
  },

  mounted() { //memanggil method
    this.getJuz();
    this.getSurah();
  },

  methods: {
    getJuz() {
      axios //librari dari luarnya
          .get("https://api.quran.com/api/v4/juzs")
          .then((response) => {
            this.juzs = response.data.juzs;
          })
          .catch((error) => {
            console.log(error);
            this.error = true;
          })
          .finally(() => (this.loading = false));
    },
    getSurah() {
      axios
          .get("https://api.quran.com/api/v4/chapters")
          .then((response) => {
            this.surahs = response.data.chapters;
          })
          .catch((error) => {
            console.log(error);
            this.error = true;
          })
          .finally(() => (this.loading = false));
    },
  },
};
</script>
<style>
.container {
  margin-bottom: 100px;
}
.y {
  font-family: Joyce;
  display: inline-block;
  margin-block: 2px;
  text-decoration: none;
  color: #8ccde1;
  position: relative;
}
.y::after {
  content: "";
  width: 100%;
  height: 2px;
  background-color: #a99b8e;
  border-radius: 4px;
  position: absolute;
  left: 0;
  bottom: 0;
  transform: scaleX(0);
  transform-origin: left;
  transition: transform 0.25s ease;
}
.y:hover::after {
  transform: scaleX(1);
}
</style>

<template>
  <nav class="navbar navbar-expand-lg bg-body-tertiary">

    <div class="container-fluid ">
      <a class="navbar-brand" href="#">Al-Qur'anku</a>
      <button class="navbar-toggler" type="button" data-bs-toggle="collapse" data-bs-target="#navbarSupportedContent" aria-controls="navbarSupportedContent" aria-expanded="false" aria-label="Toggle navigation">
        <span class="navbar-toggler-icon"></span>
      </button>
      <div class="collapse navbar-collapse " id="navbarSupportedContent ">
        <ul class="navbar-nav me-auto mb-2 mb-lg-0 ">
          <li class="nav-item text-white">
            <a class="nav-link active" aria-current="page" href="#"><router-link to="/" style="color: black;">Home</router-link></a>
          </li>
          <li class="nav-item">
            <a class="nav-link active" aria-current="page" href="#"><router-link :to="{ name: 'search' }" style="color: black;" >Search</router-link></a>
          </li>
          <li class="nav-item dropdown">
            <a class="nav-link dropdown-toggle" href="#" role="button" data-bs-toggle="dropdown" aria-expanded="false">
              Juz</a>
            <ul class="dropdown-menu">
              <li v-for="juz in juzs" :key="juz.id">
                <router-link :to="{ name: 'juzs', params: { id: juz.id } }" class="dropdown-item">{{ juz.juz_number }}
                </router-link>
              </li>
            </ul>
          </li>
          <li class="nav-item dropdown">
            <a class="nav-link dropdown-toggle" data-bs-toggle="dropdown" href="#" role="button"
               aria-expanded="false">Surah</a>
            <ul class="dropdown-menu">
              <li v-for="surah in surahs" :key="surah.id">
                <router-link :to="{ name: 'surahs', params: { id: surah.id } }" class="dropdown-item">{{
                    surah.name_complex
                  }}</router-link>
              </li>
            </ul>
          </li>
          <li class="nav-item">
            <a class="nav-link active" aria-current="page" href="#"><router-link to="/about" style="color: black;">About Us</router-link></a>
          </li>
        </ul>
      </div>
    </div>
  </nav>
  <RouterView />
</template>
<style>

</style>