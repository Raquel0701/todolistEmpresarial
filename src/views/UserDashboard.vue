<template>
  <div>
    <HeaderView />

    <div class="container-fluid d-flex flex-column pt-0 mt-5" style="height:85vh; overflow-y: scroll;">
      <div class="row flex-grow-1 mt-3 m-2">
        <!-- Usar el componente MenuView para mostrar el menú -->
        <MenuView :items="navigationItems" @selectItem="handleNavigation" class="col bg-light mt-2" />

        <div :class="['col-lg-7', !selectedTask ? 'col-lg-10' : 'col-lg-7']">
          <div v-show="activeNavItem === 'Todo'">
            <TaskForm @show-task-detail="handleShowTaskDetail" @task-added="handleTaskAdded" @update-task="actualizar" />
            <b-card class="mt-1">
              <TaskList :tasks="tasks" @edit-task="handleEditTask" @edit-task-hidden="handleEditTaskHidden"
                :menu="activeNavItem" :selectedTask="selectedTask" :refresh="refresh" />
              <!-- {{ showTaskDetail }} -->
            </b-card>
          </div>
          <div v-show="activeNavItem === 'Important'">
            <b-card class="mt-3">
              <TaskList :tasks="tasks" @edit-task="handleEditTask" @edit-task-hidden="handleEditTaskHidden"
                :menu="activeNavItem" :selectedTask="selectedTask" :refresh="refresh" />
            </b-card>
          </div>
          <div v-show="activeNavItem === 'All Tasks'">
            <b-card class="mt-3">
              <TaskList :tasks="tasks" @edit-task="handleEditTask" @edit-task-hidden="handleEditTaskHidden"
                :menu="activeNavItem" :selectedTask="selectedTask" :refresh="refresh" />
            </b-card>
          </div>
        </div>
        <div class="col-lg-3">
          <div v-if="selectedTask">
            <TaskListDetail :selectedTask="selectedTask" />
          </div>

        </div>
      </div>
    </div>

    <FooterView class="fixed-bottom bg-primary text-light"></FooterView>
  </div>
</template>

<script>
import TaskForm from '@/components/TaskForm';
import TaskListDetail from '@/components/TaskListDetail';
import TaskList from '@/components/TaskList.vue'; 
import HeaderView from './general/HeaderView';
import FooterView from './general/FooterView';
import MenuView from './general/MenuView';
import store from '@/store/index';
export default {
  components: {
    TaskForm,
    HeaderView,
    FooterView,
    MenuView,
    TaskList,
    TaskListDetail
  },
  data() {
    return {
      navigationItems: [
        { id: 1, name: 'Todo', icon: 'book', color: 'success' },
        { id: 2, name: 'Important', icon: 'exclamation-circle', color: 'warning' },
        { id: 3, name: 'All Tasks', icon: 'list-ul', color: 'secondary' }
      ],
      activeNavItem: 'Todo',
      showTaskDetail: false,
      selectedTask: null,
      tasks: [], 
      perPage: 10, 
      currentPage: 1, 
      tareas: [],
      refresh: false,
      store
    };
  },
  watch: {
    tasks(newTasks) {
      this.tasks = newTasks;
      this.internalPage = 1; 
    },
  },
  methods: {
    
    handleNavigation(selectedItem) {
      this.activeNavItem = selectedItem;
    },
    handleEditTask(task) {
      this.selectedTask = task; 
      this.showTaskDetail = true; 
    },
    handleEditTaskHidden() {
      this.selectedTask = null; 
      this.showTaskDetail = false; 
    },
    actualizar(updatedTasks) {
      this.tasks = updatedTasks; 
    },


    handleShowTaskDetail(task) {
      this.selectedTask = task; 


    },

    handleTaskAdded(updatedTasks) {
      this.tasks = updatedTasks; 
      this.refresh = true;
    },
  }

 
};
</script>


