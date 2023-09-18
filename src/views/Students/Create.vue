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
                    <button type="submit" @click="saveStudent" class="btn btn-primary">Save</button>
                </div>

            </div>
        </div>
    </div>
</template>
  
<script>
import axios from 'axios';

export default {
    name: 'studentCreate',
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
    methods: {
        saveStudent() {
            var myThis = this;
            axios.post('http://127.0.0.1:8000/api/students/create', this.model.student).then(res => {
                this.$router.push({ name: 'student' });
                this.model.student = {
                    name: '',
                    course: '',
                    email: '',
                    phone: '',
                }
                this.errorList = '',
                    console.log(res);
            }).catch(function (error) {
                if (error.response) {
                    if (error.response) {
                        if (error.response.status == 422) {
                            myThis.errorList = error.response.data.errors;
                        }
                        console.log(error.response.data);
                        console.log(error.response.status);
                        console.log(error.response.headers);
                    } else if (error.request) {
                        console.log(error.request);
                    } else {
                        console.log('Error', error.message);
                    }
                }
            })
        }
    }
};
</script>
  