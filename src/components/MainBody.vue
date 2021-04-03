<template>
  <div>
    <p class="header-text header">YouTube Bot V1.0.0</p>

    <div class="add-new-youtube">
      <input
        @keydown.enter="addYoutube"
        type="text"
        placeholder="Enter Bot Name"
        v-model="header"
      />
      <button class="btn btn-block" @click="addYoutube">Add</button>
    </div>

    <div class="row">
      <div v-for="(item, index) in Youtubes" :key="index" class="col s12">
        <div>
          <YouTube :header="item" v-on:delete-card="deleteCard(index)" />
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import YouTube from "./YouTube";

export default {
  name: "MainBody",
  components: {
    YouTube,
  },
  data() {
    return {
      header: "",
      Youtubes: ["Ko"],
    };
  },
  methods: {
    addYoutube() {
      if (this.header.replace(" ", "") !== "") {
        this.Youtubes.push(this.header);
        this.header = "";
        // console.log(this.Youtubes);
      } else {
        window.alert("Bot Name cannot be empty");
      }
    },

    deleteCard(index) {
      // console.log("called delete", index);
      this.Youtubes.splice(index, 1);
    },
  },
};
</script>

<style scoped>
.header {
  text-align: center;
}

.add-new-youtube {
  padding: 2rem;
  display: flex;
  flex-direction: row;
  justify-content: center;
}

.add-new-youtube > input {
  width: 40vw;
}

.add-new-youtube > button {
  margin: 0 0 0 2rem;
  width: 10rem;
}
</style>