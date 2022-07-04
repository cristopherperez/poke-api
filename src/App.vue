<template>
  <div class="routers">
    <router-link class="route-head" to="/">Pokemon Picker</router-link>
  </div>
  <router-view/>
</template>

<style scoped>
.routers{
  padding: 4rem;
}
.route-head{
  display: flex;
  justify-content: center;
  color: rgb(240, 166, 30);
  font-size: 20px;
}
</style>
