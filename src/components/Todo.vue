<script setup>
    import { ref } from "vue"
    import PrimaryButton from './partials/PrimaryButton.vue'
    const props = defineProps(['msg'])

    const task = ref('')
    const newTask = ref('')
    let tasks = ref([])
    let editing = ref(false)

    function addTask()
    {
        if(task.value != "")
        {
            let todo = ref({
                id : tasks.value.length + 1,
                text : task.value,
                done : false,
            })
            tasks.value.push(todo.value)
            task.value = ""
        } else {
            alert("Task can't be empty")
        }
    }

    function deleteTask(task)
    {
        let removeTaskIndex = tasks.value.indexOf(task)

        tasks.value.splice(removeTaskIndex, 1)
    }
    function markAsDone(task)
    {
        let index = tasks.value.indexOf(task)
        tasks.value[index].done = !tasks.value[index].done
    }
    function editingTask(task)
    {
        let index = tasks.value.indexOf(task)
        
        tasks.value[index].value = newTask
        task.text = tasks.value[index].value
    }

    const handleInput = (event) => {
        newTask.value = event.target.value
    }

</script>

<template>
    <h1>{{ props.msg }}</h1>
    <form @submit.prevent="addTask">
        <div>
            <label for="task">Add a new task</label>
            <input type="text" name="task" id="task" v-model="task">
        </div>
        <PrimaryButton type="button" text="Create" v-on:click="addTask"></PrimaryButton>

        <PrimaryButton v-if="editing == false" type="button" text="Start Editing" v-on:click="editing = true"></PrimaryButton>
        <PrimaryButton v-else type="button" text="Stop editing" v-on:click="editing = false"></PrimaryButton>
    </form>

    <ul :class="{editing : editing == true }">
        <li :key="task.id" v-for="task in tasks" :class="[{'not-done' : task.done == false}, {'done' : task.done == true}]">

            <span v-if="editing == false">
                <input type="checkbox" @click="markAsDone(task)"> {{ task.text }} <button @click="deleteTask(task)">x</button>
            </span>

            <input v-else @input="handleInput" @change="editingTask(task)" type="text" :value="task.text"/>

        </li>
        
        <li v-if="task != ''">{{ task }}</li>
    </ul>
</template>

<style scoped>
    .done {
        text-decoration: line-through;
    }

    .not-done {
        text-decoration:none;
    }
    
    .editing {
    list-style: none;
    }
</style>