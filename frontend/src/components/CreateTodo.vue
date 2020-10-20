<template>
  <div class="todoWrapper">
    <h3 class="todoWrapper__title">hello! co dzisiaj musisz zrobić?</h3>
    <form class="todoWrapper__form" @submit.prevent>
      <div class="form__row">
        <input
          type="text"
          class="form__controll"
          placeholder="dzisiaj zrobię..."
          v-model="name"
          @keyup.enter="addTodo($event)"
        />
        <button class="form__button" @click="addTodo()">dodaj</button>
      </div>
    </form>
  </div>
</template>
<script>
import axios from "axios";
import bus from "./../bus.js";

export default {
  data() {
    return {
      name: ""
    };
  },
  methods: {
    addTodo(event) {
      if (event) event.preventDefault();
      if (this.name.length === 0) return;
      let todo = {
        name: this.name,
        done: false //false by default
      };
      console.log(todo);
      this.$http
        .post("/", todo)
        .then(response => {
          this.clearTodo();
          this.refreshTodo();
        })
        .catch(error => {
          console.log(error);
        });
    },

    clearTodo() {
      this.name = "";
    },

    refreshTodo() {
      bus.$emit("refreshTodo");
    }
  }
};
</script>
<style lang="scss" scoped>
  .todoWrapper {
    &__title {
      margin: 50px auto;
      margin-bottom: 0px;
    }

    .form {
      &__row {
        margin: 10px;
      }

      &__controll {
        padding: 5px;
      }

      &__button {
        border: 0px;
        padding:10px;
        margin: 10px;
        cursor: pointer;
      }
    }
  }
</style>
