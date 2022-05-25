<template>
  <div class="post" v-for="post in posts">
    <div><strong>Title:</strong> {{ post.title }}</div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      posts: [
        { id: 1, title: 'Post1', body: 'Some text about js 1' },
        { id: 2, title: 'Post2', body: 'Some text about js 2' },
        { id: 3, title: 'Post3', body: 'Some text about js 3' },
      ],
    };
  },
  methods: {
    addLike() {
      this.likes += 1;
    },
    addDislike() {
      this.dislikes += 1;
    },
  },
};
</script>

<style>
* {
  margin: 0;
  padding: 0;
}

.post {
  padding: 1rem;
  border: 1px solid #000;
  margin-bottom: 1rem;
}
</style>
