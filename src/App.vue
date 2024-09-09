<script setup>
  import { ref } from 'vue'

  const currentTodo = ref(0);

  const todo = ref('')

  const toDos = ref([
    {
      "name": "Rửa bát",
      "status": "Completed",
      "isEdit": false
    },
    {
      "name": "Quét nhà",
      "status": "Completed",
      "isEdit": false
    },
    {
      "name": "Lau nhà",
      "status": "Uncompleted",
      "isEdit": false
    }
  ])

  const handleInputChange = (event) => {
    todo.value = event.target.value;
  }

  const isEdit = ref(false);

  const handleAdd = () => {
    if(todo.value.trim() !== "") {
      toDos.value.push({
        "name": todo.value,
        "status": "Uncompleted",
        "isEdit": false
      })
    }

    console.log(toDos.value);
    clear();
  }

  const handleStatus = (index) => {
    toDos.value[index].status = toDos.value[index].status === 'Completed' ? 'Uncompleted' : 'Completed';
  }
  
  const handleSave = (index) => {
    toDos.value[index].name = todo.value.name;
    clear();
  }

  const handleEdit = (index) => {
    isEdit.value = true;
    todo.value = toDos.value[index].name;
    currentTodo.value = index;
  }

  const handleDel = (index) => {
    if(toDos.value[index] !== undefined) {
      toDos.value.splice(index, 1);
    }
  }

  const clear = () => {
    todo.value = "";
    isEdit.value = false;
    currentTodo.value = 0;
  }
</script>

<template>
  <section class="vh-100" style="background-color: #eee;">
  <div class="container py-5 h-100">
    <div class="row d-flex justify-content-center align-items-center h-100">
      <div class="col col-lg-9 col-xl-7">
        <div class="card rounded-3">
          <div class="card-body p-4">

            <h4 class="text-center my-3 pb-3">To Do App</h4>

            <form class="row mb-4 pb-2">
              <div class="col-9">
                <div data-mdb-input-init class="form-outline">
                  <input type="text" id="form1" class="form-control w-100" placeholder="Enter a task here" :value="todo" @input="handleInputChange"/>
                </div>
              </div>

              <div class="col-3">
                <button class="btn btn-success" v-show="!isEdit" @click.prevent="handleAdd">Add Todo</button>
                <button class="btn btn-primary ms-1" v-show="isEdit" @click.prevent="handleSave(currentTodo)">Save</button>
                <button class="btn btn-danger ms-1" v-show="isEdit" @click="clear">Cancel</button>
              </div>
            </form>

            <table class="table mb-4">
              <thead>
                <tr>
                  <th scope="col">Todo item</th>
                  <th scope="col">Status</th>
                  <th scope="col">Actions</th>
                </tr>
              </thead>
              <tbody>
                <template v-for="(item, index) in toDos" :key="index">
                  <tr>
                    <td>{{ item.name }}</td>
                    <td>
                      <button v-show="item.status === 'Uncompleted'" class="btn btn-warning" @click="handleStatus(index)">Uncompleted</button>
                      <button v-show="item.status === 'Completed'" class="btn btn-success" @click="handleStatus(index)">Completed</button>
                    </td>
                    <td>
                      <button class="btn btn-warning" @click="handleEdit(index)" :disabled="isEdit">Edit</button>
                      <button class="btn btn-danger ms-1" @click="handleDel(index)">Delete</button>
                    </td>
                  </tr>
                </template>

              </tbody>
            </table>

          </div>
        </div>
      </div>
    </div>
  </div>
</section>
</template>

<style scoped></style>