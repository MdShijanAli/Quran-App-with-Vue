<template>
  <div class="max-w-7xl mx-auto py-10">
    <h1 class="text-5xl font-bold mx-auto text-center">{{ msg }}</h1>

   <div class="bg-gray-300 p-20 mt-10">
    <div class=" grid grid-cols-3 items-center gap-10">
      <div>
        <select @change="suraChanage" class="select select-info w-full">
          <option disabled selected>Select Surah</option>
          <option v-for="sura in suras" :key="sura?.number" :value="sura?.number">{{ sura?.number }}. {{ sura?.englishName }} - {{ sura?.name }}</option>
        </select>
      </div>
      <div class=" text-center">
        <h1 class="text-3xl font-semibold">{{ currentSurah.name }} {{ currentSurah?.englishName }}</h1>
        <p class="text-xl font-semibold mt-3">Number of Ayat: {{ currentSurah.numberOfAyahs }}</p>
      </div>
      <div>
        <select class="select select-info w-full">
          <option disabled selected>Select Surah</option>
          <option v-for="sura in suras" :key="sura?.number" :value="sura?.number">{{ sura?.number }}. {{ sura?.englishName }} - {{ sura?.name }}</option>
        </select>
      </div>
    </div>

    <div v-if="loading" class="flex items-center justify-center">
      <span class="loading loading-infinity loading-lg"></span>
    </div>

    <div v-else class="mt-10">
      <p class="py-5 flex items-center gap-5" v-for="ayah in currentSurah.ayahs" ><span class="w-10 h-10 border border-black rounded-full flex items-center justify-center">{{ ayah?.numberInSurah }}</span> <span class="text-2xl font-semibold">{{ ayah?.text }}</span></p>
    </div>
   </div>
  </div>
</template>
<script>
import axios from "axios";
// console.log(this.currentSurah)
export default {
  name: "App",

  data() {
    return {
      msg: "Al Quran",
      suras: [],
      currentSurah: [],
      loading: true,
    };
  },

  mounted() {
    axios
      .get("https://api.alquran.cloud/v1/surah")
      .then(res => {
        this.suras = res.data.data
      });
      this.querySpecificSura(1)
  },

  methods: {
    suraChanage(e) {
      // console.log(e.target.value)
      this.querySpecificSura(e.target.value)
    },
    querySpecificSura(number) {
      axios
      .get("https://api.alquran.cloud/v1/surah/" + number)
        .then(res => {
        this.loading = true
        // console.log(res.data.data.englishName)
        this.currentSurah = res.data.data
        this.loading = false
      });
    }
  }
};
</script>
<style lang=""></style>
