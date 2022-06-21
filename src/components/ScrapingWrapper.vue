<template>
  <div id="scrapping-wrapper">
    <ScrapingElement website-name="Indeed" :status="scrapingStatus"></ScrapingElement>
    <ScrapingElement website-name="Hello Work" :status="scrapingStatus"></ScrapingElement>
    <ScrapingElement website-name="LinkedIn" :status="scrapingStatus"></ScrapingElement>
    <ScrapingElement website-name="Pôle Emploi" :status="scrapingStatus"></ScrapingElement>
    <button id="run-scraping-test" @click="callApi" v-if="scrapingStatus !== 'running'">Run scraping test</button>
    <img id="loading-img" :src="`${publicPath}loading.gif`" v-else>
  </div>
</template>

<script>
import ScrapingElement from "@/components/ScrapingElement";

export default {
  name: "ScrappingWrapper",
  components: {ScrapingElement},
  data() {
    return {
      scrapingStatus: "pending",
      indeedData: null,
      helloWorkData: null,
      linkedInData: null,
      poleEmploiData: null,
      publicPath: process.env.BASE_URL
    }
  },
  methods: {
    async callApi() {
      this.scrapingStatus = 'running';
      let response = await fetch("https://ow.services.eemi.tech/api/v1/web/nils.vaede/default/scraping.json");
      if (response.ok) {
        const json = await response.json();
        this.scrapingStatus = "success";
        this.indeedData = json.indeed;
      } else {
        alert("HTTP-Error: " + response.status);
      }
    }
  }
}
</script>

<style scoped>
#scrapping-wrapper {
  width: 80vw;
  margin: auto;
  display: flex;
  flex-wrap: wrap;
}

#run-scraping-test {
  width: 30%;
  padding: 20px;
  margin: 50px auto;
  background: none;
  border: 1px solid #b8b8b8;
  border-radius: 7px;
  cursor: pointer;
}

#run-scraping-test:hover {
  background: #b8b8b8;
  color: #fff;
}

#loading-img {
  margin: auto;
}

</style>