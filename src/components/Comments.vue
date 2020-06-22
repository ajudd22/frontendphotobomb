<template>
  <div id = "comments">
  <form class="pure-form" @submit.prevent="createComment" v-if="user">
    <legend>Leave a comment:</legend>
      <fieldset>
        <textarea v-model="newComment"></textarea>
          <br />
          <button class="pure-button space-right">Cancel</button>
          <button class="pure-button pure-button-primary" type="submit">Submit</button>
      </fieldset>
    </form>
    <div class = "comment" v-for= "comment in comments" v-bind:key="comment._id">
      <h3 class = "comment-text">{{comment.comment}}</h3><h5 class = "user-name"> - {{comment.user.firstName}}  {{comment.user.lastName}}</h5>
      <h6 class = "comment-date">{{formatDate(comment.created)}}</h6>
    </div>
  </div>
</template>

<script>
import axios from 'axios';
import moment from 'moment';
  export default {
    name: 'Comments',
    data() {
      return{
        comments: null,
        newComment: '',
        error: '',
        imgID: this.$route.params.id,
      }
    },
    props: ['photoID'],
    created () {
      this.getComments();
    },
    computed: {
      user() {
        return this.$root.$data.user;
      }
    },
    methods: {
      formatDate(date) {
        if (moment(date).diff(Date.now(), 'days') < 15)
          return moment(date).fromNow();
        else
          return moment(date).format('d MMMM YYYY');
      },
      async createComment() {
        try{
          let response =  await axios.post("/api/comments/" + this.$route.params.id, {comment: this.newComment});
          this.getComments();
          this.newComment ="";
        } catch (error) {
          this.error = "Error: " + error.response.data.message;
          console.log(this.error);
        }
      },
      async getComments() {
        try{
          let response = await axios.get("/api/comments/" + this.$route.params.id);
          this.comments = response.data;
          console.log(response.data);
        } catch(error) {
          console.log(error);
        }
      },
    },
};
</script>
<style>
.user-name {
margin-left: 40px;
padding:0px;
}
comment-text{
margin:0px;
}

.comment{
width: 100%;
border: .1px solid grey;
margin: 5px;
padding:5px;
}

</style>
