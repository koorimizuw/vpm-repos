<script setup lang="ts">
import Package from "./components/Package.vue"
import axios from "axios";
import { onMounted, ref, watch, watchEffect } from "vue";

const packagesInfo = ref<any>({});
const getPackagesInfo = async () => {
  const req = await axios.get("./index.json");
  packagesInfo.value = req.data
}

const keyword = ref("")
const results = ref<any[]>([])
watchEffect(() => {
  if (Object.values(packagesInfo.value).length === 0) {
    results.value = [];
    return;
  }
  const packages = Object.values(packagesInfo.value.packages) as any[];
  if (keyword.value === "") results.value = packages;
  results.value = packages.filter((item: any) => {
    const versions = Object.values(item.versions) as any;
    return versions[0]?.displayName?.toLowerCase().includes(keyword.value.toLowerCase());
  })
})

onMounted(async () => {
  await getPackagesInfo();
})
</script>

<template>
  <div class="container">
    <h1 class="title">Yamadev vpm packages</h1>
    <v-text-field v-model="keyword" label="Search packages..."></v-text-field>
    <ul>
      <Package v-for="packageInfo in results" :key="(Object.values(packageInfo.versions)[0] as any)?.displayName"
        :packageInfo="packageInfo" :url="packagesInfo.url" />
    </ul>
  </div>

</template>

<style scoped>
.container {
  max-width: 960px;
  margin: 0 auto;
}

.title {
  font-weight: 300;
  font-size: 5em;
  margin-top: 100px;
  margin-bottom: 200px
}
</style>
