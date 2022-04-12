<template>
  <div class="flex flex-col">
    <img :src="imageUrl" />
    <div>
      <span class="vote-rating font-bold text-2xl" :class="ratingColourClasses"
        >{{ ratingColourEmojis }} {{ item.vote_average }}</span
      >
      <p class="font-bold film-title">{{ item.title }}</p>
      <a :href="rentMovieURL">Watch now</a>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    item: {
      type: Object,
      required: true,
    },
  },
  computed: {
    imageUrl: function () {
      return this.item.poster_path === null
        ? "https://via.placeholder.com/300x450"
        : `https://image.tmdb.org/t/p/w300${this.item.poster_path}`;
    },
    rentMovieURL: function () {
      return `https://www.google.com/search?q=rent ${this.item.title}`;
    },
    ratingColourClasses() {
      if (this.item.vote_average >= 7.5) {
        return "text-green-500";
      } else if (this.item.vote_average >= 4) {
        return "text-orange-500";
      } else {
        return "text-red-500";
      }
    },
    ratingColourEmojis() {
      if (this.item.vote_average >= 8) {
        return "💎";
      } else if (this.item.vote_average > 7) {
        return "💯";
      } else if (this.item.vote_average > 6) {
        return "🗑️";
      } else if (this.item.vote_average > 5) {
        return "🤮";
      } else {
        return "💩";
      }
    },
  },
};
</script>
