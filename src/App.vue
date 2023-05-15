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
  <section v-if="error">
    <div class="container">
      <h1 class="text-center">KODINGAN ERROR!</h1>
    </div>
  </section>

  <section v-else>
    <div v-if="loading" class="container">
      <h1 class="text-center">Loading...</h1>
    </div>

    <div v-else class="container">
      <ul class="nav nav-pills justify-content-center">
        <li class="nav-item">
          <router-link
              :to="{ name: 'search' }"
              class="y nav-link"
              style="color: black"
          >Search</router-link
          >
        </li>
        <li class="nav-item dropdown">
          <a
              class="y nav-link dropdown-toggle"
              data-bs-toggle="dropdown"
              href="#"
              role="button"
              aria-expanded="false"
              style="color: black"
          >Juz</a
          >
          <ul class="dropdown-menu">
            <li v-for="juz in juzs" :key="juz.id">
              <router-link
                  :to="{ name: 'juzs', params: { id: juz.id } }"
                  class="dropdown-item"
              >{{ juz.juz_number }}
              </router-link>
            </li>
          </ul>
        </li>
        <li class="nav-item dropdown">
          <a
              class="y nav-link dropdown-toggle"
              data-bs-toggle="dropdown"
              href="#"
              role="button"
              aria-expanded="false"
              style="color: black"
          >Surah</a
          >
          <ul class="dropdown-menu">
            <li v-for="surah in surahs" :key="surah.id">
              <router-link
                  :to="{ name: 'surahs', params: { id: surah.id } }"
                  class="dropdown-item"
                  style="color: black"
              >{{ surah.name_complex }}</router-link
              >
            </li>
          </ul>
        </li>
        <li class="nav-item">
          <router-link
              :to="{ name: 'random' }"
              class="y nav-link"
              style="color: black"
          >Random</router-link
          >
        </li>
      </ul>
      <RouterView />
    </div>
  </section>

</template>
