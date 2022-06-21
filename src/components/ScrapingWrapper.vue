<template>
  <div id="scrapping-wrapper">
    <ScrapingElement website-name="Indeed" :status="indeedStatus"></ScrapingElement>
    <ScrapingElement website-name="Hello Work" :status="helloWorkStatus"></ScrapingElement>
    <ScrapingElement website-name="LinkedIn" :status="linkedInStatus"></ScrapingElement>
    <ScrapingElement website-name="Pôle Emploi" :status="poleEmploiStatus"></ScrapingElement>
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
      scrapingStatus: 'pending',
      indeedStatus: 'pending',
      helloWorkStatus: 'pending',
      linkedInStatus: 'pending',
      poleEmploiStatus: 'pending',
      publicPath: process.env.BASE_URL
    }
  },
  methods: {
    async callApi() {
      this.scrapingStatus = 'running';
      this.indeedStatus = 'running';
      this.helloWorkStatus = 'running';
      this.linkedInStatus = 'running';
      this.poleEmploiStatus = 'running';
      let response = await fetch("https://ow.services.eemi.tech/api/v1/web/nils.vaede/default/scraping.json");
      if (response.ok) {
        const json = await response.json();
        // eslint-disable-next-line no-prototype-builtins
        if (json.helloWork.hasOwnProperty("error")) {
          this.helloWorkStatus = 'error'
        } else {
          this.helloWorkStatus = 'success'
        }
        // eslint-disable-next-line no-prototype-builtins
        if (json.indeed.hasOwnProperty("error")) {
          this.linkedInStatus = 'error'
        } else {
          this.indeedStatus = 'success'
        }
        // eslint-disable-next-line no-prototype-builtins
        if (json.linkedIn.hasOwnProperty("error")) {
          this.linkedInStatus = 'error'
        } else {
          this.linkedInStatus = 'success'
        }
        // eslint-disable-next-line no-prototype-builtins
        if (json.poleEmploi.hasOwnProperty("error")) {
          this.poleEmploiStatus = 'error'
        } else {
          this.poleEmploiStatus = 'success'
        }
        this.scrapingStatus = 'pending';
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