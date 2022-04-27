<script setup>
import { onMounted, ref } from "vue";
const images = ref([]);
const getImages = async () => {
  const res = await axios.get("http://localhost:8000/api/get");
  images.value = res.data;
};
const deleteImage = async (id) => {
  const res = await axios.delete("http://localhost:8000/api/delete/" + id);
  if (res) {
    alert("Delete successfully");
    getImages();
  }
};
onMounted(() => {
  getImages();
});
</script>

<template>
  <div class="text-center my-3">
    <router-link to="/create">
      <button class="btn btn-success">Create</button></router-link
    >
  </div>
  <div class="container my-3">
    <div class="table-responsive">
      <table class="table-bordered table">
        <thead>
          <tr>
            <th>ID</th>
            <th>Title</th>
            <th>Image</th>
            <th>Edit</th>
            <th>Delete</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="(image, index) in images" :key="index">
            <td>{{ index + 1 }}</td>
            <td>{{ image.title }}</td>
            <td>
              <img
                :src="`http://localhost:8000/storage/${image.image}`"
                style="width: 76px; height: 76px"
                alt=""
              />
            </td>
            <td>
              <router-link :to="`/updates/${image.id}`"
                ><button class="btn btn-success">Edit</button></router-link
              >
            </td>
            <td>
              <button @click="deleteImage(image.id)" class="btn btn-danger">
                Delete
              </button>
            </td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>

<script>
import axios from "axios";
export default {
  name: "HomeView",
  components: {},
};
</script>
