<template>
  <div id="app">
<!--    vvv came from $emit in NewStudentForm, use the name used in this string
        in NewStudentForm (this.$emit('student-added', student)-->
<!--    when the v-on:student-added happens, call a method in app.vue called-->
    <new-student-form v-on:student-added="newStudentAdded"> </new-student-form>
<!--    in order to get the data to show up on the webpage, you have to get the app.vue (parent)
        to connect the StudentTable's prop (students) has to be provided by the parent of
         StudentTable(App.vue) using v-bind.vvv
        (name of prop and the data are the same here, but they don't have to be.-->
<!--    <student-table v-bind:students="students"> </student-table>-->
<!--    just came from StudentTable, emits:, show the events we expect to receive using
        v-on: (name of event, what does App.vue do when that event happens?)  -->
    <student-table
        v-bind:students="students"
        v-on:student-present="studentArrivedOrLeft"
        v-on:delete-student="studentDeleted">
    </student-table>
<!--    now add method StudentArrivedOrLeft in App.vue below:-->
<!--    then add deleteStudent to methods-->

    <!--vvv        v-bind = "name of prop in StudentMessage" = "data from App.vue"-->
    <student-message
        v-bind:student="mostRecentStudent">
    </student-message>

 </div>
</template>

<script>
import NewStudentForm from './components/NewStudentForm.vue'
import StudentMessage from './components/StudentMessage.vue'
import StudentTable from './components/StudentTable.vue'

export default {
  name: 'App',
  components: {
    NewStudentForm,
    StudentMessage,
    StudentTable
  },
  //needs to be a function that returns an object:, will use the data from newStudentAdded above:
  data() {
    return {
      //empty array to start with vvv:
      students: [],
    //  from studentMessage student: Object:
      mostRecentStudent: {}
    //  then update studentArrivedOrLeft below VV
    }
  },
  //then add a method to be called when the message is emitted from New Student Form, added to app.vue above,
  //and then the array students: [] will have something in it.
    methods: {
      newStudentAdded(student) {
        //this will push the data to the array
        this.students.push(student)
      // sort the students:vv
        this.students.sort(function(s1, s2) {
          // return minus or plus one depending on the order of the newly added student name
          //( ? -1 : 1 )
          return s1.name.toLowerCase() < s2.name.toLowerCase() ? -1 : 1
        })
      },
      // now add method StudentArrivedOrLeft in App.vue from <student-table> above in app.vue
      // will have 2 arguments:
      studentArrivedOrLeft(student, present) {
      //    this will arrive from StudentTable, go through above <student-table> and be unpacked here in the
      //    same order (student, present) as listed in the method (studentArrivedOrLeft(student,present))
      //  todo find student in array of students
      //  todo update their present attribute
      //  use the find function in javascript (https://developer.mozilla.org/en-US/docs/Web/javaScript/reference/global_Objects/array/find)
      //  to find the first matching element in an array
      //  here expecting only one matching student (or zero if there is an error)
      //  student = s, return true if that's the student we're looking for.
        let updateStudent = this.students.find( function(s){
          //student is the argument
          if (s.name === student.name && s.starID === student.starID){
          //  this is the student we were looking for , so return true:
            return true
          }
        })

        if (updateStudent) {
          updateStudent.present = present
          // from mostRecentStudent above:
          this.mostRecentStudent = student
        //  now tell studentMessage about the updated student in the template using v-bind (way at the top)
        }
      },
      studentDeleted(student) {
        this.students = this.students.filter( function(s){
        //  filter returns a new array of all students for whom the function returns true.
        if (s != student) {
          return true
        }
      })
    //  ^^^filter has a function as an argument (s), it will check every student in the students array
    //    if the student matches a condition the student will be kept.
    //  if the student doesn't match the condition, they will be filtered out **removed.
    //    todo clear the welcome and goodbye message:
        this.mostRecentStudent =''
    }
  }
}
</script>

<style>
/*to apply styles from bootstrap to the whole page, go here: https://getbootstrap.com/docs/4.6/getting-started/introduction/
snag the href from the css listed:
This will import all the bootstrap styles, so it looks a lot like the original student sign in page we made.
Import Boostrap into App.vue using the CSS @import rule
https://www.w3schools.com/cssref/pr_import_rule.asp

*/

@import "https://cdn.jsdelivr.net/npm/bootstrap@4.6.0/dist/css/bootstrap.min.css";

</style>
