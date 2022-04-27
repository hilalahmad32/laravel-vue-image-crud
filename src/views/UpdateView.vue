<script setup>
import { onMounted, ref } from "vue";
import { useRoute, useRouter } from "vue-router";
import axios from "axios";
const title = ref("");
const image = ref("");
const new_image = ref("");
const router = useRouter();
const route = useRoute();
const onFileChange = (e) => {
  new_image.value = e.target.files[0];
};
const getImage = async () => {
  const res = await axios.patch(
    `http://localhost:8000/api/edit/${route.params.id}`
  );
  title.value = res.data.title;
  image.value = res.data.image;
};

const submit = async () => {
  if (!title.value) {
    alert("Please fill the field");
  } else {
    const formData = new FormData();
    formData.append("title", title.value);
    formData.append("image", image.value);
    formData.append("new_image", new_image.value);
    const res = await axios.post(
      `http://localhost:8000/api/update/${route.params.id}`,
      formData
    );
    if (res) {
      alert("image update successfully");
      router.push("/");
    }
  }
};

onMounted(() => {
  getImage();
});
</script>

<template >
  <div>
    <div class="container my-5">
      <div class="row d-flex justify-content-center">
        <div class="col-xl-5 col-lg-5 col-md-8 col-sm-12">
          <div class="my-3">
            <router-link to="/"
              ><button class="btn btn-secondary">Go Back</button></router-link
            >
          </div>
          <div class="card">
            <div class="card-header">
              <h3>Update Image</h3>
            </div>
            <div class="card-body">
              <form action="" @submit.prevent="submit">
                <div class="my-2">
                  <label for=""><b>Title</b></label>
                  <input type="text" v-model="title" class="form-control" />
                </div>
                <div class="my-2">
                  <label for=""><b>Image</b></label>
                  <input
                    type="file"
                    @change="onFileChange"
                    class="form-control"
                  />
                  <input type="hidden" v-model="image" />
                  <img
                    :src="`http://localhost:8000/storage/${image}`"
                    alt=""
                    style="width: 70px; height: 70px"
                  />
                </div>
                <button class="btn btn-primary" type="submit">Update</button>
              </form>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>
<script>
export default {
  name: "UpdateView",
};
</script>
<style >
</style>