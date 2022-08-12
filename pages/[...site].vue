<template>
   <div class="news-view">
      <div class="news-list-nav">
         <NuxtLink v-if="page > 1" class="page-link" :to="`/${type}?page=${page - 1}`" aria-label="Previous Page">
            &lt; prev
         </NuxtLink>
         <span v-else class="page-link disabled" aria-hidden="true">&lt; prev</span>

         <span>page {{ page }}</span>
         <!-- sometimes the length isn't 30? -->
         <NuxtLink v-if="stories.length >= 28" class="page-link" :to="`/${type}?page=${page + 1}`"
            aria-label="Next Page">
            more &gt;
         </NuxtLink>
         <span v-else class="page-link" aria-hidden="true">more &gt;</span>
      </div>
      <main class="news-list">
         <ul>
            <LazyStory v-for="(story, key) in stories" :key="key" :story="story" />
         </ul>
      </main>
   </div>
</template>

<script setup lang="ts">
const route = useRoute();
console.log(route.params)
const type = computed(() => route.params.site[0] ?? "top");
const page = computed(() => {
   if (route.query.page) {
      return parseInt(route.query.page.toString())
   }
   return 1
}
);
const { data: stories } = await useAsyncData(async () => await useGetStories(type.value, page.value) as [], {
   watch: [page, type],
   initialCache: false
});
</script>