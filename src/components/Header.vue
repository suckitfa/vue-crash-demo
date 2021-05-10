<template>
    <header>
        <h1>{{title}}</h1>
        <Button v-show="homePage"
            :text=" showAddTask? 'Add Task' :'close' " 
            :color="showAddTask? 'green' :'red' " 
            @btn-click="$emit('toggle-add-task')"/>
    </header>
</template>
<script>
import Button from './Button'
export default {
    name:"Header",
    props:{
        title:{
            type:String,
            default:"Hello World"
        },
        showAddTask:Boolean,
    },
    components:{
        Button
    },
    computed:{
        homePage() {
            console.log(this.$router.path);
            if(this.$router.path === '/'){
                return true;
            } else {
                return false;
            }
        }
    }
}
</script>
<style scoped>
    header {
        display: flex;
        justify-content: space-between;
        align-items: center;
        margin-bottom: 20px;
    }
</style>