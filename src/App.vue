<template>
  <!-- <nav>
    <router-link to="/">Home</router-link> |
    <router-link to="/about">About</router-link>
  </nav> -->
  <router-view/>
</template>

<style>
*{
  margin:0;
  padding:0;
  box-sizing: border-box;
  font-family: Avenir, Helvetica, Arial, sans-serif;

}
body{
  background: #2c3e50;
  color:#fff;
}
a{
  text-decoration: none;
  color: #2aeb74;
}
button{
  appearance: none;
  border: none;
  background: none;
  cursor: pointer;

  background-color: #2aeb74;
  font-size: 1.125rem;
  color: #030e1b;
  padding:0.5rem 1rem;
  border-radius: 0.5rem;
}

</style>
