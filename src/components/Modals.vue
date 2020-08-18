<template>
  <div class="modals">
    <b-modal id="modal-card" size="lg" centered
    @hidden="resetModal"
    >
      <template v-slot:modal-header="{ }">
        <input v-if="titleEdit==true" type="text" maxlength="36" class="titleinput" v-model="titlePlaceholder" @blur="post.title=titlePlaceholder;titleEdit = !titleEdit;"/>
        <div v-if="titleEdit==false" @click="titleEdit=true" class="title">{{ post.title }}</div>
      </template>

      <template v-slot:default="{ }">
        <textarea v-if="textEdit==true" class="textinput" maxlength="480" v-model="textPlaceholder" @blur="post.text=textPlaceholder;textEdit = !textEdit;"/>
        <div v-if="textEdit==false" @click="textEdit=true" class="text">{{ post.text }}</div>
      </template>

      <template v-slot:modal-footer="">
        <div class="comments-wrapper">
          <b-button @click="newComment()" variant="primary" class="newcomment">
            Create a new comment
          </b-button>
          <b-button @click="deleteThePost(post.id)" variant="danger" class="newcomment">
            Delete this post
          </b-button>
          <div class="comment" @click="dropComment(comment.commentid)" v-for="comment in post.comments" v-bind:key="comment.commentid">
            <p class="comment-author">{{comment.author}}</p>
            <p class="comment-text">{{comment.text}}</p>
          </div>
        </div>
      </template>
    </b-modal>

    <b-modal id="modal-newcomment" size="md" centered
    @cancel = "handleHidden"
    @hidden = "handleHidden"
    @ok = "handleOk"
    >
    <template v-slot:modal-header="{ }">
      Your name: <input type="text" class="authorinput" maxlength="36" v-model="authorPlaceholder" placeholder="Anonymous"/>
    </template>
    <template v-slot:default="{ }">
      <textarea class="textinput" maxlength="128" v-model="commentPlaceholder" placeholder="Write your comment here. . ."/>
    </template>
    </b-modal>

    <b-modal id="modal-newpost" size="lg" centered
    @cancel = "clearPostData"
    @hidden = "clearPostData"
    @ok = "createPost"
    >
    <template v-slot:modal-header="{ }">
      <input type="text" class="newpostText" maxlength="36" v-model="postTitlePlaceholder" placeholder="Title of your post"/>
    </template>
    <template v-slot:default="{ }">
      <textarea class="textinput" maxlength="60" v-model="postCaptionPlaceholder" placeholder="Caption of your post. Try to keep it brief."/>
      <textarea class="textinput" v-model="postTextPlaceholder" placeholder="Write your post here..."/>
    </template>
    </b-modal>
  </div>
</template>

<script>
import { bus } from '../main'
import { nanoid } from 'nanoid'

export default {
  name: 'Modal',
  props: {
    posts: {
      type: Array,
    }
  },
  data () {
    return {
      post: {},
      titleEdit: false,
      titlePlaceholder: '',
      textEdit: false,
      textPlaceholder: '',
      authorPlaceholder: '',
      commentPlaceholder: '',
      postTitlePlaceholder: '',
      postTextPlaceholder: '',
      postCaptionPlaceholder: ''
    }
  },
  created() {
    bus.$on('openThePost', data => {
      let result = this.posts.find(obj => {
        return obj.id === data
      })
      this.post = result;
      this.titlePlaceholder = this.post.title;
      this.textPlaceholder = this.post.text;
    });
  },
  methods: {
    dropComment: function(commentid) {
        let array = this.post.comments.filter(function( obj ) {
          return obj.commentid !== commentid;
        });
        this.post.comments = array;
        this.updateData();
    },
    resetModal: function() {
      this.titleEdit = false,
      this.textEdit = false,
      this.titlePlaceholder = '',
      this.textPlaceholder = ''
    },
    newComment: function() {
      this.$bvModal.show('modal-newcomment');
    },
    deleteThePost: function(index) {
        let array = this.posts.filter(function ( obj ) {
          return obj.id !== index;
        });
        this.posts = array;
        this.$bvModal.hide('modal-card');
        this.updateData();
    },
    handleOk: function() {
      this.post.comments.push({"author": this.authorPlaceholder, "commentid": nanoid(), "text": this.commentPlaceholder});
      this.handleHidden();
    },
    handleHidden: function() {
      this.authorPlaceholder = '';
      this.commentPlaceholder = '';
    },
    createPost: function() {
      this.posts.push({"id": nanoid(), "title": this.postTitlePlaceholder, "caption": this.postCaptionPlaceholder, "text": this.postTextPlaceholder, "comments": []});
      this.updateData();

      this.clearPostData();
    },
    clearPostData: function() {
      this.postTitlePlaceholder = '',
      this.postCaptionPlaceholder = '',
      this.postTextPlaceholder = ''
    },
    updateData: function() {
      window.localStorage.setItem('cards', JSON.stringify(this.posts));
      bus.$emit('loadData');
    }
  }
}
</script>

<style scoped>
.text {
  word-wrap: break-word;
}

.modal-header {
  display: flex;
  flex-direction: row;
  justify-content: center;
  align-items: center;
  align-self: center;
}

.title {
  font-size: 24px;
  min-width: 48px;
  min-height: 32px;
  width: 100%;
  background-color: rgba(255,255,255,0.9);
  cursor: pointer;
}

.text {
  cursor: pointer;
  min-width: 64px;
  min-height: 64px;
}

.textinput {
  width: 100%;

}

.modal-footer {
  display: flex;
  flex-direction: row;
  justify-content: flex-start;
}

.newcomment {
  margin-bottom: 16px;
  background-color: rgba(0,0,255,0.04);
  box-shadow: 0;
  border-radius: 12px;
  color: black;
  margin-right: 4px;
}

.comments-wrapper {
  width: 100%;
}

.comment {
  cursor: pointer;
  padding: 4px;
  transition: 0.3s;
  border: 1px solid rgba(0,0,0,0.09);
  margin: 2px;
}

.comment:hover {
  transition: 0.3s;
  background-color: rgba(255,0,0,0.12);
}
</style>
