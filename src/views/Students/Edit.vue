<template>
  <div class="container mt-5">
    <div class="card">
      <div class="card-header">
        <h4>Edit Students</h4>
      </div>
      <div class="card-body">
        <ul
          class="alert alert-warning"
          v-if="Object.keys(this.errorList).length > 0"
        >
          <li
            class="mb-0 ms-3"
            v-for="(error, index) in this.errorList"
            :key="index"
          >
            {{ error[0] }}
          </li>
        </ul>

        <div class="mb-3">
          <label for="">Name</label>
          <input
            type="text"
            v-model="model.student.name"
            class="form-control"
          />
        </div>
        <div class="mb-3">
          <label for="">Address</label>
          <input
            type="text"
            v-model="model.student.address"
            class="form-control"
          />
        </div>
        <div class="mb-3">
          <label for="">Email</label>
          <input
            type="text"
            v-model="model.student.email"
            class="form-control"
          />
        </div>
        <div class="mb-3">
          <label for="">Phone</label>
          <input
            type="text"
            v-model="model.student.phone"
            class="form-control"
          />
        </div>
        <div class="mb-3">
          <button type="button" @click="updateStudent" class="btn btn-primary">
            Update
          </button>
          <RouterLink
            to="/students"
            class="btn btn-success mx-2"
          >
            Back
          </RouterLink>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
export default {
  name: "studentCreate",
  data() {
    return {
      studentId: "",
      errorList: "",
      model: {
        student: {
          name: "",
          address: "",
          email: "",
          phone: "",
        },
      },
    };
  },
  methods: {
    updateStudent() {
      var mythis = this;
      axios
        .put(
          `http://127.0.0.1:8000/api/students/${this.studentId}/edit`,
          this.model.student
        )
        .then((response) => {
          console.log(response);
          alert(response.data.message);
          this.errorList = "";
        })
        .catch(function (error) {
          if (error.response) {
            if (error.response.status == 422) {
              mythis.errorList = error.response.data.errors;
            }

            if (error.response.status == 404) {
              alert(error.response.data.message);
            }
          } else if (error.request) {
            console.log(error.request);
          } else {
            console.log("Error", error.message);
          }
          console.log(error.config);
        });
    },
    getStudentData(studentId) {
      axios
        .get(`http://127.0.0.1:8000/api/students/${studentId}/edit`)
        .then((response) => {
          console.log(response.data.student);
          this.model.student.name = response.data.student.name;
          this.model.student.address = response.data.student.address;
          this.model.student.email = response.data.student.email;
          this.model.student.phone = response.data.student.phone;
        })
        .catch(function (error) {
          if (error.response) {
            if (error.response.status == 404) {
              alert(error.response.data.message);
            }
          }
        });
    },
  },
  mounted() {
    // console.log(this.$route.params.id)
    this.getStudentData(this.$route.params.id);
    this.studentId = this.$route.params.id;
  },
};
</script>

<style>
</style>