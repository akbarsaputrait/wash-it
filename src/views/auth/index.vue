<template>
  <div id="root">
    <main>
      <div class="container">
        <router-view/>
      </div>
    </main>
  </div>
</template>
<script>
export default {
  mounted () {
    document.body.classList.add('background')
  },
  beforeDestroy () {
    document.body.classList.remove('background')
  }
}
</script>
