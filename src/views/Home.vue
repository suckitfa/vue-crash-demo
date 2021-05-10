<template>
    <div v-if="showAddTask">
      <AddTask @add-task="addTask"/>
    </div>
    <Tasks :tasks="tasks"
    @delete-task="deleteTask"
    @toggle-reminder="toggleReminder"
    />
</template>
<script>
import AddTask from '../components/AddTask';
import Tasks from '../components/Tasks'
export default {
    name:"Home",
    components:{
        Tasks,
        AddTask
    },
    data() {
        return {
            tasks:[]
        }
    },
    props:{
        showAddTask:Boolean
    },
    methods:{
    async deleteTask(id) {
      // 弹出对话框，询问用户是否真的要删除该任务
      if (confirm("Are you sure?")) {
        const res = await fetch(`api/tasks/${id}`,{
          method:'DELETE',
        });
        res.status === 200 ? (this.tasks = this.tasks.filter((task=>task.id !== id))):alert('Eorro deleting tasks');
        // 使用filter函数，过滤符合条件的元素
        
      }
    },
    async toggleReminder(id) {
      const taskToggle = await this.fetchTasks(id);
      const updTask = {...taskToggle,reminder:!taskToggle.reminder};
      const res = await fetch(`api/tasks/${id}`,{
        method:"PUT",
        headers:{
          'Content-type':"application/json"
        },
        body:JSON.stringify(updTask)
      });
      const data = await res.json();
      this.tasks = this.tasks.map(task=> task.id === id ? {...task,reminder:!data.reminder}:task);
    },

    async addTask(newTask) {
      const res = await fetch(`api/tasks`,{
        method:"post",
        headers:{
          'Content-Type':"application/json",
        },
        body:JSON.stringify(newTask)
      });
      const data = await res.json();
      this.tasks = [...this.tasks,newTask];
    },
    async fetchTasks() {
      const res = await fetch("api/tasks");
      const data = await res.json();
      return data;
    },
    async fetchTaskById(id) {
      const res = await fetch(`api/tasks/${id}`);
      const data = await res.json();
      return data;  
    }
  },
  // 加载数据
  async created(){
    this.tasks = await this.fetchTasks();
  }  
}
</script>