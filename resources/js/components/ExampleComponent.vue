<template>
  <div class="container">
    <h1 class="text-center mt-4">Blog Site</h1>
    <formCom v-on:reloadlist="getData()"></formCom>
    <!-- Printing the data from the database  -->
    <h1 class="h1">Posts</h1>
    <table class="table">
      <thead>
        <th></th>
        <th></th>
        <th></th>
      </thead>
      <div v-for="(post, index) in posts" :key="post.id">
        <tbody>
          <tr>
            <th scope="row">{{ index + 1 }}</th>
            <td>
              <input type="text" v-model="post.title" class="form-control" />
            </td>
            <td>
              <input
                type="text"
                v-model="post.description"
                class="form-control"
              />
            </td>

            <td>
              <button class="btn btn-dark" @click="deletePost(post)">
                Del
              </button>
            </td>
            <td>
              <button class="btn btn-dark" @click="editPost(post)">Edit</button>
            </td>
          </tr>
        </tbody>
      </div>
    </table>
  </div>
</template>

<script>
import formCom from "./form.vue";

export default {
  components: {
    formCom,
  },
  data: function () {
    return {
      posts: [],
    };
  },
  methods: {
    getData() {
      axios
        .get("/api/posts")
        .then((response) => {
          this.posts = response.data;
          console.log(this.posts);
        })
        .catch((error) => {
          console.log(error);
        });
    },
    //Delete the post
    deletePost(post) {
      console.log(post.id);

      axios
        .delete("/api/post/" + post.id)
        .then((response) => {
          console.log("element deleted");
          this.getData();
        })
        .catch((error) => {
          console.log(error);
        });
    },
    editPost(post) {
      // console.log(post.title);

      axios
        .put("/api/post/" + post.id, {
          title: `${post.title}`,
          description: `${post.description}`,
        })
        .then((response) => {
          if (response.status == 200) {
            this.getData();
          }
        })
        .catch((error) => {
          console.log(error);
        });
    },
  },
  created() {
    this.getData();
  },
};
</script>

<style scoped>
.form-control {
  outline: none;
  border: none;
}
</style>
