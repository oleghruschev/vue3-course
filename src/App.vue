<template>
  <div class="app">
    <my-button @click="openDialog">Создать пост</my-button>
    <my-dialog v-model:show="dialogVisible">
      <post-form @create="createPost" />
    </my-dialog>
    <post-list :posts="posts" @remove="removePost" />
  </div>
</template>

<script>
import PostForm from '@/components/PostForm';
import PostList from '@/components/PostList';

export default {
  components: { PostForm, PostList },
  data() {
    return {
      title: '',
      body: '',
      posts: [
        { id: 1, title: 'Post1', body: 'Some text about js 1' },
        { id: 2, title: 'Post2', body: 'Some text about js 2' },
        { id: 3, title: 'Post3', body: 'Some text about js 3' },
      ],
      dialogVisible: false,
    };
  },
  methods: {
    createPost(newPost) {
      this.posts.push(newPost);
      this.closeDialog();
    },
    removePost(post) {
      this.posts = this.posts.filter(({ id }) => id !== post.id);
    },
    openDialog() {
      this.dialogVisible = true;
    },
    closeDialog() {
      this.dialogVisible = false;
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
</style>
