<template>
  <div class="app">
    <form class="form" @submit.prevent>
      <input
        class="input"
        type="text"
        v-bind:value="title"
        @input="inputTitle"
        placeholder="title"
      />
      <input
        class="input"
        type="body"
        v-bind:value="body"
        @input="body = $event.target.value"
        placeholder="body"
      />
      <button class="btn" @click="createPost">add post</button>
    </form>

    <div class="post" v-for="post in posts" :key="post.id">
      <div><strong>Title:</strong> {{ post.title }}</div>
      <div><strong>Description:</strong> {{ post.body }}</div>
    </div>
  </div>
</template>

<script lang="ts">
export default {
  data() {
    return {
      title: '',
      body: '',
      posts: [
        { id: 1, title: 'Post1', body: 'Some text about js 1' },
        { id: 2, title: 'Post2', body: 'Some text about js 2' },
        { id: 3, title: 'Post3', body: 'Some text about js 3' },
      ],
    };
  },
  methods: {
    createPost() {
      if (!this.title || !this.body) return;

      const newPost = {
        id: new Date(),
        title: this.title,
        body: this.body,
      };
      // @ts-ignore
      this.posts.push(newPost);
      this.title = '';
      this.body = '';
    },
    inputTitle(e: any) {
      this.title = e.target.value;
    },
    inputBody(e: any) {
      this.body = e.target.value;
    },
  },
};
</script>

<style>
* {
  margin: 0;
  padding: 0;
}

.app {
  padding: 20px;
}

.post {
  padding: 15px;
  border: 2px solid teal;
  margin-top: 15px;
  display: flex;
  align-items: center;
  justify-content: space-between;
}

.input {
  border: 1px solid teal;
  padding: 10px 15px;
  margin-bottom: 15px;
}

.btn {
  padding: 10px 15px;
  background: none;
  color: teal;
  border: 1px solid teal;
  margin-bottom: 15px;
  align-self: flex-end;
}

.form {
  display: flex;
  flex-direction: column;
}
</style>
