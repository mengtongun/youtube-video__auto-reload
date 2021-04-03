<template>
  <div class="box-wrapper">
    <p class="header-text">{{ header }}</p>
    <div class="wrapper">
      <div class="child-1 youtube-box">
        <iframe
          v-if="youtubeVideoID.length > 0 && render"
          id="ytplayer"
          type="text/html"
          width="640"
          height="320"
          :src="`https://www.youtube.com/embed/${youtubeVideoID}?autoplay=1&mute=1`"
          title="YouTube video player"
          frameborder="0"
          allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture"
          allowfullscreen
        ></iframe>
      </div>
      <div class="child-2">
        <!-- <div class="row container"> -->
        <label for="ytlink">YouTube URL</label>
        <input v-model="url" class="input" type="text" name="ytlink" />

        <label for="counter">Refresh Time (Second)</label>
        <input v-model="counter" type="text" name="counter" id="counter" />
        <button class="btn btn-primary" @click="onSubmit">Start</button>
        <button class="btn red" @click="stopReload">Stop</button>
        <button class="btn red" @click="deleteCard">Delete</button>
        <p>Refreshed = {{ refreshTime }}</p>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "YouTube",
  props: {
    header: String,
  },
  data() {
    return {
      counter: 5,
      youtubeVideoID: "",
      url: "",
      render: true,
      timer: "",
      refreshTime: 0,
    };
  },
  methods: {
    onSubmit() {
      // this.startReload = true;
      this.reload();
      // console.log("called on submit");
    },

    reload() {
      this.getYoutubeVideoIDTest();
      if (this.youtubeVideoID == "") {
        console.log("No call");
      } else {
        var reloadTime =
          this.counter * 1000 > 5000 ? this.counter * 1000 + 2000 : 7000;

        this.timer = setInterval(() => {
          this.render = false;

          this.$nextTick(() => {
            this.refreshTime++;
            this.render = true;
          });
          // console.log("called");
        }, reloadTime);
      }
    },

    getYoutubeVideoID() {
      const url = this.url;
      var video_id = url.split("v=")[1];
      var ampersandPosition = video_id.indexOf("&");
      if (ampersandPosition != -1) {
        video_id = video_id.substring(0, ampersandPosition);
      }
      this.youtubeVideoID = video_id;
    },
    getYoutubeVideoIDTest() {
      const url = this.url;
      var regExp = /^.*((youtu.be\/)|(v\/)|(\/u\/\w\/)|(embed\/)|(watch\?))\??v?=?([^#&?]*).*/;
      var match = url.match(regExp);
      this.youtubeVideoID = match && match[7].length == 11 ? match[7] : false;
    },
    stopReload() {
      this.youtubeVideoID = "";
      clearInterval(this.timer);
    },

    deleteCard() {
      this.$emit("delete-card");
    },
  },
  emits: ["delete-card"],
  // created() {
  //   this.url = this.getYoutubeVideoID();
  // },
};
</script>

<style>
@import url("https://fonts.googleapis.com/css2?family=Poppins:ital,wght@0,600;1,400&display=swap");
.header-text {
  font-family: "Poppins", sans-serif;
  font-weight: 600;
  font-size: 2em;
  margin: 1rem 0 0 2rem;
}

.wrapper {
  display: flex;
  flex-direction: row;
  flex-wrap: wrap;
}

.wrapper > .child-1 {
  order: 1;
  margin: 2rem;
  width: 650px;
  height: 330px;
}
.wrapper > .child-2 {
  order: 2;
  padding: 2em;
  margin: 2rem;
}

@media (min-width: 1200px) {
  .wrapper > .child-2 {
    width: 45vw;
  }
}

.wrapper > .child-2 > button {
  margin-right: 5px;
}

.youtube-box {
  border: solid 5px#FFF;
  border-radius: 5px;
  box-shadow: 0 0 30px rgba(4, 100, 56, 0.318);
}

.box-wrapper {
  border: solid 5px white;
  border-radius: 5px;
  box-shadow: 0 0 20px rgb(0, 0, 0, 0.3);
  margin-bottom: 100px;
}
</style>