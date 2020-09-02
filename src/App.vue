<template>
  <div id="app">
    <!-- <img src="./assets/cii_small.jpg" alt="cii_cover" width="240" height="240" /> -->
    <header>
      <h1 class="cii-title">Continuity In Itself</h1>
    </header>
    <main>
      <section class="player">
        <h2 class="song-title">
          {{ currentSong.artist }}
          -
          <span>{{ currentSong.title }}</span>
        </h2>
        <div class="controls">
          <button class="prev" @click="prev">PREV</button>
          <button class="play" v-if="!isPlaying" @click="play">&#9658;</button>
          <button class="pause" v-else @click="pause">&#9646; &#9646;</button>
          <button class="next" @click="next">NEXT</button>
        </div>
      </section>
      <section class="playlist">
        <h3>Playlist</h3>
        <button
          v-for="song in songs"
          :key="song.src"
          @click="play(song)"
          :class="(song.src == currentSong.src) ? 'song playing' : 'song'"
        >{{ song.artist }} - {{ song.title }}</button>
        <!-- conditional (ternary) operator -->
      </section>
    </main>
  </div>
</template>

<script>
// import SongsFromJson from "./songs.json";

export default {
  name: "App",
  components: {},
  data() {
    return {
      currentSong: {},
      index: 0,
      isPlaying: false,
      songs: [
        {
          title: "In The 21st Century",
          artist: "Continuity In Itself",
          src: require("./assets/InThe21stCentury.mp3")
        },
        {
          title: "Ejected",
          artist: "Continuity In Itself",
          src: require("./assets/Ejected.mp3")
        }
      ],
      player: new Audio()
      //https://developer.mozilla.org/en-US/docs/Web/API/HTMLAudioElement/Audio
    };
  },
  methods: {
    play(song) {
      if (typeof song.src != "undefined") {
        this.currentSong = song;
        this.player.src = this.currentSong.src;
      }
      this.player.play();
      this.player.addEventListener(
        "ended",
        function() {
          //next song:
          this.index++;
          if (this.index > this.songs.length - 1) {
            this.index = 0;
          }
          this.currentSong = this.songs[this.index];
          this.play(this.currentSong);
        }.bind(this)
      );
      this.isPlaying = true;
    },
    pause() {
      this.player.pause();
      this.isPlaying = false;
    },
    prev() {
      this.index--;
      if (this.index < 0) {
        this.index = this.songs.length - 1;
      }
      this.currentSong = this.songs[this.index];
      this.play(this.currentSong);
    },
    next() {
      this.index++;
      if (this.index > this.songs.length - 1) {
        this.index = 0;
      }
      this.currentSong = this.songs[this.index];
      this.play(this.currentSong);
    }
  },
  created() {
    this.currentSong = this.songs[this.index];
    this.player.src = this.currentSong.src;
    //this.player.play();
  }
};
</script>

<style>
@import url(https://fonts.googleapis.com/css?family=Abel);
@import url(https://fonts.googleapis.com/css?family=Advent+Pro);

* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  color: #ddd;
}

body {
  width: 600px;
  margin: 0 auto;
  font-family: "Abel", sans-serif;
  /* font-family: Arial, Helvetica, sans-serif; */
  background-color: #111;
}

.cii-title {
  font-family: "Advent Pro", sans-serif;
  font-size: 3em;
  letter-spacing: 0.2em;
  /* text-decoration: #999 underline overline; */
  opacity: 0.3;
}

header {
  display: flex;
  justify-content: center;
  align-items: center;
  padding: 16px;
  background-image: url("./assets/cii_small.jpg");
  background-size: cover;
  color: #ccc;
}

button {
  background-color: #333;
  margin: 2px;
  padding: 2px;
  border-radius: 2px;
  border: #333 solid 1px;
  background-image: linear-gradient(to bottom, #444, #222);
  cursor: pointer;
}

button:hover {
  background-color: #666;
}

main {
  width: 100%;
  max-width: 768px;
  margin: 0 auto;
  padding: 32px;
}

.song-title {
  color: #ddd;
  font-size: 32px;
  margin: 8px;
  text-align: center;
}

.controls {
  display: flex;
  justify-content: center;
  align-items: center;
}

.play,
.pause {
  width: 160px;
  font-size: 20px;
  font-weight: 700;
  padding: 16px 24px;
}

.prev,
.next {
  width: 80px;
  font-size: 16px;
  font-weight: 700;
  padding: 16px 24px;
  display: flex;
  justify-content: center;
  align-items: center;
}

.playlist {
  padding: 30px 30px;
}

.playlist h3 {
  font-size: 28px;
  margin-bottom: 30px;
  text-align: center;
}

.playlist .song {
  font-family: "Advent Pro", sans-serif;
  display: block;
  width: 100%;
  font-size: 16px;
  font-weight: 700;
  padding: 15px;
  cursor: pointer;
  background-image: linear-gradient(to bottom, #444, #222);
  opacity: 0.4;
}
.playlist .song:hover {
  color: #eee;
  opacity: 1;
}

.playlist .song.playing {
  color: #ddd;
  background-image: linear-gradient(to bottom, #777, #111);
  opacity: 1;
}
</style>