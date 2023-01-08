<template>
  <!-- <div v-if="posts.length"> -->
  <div v-show="posts.length">
    <h3>Список постов</h3>
    <transition-group name="list">
      <post-item
        v-for="post in posts"
        :post="post"
        :key="post.id"
        @remove="$emit('remove', post)"
      />
    </transition-group>
  </div>
  <!-- <h2 v-else style="color: red">Спсиок постов пуст</h2> -->
  <h2 v-show="!posts.length" style="color: red">
    Спсиок постов пуст
  </h2>
</template>

<style scoped>
.list-enter-active,
.list-leave-active {
  transition: all 0.5s ease;
}
.list-enter-from,
.list-leave-to {
  opacity: 0;
  transform: translateY(30px);
}

.list-move {
  transition: transform 0.4s ease;
}
</style>

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
