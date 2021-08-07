<template>
   <div id="app" class="container my-3">
    <div class="input-group mb-3">
      <span class="input-group-text">待辦事項</span>
      <input type="text" class="form-control" placeholder="準備要做的任務"
        id="newTodo" v-model="newTodo" @keyup.enter="addItem">
      <button class="btn btn-primary" type="button" id="addTodo" @click="addItem">新增</button>
    </div>
    <div class="card text-center">
      <ul class="list-group list-group-flush text-left" id="todoList">
        <li  class="list-group-item" v-for="item in filterData" :key="item.id">
          <div class="d-flex">
            <div class="form-check">
              <input
                class="form-control"
                type="text"
                v-model="tempTitle"
                @keyup.enter="done"
                 v-if="tempId === item.id"
              />
              <div v-else>
                <input
                  :id="item.id"
                  type="checkbox"
                  class="form-check-input"
                  v-model="item.checked"
                >
                <label
                  :for="item.id"
                  :class="{ 'completed': item.checked }"
                  @dblclick="editItem(item)"
                >
                  {{ item.title }}
                </label>
              </div>
            </div>
            <button type="button" class="btn-close ms-auto remove" @click="removeItem(item)">
            </button>
          </div>
        </li>
      </ul>
      <div class="card-footer d-flex justify-content-between">
        <span>有 <span id="taskCount">{{ filterData.length }}</span> 筆任務</span>
        <div>
          <a href="#" class="me-3" @click.prevent="filterStatus = 'all'">全部</a>
          <a href="#" class="me-3" @click.prevent="filterStatus = 'active'">未完成</a>
          <a href="#" class="me-3" @click.prevent="filterStatus = 'completed'">已完成</a>
          <a href="#" @click="removeAll">清除所有任務</a>
        </div>
      </div>
    </div>
  </div>
</template>

<script>

export default {
  name: 'Home',
  data() {
    return {
      newTodo: '',
      todoList: [],
      tempTitle: '',
      tempId: '',
      filterStatus: '',
    };
  },
  computed: {
    filterData() {
      if (this.filterStatus === 'active') {
        return this.todoList.filter(el => el.checked === false);
      }
      if (this.filterStatus === 'completed') {
        return this.todoList.filter(el => el.checked !== false);
      }
      return this.todoList;
    },
  },
  methods: {
    addItem() {
      const item = {
        id: Date.now(),
        title: this.newTodo,
        checked: false,
        showInput: false,
      };
      this.todoList.push(item);
      this.newTodo = '';
    },
    removeItem(item) {
      const index = this.todoList.findIndex((el) => el.id === item.id);
      this.todoList.splice(index, 1);
    },
    editItem(item) {
      this.tempTitle = item.title;
      this.tempId = item.id;
    },
    done() {
      this.todoList.forEach((el) => {
        if (el.id === this.tempId) {
          el.title = this.tempTitle;
        }
      });
      this.tempId = '';
    },
    removeAll() {
      this.todoList = [];
    },
  },
};
</script>
<style>
  .completed {
    text-decoration: line-through
  }
</style>
