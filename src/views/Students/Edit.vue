<template>
    <div class="container mt-4">
        <div class="card">
            <div class="card-header d-flex justify-content-between">
                <h4 class="text-uppercase">New Student</h4>
            </div>
            <div class="card-body">
                <ul class="alert alert-warning" v-if="Object.keys(this.errorList).length > 0">
                    <li class="md-2" v-for="(error, index) in this.errorList" :key="index">{{ error[0] }}</li>
                </ul>
                <!-- Student Form -->
                <div class="col-12">
                    <div class="mb-3">
                        <label for="name" class="form-label">Name</label>
                        <input v-model="model.student.name" type="text" class="form-control">
                    </div>
                    <div class="mb-3">
                        <label for="course" class="form-label">Course</label>
                        <input v-model="model.student.course" type="text" class="form-control">
                    </div>
                </div>
                <div class="col-12">
                    <div class="mb-3">
                        <label for="email" class="form-label">Email</label>
                        <input v-model="model.student.email" type="email" class="form-control">
                    </div>
                    <div class="mb-3">
                        <label for="phone" class="form-label">Phone</label>
                        <input v-model="model.student.phone" type="number" class="form-control">
                    </div>
                </div>
                <div class="text-end">
                    <button type="submit" @click="updateStudent" class="btn btn-primary">Save</button>
                </div>

            </div>
        </div>
    </div>
</template>
  
<script>
import axios from 'axios';

export default {
    studentId: '',
    name: 'studentEdit',
    data() {
        return {
            errorList: '',
            model: {
                student: {
                    name: '',
                    course: '',
                    email: '',
                    phone: '',
                }
            }
        }
    },
    mounted() {
        this.studentId = this.$route.params.id;
        this.getStudentData(this.$route.params.id);
    },
    methods: {
        getStudentData(studentId) {
            axios.get(`http://127.0.0.1:8000/api/students/edit/${studentId}`).then(res => {
                console.log(res.data.message);
                this.model.student = res.data.message
            }).catch(function (error) {
                if (error.response) {

                    alert(error.response.data.message)
                }
            });
        },
        updateStudent() {
            var myThis = this;
            axios.put(`http://127.0.0.1:8000/api/students/update/${this.studentId}`, this.model.student).then(res => {
                this.$router.push({ name: 'student' });
                this.errorList = '',
                    console.log(res);
            }).catch(function (error) {
                if (error.response) {
                    if (error.response) {
                        if (error.response.status == 422) {
                            myThis.errorList = error.response.data.errors;
                        }
                    }
                }
            })
        }
    }
};
</script>
  