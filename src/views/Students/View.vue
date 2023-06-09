<template>
  <div class="container mt-4">
    <div class="card">
      <div class="card-header">
        <h4>
          Students
          <RouterLink to="/students/create" class="btn btn-primary">
            Add Student
          </RouterLink>
        </h4>
      </div>
      <div class="card-body">
        <table class="table">
          <thead>
            <tr>
              <th>ID</th>
              <th>Name</th>
              <th>Address</th>
              <th>Email</th>
              <th>Phone</th>
              <th>Created At</th>
              <th>Action</th>
            </tr>
          </thead>
          <tbody v-if="this.students.length > 0">
            <tr v-for="(student, index) in this.students" :key="index">
              <td>{{ student.id }}</td>
              <td>{{ student.name }}</td>
              <td>{{ student.address }}</td>
              <td>{{ student.email }}</td>
              <td>{{ student.phone }}</td>
              <td>{{ student.created_at }}</td>
              <td>
                <RouterLink
                  :to="{ path: '/students/' + student.id + '/edit' }"
                  class="btn btn-success mx-2"
                >
                  Edit
                </RouterLink>
                <button
                  type="button"
                  class="btn btn-danger mx-2"
                  @click="deleteStudent(student.id)"
                >
                  Delete
                </button>
              </td>
            </tr>
          </tbody>
          <tbody v-else>
            <tr>
              <td colspan="7">No records found.</td>
            </tr>
          </tbody>
        </table>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
export default {
  name: "students",
  data() {
    return {
      students: [],
    };
  },
  mounted() {
    this.getStudents();
  },
  methods: {
    getStudents() {
      axios.get("http://127.0.0.1:8000/api/students").then((response) => {
        this.students = response.data.students;
        console.log(this.students);
      });
    },
    deleteStudent(studentId) {
      if (confirm("Are you sure, you want to delete this data?")) {
        // console.log(studentId)
        axios
          .delete(`http://127.0.0.1:8000/api/students/${studentId}/delete`)
          .then((response) => {
            alert(response.data.message);
            this.getStudents();
          })
          .catch(function (error) {
            if (error.response) {

              if (error.response.status == 404) {
                alert(error.response.data.message);
              }
            }
          }
        );
      }
    },
  },
};
</script>

<style>
</style>
