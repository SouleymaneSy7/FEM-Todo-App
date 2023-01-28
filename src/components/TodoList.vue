<template>
  <section class="todo">
    <div class="todo__header">
      <h1>Todo</h1>

      <a
        href="#"
        title="Theme Switcher"
        class="todo__toggle--btn"
        @click="onClick()"
      ></a>
    </div>

    <form class="todo__form" @submit.prevent="submitedTask">
      <span for="todo__input" class="todo__input-icon">
        <input
          type="text"
          class="todo__input"
          placeholder="Create a new todo..."
          v-model="task"
        />
      </span>
    </form>

    <main class="todos-container">
      <div
        class="todo__main"
        v-for="(task, index) in filteredTodos"
        :key="index"
      >
        <div class="todo__item" :class="{ complete: task.status }">
          <label class="todo__checkbox">
            <input type="checkbox" v-model="task.status" />
            <div class="todo__check"></div>
          </label>

          <p class="todo__text">{{ task.name }}</p>

          <div @click="deleteTask(index)" class="todo__delete-icon"></div>
        </div>
      </div>

      <div class="todo__footer" v-show="tasks.length > 0">
        <div class="items__left">
          <p>{{ remainedTodos }} items left</p>
        </div>

        <div class="todo__filter">
          <button
            class="all__filter"
            :class="{ selected: filter == 'all' }"
            @click.prevent="filter = 'all'"
          >
            All
          </button>
          <button
            class="active__filter"
            :class="{ selected: filter == 'active' }"
            @click.prevent="filter = 'active'"
          >
            Active
          </button>
          <button
            class="complete__filter"
            :class="{ selected: filter == 'complete' }"
            @click.prevent="filter = 'complete'"
          >
            Complete
          </button>
        </div>

        <div class="clear__all" @click.prevent="deleteCompleted()">
          <button class="clear__all--btn">Clear Completed</button>
        </div>
      </div>
    </main>

    <div class="tips" v-show="tasks.length > 0">
      <p>Drag and drop to reorder list</p>
    </div>

    <div class="attribution">
      <p>
        Challenge by
        <a href="https://www.frontendmentor.io?ref=challenge" target="_blank"
          >Frontend Mentor</a
        >
      </p>

      <p>
        Coded <span>♥</span> by
        <a href="https://github.com/SouleymaneSy7" target="_blank"
          >Souleymane Sy</a
        >
      </p>
    </div>
  </section>
</template>

<script>
export default {
  name: "TodoList",
  data() {
    return {
      task: "",
      tasks: [],
      filter: "all",
    };
  },
  methods: {
    submitedTask() {
      if (this.task.length === 0) {
        alert("Opps, You have to write something in the field");
        return;
      }

      this.tasks.push({
        id: this.tasks.length,
        name: this.task,
        status: false,
      });
      this.task = "";
    },
    deleteTask(index) {
      this.tasks.splice(index, 1);
    },
    deleteCompleted() {
      this.tasks = this.tasks.filter((task) => !task.status);
    },
    // Theme Switcher
    onClick() {
      this.$emit("toggle-theme");
    },
  },
  computed: {
    remainedTodos() {
      return this.tasks.filter((task) => !task.status).length;
    },

    filteredTodos() {
      if (this.filter === "active") {
        return this.tasks.filter((task) => !task.status);
      } else if (this.filter === "complete") {
        return this.tasks.filter((task) => task.status);
      }
      return this.tasks;
    },
  },
};
</script>

<style lang="scss" scoped>
// Colors
$lightTheme-bright-blue: hsl(220, 98%, 61%);
$check-background-from: hsl(192, 100%, 67%);
$check-background-to: hsl(280, 87%, 65%);

// Light Theme Colors
$lightTheme-very-light-gray: hsl(0, 0%, 98%);
$lightTheme-very-light-grayish-blue: hsl(236, 33%, 92%);
$lightTheme-light-grayish-blue: hsl(233, 11%, 84%);
$lightTheme-dark-grayish-blue: hsl(236, 9%, 61%);
$lightTheme-very-dark-grayish-blue: hsl(235, 19%, 35%);

// Dark Theme Colors
$very-dark-blue: hsl(235, 21%, 11%);
$very-dark-desaturated-blue: hsl(235, 24%, 19%);
$light-grayish-blue: hsl(234, 39%, 85%);
$light-grayish-blue-hover: hsl(236, 33%, 92%);
$dark-grayish-blue: hsl(234, 11%, 52%);
$very-dark-grayish-blue: hsl(233, 14%, 35%);
$very-dark-grayish-blue-2: hsl(237, 14%, 26%);

.todo {
  width: 100%;
  max-width: 34.375rem;
  position: relative;
  z-index: 100;
}

.todo__header {
  display: flex;
  justify-content: space-between;
  align-items: center;

  margin-bottom: 2.5rem;

  & h1 {
    font-size: 1.8125rem;
    font-weight: 700;
    color: $lightTheme-very-light-gray;
    text-transform: uppercase;
    letter-spacing: 0.3125rem;
  }

  & .todo__toggle--btn {
    border: none;
    outline: none;
    background: none;

    width: 1.6875rem;
    height: 1.6875rem;

    background: url(../assets/icon-moon.svg);
    background-size: contain;

    transition: 300ms ease;
    cursor: pointer;
  }
}

.todo__form {
  width: 100%;

  & .todo__input-icon {
    position: relative;

    &::before {
      content: "";
      position: absolute;
      top: 50%;
      left: 1.875rem;
      transform: translate(-50%, -50%);
      display: inline-block;
      width: 1.25rem;
      height: 1.25rem;
      border-radius: 50%;
      border: 1px solid $lightTheme-very-light-grayish-blue;

      z-index: 1;
    }
  }

  & .todo__input {
    position: relative;
    min-width: 100%;
    height: 3.25rem;

    border: none;
    outline: none;

    font-family: inherit;
    font-size: 1.0625rem;
    font-weight: 500;
    color: $lightTheme-very-dark-grayish-blue;

    box-shadow: 0 0.3125rem 1.25rem
      rgba($color: $lightTheme-very-dark-grayish-blue, $alpha: 0.5);
    border-radius: 0.3125rem;
    padding: 0 0.9375rem 0 3.75rem;
    margin-bottom: 1.25rem;

    &::placeholder {
      font-family: inherit;
      font-size: 0.8125rem;
      font-weight: 500;
      color: $lightTheme-dark-grayish-blue;
    }
  }
}

.todos-container {
  box-shadow: 0 0.625rem 2.5rem
    rgba($color: $lightTheme-very-dark-grayish-blue, $alpha: 0.5);
  border-radius: 0.3125rem;
}

.todo__main {
  min-width: 100%;
  background: $lightTheme-very-light-gray;
  cursor: pointer;
  overflow: hidden;
}

.todo__item {
  display: flex;
  align-items: center;
  justify-content: space-between;

  width: 100%;
  height: 3.375rem;

  border-bottom: 1px solid $lightTheme-very-light-grayish-blue;

  padding: 0 1.25rem;

  & .todo__checkbox {
    position: relative;
    margin-right: 1.25rem;
    width: 1.25rem;
    height: 1.25rem;

    border: none;
    outline: none;

    border: 2px solid $lightTheme-very-light-grayish-blue;
    border-radius: 50%;
    transition: background-image 500ms ease;
    cursor: pointer;

    &:hover {
      border: none;
      background-image: linear-gradient(
        145deg,
        $check-background-from,
        $check-background-to
      );
    }

    & input {
      width: 100%;
      height: 100%;
      -webkit-appearance: none;
      appearance: none;
      opacity: 0;
    }

    & .todo__check {
      position: absolute;
      top: 50%;
      left: 50%;
      transform: translate(-50%, -50%);

      display: flex;
      justify-content: center;
      align-items: center;

      width: 1.125rem;
      height: 1.125rem;
      background: $lightTheme-very-light-gray;
      border-radius: 50%;
      transition: background-image 300ms ease-out;
    }

    & input:checked + .todo__check {
      width: 1.25rem;
      height: 1.25rem;
      background-image: linear-gradient(
        145deg,
        $check-background-from,
        $check-background-to
      );

      &::before {
        content: "";
        width: 0.625rem;
        height: 0.625rem;
        background-color: $lightTheme-very-light-gray;
        transform-origin: center center;
        clip-path: polygon(14% 44%, 0 65%, 50% 100%, 100% 16%, 80% 0%, 43% 62%);
      }
    }
  }

  & .todo__text {
    flex-grow: 1;
    font-size: 0.8125rem;
    font-weight: 500;
    color: $lightTheme-very-dark-grayish-blue;
  }

  & .todo__delete-icon {
    width: 0.9375rem;
    height: 0.9375rem;

    background-image: url(../assets/icon-cross.svg);
    background-size: contain;
    background-repeat: no-repeat;
    background-position: center center;
    transition: 300ms ease;

    cursor: pointer;
  }

  &.complete .todo__text {
    text-decoration: line-through;
    color: $lightTheme-light-grayish-blue;
  }
}

.todo__main:nth-child(1) {
  border-top-left-radius: 0.3125rem;
  border-top-right-radius: 0.3125rem;
}

.todo__footer {
  position: relative;
  min-width: 100%;
  height: 3.25rem;

  display: flex;
  justify-content: space-between;
  align-items: center;

  background: $lightTheme-very-light-gray;
  border-bottom-left-radius: 0.3125rem;
  border-bottom-right-radius: 0.3125rem;

  padding: 0 1.25rem;

  & .items__left {
    font-size: 0.8125rem;
    font-weight: 500;
    color: $lightTheme-dark-grayish-blue;
  }

  & .todo__filter {
    position: absolute;
    bottom: -4.375rem;
    left: 0;
    width: 100%;
    height: 3rem;

    display: flex;
    justify-content: center;
    align-items: center;

    border-radius: 0.3125rem;
    background: $lightTheme-very-light-gray;
    box-shadow: 0.9375rem 0.9375rem 2.5rem
      rgba($color: $lightTheme-very-dark-grayish-blue, $alpha: 0.3);

    & button {
      border: none;
      outline: none;
      background: none;

      font-family: inherit;
      font-size: 0.9375rem;
      font-weight: 700;
      color: $lightTheme-dark-grayish-blue;

      margin: 0 0.9375rem;
      transition: color 300ms ease;
      cursor: pointer;

      &:hover,
      &:focus {
        color: $lightTheme-very-dark-grayish-blue;
      }

      &.selected {
        color: $lightTheme-bright-blue;
      }
    }
  }

  & .clear__all .clear__all--btn {
    border: none;
    outline: none;
    background: none;

    font-family: inherit;
    font-size: 0.8125rem;
    font-weight: 500;
    color: $lightTheme-dark-grayish-blue;

    transition: color 300ms ease;
    cursor: pointer;

    &:hover,
    &:focus {
      color: $lightTheme-very-dark-grayish-blue;
    }
  }
}

.tips {
  display: flex;
  justify-content: center;

  margin-top: 7.5rem;

  & p {
    font-size: 0.9375rem;
    font-weight: 500;
    color: $lightTheme-dark-grayish-blue;
  }
}

.attribution {
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;
  margin-top: 9.375rem;
  margin-bottom: 2.5rem;

  & p {
    font-size: 1.0625rem;
    font-weight: 500;
    color: $lightTheme-dark-grayish-blue;
  }

  & p span {
    font-size: 1.5rem;
    margin: 0 0.1875rem;
    background: linear-gradient(
      to bottom,
      $check-background-from,
      $check-background-to
    );
    background-clip: text;
    -webkit-text-fill-color: transparent;
  }

  & p a {
    font-size: 1.125rem;
    font-weight: 700;
    background: linear-gradient(
      to right,
      $check-background-from,
      $check-background-to
    );
    background-clip: text;
    -webkit-text-fill-color: transparent;
    text-transform: capitalize;
    text-decoration: none;
  }
}
// ===================
//    Dark Theme
// ===================

.todo.darkTheme {
  & .todo__toggle--btn {
    background: url(../assets/icon-sun.svg);
    background-size: contain;

    cursor: pointer;
  }
  .todo__form {
    & .todo__input-icon {
      &::before {
        content: "";
        border: 1px solid $very-dark-grayish-blue;
      }
    }
    .todo__input {
      color: $light-grayish-blue;
      background: $very-dark-desaturated-blue;
      box-shadow: 0 10px 40px rgba($color: $very-dark-blue, $alpha: 0.5);
      &::placeholder {
        color: $dark-grayish-blue;
      }
    }
  }

  .todos-container {
    box-shadow: 0 0.625rem 2.5rem rgba($color: $very-dark-blue, $alpha: 0.5);
  }

  .todo__main {
    background: $very-dark-desaturated-blue;
  }

  .todo__item {
    border-bottom: 1px solid $very-dark-grayish-blue-2;

    & .todo__checkbox {
      border: 2px solid $very-dark-grayish-blue-2;

      &:hover {
        border: none;
        background-image: linear-gradient(
          145deg,
          $check-background-from,
          $check-background-to
        );
      }

      & .todo__check {
        background: $very-dark-desaturated-blue;
      }

      & input:checked + .todo__check {
        border: none;
        background-image: linear-gradient(
          145deg,
          $check-background-from,
          $check-background-to
        );

        &::before {
          background-color: $lightTheme-very-light-grayish-blue;
        }
      }
    }

    & .todo__text {
      color: $light-grayish-blue;
    }

    &.complete .todo__text {
      color: $very-dark-grayish-blue;
    }
  }

  .todo__footer {
    background: $very-dark-desaturated-blue;
    & .items__left {
      color: $dark-grayish-blue;
    }

    & .todo__filter {
      background: $very-dark-desaturated-blue;
      box-shadow: 0 0.625rem 2.5rem rgba($color: $very-dark-blue, $alpha: 0.9);

      & button {
        color: $dark-grayish-blue;

        &:hover,
        &:focus {
          color: $light-grayish-blue-hover;
        }

        &.selected {
          color: $lightTheme-bright-blue;
        }
      }
    }

    & .clear__all .clear__all--btn {
      color: $dark-grayish-blue;
      &:hover,
      &:focus {
        color: $light-grayish-blue-hover;
      }
    }
  }

  .tips {
    & p {
      color: $dark-grayish-blue;
    }
  }
}

// =============================
// Media Queries for Desktop
// =============================

@media screen and (min-width: 31.875rem) {
  .todo__header h1 {
    font-size: 2.5rem;
    letter-spacing: 0.625rem;
  }

  .todo__form .todo__input-icon {
    margin-bottom: 0.3125rem;
  }
  .todo__form .todo__input-icon::before {
    width: 1.5rem;
    height: 1.5rem;
    top: 40%;
  }
  .todo__form .todo__input {
    font-size: 1.25rem;
    height: 3.75rem;

    &::placeholder {
      font-family: inherit;
      font-size: 1.0625rem;
    }
  }
  .todo__main .todo__item .todo__delete-icon {
    background: none;
    cursor: default;
  }

  .todo__item:hover .todo__delete-icon {
    width: 0.9375rem;
    height: 0.9375rem;

    background-image: url(../assets/icon-cross.svg);
    background-size: contain;
    background-repeat: no-repeat;
    background-position: center center;

    cursor: pointer;
  }

  .todo__item {
    height: 3.75rem;

    .todo__checkbox {
      width: 1.5rem;
      height: 1.5rem;

      & input:checked + .todo__check {
        width: 1.5rem;
        height: 1.5rem;
      }
    }

    .todo__text {
      font-size: 1.0625rem;
    }
  }

  .todo.darkTheme .todo__footer .todo__filter {
    box-shadow: none;
    background: transparent;
  }

  .todo__footer {
    height: 3.75rem;

    .items__left {
      font-size: 0.9375rem;
      font-weight: 500;
    }

    & .todo__filter {
      position: relative;
      top: 0;
      left: 0;
      width: auto;
      height: auto;

      box-shadow: none;
      background: transparent;
      border-radius: 0;
    }

    .clear__all .clear__all--btn {
      font-size: 0.9375rem;
      font-weight: 500;
    }
  }

  .tips {
    margin-top: 3.75rem;
  }

  .attribution {
    & p {
      font-size: 1.125rem;
    }

    & p a {
      font-size: 1.25rem;
    }
  }
}
</style>
