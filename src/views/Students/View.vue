<template>
  <div class="container mt-4">
    <div class="card">
      <div class="card-header d-flex justify-content-between">
        <h4 class="text-uppercase">Student List
        </h4>
        <router-link to="/student/create" class="btn btn-primary">Add New</router-link>
      </div>
      <div class="card-body">
        <table class="table table-hover">
          <thead>
            <tr>
              <th>No.</th>
              <th>Name</th>
              <th>Course</th>
              <th>Email</th>
              <th>Phone</th>
              <th>Actions</th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="(student, index) in this.students" :key="index">
              <td>{{ student.id }}</td>
              <td>{{ student.name }}</td>
              <td>{{ student.course }}</td>
              <td>{{ student.email }}</td>
              <td>{{ student.phone }}</td>
              <td class="d-flex gap-2"> <router-link :to="{ path: '/student/edit/' + student.id }"
                  class="btn btn-info">Edit</router-link>
                <button type="button" @click="deleteStudent(student.id)" class="btn btn-warning">Delete</button>
              </td>
            </tr>
          </tbody>
        </table>
      </div>
    </div>
  </div>
</template>

<script>
import axios, { all } from 'axios';
export default {
  name: 'students',
  data() {
    return {
      students: []
    }
  },
  mounted() {
    this.getStudents();
  },
  methods: {
    getStudents() {
      axios.get('http://127.0.0.1:8000/api/students').then(res => {
        this.students = res.data.students
        console.log(this.students);
      });
    },
    deleteStudent(studentId) {
      if (confirm('Are you sure to delete?')) {
        axios.delete(`http://127.0.0.1:8000/api/students/${studentId}`).then(res => {
          alert(res.data.message)
          this.getStudents()
        })
      }
    }
  }
}
</script>
  
  