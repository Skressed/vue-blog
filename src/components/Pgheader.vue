<template>
  <div class="hello">
    <p class="title">Hello, welcome to my blog!</p>
    <p class="caption">I enjoy posting things here. Maybe you will enjoy reading them.</p>
    <b-button @click="newPost()" variant="primary" class="newpostbtn">Create a new post</b-button>
  </div>
</template>

<script>

export default {
  name: 'Pgheader',
  methods: {
    newPost: function() {
      this.$bvModal.show('modal-newpost');
    }
  }
}
</script>

<style scoped>
p {
  margin:0;padding:0;
}

.hello {
  background-color: rgba(153,204,255, 0.1);
  padding-top: 2em;
  padding-bottom: 2em;
  padding-left: 3em;
  padding-right: 3em;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  box-shadow: 0px 10px 5px 0px rgba(153,204,255, 0.1);
}

.title {
  font-size: 36px;
  margin-bottom: 0.2em;
}

.caption {
  font-size: 16px;
}

.newpostbtn {
  margin-top: 12px;
}
</style>
