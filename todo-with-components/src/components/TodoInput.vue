<template>
    <div class="flex mt-4">
        <input @keyup="pressEnter" v-model="description" class="shadow appearance-none border rounded w-full py-2 px-3 mr-4 text-gray-700" placeholder="Add Todo">
        <button @click="addTodo()" class="flex p-2 border-2 rounded text-blue-500 border-blue-500 hover:text-white hover:bg-blue-500">Ajouter</button>
    </div>
</template>

<script>
    export default {
        data() {
            return {
                description: ""
            }
        },
        methods: {
            addTodo() {
                this.$emit("onAddTodo", this.description)
                this.description = "";
            },
            pressEnter(event) {
                if (event.key === "Enter") {
                    this.addTodo();
                }
            }
        }
    }
</script>