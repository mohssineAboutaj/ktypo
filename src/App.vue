<template>
  <nav class="navbar bg-base-100 fixed">
    <span class="text-xl mx-auto">{{ title }}</span>
  </nav>
  <main class="container mx-auto my-10 pt-16">
    <h1 class="text-4xl font-bold">Hello World!</h1>
    <p class="text-lg">
      Lorem ipsum dolor sit amet consectetur adipisicing elit. Quisquam,
      voluptatum.
    </p>
  </main>
</template>

<script setup>
// data
/// static
const title = require("../package.json").name
</script>
