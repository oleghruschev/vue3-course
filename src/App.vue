<template>
  <navbar></navbar>
  <div class="app">
    <router-view></router-view>
  </div>
</template>
>

<script>
import Navbar from '@/components/Navbar';

export default {
  components: {
    Navbar,
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
  max-width: 768px;
  margin: 0 auto;
}

.app-buttons {
  display: flex;
  justify-content: space-between;
}
</style>
