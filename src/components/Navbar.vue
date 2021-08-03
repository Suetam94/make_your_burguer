<template>
  <div id="nav">
    <router-link to="/" id="logo-url">
      <img id="logo" :src="logo" :alt="alt">
    </router-link>
    <router-link to="/">Home</router-link>
    <router-link to="/pedidos">Pedidos</router-link>
  </div>
</template>

<script>
export default {
  name: "Navbar",
  props: ["logo", "alt"]
}
</script>

<style scoped>
#nav {
  background-color: #222;
  border-bottom: 4px solid #111;
  padding: 15px 50px;
  display: flex;
  justify-content: flex-end;
  align-content: center;
}

#logo-url {
  margin: auto auto auto 0 !important;
}

#logo {
  width: 40px;
  height: 40px;
}

#nav a {
  color: #FCBA03;
  text-decoration: none;
  margin: 12px;
  transition: 0.5s;
}

#nav a:hover {
  color: #fff;
}
</style>