<template>
  <!-- <div v-if="posts.length"> -->
  <div v-show="posts.length">
    <h3>Список постов</h3>
    <post-item
      v-for="post in posts"
      :post="post"
      :key="post.id"
      @remove="$emit('remove', post)"
    />
  </div>
  <!-- <h2 v-else style="color: red">Спсиок постов пуст</h2> -->
  <h2 v-show="!posts.length" style="color: red">
    Спсиок постов пуст
  </h2>
</template>

<script>
import PostItem from '@/components/PostItem';

export default {
  components: { PostItem },
  props: {
    posts: {
      type: Array,
      required: true,
    },
  },
};
</script>
