<template>
  <div class="row">
    <div class="col-2"></div>
    <div class="col-7">
      <form @submit.prevent="postNewImage" class="form-inline mb-5">
        <div class="form-group">
          <label for="imageUrl">Image URL</label>
          <input
            v-model="newImageUrl"
            type="text"
            class="form-control ml-2"
            placeholder="Enter the image URL"
            id="imageUrl"
          />
        </div>
        <div class="form-group">
          <label for="imageDescription">Description</label>
          <input
            v-model="newImageDescription"
            type="text"
            class="form-control ml-2"
            placeholder="Enter the image description"
            id="imageDescription"
          />
        </div>
        <button type="submit" class="btn btn-primary ml-2">Post image</button>
      </form>
      <instagram-card v-for="card in cards" :key="card.url" :info="card" />
    </div>
    <div class="col-3">Sidebar</div>
  </div>
</template>
<script>
import InstagramCard from "@/components/InstagramCard.vue";
import { db } from "@/firebase";
import store from "@/store.js";
/*
let cards = [];
cards = [
  {
    url: "https://picsum.photos/id/1/400/400",
    description: "laptop",
    time: "few minutes ago...",
  },
  {
    url: "https://picsum.photos/id/2/400/400",
    description: "laptop #2",
    time: "hour ago...",
  },
  {
    url: "https://picsum.photos/id/3/400/400",
    description: "laptop #3",
    time: "few hours ago...",
  },
];*/
export default {
  name: "home-ins",
  data: function () {
    return {
      cards: [],
      newImageDescription: "",
      newImageUrl: "",
    };
  },
  mounted() {
    this.getPosts();
  },
  computed: {
    filteredCards() {
      // logika koja filtrira cards
      let termin = this.store.searchTerm;
      return this.cards.filter((card) => card.description.includes(termin));
    },
  },
  components: { InstagramCard },
  methods: {
    getPosts() {
      console.log("dohvat sa firebsea");
      db.collection("posts")
        .get()

        .then((query) => {
          this.cards = [];
          query.forEach((doc) => {
            const data = doc.data();
            this.cards.push({
              id: doc.id,
              time: data.posted_at,
              description: data.description,
              url: data.url,
            });
          });
        });
    },
    postNewImage() {
      const imageURL = this.newImageUrl;
      const imageDescription = this.newImageDescription;
      console.log(imageURL, imageDescription);
      db.collection("posts")
        .add({
          url: imageURL,
          description: imageDescription,
          email: store.userEmail,
          posted_at: Date.now(),
        })
        .then(() => {
          console.log("spremljeno", document);
          this.getPosts();
          this.newImageUrl = "";
          this.newImageDescription = "";
        })
        .catch((e) => {
          console.error("greška", e);
        });
    },
  },
};
</script>
