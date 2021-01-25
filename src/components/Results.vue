<template>
  <v-container fluid>
    <div class="d-flex flex-wrap justify-center">
      <div
        v-for="gif in gifs.slice(page * 10 - 10, page * 10)"
        :key="gif.id"
        class="px-4 py-8"
      >
        <a :href="gif.url" target="_blank">
          <video
            autoplay
            loop
            :src="gif.images.preview.mp4"
            height="100"
            width="130"
          />
        </a>
      </div>
    </div>
    <v-pagination
      v-model="page"
      :length="numberOfPages"
      :total-visible="7"
      color="cyan"
    >
    </v-pagination>
    <v-btn
      v-if="this.page === this.numberOfPages"
      outlined
      @click="getMoreGifs"
      color="cyan"
    >
      get more gifs
    </v-btn>
  </v-container>
</template>

<script>
export default {
  name: "Results",
  props: ["gifs", "numberOfGifs", "getMoreGifs"],

  data: () => ({
    page: 1
  }),
  methods: {},
  computed: {
    numberOfPages() {
      return Math.ceil(this.numberOfGifs / 10);
    }
  }
};
</script>
