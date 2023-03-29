<template>
  <div>
    <h1>Страница с постами</h1>
    <my-input v-model="searchQuery" placeholder="Search..." />
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
    <post-list :posts="sortedAndSearchPosts" @remove="removePost" />
    <div v-if="isLoading">Идет загрузка...</div>
    <div ref="observer" class="observer"></div>
    <!-- <div class="page__wrapper">
      <div
        v-for="pageNumber in totalPages"
        :key="pageNumber"
        class="page"
        :class="{
          'current-page': this.page === pageNumber,
        }"
        @click="changePage(pageNumber)"
      >
        {{ pageNumber }}
      </div> -->
    <!-- </div> -->
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
      page: 1,
      totalPages: 1,
      limit: 6,
      dialogVisible: false,
      isLoading: false,
      selectedSort: '',
      searchQuery: '',
      sortOptions: [
        { value: 'title', name: 'По названию' },
        { value: 'body', name: 'По содержимому' },
      ],
    };
  },
  methods: {
    // changePage(page) {
    //   this.page = page;
    // },
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
    async loadMorePosts() {
      try {
        this.isLoading = true;
        const response = await axios.get(
          'https://jsonplaceholder.typicode.com/posts',
          {
            params: {
              _page: this.page,
              _limit: this.limit,
            },
          },
        );
        this.totalPages = Math.ceil(
          response.headers['x-total-count'] / this.limit,
        );
        this.posts = [...this.posts, ...response.data];
        this.page += 1;
      } catch (e) {
        alert(e);
      } finally {
        this.isLoading = false;
      }
    },
  },
  mounted() {
    this.loadMorePosts();

    let observer;

    let options = {
      rootMargin: '0px',
      threshold: 1.0,
    };

    const handleIntersect = (entries, observer) => {
      if (entries[0].isIntersecting && this.page < this.totalPages) {
        this.loadMorePosts();
      }
    };

    observer = new IntersectionObserver(handleIntersect, options);
    observer.observe(this.$refs.observer);
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
  watch: {
    // Имя наблюдателя должно быть таким же как и имя свойства из data
    // за которым он наблюдает
    // page() {
    //   this.loadMorePosts();
    // },
  },
  computed: {
    sortedPosts() {
      return [...this.posts].sort((post1, post2) =>
        post1[this.selectedSort]?.localeCompare(post2[this.selected]),
      );
    },
    sortedAndSearchPosts() {
      return this.sortedPosts.filter((post) =>
        post.title.toLowerCase().includes(this.searchQuery),
      );
    },
  },
};
</script>

<style>
.page__wrapper {
  display: flex;
  margin-top: 16px;
}

.page {
  border: 1px solid black;
  padding: 10px;
  cursor: pointer;
}

.current-page {
  border: 2px solid teal;
}

.observer {
  height: 30px;
  background: grey;
}
</style>
