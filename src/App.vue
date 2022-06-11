<template>
  <div class="container">
    <form @submit.prevent class="card input">
      <h2>Add new post</h2>
      <select class="select" id="type" v-model="type">
        <option value="title">Title</option>
        <option value="text">Text</option>
        <option value="pic">Picture</option>

      </select>
      <KeepAlive>
        <app-title @passTitle="addTitle" v-if="type === 'title'"></app-title>
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
      <div class="post" v-for="post in posts" :key="post">
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


let postsAll = [{
  "title": "Lorem Ipsum",
  "date": "Fri Jul 02 2021",
  "items": [{
    "value": "Lorem ipsum, dolor sit amet consectetur adipisicing elit. Voluptatem ipsum veritatis odio maxime molestiae suscipit perspiciatis ratione sequi natus, laudantium nisi exercitationem non? Optio voluptatem corporis sunt expedita reiciendis ut dolores autem officiis veritatis. Non modi at expedita nisi corporis, aut tempore suscipit quas maxime praesentium quo nesciunt quaerat laudantium?",
    "type": "text"
  }, {
    "value": "https://images.pexels.com/photos/10890741/pexels-photo-10890741.jpeg?auto=compress&cs=tinysrgb&w=1260&h=750&dpr=1",
    "type": "url"
  }, {
    "value": "Sed ut perspiciatis unde omnis iste natus error sit voluptatem accusantium doloremque laudantium, totam rem aperiam, eaque ipsa quae ab illo inventore veritatis et quasi architecto beatae vitae dicta sunt explicabo. Etiam dui sem, fermentum vitae, sagittis id, malesuada in, quam. Integer imperdiet lectus quis justo. Etiam dictum tincidunt diam. Nunc auctor. Cras pede libero, dapibus nec, pretium sit amet, tempor quis. Aliquam ante. Etiam dui sem, fermentum vitae, sagittis id, malesuada in, quam. Proin in tellus sit amet nibh dignissim sagittis. Morbi scelerisque luctus velit. Etiam dui sem, fermentum vitae, sagittis id, malesuada in, quam. Etiam sapien elit, consequat eget, tristique non, venenatis quis, ante. Phasellus et lorem id felis nonummy placerat. Maecenas lorem. Maecenas libero. Nullam justo enim, consectetuer nec, ullamcorper ac, vestibulum in, elit. Integer pellentesque quam vel velit. Pellentesque habitant morbi tristique senectus et netus et malesuada fames ac turpis egestas. Fusce dui leo, imperdiet in, aliquam sit amet, feugiat eu, orci.",
    "type": "text"
  }, {
    "value": "https://images.pexels.com/photos/11937310/pexels-photo-11937310.jpeg?auto=compress&cs=tinysrgb&w=1260&h=750&dpr=1",
    "type": "url"
  }, {
    "value": "https://images.pexels.com/photos/770016/pexels-photo-770016.jpeg?auto=compress&cs=tinysrgb&w=1260&h=750&dpr=1",
    "type": "url"
  }, {
    "value": "Duis condimentum augue id magna semper rutrum. Duis pulvinar. Aenean fermentum risus id tortor. Fusce wisi. Duis condimentum augue id magna semper rutrum. Fusce wisi.",
    "type": "text"
  }]
}];



export default {
  data() {
    return {
      postIs: false,
      type: 'title',
      title: '',
      postItems: [],
      posts: postsAll,
      date: '',




    }
  },
  mounted() {

  },
  methods: {
    addTitle(value) {
      this.title = value;
      this.postIs = true;

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
      this.posts.unshift(data);
      this.postData();
      this.title = '';
      this.postItems = '';
      this.postIs = false
    },
    async postData() {
      const url = '././public/posts.json';
      const data = {
        title: this.title,
        date: this.date,
        items: this.postItems,
      };
      let formDataJS = JSON.stringify(data);
      let fetchOpt = {
        method: "POST",
        headers: {
          "Content-Type": "application/json",
          Accept: "application/json",
        },
        body: formDataJS
      };
      let res = await fetch(url, fetchOpt);
      return res.status(404).json()

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
