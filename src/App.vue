<template>
  <h1>Blog About Web</h1>
  <div class="container">
    <form @submit.prevent class="card input">
      <h2>Add new post</h2>
      <select class="select" id="type" v-model="type">
        <option value="title">Title</option>
        <option value="text">Text</option>
        <option value="pic">Picture</option>

      </select>
      <KeepAlive>
        <app-title @clearTitle="clear" @passTitle="addTitle" v-if="type === 'title'"></app-title>
      </KeepAlive>
      <app-text @passText="addText" v-if="type === 'text'"></app-text>
      <app-pic @passPicture="addPicture" v-if="type === 'pic'"></app-pic>


      <app-button @click="submit" type="submit" title="Publish Post"></app-button>


    </form>
    <div class="card">
      <div v-if="postIs">
        <h2>EDITOR</h2>
        <h3>{{ title }}</h3>
        <div class="post" v-for="(item, id) in postItems" :key="id">
          <div class="pic" v-if="item.type === 'url'">
            <img :src="item.value" />
          </div>
          <div v-else>{{ item.value }}</div>
          <div @click="removeItem(id)" class="close"></div>
        </div>
      </div>
      <h2>Published Posts</h2>
      <div class="post" v-for="post in newArrayOfPosts" :key="post">
        <h3>{{ post.title }}</h3>
        <div v-for="item in post.items" :key="item">
          <div class="pic" v-if="item.type === 'url'">
            <img :src="item.value" />
          </div>
          <div v-else>{{ item.value }}</div>

        </div>
        <div class="date">{{ post.date }}</div>
      </div>

    </div>



  </div>

</template>

<script>
import AppTitle from "./components/AppTitle";
import AppText from "./components/AppText";
import AppPic from "./components/AppPic";
import AppButton from "./components/AppButton.vue";
import axios from 'axios'



export default {
  data() {
    return {
      postIs: false,
      type: 'title',
      title: '',
      postItems: [],
      posts: [],
      date: '',
      used: false, 
      newArrayOfPosts: []
      




    }
  },
  mounted() {
      this.loadPosts();
    
  },
  methods: {
    arrayReverseObj(obj) {
      let newArray = []
      Object.keys(obj)
        .sort()
        .reverse()
        .forEach(key => {
         
          newArray.push(
            { ...obj[key], key: key }
          )
        })

     
      return newArray
    },
    clear() {
      this.title = '';
      this.used = false;
      if(this.postItems.length === 0)
      {
          this.postIs = false;
      }


    },
    addTitle(value) {
      this.title = value;
      this.postIs = true;
      this.used = true;

    },
    addText(text) {
      this.postItems.push({ value: text, type: 'text' }
      )

      this.postIs = true;

    },
    addPicture(url) {
      this.postItems.push({ value: url, type: 'url' })

      this.postIs = true;

    },
    removeItem(id) {
      this.postItems.splice(id, 1);

    },
    submit() {
      this.date = new Date().toDateString();

      const data = {
        title: this.title,
        date: this.date,
        items: this.postItems,
      };
      
      this.addPost(data);
      this.newArrayOfPosts.unshift(data);
      this.title = '';
      this.postItems = '';
      this.postIs = false
    },
    async addPost(post) {
      const axios = require('axios');
       await axios.post('https://vue-blog-fad0e-default-rtdb.europe-west1.firebasedatabase.app/posts.json', post);
    }, 
    async loadPosts() {
      try {
        const { data } = await axios.get('https://vue-blog-fad0e-default-rtdb.europe-west1.firebasedatabase.app/posts.json', )
        this.posts = data;
        this.newArrayOfPosts = this.arrayReverseObj(this.posts)
        
      } catch(e) {
        console.log(e)
      }
    }

  },
  provide() {
    return {
      used: this.used
    }
  },
  components: {
    AppTitle,
    AppText,
    AppPic,
    AppButton

  }, computed: {

  }

}
</script>

<style scoped>
.post {
  font-family: Arial, Helvetica, sans-serif;
  color: rgb(54, 54, 54);
  font-size: 0.9rem;
}

.date {
  text-align: right;
  font-size: 0.8rem;
  color: rgb(95, 95, 95);
  margin-top: 50px;
}

h3 {
  font-size: 1.2rem;
}

button {
  margin-top: 30px;
  background-color: rgb(54, 77, 75);
  border-color: azure;
  color: azure;
  font-weight: 300;
}

button:hover {
  color: rgb(54, 77, 75);
  background-color: #fff;
  border-color: rgb(54, 77, 75);

}
</style>
