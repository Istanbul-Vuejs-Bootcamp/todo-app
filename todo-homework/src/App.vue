<template>
  <div id="app">
    <div class="container" ref="myContainer">
      <todoForm @onSubmit="saveFormData" ref="todoForm" />
      <div class="mt-3">
        <select v-model="selectedFilterValue">
          <template v-for="status in Object.keys(STATUSSES)">
            <option :value="STATUSSES[status].VALUE">
              {{ STATUSSES[status].TEXT }}
            </option>
          </template>
        </select>
        <todoItem
                v-for="(item, index) in filteredItems"
                :index="index"
                :title="item.title"
                :detail="item.detail"
                :status="item.status"
                @onUpdate="setUpdateForm"
        />
      </div>
    </div>
  </div>
</template>

<script>
  import {STATUSSES} from './statuses';
  import todoForm from "./components/todoForm";
  import todoItem from "./components/todoItem";

export default {
  name: 'App',
  components: {
    todoForm,
    todoItem
  },
  data() {
    return {
      STATUSSES,
      todoList: [],
      updatedIndex: null,
      selectedFilterValue: STATUSSES.ALL.VALUE
    }
  },
  methods: {
    saveFormData(data) {
      if(this.updatedIndex !== null) {
        this.todoList.splice(this.updatedIndex, 1, data);
      } else {
        this.todoList.push(data);
      }
    },
    setUpdateForm(index) {
      this.updatedIndex = index;
      this.$refs.todoForm.isUpdate = true;
      this.$refs.todoForm.title = this.todoList[index].title;
      this.$refs.todoForm.detail = this.todoList[index].detail;
      this.$refs.todoForm.status = this.todoList[index].status;
    },
  },
  computed: {
    filteredItems() {
      if (this.selectedFilterValue === STATUSSES.ALL.VALUE) {
        return this.todoList;
      } else {
        return this.todoList.filter((item) => item.status === this.selectedFilterValue);
      }
    }
  },
  mounted() {
  }
}
</script>
