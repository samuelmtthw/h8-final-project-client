<template>
  <section
    id="UpdateVideo"
    class="stationary-page d-flex justify-content-center"
  >
    <div class="wrapper d-flex justify-content-center align-items-center">
      <div class="card p-5 rounded">
        <form class="d-flex flex-column" @submit.prevent="updateVideo">
          <h2 class="mb-4">Edit Video</h2>
          <div class="videoContainer">
            <iframe :src="video.videoUrl" frameborder="0"></iframe>
            <iframe :src="video.videoUrl" frameborder="0"></iframe>
          </div>
          <span class="mt-3">Video Title</span>
          <input
            type="text"
            v-model="video.name"
            class="form-control mb-3 mt-1"
            required
          />

          <input type="submit" value="Edit" class="btn form-control mt-3" />

          <router-link
            class="btn btn-back mt-2"
            :to="`/admin/course/${video.CourseId}`"
          >
            Back
          </router-link>
        </form>
      </div>
    </div>
  </section>
</template>

<script>
import { alertError, alertLoading } from "@/apis/swal.js";
import { alertSuccess, swalDone } from "../../apis/swal";
export default {
  name: "UpdateVideo",
  data: function () {
    return {
      video: {
        name: "",
        videoUrl: "",
        CourseId: "",
      },
    };
  },
  methods: {
    updateVideo() {
      const payload = {
        name: this.video.name,
        videoId: this.$route.params.videoId,
      };
      alertLoading();
      this.$store
        .dispatch("updateVideoAdmin", payload)
        .then(() => {
          alertSuccess(`Video with id ${this.video.id} has been changed!`);
          this.$router.push(`/admin/course/${this.video.CourseId}`);
        })
        .catch((err) => {
          const message = err.message.join(", ");
          alertError(message);
        });
    },
  },
  created() {
    alertLoading();
    this.$store
      .dispatch("readVideoDetail", this.$route.params.videoId)
      .then((result) => {
        swalDone();
        this.video = result;
      })
      .catch((err) => {
        alertError(err.message);
      });
  },
};
</script>

<style>
#UpdateVideo .card {
  min-width: 600px;
  height: fit-content;
}

#UpdateVideo .videoContainer {
  position: relative;
  overflow: hidden;
  width: 100%;
  padding-top: 56.25%;
}

#UpdateVideo .videoContainer iframe {
  position: absolute;
  top: 0;
  left: 0;
  bottom: 0;
  right: 0;
  width: 100%;
  height: 100%;
}

#UpdateVideo .btn {
  background-color: #fc7901;
  color: #ffffff;
  font-family: "Poppins", sans-serif;
  font-weight: 700;
  font-style: italic;
}

#UpdateVideo .btn:hover {
  background-color: #eb5e0b;
}
#UpdateVideo .btn-back {
  background-color: #6c757d;
  color: #ffffff;
}
#UpdateVideo .btn-back:hover {
  background-color: #5c636a;
}
</style>
