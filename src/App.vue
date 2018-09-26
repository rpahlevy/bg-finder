<template>
  <div id="app">
    <router-view/>
  </div>
</template>


<script>
export default {
  name: 'App'
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  margin-top: 60px;
}
.text-center {
	text-align: center;
}
.text-left {
	text-align: left;
}
.text-muted {
	color: #999;
}
.text-danger {
	color: #fab;
}
.f-small {
	font-size: .75rem;
}
</style>
