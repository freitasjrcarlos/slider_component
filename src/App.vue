<template>
  <div id="app">
    <main>
      <SliderComponent />
    </main>
  </div>
</template>

<script>
import SliderComponent from "@/components/SliderComponent.vue";

export default {
  components: {
    SliderComponent,
  },
  data() {
    return {};
  },
};
</script>

<style>
body,
h1,
h2,
h3,
h4,
h4,
h5,
ul,
li,
p {
  margin: 0;
  padding: 0;
}

ul {
  list-style: none;
}

* {
  box-sizing: border-box;
}

img {
  max-width: 100%;
  display: block;
}

#app {
  display: flex;
  flex-direction: column;
  min-height: 100vh;
}

main {
  flex: 1;
}
</style>
