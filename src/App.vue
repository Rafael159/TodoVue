<template>
  <div class="main">
    <h2>Things to do</h2>

    <form @submit.prevent="addTodo" class="form-addTodo">
      <input class="input-group" name="field-addTodo" v-model="item" autocomplete="off">
      <button class="btn btn-success" name="btn-addTodo">Add item +</button>
    </form>

    <div class="checklist">
      <h3>What do I have to do today?</h3>
      <span v-if="!listTodo.length">You don't have anything to do today. You are free to go :) Act wisely!</span><br>
      <div class="actions">
        <div class="doneGroup group">
          <span v-if="listTodo.length">Set all itens as done</span><br>
          <button class="btnAllDone btn" @click="allDone">Add all</button>
        </div>
        <div class="doneGroup group">
          <span v-if="listTodo.length">Set all itens as undone</span><br>
          <button class="btnAllUndone btn" @click="allUndone">Remove all</button>
        </div>
      </div>
      
      <ul class='checklist-container'>
        <li class="checklist-item" v-for="todo in listTodo" :key="todo.id">
            <label :for="'item_' + todo.id">
              <input type="checkbox" :id="'item_' + todo.id" v-model="todo.done"> <span :class="{ done : todo.done}">{{ todo.title }}</span>
              <button class="removeBtn" name="removeTodo" @click="removeTodo(todo)">Remove item</button>
            </label>
        </li>
      </ul>
    </div>
  </div>
</template>

<script>

export default {
  name: 'Checklist',
  data() {
    return {
      item: '',
      counter: '',
      listTodo: [],
      listFromDB: []
    }
  },
  methods: {
    addTodo() {
      console.log(this.listTodo.length);
      if (this.item) {

        this.counter = this.listTodo.length;
        console.log(this.counter);

        this.listTodo.push({
          id: this.counter + 1,
          title: this.item,
          done: false
        })
        this.item = '';
        this.counter = this.counter + 1;
      }
    },
    removeTodo(todo) {
      var answer = true;

      if (todo.done) {
        answer = window.confirm(todo.title + " is set as done. Are you sure you want to remove it?");
      }

      if (!answer) return; 

      const todoIndex = this.listTodo.indexOf(todo);
      this.listTodo.splice(todoIndex, 1);
    },
    allDone() {
      this.listTodo.forEach(item => {
        item.done = true;
      });
    },
    allUndone() {
      this.listTodo.forEach(item => {
        item.done = false;
      });
    }
  },
  created() {
    // Get the saved list from the database.
    // Connect to a PHP controller that would take the data and return.
    this.listFromDB = [
      {
        id: 1,
        title: 'Wash the car',
        done: false
      },
      {
        id: 2,
        title: 'Study Java',
        done: true
      },
      {
        id: 3,
        title: 'Study Javascript',
        done: true
      },
    ];

    this.listTodo = this.listFromDB;
  }
}
</script>

<style>
body {
  background: #007ea6;
}
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
.main h2 {
  color: #fff;
}
.form-addTodo input {
  padding: 10px;
  color: #333;
  border-radius: 3px;
  width: 40%;
  border: none;
  font-size: 1.3em;
}

.form-addTodo button {
  background: #069;
  color: #fff;
  font-size: 1.3em;
  border: none;    
  padding: 10px;
  cursor: pointer;
}

.checklist {
  width: 50%;
  margin: 35px auto;
  background: #ffffff;
  padding: 15px;
  border-radius: 5px;
  box-shadow: 0px 10px 11px -10px #fff;
}

.checklist-container .checklist-item {
  width: 100%;
  list-style: none;
  color: #333;
  text-align: left;
  font-size: 1.2em;
  line-height: 3rem;
}
.checklist-container .checklist-item label {
    width: 100%;
    display: block;
    cursor: pointer;
}

.checklist-container .checklist-item label .done {
  text-decoration: line-through;
}

.removeBtn {
  background: #c84343;
  color: #fff;
  font-size: 1rem;
  border: none;
  padding: 10px;
  cursor: pointer;
  border-radius: 5px;
  float: right;
}
.btn {
  color: #fff;
  font-size: 1rem;
  border: none;
  padding: 10px;
  cursor: pointer;
  border-radius: 5px;
}
.btnAllDone {
  background: #197e19;
}

.btnAllUndone {
  background: #c84343;
}

.actions {
  width: 100%;
}

.actions .group {
  display: inline-block;
  width: 50%;
}
</style>
