<template>
  <div class="scraping-element" :class="status">
    <div class="scraping-element-title">
      <h3>{{ websiteName }}</h3>
    </div>
    <div class="scraping-element-icon-wrapper">
      <i class="fa-solid fa-clock scraping-element-icon" v-if="status === 'pending'"></i>
      <i class="fa-solid fa-arrows-rotate scraping-element-icon" v-else-if="status === 'running'"></i>
      <i class="fa-solid fa-circle-check scraping-element-icon" v-else-if="status === 'success'"></i>
      <i class="fa-solid fa-circle-xmark scraping-element-icon" v-else-if="status === 'error'"></i>
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