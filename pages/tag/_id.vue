<template>
<div class="flex flex-wrap p-2">
    <div v-if="$fetchState.pending" class="absolute inset-0 bg-green-100 p-10 text-black text-4xl">
        Loading...
    </div>
    <div v-for="post in posts" :key="post.id" class="w-1/2 p-2" v-else>
        <div class="p-3 shadow-lg bg-white rounded-sm">
            <div class="font-bold pb-2">{{ post.title }}</div>
            <div class="text-sm pb-3">{{ post.description }}</div>
            <img :src="post.social_image"/>
            <div class="flex flex-wrap">
                <div v-for="tag in post.tag_list" :key="tag" class="px-2 py-1 m-2 bg-green-200 text-sm">
                    <nuxt-link :to="`/tag/${tag}/`">{{ tag }}</nuxt-link>
                </div>
            </div>
        </div>
    </div>
</div>
</template>

<script>
export default {
  data() {
    return {
      posts: [],
    };
  },
  async fetch() {
    this.posts = await fetch(
      `https://dev.to/api/articles?tag=${this.$route.params.id}&state=rising&per_page=10`
    ).then((resp) => resp.json());
  },
};
</script>