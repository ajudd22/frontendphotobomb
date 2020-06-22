<template>
  <div class = "photo">
  <div class = "photo-container">
    <img class = "photo-img" :src= this.path />
    <div class = "right-panel">
      <div class = "photo-info">
        <h3>{{title}}</h3>
        <p>{{description}}</p>
        </div>
    <Comments :photoID = this.path />
    </div>
  </div>
  </div>
</template>

<script>
import axios from 'axios';
import Comments from '@/components/Comments.vue';

  export default{
    name: 'Photo',
    components: {
      Comments,
    },
    data (){
      return{
        headingMessage: "",
        photoID: this.$route.params.id,
        photo: null,
        path: '',
        title:"",
        description:""
      }
    },
    created(){
      this.getPhoto();
    },
    methods:{
    async getPhoto() {
      let response = await axios.get("/api/photos/" + this.photoID);
      this.photo = response.data[0];
      this.description = this.photo.description;
      this.title = this.photo.title;
      this.path = this.photo.path;
    }
    }
  }
</script>
<style scoped>
.photo-container{
display:flex;
flex-direction:row;
align-items:center;
}
.photo-img{
width:50%;
padding-top:10px;
padding-bottom:10px;
}
.right-panel{
margin-top:30px;
padding-left:40px;
width:100%;
}
h3{
font-size:36px;
}

</style>
