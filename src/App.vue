<template>
  <div id="app">
    <nav>
      <ul>
        <li><a href="#" >Home</a></li>
        <li><a href="#">About</a> </li>
      </ul>
    </nav>
    <div class="course-list">
      <h2>Courses</h2>
      <div class="" v-for="course in courses" :key="course.id">
        <p @click="setEdit(course)" v-if="!editing">
          {{course.name}}
        </p>
        <div class="" v-else>
          <input type="text" v-model="course.name">
          <button @click="saveCourse(course)">Save</button>
          <button @click="removeCourse(course)">Remove</button>
        </div>
      </div>
    </div>
    <div class="add-course" v-if="!editing">
      <input type="text" v-model="courseName" name="" placeholder="Course Name">
      <button @click="addCourse(courseName)">Add</button>
    </div>
  </div>
</template>

<script>

import axios from 'axios'
export default {
  name: 'App',
  components: {

  },
  data(){
    return {
      ROOT_URL:'http://localhost:3000/courses',
      courses:[],
      courseName:"",
      editing:false,
    }
  },
  mounted(){
    this.getCourseList();
  },
  methods:{
    getCourseList(){
      axios.get(this.ROOT_URL)
      .then(response=>{
        this.courses= response.data;
      })
      .catch(err=>console.log(err))
    },
    addCourse(name){
      axios.post(this.ROOT_URL,{name})
      .then(response=>{
        this.courses.push(response.data);
        this.courseName=""
      })
      .catch(err=>console.log(err))
    },
    setEdit(course){
      this.editing =!this.editing;
      console.log(course);
    },
    saveCourse(course){
      axios.put(`${this.ROOT_URL}/${course.id}`,{...course})
      .then(response=>{
        console.log(response.data);
      })
      .catch(err=>console.log(err))
    },
    removeCourse(course){
      axios.delete(`${this.ROOT_URL}/${course.id}`)
      .then(response=>{
        console.log(response.data);
        this.setEdit();
        this.courses=this.courses.filter(c=>c.id !=course.id)
      })
      .catch(err=>console.log(err))
    }
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 5px;
}
.course-list{
  text-align: center;
  padding-top: 10px;

}
.add-course{
  margin-top:5px;
}

nav li{
  display: inline;
  margin-left: 10px;
}
nav li a{
  color: #fff;
  text-decoration: none;
}
nav{
  display: flex;
  justify-content: flex-start;
  width: 100%;
  background-color: black;
  color:#fff;
  margin:1px;
}
body{
  /* background: #777; */
  margin:5px;
}
</style>
