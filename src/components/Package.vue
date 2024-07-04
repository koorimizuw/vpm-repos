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

const getGitHubUrl = () => {
  const repo = source.githubRepos.filter(i => i.includes(name.value))[0];
  return repo ? `https://github.com/${repo}` : "";
}
</script>

<template>
  <li>
    <div class="left">
      <h1 class="name">{{ name }}</h1>
      <div class="description">{{ description }}</div>
    </div>
    <div class="right">
      <v-btn variant="plain" :href="getGitHubUrl()" target="_blank">View on Github</v-btn>
      <v-btn variant="plain" :href="`vcc://vpm/addRepo?url=${props.url}`">Add to VCC</v-btn>
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
