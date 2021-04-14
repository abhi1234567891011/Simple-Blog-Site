<template>
    <div class="container-form">

  <div class="form-group">
    <label for="title">Title</label>
    <input type="text" class="form-control" v-model="post.title"  placeholder="Enter title">
    <small  class="form-text text-muted">Please enter your title for blog.</small>
  </div>
  <div class="form-group">
    <label for="description">Description</label>
    <input type="text" class="form-control" v-model="post.description" placeholder="Enter Description">
  </div>
  
  <button type="submit" @click="addItem()" class="btn btn-dark">Add</button>
 
  </div>
</template>

<script>
    export default {
        data: function (){
            return{
            post: {
                title: "",
                description: "",
            }
        }
        },
        methods: {
            addItem(){
                // console.log(this.post.title);
                // console.log(this.post.description);

                if(this.post.title === ''){
                    return;
                }

                axios({
                    method: "post",
                    url: "/api/post/store",
                    data: {
                        title: `${this.post.title}`,
                        description:  `${this.post.description}`
                    }
                }).then(response => {
                    if(response.status == 201){
                        console.log("Sucess List");
                        this.post.title = "";
                        this.post.description = "";
                        this.$emit('reloadlist');

                    }
                }).catch(error => {
                    console.log(error);
                })
            }
        }

    }
</script>

<style scoped>
.container-form{
    margin: 5rem auto;
}
</style>