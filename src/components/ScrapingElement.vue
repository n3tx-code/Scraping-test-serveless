<template>
  <div class="scraping-element" :class="scrapingStatus" @click="doSpecificScraping">
    <div class="scraping-element-title">
      <h3>{{ websiteName }}</h3>
    </div>
    <div class="scraping-element-icon-wrapper">
      <i class="fa-solid fa-clock scraping-element-icon" v-if="scrapingStatus === 'pending'"></i>
      <i class="fa-solid fa-arrows-rotate scraping-element-icon" v-else-if="scrapingStatus === 'running'"></i>
      <i class="fa-solid fa-circle-check scraping-element-icon" v-else-if="scrapingStatus === 'success'"></i>
      <i class="fa-solid fa-circle-xmark scraping-element-icon" v-else-if="scrapingStatus === 'error'"></i>
    </div>
  </div>
</template>

<script>
export default {
  name: "ScrapingElement",
  props: {
    websiteName: String,
    status: {
      type: String,
      default: "pending"
    }
  },
  data() {
    return {
      scrapingStatus: this.status
    }
  },
  watch: {
    status(val) {
      this.scrapingStatus = val
    }
  },
  methods: {
    async doSpecificScraping() {
      this.scrapingStatus = "running";
      const json_data = {
        websiteToTest: this.websiteName
      };

      const response = await fetch('https://ow.services.eemi.tech/api/v1/web/nils.vaede/default/scraping.json', {
        method: 'POST',
        headers: {
          'Content-Type': 'application/json;charset=utf-8'
        },
        body: JSON.stringify(json_data)
      });

      const result = await response.json();
      switch (this.websiteName) {
        case "Indeed" :
          // eslint-disable-next-line no-prototype-builtins
          if (result.indeed.hasOwnProperty("error")) {
            this.scrapingStatus = "error";
          } else {
            this.scrapingStatus = "success";
          }
          break;
        case "Hello Work" :
          // eslint-disable-next-line no-prototype-builtins
          if (result.helloWork.hasOwnProperty("error")) {
            this.scrapingStatus = "error";
          } else {
            this.scrapingStatus = "success";
          }
          break;
        case "LinkedIn" :
          // eslint-disable-next-line no-prototype-builtins
          if (result.linkedIn.hasOwnProperty("error")) {
            this.scrapingStatus = "error";
          } else {
            this.scrapingStatus = "success";
          }
          break;
        case "Pôle Emploi" :
          // eslint-disable-next-line no-prototype-builtins
          if (result.poleEmploi.hasOwnProperty("error")) {
            this.scrapingStatus = "error";
          } else {
            this.scrapingStatus = "success";
          }
          break;
      }
    }
  }
}
</script>

<style scoped>
.scraping-element {
  display: flex;
  justify-content: space-between;
  width: 100%;
  border: 1px solid #b8b8b8;
  border-radius: 7px;
  margin-top: 32px;
  padding: 10px 50px;
  cursor: pointer;
}

.scraping-element-icon-wrapper {
  display: flex;
  flex-direction: column;
  justify-content: center;
}

.scraping-element-icon {
  font-size: 22px;
}

.running {
  background: #b8b8b8;
  color: initial;
}

.success {
  background: #26bd26;
  color: #fff;
}

.error {
  background: #c51c1c;
  color: #fff;
}
</style>