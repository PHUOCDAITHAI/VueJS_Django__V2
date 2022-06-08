<template>
  <div id="app">
    <form @submit.prevent="submitForm">
      <div class="form-group row">
        <div class="col-3">
          <input type="text" v-model="student.name" class="form-control mx-2" placeholder="Name" />
        </div>
        <div class="col-3">
          <input type="text" v-model="student.course" class="form-control mx-2" placeholder="Course" />
        </div>
        <div class="col-3">
          <input type="number" v-model="student.rating" class="form-control mx-2" placeholder="Rating" />
        </div>
        <div class="col-3">
          <button class="btn btn-success">Submit</button>
        </div>
      </div>
    </form>
    <table class="table table-striped">
      <thead>
        <th>Name</th>
        <th>Course</th>
        <th>Rating</th>
      </thead>
      <tbody>
        <tr v-for="student in students" :key="student.id" @dblclick="$data.student = student">
          <td>{{student.name}}</td>
          <td>{{student.course}}</td>
          <td>{{student.rating}}</td>
          <td>
            <button @click="deleteStudent(student.id)" class="btn btn-outline-danger btn-sm mx-1">
              x
            </button>
          </td>
        </tr>
      </tbody>
    </table>
    <!-- <router-view/> -->
  </div>
</template>
<script>
export default {
  name: 'App',
  data() {
    return {
      student: {},
      students: []
    }
  },
  methods: {
    submitForm(){
      if(this.student.id === undefined){
        this.createStudent();
      }else {
        this.editStudent();
      }
    },
    async getStudents(){
      let response = await fetch('http://127.0.0.1:8000/api/students/')
      this.students = await response.json()
    },
    async createStudent(){
      await fetch('http://127.0.0.1:8000/api/students/', {
        method: 'post',
        headers: {
          'Content-Type': 'application/json'
        },
        body: JSON.stringify(this.student)
      });
      await this.getStudents()
    },
    async editStudent(){
      await fetch(`http://127.0.0.1:8000/api/students/${this.student.id}/`, {
        method: 'put',
        headers: {
          'Content-Type': 'application/json'
        },
        body: JSON.stringify(this.student)
      });
      await this.getStudents();
      this.student = {}
    },
    async deleteStudent(id){
      await fetch(`http://127.0.0.1:8000/api/students/${id}/`, {
        method: 'delete',
        headers: {
          'Content-Type': 'application/json'
        },
        body: JSON.stringify(this.student)
      });
      await this.getStudents();
      this.student = {}
    },
  },
  async created() {
    await this.getStudents()
  },
}
</script>
<style>
</style>
