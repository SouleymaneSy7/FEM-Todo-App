<template>
  <main class="container" :class="theme === true ? 'darkTheme' : 'lightTheme'">
    <TodoList @toggle-theme="toggleTheme" :class="{ darkTheme: theme }" />
  </main>
</template>

<script>
import TodoList from "./components/TodoList.vue";

export default {
  name: "App",
  components: {
    TodoList,
  },
  data() {
    return {
      theme: false,
    };
  },
  methods: {
    toggleTheme() {
      this.theme = !this.theme;
    },
  },
};
</script>

<style lang="scss">
*,
*::before,
*::after {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  font-family: "Josefin Sans", sans-serif;
}

// Colors
$very-light-gray: hsl(0, 0%, 98%);
$very-dark-blue: hsl(235, 21%, 11%);
$bright-blue: hsl(220, 98%, 61%);

html {
  scroll-behavior: smooth;
  scroll-padding-top: 3.75rem;

  &::-webkit-scrollbar {
    width: 0.3125rem;
  }

  &::-webkit-scrollbar-track {
    background: transparent;
  }

  &::-webkit-scrollbar-thumb {
    background: $bright-blue;
    border-radius: 0.9375rem;
  }
}
scrollbar {
  width: 0.3125rem;
}

scrollbar-track {
  background: transparent;
}

scrollbar-thumb {
  background: $bright-blue;
  border-radius: 0.9375rem;
}

.container {
  position: relative;

  width: 100%;
  min-height: 100vh;

  display: flex;
  justify-content: center;

  background: $very-light-gray;

  padding: 5rem 1.25rem 0;
  overflow: hidden;
}

.container::before {
  content: "";
  position: absolute;
  top: 0;
  left: 0;

  width: 100%;
  height: 17.5rem;

  background-image: url(./assets/bg-mobile-light.jpg);
  background-repeat: no-repeat;
  background-size: cover;
}

// ============
// Dark Theme
// ============

.container.darkTheme {
  background: $very-dark-blue;
}

.container.darkTheme::before {
  background-image: url(./assets/bg-mobile-dark.jpg);
}

// ==========================
//    Media Queries Desktop
// ==========================

@media screen and (min-width: 510px) {
  .container::before {
    background-image: url(./assets/bg-desktop-light.jpg);
    background-repeat: no-repeat;
    background-size: cover;
  }

  .container.darkTheme::before {
    background-image: url(./assets/bg-desktop-dark.jpg);
    background-repeat: no-repeat;
    background-size: cover;
  }
}
</style>
