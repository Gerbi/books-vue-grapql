<template>
  <div id="app" class="font-sans antialiased text-grey-lighter">
    <div id="nav" class="bg-grey-darkest">
      <div class="container flex items-center justify-between py-4">
        <div>
          <a href="/" class="text-white text-xl font-bold">Store Books</a>
        </div>
        <div>
          <router-link to="/" class="text-grey-light hover:text-white">Home</router-link>
          <router-link to="/about" class="text-grey-light hover:text-white pl-6">About</router-link>
        </div>
      </div>
    </div>

    <router-view/>
  </div>
</template>

<style src="./assets/main.css"></style>