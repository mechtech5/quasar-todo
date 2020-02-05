<template>
  <q-page class="bg-grey-3 column">
    <div class="row q-pa-sm bg-primary">
      <q-input
        v-model="newTask"
        @keyup.enter="addTask"
        class="col"
        bg-color="white"
        placeholder="Add task"
        square
        filled
        dense>
        <q-btn
          @click="addTask"
          round 
          dense 
          flat 
          icon="add" />
      </q-input>
    </div>
    <q-list class="bg-white" separator bordered>
      <q-item 
        v-for="(todo, index) in todos" :key="index"
        @click="todo.completed = !todo.completed"
        :class="{'completed bg-blue-1': todo.completed}"
        clickable
        v-ripple >
        <q-item-section avatar>
          <q-checkbox 
            v-model="todo.completed"
            class="no-pointer-events"
            color="primary" />
        </q-item-section>
        <q-item-section>
          <q-item-label>{{ todo.title }}</q-item-label>
        </q-item-section>
        <q-item-section 
          v-if="todo.completed"
          side>
          <q-btn
            @click.stop="deleteTask(index)"
            flat
            round
            dense
            color="primary"
            icon="delete" 
          />
        </q-item-section>
      </q-item>
    </q-list>
    <div
      v-if="!todos.length"
      class="no-tasks absolute-center">
      <q-icon 
        name="check"
        size="100px"
        color="primary" />
      <div class="text-h5 text-primary text-center">
        No tasks
      </div> 
    </div>
  </q-page>
</template>

<script>
export default {
  data() {
    return {
      newTask: '',
      todos: [
        {
          title: 'todo 1',
          completed: true
        },
        {
          title: 'todo 2',
          completed: false
        },
        {
          title: 'todo 3',
          completed: true
        },
        
      ]
    }
  },
  methods: {
    addTask() {
      this.todos.push({
        title: this.newTask,
        completed: false
      });
      this.newTask = '';
    },
    deleteTask(index) {
      this.$q.dialog({
        title: 'Confirm',
        message: 'Are you sure?',
        cancel: true,
        persistent: true
      }).onOk(() => {
        this.todos.splice(index, 1);
        this.$q.notify('Task Deleted!');
      })
    }
  }
}
</script>

<style lang="scss">
  .completed {
    .q-item__label {
      text-decoration: line-through;
      color: #bbb;
    }
  }
  
</style>
