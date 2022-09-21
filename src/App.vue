<template>
  <div class="app">
    <h1>Страница с постами</h1>
    <div class="app-buttons">
      <my-button @click="openDialog">Создать пост</my-button>
      <my-select v-model="selectedSort" :options="sortOptions" />
    </div>
    <my-dialog v-model:show="dialogVisible">
      <post-form @create="createPost" />
    </my-dialog>
    <!-- <post-list
      :posts="posts"
      @remove="removePost"
      v-if="!isLoading"
    /> -->
    <post-list
      :posts="sortedPosts"
      @remove="removePost"
      v-if="!isLoading"
    />
    <div v-else>Идет загрузка...</div>
  </div>
</template>

<script>
import PostForm from '@/components/PostForm';
import PostList from '@/components/PostList';
import axios from 'axios';

export default {
  components: { PostForm, PostList },
  data() {
    return {
      title: '',
      body: '',
      posts: [],
      dialogVisible: false,
      isLoading: false,
      selectedSort: '',
      sortOptions: [
        { value: 'title', name: 'По названию' },
        { value: 'body', name: 'По содержимому' },
      ],
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
    async fetchPosts() {
      try {
        this.isLoading = true;
        const response = await axios.get(
          'https://jsonplaceholder.typicode.com/posts?_limit=10',
        );

        this.posts = response.data;
      } catch (e) {
        alert(e);
      } finally {
        this.isLoading = false;
      }
    },
  },
  mounted() {
    this.fetchPosts();
  },
  // watch: {
  //   selectedSort(newValue) {
  //     this.posts.sort((post1, post2) => {
  //       // return post1[this.selectedSort]?.localeCompare(
  //       //   post2[this.selectedSort],
  //       // );
  //       return post1[newValue]?.localeCompare(post2[newValue]);
  //     });
  //   },
  // },
  computed: {
    sortedPosts() {
      return [...this.posts].sort((post1, post2) =>
        post1[this.selectedSort]?.localeCompare(post2[this.selected]),
      );
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

.app-buttons {
  display: flex;
  justify-content: space-between;
}
</style>
