<template>
  <v-container>
    <v-text-field
      label="Search your perfect gif"
      hide-details="auto"
      :loading="input.isLoading"
      :messages="messages()"
      v-model="input.text"
      append-icon="mdi-magnify"
      @click:append="getGifs"
      @keyup.enter="getGifs"
      @keydown.esc="input.text = ''"
    >
    </v-text-field>
    <div v-if="history.length > 0" class="d-flex justify-end">
      <v-btn x-small text @click="showHistory = !showHistory"
        >{{ showHistory ? "hide" : "show" }} history</v-btn
      >
    </div>
    <div v-if="showHistory">
      <v-btn
        v-for="(item, i) in history"
        :key="i"
        x-small
        rounded
        color="cyan white--text"
        class="mx-1"
        @click="historyBtnClicked(item)"
        >{{ item }}</v-btn
      >
    </div>

    <div v-if="numberOfGifs > 0">
      <Results
        :gifs="gifs"
        :number-of-gifs="numberOfGifs"
        :getMoreGifs="getGifs"
        class="px-0"
      />
    </div>
    <div v-if="showNoMoreResults">
      no more gifs
    </div>
    <div v-if="showErrorMsg" class="pt-3 d-flex justify-center">
      <video
        src="https://media2.giphy.com/media/KKOMG9EB7VqBq/giphy.mp4?cid=ecf05e4737a39d88599f17d40d6906af00fd3c767ad3d6c4&rid=giphy.mp4"
        autoplay
        loop
      ></video>
    </div>
  </v-container>
</template>

<script>
import Results from "@/components/Results";

export default {
  name: "Search",
  components: {
    Results
  },
  data: () => ({
    input: {
      isLoading: false,
      key: "",
      apiUrl: "https://api.giphy.com/v1/gifs",
      text: ""
    },
    gifs: [],
    showNoMoreResults: false,
    showGetMoreGifs: false,
    showErrorMsg: false,
    lastSearchQuery: "",
    history: [],
    showHistory: false
  }),
  methods: {
    getGifs() {
      this.handleHistory();
      this.input.isLoading = true;
      fetch(this.searchQuery)
        .then(response => response.json())
        .then(data => {
          console.log("data: ", data);
          this.gifs.push(...data.data);
          this.input.isLoading = false;
          this.showNoMoreResults = false;
          if (this.gifs.length === 0) {
            this.showErrorMsg = true;
          } else {
            if (data.data.length === 0) this.showNoMoreResults = true;
            this.showErrorMsg = false;
          }
        })
        .catch(err => console.log("An error accrued: ", err.message));
    },
    messages() {
      if (this.gifs.length > 0) return this.numberOfGifs + " gifs found";
      if (this.input.isLoading) return "Searching";
      return "";
    },
    handleHistory() {
      if (
        this.input.text.toLowerCase() !== this.lastSearchQuery.toLowerCase()
      ) {
        if (!this.history.includes(this.input.text)) {
          this.history.push(this.input.text);
          localStorage.setItem("history", JSON.stringify(this.history));
        } else if (this.history.length > 10) {
          this.history.shift();
        }
        this.lastSearchQuery = this.input.text;
        this.gifs = [];
      }
    },
    historyBtnClicked(term) {
      this.input.text = term;
      this.getGifs();
    }
  },
  computed: {
    searchQuery: function() {
      return `${this.input.apiUrl}/search?api_key=${this.input.key}&q=${this.input.text}&offset=${this.gifs.length}`;
    },
    numberOfGifs() {
      return this.gifs.length;
    }
  },
  mounted() {
    this.history = JSON.parse(localStorage.getItem("history")) || [];
    this.input.key = localStorage.getItem("apiKey");
  }
};
</script>
