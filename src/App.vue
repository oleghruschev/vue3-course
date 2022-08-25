<template>
  <div class="app">
    <post-form @create="createPost" />
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
    };
  },
  methods: {
    createPost(newPost) {
      this.posts.push(newPost);
    },
    removePost(post) {
      this.posts = this.posts.filter(({ id }) => id !== post.id);
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
