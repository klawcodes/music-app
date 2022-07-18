<template>
  <div id="app"></div>
  <header></header>

  <main>
    <section class="player">
      <img class="image" v-bind:src="current.img" />

      <h2 class="song-title">
        {{ current.artist }} - <span>{{ current.title }}</span>
      </h2>
      <div class="control">
        <button class="prev" @click="prev">Prev</button>
        <button class="play" v-if="!isPlaying" @click="play">Play</button>
        <button class="pause" v-else @click="pause">Pause</button>
        <button class="next" @click="next">Next</button>
      </div>
    </section>
    <section class="playlist">
      <h2>pleylis guweh</h2>
      <button v-for="song in songs" :key="song.src" @click="play(song)" :class="song.src == current.src ? 'song playing' : 'song'">{{ song.artist }} - {{ song.title }}</button>
    </section>
  </main>
</template>
<script>
export default {
  name: 'App',
  data() {
    return {
      current: {},
      index: 0,
      isPlaying: false,
      songs: [
        {
          img: require('./assets/yourlove.jpg'),
          title: 'your love',
          artist: 'brb, jimmy brown',
          src: require('./assets/yourlove.mp3'),
        },

        {
          img: require('./assets/aea.jpg'),
          title: 'Atomic Ever After',
          artist: 'Kid Travis',
          src: require('./assets/aea.mp3'),
        },

        {
          img: require('./assets/gou.jpg'),
          title: 'Glimpse of Us',
          artist: 'Joji',
          src: require('./assets/gou.mp3'),
        },

        {
          img: require('./assets/beam.jpg'),
          title: 'Beam',
          artist: 'ISOxo',
          src: require('./assets/beam.mp3'),
        },
      ],
      player: new Audio(),
    };
  },
  methods: {
    play(song) {
      if (typeof song.src != 'undefined') {
        this.current = song;

        this.player.src = this.current.src;
      }

      this.player.play();
      this.player.addEventListener(
        'ended',
        function () {
          this.index++;
          if (this.index > this.songs.length - 1) {
            this.index = 0;
          }

          this.current = this.songs[this.index];
          this.play(this.current);
        }.bind(this)
      );
      this.isPlaying = true;
    },
    pause() {
      this.player.pause();
      this.isPlaying = false;
    },
    next() {
      this.index++;
      if (this.index > this.songs.length - 1) {
        this.index = 0;
      }

      this.current = this.songs[this.index];
      this.play(this.current);
    },
    prev() {
      this.index--;
      if (this.index < 0) {
        this.index = this.songs.length - 1;
      }

      this.current = this.songs[this.index];
      this.play(this.current);
    },
  },

  created() {
    this.current = this.songs[this.index];
    this.player.src = this.current.src;
  },
};
</script>
<style>
@import 'app.css';
</style>
