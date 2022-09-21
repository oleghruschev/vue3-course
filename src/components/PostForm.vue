<template>
  <form class="form" @submit.prevent>
    <my-input placeholder="title" v-model="post.title" />
    <my-input placeholder="body" v-model="post.body" />
    <my-button @click="createPost" style="align-self: flex-end">
      Add post
    </my-button>
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
  watch: {
    // post(newValue) {
    //   console.log(111, newValue);
    // },
    post: {
      handler(newValue) {
        console.log(22, newValue);
      },
      deep: true,
    },
  },
};
</script>

<style scoped>
.form {
  display: flex;
  flex-direction: column;
}
</style>
