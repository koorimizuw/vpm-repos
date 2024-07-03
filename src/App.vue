<script setup lang="ts">
import Package from "./components/Package.vue"
import axios from "axios";
import { onMounted, ref } from "vue";

const packagesInfo = ref<any>({});
const getPackagesInfo = async () => {
  const req = await axios.get("./index.json");
  packagesInfo.value = req.data
}

const keyword = ref("")
const search = () => {
  if (Object.values(packagesInfo.value).length === 0) return [];
  const packages = Object.values(packagesInfo.value.packages);
  if (keyword.value === "") return packages;
  return packages.filter((item: any) => {
    const versions = Object.values(item.versions) as any;
    return versions[0]?.displayName?.toLowerCase().includes(keyword.value.toLowerCase());
  })
};

onMounted(async () => {
  await getPackagesInfo();
})
</script>

<template>
  <div class="container">
    <h1 class="title">Yamadev vpm packages</h1>
    <v-text-field v-model="keyword" label="Search packages..."></v-text-field>
    <ul>
      <Package v-for="(packageInfo, i) in search()" :key="i" :packageInfo="packageInfo as any"
        :url="packagesInfo.url" />
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
