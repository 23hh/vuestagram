<template>
  <div class="header">
    <ul class="header-button-left">
      <li @click="step = 0">Cancel</li>
    </ul>
    <ul class="header-button-right">
      <li v-if="step == 1" @click="step++">Next</li>
      <li v-if="step == 2" @click="publish">acount</li>
    </ul>
    <img src="./assets/logo.png" class="logo" alt="logo" />
  </div>

  <Container @write="myContent = $event" :img="img" :step="step" :data="data" />
  <!-- <div>
    <p> {{ $store.state.more }} </p>
    <button @click="$store.dispatch('getData')"> More </button>
  </div> -->
  <div>
    <button @click="more">More</button>
  </div>
  <div class="footer">
    <ul class="footer-button-plus">
      <input @change="upload" type="file" id="file" class="inputfile" />
      <label for="file" class="input-plus">+</label>
    </ul>
  </div>

</template>

<script>
import Container from "./components/GramContainer.vue";
import data from "../data.js";
import axios from "axios";

export default {
  name: 'App',
  data() {
    return {
      data,
      moreData : 0,
      step: 0,
      img : "",
      myContent : "",
      selectedFilter: "",
    }
  },
  mounted() {
    this.emitter.on('clickFilter', (a) => {
      this.selectedFilter = a
    })
  },
  components: {
    Container,
  },
  methods : {
    publish() {
      let myContent = {
        name: "Kim Hyun",
        userImage: "https://placeimg.com/100/100/arch",
        postImage: this.img,
        likes: 36,
        date: "May 15",
        liked: false,
        content: this.myContent,
        filter: this.selectedFilter,
      };
      this.data.unshift(myContent);
      console.log(myContent);
      this.step = 0;
    },
    more() {
      axios.get(`https://codingapple1.github.io/vue/more${this.moreData}.json`)
      .then(res => {
        console.log(res.data);
          this.data.push(res.data);
          this.moreData++;
      });
    },
    upload(e){
      let file = e.target.files
      console.log(file);
      let url = URL.createObjectURL(file[0]);
      console.log(url);
      this.img = url;
      this.step++;
    }
  }
}

</script>

<style>
 
body {
  margin: 0;
}
ul {
  padding: 5px;
  list-style-type: none;
}
.logo {
  width: 22px;
  margin: auto;
  display: block;
  position: absolute;
  left: 0;
  right: 0;
  top: 13px;
}
.header {
  width: 100%;
  height: 40px;
  background-color: white;
  padding-bottom: 8px;
  position: sticky;
  top: 0;
}
.header-button-left {
  color: skyblue;
  float: left;
  width: 50px;
  padding-left: 20px;
  cursor: pointer;
  margin-top: 10px;
}
.header-button-right {
  color: skyblue;
  float: right;
  width: 50px;
  cursor: pointer;
  margin-top: 10px;
}
.footer {
  width: 100%;
  position: sticky;
  bottom: 0;
  padding-bottom: 10px;
  background-color: white;
}
.footer-button-plus {
  width: 80px;
  margin: auto;
  text-align: center;
  cursor: pointer;
  font-size: 24px;
  padding-top: 12px;
}
.sample-box {
  width: 100%;
  height: 600px;
  background-color: bisque;
}
.inputfile {
  display: none;
}
.input-plus {
  cursor: pointer;
}
#app {
  box-sizing: border-box;
  font-family: "consolas";
  margin-top: 60px;
  width: 100%;
  max-width: 460px;
  margin: auto;
  position: relative;
  border-right: 1px solid #eee;
  border-left: 1px solid #eee;
}

</style>