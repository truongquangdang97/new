<template>
  <div class="hello">
    <div>
      <h1>Lọc theo title và description</h1>
      <input type="text" placeholder="Nhập title" v-model="searchObj.title" >
      <input type="text" placeholder="Nhập  description" v-model="searchObj.description">
      <button @click="searchNew()">Search</button>
    </div>

    <table class="table">
      <thead>
        <tr>
          <th>Id</th>
          <th>Title</th>
          <th>image</th>
          <th>description</th>
          <th>category</th>
          <th>status</th>
          <th>createDate</th>
          <th>updateDate</th>
          <th>Action</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(item, index) in dataNew" :key="index">
          <th>{{ item.id }}</th>
          <th>{{ item.title }}</th>
          <th>{{ item.image }}</th>
          <th>{{ item.description }}</th>
          <th>{{ item.category }}</th>
          <th>{{ item.status }}</th>
          <th>{{ item.createDate }}</th>
          <th>{{ item.updateDate }}</th>
          <th>
            <button @click="deleteNew(item.id)">delete</button>
            <button @click="editNew(item.id)">edit</button>
          </th>
        </tr>
      </tbody>
    </table>
    <div>
      <button v-if="obj.id > 0" @click="updateNew(obj.id)">Update</button>
      <button v-else @click="addNew()">Open</button>
      <label for="">Title</label>
      <input type="text" v-model="obj.title" />

      <label for="">image</label>
      <input type="text" v-model="obj.image" />

      <label for="">description</label>
      <input type="text" v-model="obj.description" />

      <label for="">category</label>
      <input type="text" v-model="obj.category" />

      <label for="">status</label>
      <input type="text" v-model="obj.status" />
    </div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  name: "HelloWorld",
  props: {
    msg: String,
  },
  data() {
    return {
      dataNew: [],
      obj: {
        title: "",
        image: "",
        description: "",
        category: "",
        status: "",
      },
      action: false,
      searchObj:{
        title:'',
        description:''
      }
    };
  },
  created() {
    this.all();
  },
  methods: {
    searchNew(){
      axios
        .get("http://localhost:8080/api/v1/news?title="+this.searchObj.title+"&"+"description="+this.searchObj.description)
        .then((response) => (this.dataNew = response.data));
    },

    all() {
      axios
        .get("http://localhost:8080/api/v1/news")
        .then((response) => (this.dataNew = response.data));
    },
    addNew() {
      axios
        .post("http://localhost:8080/api/v1/news", this.obj)
        .then((response) => {
          console.log(response);
          this.all();
          this.obj = [];
        });
    },
    deleteNew(id) {
      console.log(id);
      axios
        .delete("http://localhost:8080/api/v1/news/" + id)
        .then((response) => {
          console.log(response);
          this.all();
          this.obj = [];
        });
    },
    editNew(id) {
      axios.get("http://localhost:8080/api/v1/news/" + id).then((response) => {
        this.obj = response.data;
        console.log(this.obj);
      });
    },
    updateNew(id) {
      axios
        .put("http://localhost:8080/api/v1/news/" + id, this.obj)
        .then((response) => {
          console.log(response);
          this.all();
          this.obj = [];
        });
      this.obj = [];
    },
  },
};
</script>


<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
