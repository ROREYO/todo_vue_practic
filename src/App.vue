<template>
  <div
    class="body"
    :class="{ dark: theme === 'dark', light: theme === 'light' }"
  >
    <div class="head">
      <div class="wrapper">
        <a href="https://t.me/rey_0o0" class="tg_title">rey_0o0</a>
        <form class="form_inline">
          <select class="select" v-model="theme">
            <option value="dark">Theme DARK</option>
            <option value="light">Theme LIGHT</option>
          </select>
        </form>
      </div>
    </div>

    <header>
      <h1>ToDo LIST</h1>
      <form class="form_title" @submit.prevent="addTask">
        <input
          ref="input"
          v-model="taskValue"
          type="text"
          class="task-new-input"
          placeholder="What do you have planned?"
        />
        <input type="submit" class="submit-new-input" value="Add task" />
      </form>
    </header>

    <main>
      <h2>You tasks</h2>
      <ul class="tasks" v-if="taskList.length > 0">
        <li class="task" v-for="item in taskList" :key="item.id">
          <div class="content">
            <input
              type="text"
              class="text"
              :class="{ done: item.done }"
              :readonly="item.readonly"
              ref="task"
              v-model="item.value"
            />
          </div>
          <div class="actions">
            <button class="edit" @click="() => editTask(item.id)">
              {{ item.readonly ? "EDIT" : "SAVE" }}
            </button>
            <button class="done" @click="() => doneTask(item.id)">DONE</button>
            <button class="delete" @click="() => deleteTask(item.id)">
              DELETE
            </button>
          </div>
        </li>
      </ul>
      <div v-else>Список пуст</div>
    </main>
  </div>
</template>

<script>
export default {
  data() {
    return {
      taskValue: "",
      taskList: [],
      theme: "dark",
    };
  },
  methods: {
    addTask() {
      if (this.taskValue === "") return;

      this.taskList.push({
        id: Date.now(),
        value: this.taskValue,
        readonly: true,
        done: false,
      });
      this.taskValue = "";
      this.$refs.input.focus();
    },
    deleteTask(id) {
      this.taskList = this.taskList.filter((item) => item.id !== id);
    },
    doneTask(id) {
      const index = this.taskList.findIndex((item) => item.id === id);
      if (index > -1) {
        this.taskList[index].done = !this.taskList[index].done;
      }
    },
    editTask(id) {
      const index = this.taskList.findIndex((item) => item.id === id);
      if (index > -1) {
        this.taskList[index].readonly = !this.taskList[index].readonly;
        this.$refs.task[index].focus();
      }
    },
  },
};
</script>

<style>
* {
  padding: 0;
  margin: 0;
  border: 0;
}
*,
*:before,
*:after {
  -moz-box-sizing: border-box;
  -webkit-box-sizing: border-box;
  box-sizing: border-box;
}
:focus,
:active {
  outline: none;
}
a:focus,
a:active {
  outline: none;
}
nav,
footer,
header,
aside {
  display: block;
}
html,
body {
  height: 100%;
  width: 100%;
  font-size: 100%;
  line-height: 1;
  font-size: 14px;
  -ms-text-size-adjust: 100%;
  -moz-text-size-adjust: 100%;
  -webkit-text-size-adjust: 100%;
}
input,
button,
textarea {
  font-family: inherit;
  background: none;
  font-size: inherit;
  font-weight: inherit;
  transition: inherit;
}
input::-ms-clear {
  display: none;
}
button {
  cursor: pointer;
}
button::-moz-focus-inner {
  padding: 0;
  border: 0;
}
a,
a:visited {
  text-decoration: none;
}
a:hover {
  text-decoration: none;
}
ul li {
  list-style: none;
}
img {
  vertical-align: top;
}
h1,
h2,
h3,
h4,
h5,
h6 {
  font-size: inherit;
  font-weight: inherit;
}

/* Обнуление */

.dark {
  --dark: #374151;
  --darked: #1f2937;
  --darkest: #111827;
  --gray: #6b7280;
  --pink: #ec4899;
  --purple: #885cf6;
  --light: #eee;
  --done: #2e8b57;
  --delete: #dc143c;
}
.light {
  --dark: #ececec;
  --darked: #a8ccff;
  --darkest: #4a83ff;
  --gray: #000000;
  --pink: #696969;
  --purple: #3d3d3d;
  --light: #eee;
  --done: #11f769;
  --delete: #ff1c49;
}

.body {
  display: flex;
  flex-direction: column;
  flex-grow: 1;
  font-family: "Signika Negative", sans-serif;
  background-color: var(--dark);
  color: var(--light);
  margin: 0;
  box-sizing: border-box;
  height: 100vh;
}
.wrapper {
  display: flex;
  max-width: 1024px;
  margin: 0 auto;
  justify-content: space-between;
  padding: 10px 0;
}

.head {
  background-color: var(--darked);
  opacity: 0.9;
}
.tg_title {
  color: var(--gray);
  font-weight: 700;
}
.select {
  font-family: "Signika Negative", sans-serif;
  background-color: var(--light);
  padding: 0px 10px;
}

header {
  padding: 50px 50px;
  max-width: 500px;
  width: 100%;
  margin: 0 auto;
}
h1 {
  font-size: 25px;
  color: var(--gray);
  padding-bottom: 5px;
}
.form_title {
  display: flex;
  justify-content: space-between;
}
.task-new-input {
  width: 100%;
  padding: 10px;
  background-color: var(--darked);
  color: var(--light);
  border-radius: 10px;
  margin-right: 10px;
}
.task-new-input::placeholder {
  color: var(--gray);
}
.submit-new-input {
  font-weight: 700;
  text-transform: uppercase;
  background-image: linear-gradient(to right, var(--pink), var(--purple));
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  transition: 0.4s;
  cursor: pointer;
}
.submit-new-input:hover {
  opacity: 0.8;
}
.submit-new-input:active {
  opacity: 0.5;
}

main {
  padding: 50px;
  max-width: 500px;
  width: 100%;
  margin: 0 auto;
}
h2 {
  font-size: 23px;
  color: var(--gray);
  padding-bottom: 5px;
}
.task {
  display: flex;
  justify-content: space-between;
  background-color: var(--darkest);
  padding: 10px;
  border-radius: 10px;
  margin-bottom: 10px;
}
.content {
  flex: 1 1 0%;
  margin-right: 10px;
}
.text {
  color: var(--light);
  width: 100%;
}
.text:not(:read-only) {
  color: var(--pink);
}
.actions {
  display: flex;
  font-size: 15px;
  font-weight: 700;
  transition: 0.4s;
  gap: 10px;
}
.edit {
  background-image: linear-gradient(to right, var(--pink), var(--purple));
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
}
.done {
  color: var(--done);
}
.delete {
  color: var(--delete);
}
.edit:hover,
.done:hover,
.delete:hover {
  opacity: 0.8;
}
.edit:active,
.done:active,
.delete:active {
  opacity: 0.5;
}
</style>
