<template>
  <tr>
    <td class="text-center">{{ course.id }}</td>
    <td>{{ course.name }}</td>
    <td>
      <select
        name="status"
        v-model="status"
        @change="handleStatus"
        class="form-select"
      >
        <option value="active" :selected="course.status === 'active'">
          Active
        </option>
        <option value="inactive" selected="course.status === 'inactive'">
          Inactive
        </option>
      </select>
    </td>
    <td>{{ course.Category.name }}</td>
    <td>
      <div class="d-flex flex-row justify-content-around">
        <button class="btn detailsButton" @click.prevent="toDetails(course.id)">
          Details
        </button>

        <button class="btn btn-primary" @click="toAddVideo(course.id)">
          Add Video
        </button>

        <button class="btn btn-secondary" @click="toUpdate(course.id)">
          Update
        </button>
      </div>
    </td>
  </tr>
</template>

<script>
import { alertError, alertSuccess } from "../apis/swal";
export default {
  name: "CourseRow",
  props: ["course"],
  data: function () {
    return {
      status: "",
    };
  },
  methods: {
    toAddVideo(courseId) {
      this.$router.push(`/add-video/${courseId}`);
    },
    toUpdate(courseId) {
      this.$router.push(`/update-course/${courseId}`);
    },
    toDetails(courseId) {
      this.$router.push(`/admin/course/${courseId}`);
    },
    handleStatus() {
      const payload = {
        courseId: this.course.id,
        status: this.status,
      };
      this.$store
        .dispatch("updateCourseStatusAdmin", payload)
        .then(() => {
          alertSuccess(
            `Course status with id ${this.course.id} has been updated`
          );
          this.$emit("updateStatus");
        })
        .catch((err) => {
          alertError(err.message);
        });
    },
  },
  created() {
    this.status = this.course.status;
  },
};
</script>

<style>
.detailsButton {
  background-color: #fc7901;
  color: #fff;
}

.detailsButton:hover {
  background-color: #eb5e0b;
  color: #fff;
}
</style>
