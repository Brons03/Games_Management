<template>
  <div class="body">
    <nav-menu class="nav-menu"></nav-menu>
    <router-view/>
    <about id="about" class="nav-menu"></about>
  </div>
</template>
<script>
  import About from '@/components/common/About'
  import NavMenu from '@/components/common/NavMenu'
  export default {
    components: {NavMenu, About}
  }
</script>

<style>
  .body{
    height: 100%;
    display: flex;
    flex-direction: column;
  }
  .nav-menu {
    /*margin-bottom: 40px;*/
    box-shadow: 0 2px 4px 0 rgba(0,0,0,.05);
  }
</style>
