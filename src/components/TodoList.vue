<template>
    <div class="task-container">
        <h1 style="color: gray;">Today's Task</h1>
        <p>Check Marked todos:{{ checkMarkedtodos }}</p>
        <p>To do:</p>
        <div class="progressBar">
            <div class="completionProgressBar" >  
                <div class="completionProgressGreenBar text-center"></div>
                <div class="completionProgressGreenBar reversebar text-center" :style="{width: '100%'}"></div>
                <div class="reverseProgressGreenBar reversebar text-center" :style="{width: reversebar +'%'}"></div>
            </div>
        </div>
        
        <div v-for="(todo, index) in todos" :key="todo.id" id="edit" class="todo-item" @dblclick="edititem(index)">     
            <div class="todo-item-label" :class="{ completed : todo.completed }" :id="'todo_' + todo.id">
                <input type="checkbox" v-model="todo.completed">

                <input v-if="todo.isEdit" type="text" v-model="todo.title" value="todo.title">

                <span v-else :class="{isEditClass: todo.isEdit}">{{ todo.title }}</span>
                <button class="save-btn">Save</button>
                <button class="cancle-btn">Cancel</button>
            </div>
            
            <div class="remove-item"  @click="removeTodo(index)">
            &times;
            </div>
            <!--<div class="edit-item">
                <input type="text">
                <button>save</button>
                <button>cancle</button>
            </div>-->
        </div>   

        <input type="text" class="todo-input" placeholder="what needs to be done" v-model="newTodo" @keyup.enter="addTodo">
        
        <p>Done:</p>
        <div class="progressBar">
            <div class="completionProgressBar">
                <div class="completionProgressGreyBar text-center"></div>
                <div class="completionProgressGreyBar text-center" :style="{width:percentageOfTasksCompleted +'%'}"></div>
            </div>
        </div>
    </div>
</template>

<script>
export default{
    name:"TodoList",
    // State of TODO-List component
    data() {
        return {
            newTodo:'',
            todos:[],
            idForTodo:0,
        }
    },
    methods:{
        addTodo(){
           //this.todos.push(this.newTodo)
           //this.newTodo = " " 
           console.log("New item: " , this.todos)
           this.todos.push({
            id: this.idForTodo,
            title: this.newTodo,
            completed: false,
            isEdit:false
           })
           this.newTodo =''
           this.idForTodo++

        },
        removeTodo(index){
            console.log("iden ", index)
           this.todos.splice(index, 1)
        },
        edititem(index){
            // document.getElementById("edit").innerHTML = " Welcome to the javaTpoint.com ";
            console.log("On edit: " , this.todos)
            console.log("value: " , index)
            this.todos[index].isEdit = !this.todos[index].isEdit
        }

    },
    computed:{
        checkMarkedtodos(){
            let count = 0;
            for (let i=0; i < this.todos.length; ++i){
                if (this.todos[i].completed ==true){
                    count ++;
                }      
            }
            return count;
        },
        percentageOfTasksCompleted(){
            if (this.todos.length == 0){
                return 0;
            }else {
                return(this.checkMarkedtodos / this.todos.length) * 100;
            }
        },
        reversebar(){
            return(100-this.percentageOfTasksCompleted )
        }
        
    }
}
</script>

<style>
.task-container{
    padding: 30px;
    width: 40%;
    margin: auto;
    box-shadow: 0 4px 10px 2px rgba(90,90,90,0.4);
}

.todo-input {
    width: 90%;
    padding: 8px 8px;
    font-size:10px;
    margin: 20px 10px;
}

.todo-item{   
    margin-bottom: 12px;
    display: flex;
    align-items: center;
    justify-content: space-between;
    padding-right: 10px;
    padding-left: 10px;
}

.todo-item:hover {
    /* background-color: #b9d4ed; */
    box-shadow: 5px 5px 5px #b9d4ed;
}
.remove-item{
    display: none;
}

.todo-item:hover > .remove-item{
    display: block;
    color: red;
    cursor: pointer;
    margin-left: 14px;
}

.completed{
    text-decoration: line-through;
    color: gray;
}

.isEditClass {
    color: red;
}
.completionProgressGreenBar{
    height: 4px;
    z-index: 1;
    background-color: #b9ecb5;
}

.reverseProgressGreenBar{
    height: 4px;
    margin-top: -4px;
    z-index: 999;
    background-color: #fff;
}


.completionProgressGreyBar{
    height: 4px;
    background-color: #b9d4ed;
   
}
.progressBar{
    margin-bottom: 30px;
    
}
.save-btn{
    margin: 0px 8px;
}

</style>