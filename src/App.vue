<template>
  <div id="app">
    <header>
      <h1>My Music</h1>
    </header>
    <main>
      <section class="player">
        <h2 class="song-title">{{ current.artist }} - <span>{{ current.title }}</span></h2>
        <div class="control">
          <button class="prev" @click="prev">Prev</button>
          <button class="play" v-if="!isPlay" @click="play">Play</button>
          <button class="pause" v-else @click="pause">Pause</button>
          <button class="next" @click="next">Next</button>
        </div>
      </section>
      <section class="playlist">
        <h3>The Playlist</h3>
        <button v-for="song in songs" :key="song.src" @click="play(song)" :class="(song.src == current.src) ? 'song playing' : 'song'">
          {{ song.title }} - {{ song.artist }}
        </button>
      </section>
    </main>
  </div>
</template>

<script>

export default {
  name: 'app',
  data() {
    return {
      current: {},
      index: 0,
      isPlay: false,
      songs: [
        {
          title: 'Invincible',
          artist: 'DEAF KEV',
          src: require('./assets/DEAF KEV - Invincible [NCS Release].mp3')
        },
        {
          title: 'On & On',
          artist: 'Cartoon',
          src: require('./assets/Cartoon - On & On (feat. Daniel Levi) [NCS Release].mp3')
        },
      ],
      player: new Audio()
    }
  },
  methods: {
  play(song) {
      if (typeof song.src != "undefined") {
        this.current = song;

        this.player.src = this.current.src;
      }
      this.player.play();
      this.player.addEventListener('ended', function() {
        this.index++;
        if(this.index > this.songs.length -1 ) {
          this.index = 0;
        }
        this.current = this.songs[this.index];
        this.play(this.current);
      }.bind(this));
      this.isPlay = true;
    },
    pause() {
      this.player.pause();
      this.isPlay = false;
    },
    next() {
      this.index++;
      if(this.index > this.songs.length -1 ) {
        this.index = 0;
      }
      this.current = this.songs[this.index];
      this.play(this.current);
    },
    prev() {
      this.index--;
      if(this.index < 0) {
        this.index = this.songs.length -1;
      }
      this.current = this.songs[this.index];
      this.play(this.current);
    }
  },
  created() {
    this.current = this.songs[this.index];
    this.player.src = this.current.src;
    this.player.pause();
  },
  computed: {
    
  },
  components: {
    
  }
}
</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
body {
  font-family: sans-serif;
}
header {
  display: flex;
  justify-content: center;
  align-items: center;
  padding: 15px;
  background-color: #212121;
  color: #FFF;
}
main {
  width: 100%;
  max-width: 768px;
  margin: 0 auto;
  padding: 25px;
}
.song-title {
  color: #53565a;
  font-size: 32px;
  font-weight: 700;
  text-transform: uppercase;
  text-align: center;
}
.control {
  display: flex;
  justify-content: center;
  padding: 30px 15px;
}
button {
  appearance: none;
  background: none;
  border: none;
  outline: none;
  cursor: pointer;
}
.play, .pause{
  font-size: 30px;
  font-weight: 700;
  padding: 15px 25px;
  margin: 0px 15px;
}
.playlist {
  padding: 0px 30px;
}
.playlist h3 {
  color: #212121;
  font-size: 28px;
  font-weight: 400;
  margin-bottom: 30px;
  text-align: center;
}
.playlist .song {
  display: block;
  width: 100%;
  padding: 15px;
  font-size: 20px;
  font-weight: 700;
  cursor: pointer;
}
.playlist .song.playing {
  color: #FFF;
  background-image: linear-gradient(to right, rgb(206, 176, 202), rgb(255, 4, 234))
}
</style>
