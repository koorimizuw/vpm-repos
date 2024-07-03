<script setup lang="ts">
import { ref } from 'vue'
import source from "../../source.json"

const props = defineProps<{
  packageInfo: {
    versions: {
      [x: string]: {
        name: string;
        displayName: string;
        version: string;
        description: string;
        vpmDependencies: {
          [x: string]: string;
        },
        url: string;
      }
    }
  },
  url: string;
}>()

const name = ref(Object.values(props.packageInfo.versions)[0]?.displayName)
const description = ref(Object.values(props.packageInfo.versions)[0]?.description)
const addVCC = () => {
  open(`vcc://vpm/addRepo?url=${props.url}`, "_self");
}
const openGithub = () => {
  const repo = source.githubRepos.filter(i => i.includes(name.value))[0];
  open(`https://github.com/${repo}`, "_blank");
}
</script>

<template>
  <li>
    <div class="left">
      <h1 class="name">{{ name }}</h1>
      <div class="description">{{ description }}</div>
    </div>
    <div class="right">
      <v-btn variant="plain" @click="openGithub">View on Github</v-btn>
      <v-btn variant="plain" @click="addVCC">Add to VCC</v-btn>
    </div>
  </li>
</template>

<style scoped>
li {
  list-style: none;
  margin: 20px 0;
  display: flex;
  justify-content: space-between;
  font-weight: 300;
}

h1 {
  font-weight: 400;
}

.right {
  display: flex;
  align-items: center;
}

.v-btn {
  margin-left: 10px;
}
</style>
