<template>
  <div class="notification is-success">
    <button class="delete" @click="$emit('closeMess')"></button>
    {{msg}}
  </div>
</template>

<script>
export default {
  name: 'notify',
  props: [ 'msg' ],
  data () {
    return { }
  }
}
</script>

<style scoped>
  .notification {
    position: fixed !important;
    top: 3px;
    right: 3px;
    left: 3px;
    z-index: 100; 
  }
</style>
