<template>
  <div class="wrapper">
    <div class="grid">
      <div @click="openPost(post.id)" class="post" v-for="post in posts" v-bind:key="post.id">
        <div class="post-header">
          <p class="post-title">{{ post.title }}</p>
          <p class="post-caption">{{ post.caption }}</p>
        </div>
        <div class="post-footer">
          <p class="post-comments-count">{{ post.comments.length }} comments</p>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { bus } from '../main'

export default {
  name: 'Cards',
  props: {
   posts: {
     type: Array,
   }
 },
  methods: {
    openPost: function(index) {
      this.$bvModal.show('modal-card');
      bus.$emit('openThePost', index);
    }
  }
}
</script>

<style scoped>
p {
  margin: 0;
  padding:0;
  word-wrap: break-word;
}

.wrapper {
  padding-top: 4em;
  padding-left: 36em;
  padding-right: 36em;
}

.grid {
  display: grid;
  grid-template-columns: 1fr;
  transition: 0.3s;
  align-items: flex-start;
}

.post {
  border: 1px solid black;
  margin: 16px;
  padding: 24px;
  padding-bottom: 4px;
  display: flex;
  flex-direction: column;
  align-items: center;
  cursor: pointer;
  min-height: 170px;
  max-height: 640px;
  transition: 0.3s;
  box-shadow: 0px 10px 5px 0px rgba(153,204,255, 0.4);
}

.post:hover {
  transition: 0.3s;
  box-shadow: 0px 12px 5px 0px rgba(153,204,255, 0.7);
}

.post-header {
  display: flex;
  flex-direction: column;
  align-items: center;
  transition: 0.3s;
  height: 8em;
}

.post-title {
  font-size: 24px;
  margin-bottom: 4px;
  height: 3em;
  transition: 0.3s;
  text-align: center;
}

.post-caption {
  overflow-wrap: break-word;
}

.post-footer {
  display: flex;
  width: 100%;
  height: 100%;
  flex-direction: row;
  justify-content: flex-end;
  align-items: flex-end;
}

.post-comments-count {
  font-size: 16px;
  opacity: 0.6;
  transition: 0.3s;
  height: 3em;
}

@media screen and (max-width: 1600px) {
  .wrapper {
    padding-left: 18em;
    padding-right: 18em;
  }
}

@media screen and (max-width: 1100px) {
  .wrapper {
    padding-left: 12em;
    padding-right: 12em;
  }

  .post {
    min-height: 170px;
    padding: 16px;
  }
}

@media screen and (max-width: 950px) {
  .wrapper {
    padding-left: 4em;
    padding-right: 4em;
  }

  .post {
    min-height: 160px;
  }

  .post-header {
    height: 12em;
  }

  .post-title {
    font-size: 20px;
    height: 5em;
  }
}

@media screen and (max-width: 740px) {
  .wrapper {
    padding-left: 0;
    padding-right: 0;
  }

  .grid {
    justify-content: center;
    align-items: center;
    text-align: center;
    grid-template-columns: auto;
  }
  .post {
    min-height: 240px;
    max-width: 260px;
  }

  .post-header {
    height: 8em;
  }

  .post-caption {
    max-width: 260px;
  }

  .post-footer{
    margin-top: 48px;
  }
}
</style>
