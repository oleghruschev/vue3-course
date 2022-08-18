<template>
  <form class="form" @submit.prevent>
    <input
      class="input"
      type="text"
      placeholder="title"
      v-model="post.title"
      @input="inputTitle"
    />
    <input
      class="input"
      type="body"
      placeholder="body"
      v-model="post.body"
      @input="body = $event.target.value"
    />
    <button class="btn" @click="createPost">add post</button>
  </form>
</template>

<script>
export default {
  data() {
    return {
      post: {
        title: '',
        body: '',
      },
    };
  },
  methods: {
    createPost() {
      const newPost = {
        id: new Date(),
        ...this.post,
      };

      if (this.post.title && this.post.body) {
        this.$emit('create', newPost);
        this.post = {
          title: '',
          body: '',
        };
      }
    },
  },
};
</script>

<style scoped>
.form {
  display: flex;
  flex-direction: column;
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
</style>
