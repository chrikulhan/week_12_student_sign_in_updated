<template>
    <div>
<!--    template here-->

    <div class="alert alert-danger" v-show="errors.length > 0">
      <!--            have an array of potential error messages-->
      <!--            make an unordered array -->
      <ul>
        <li v-for="error in errors" v-bind:key="error">{{ error }}</li>
      </ul>

    </div>

    <div class="card add-student m-2 p-2">
      <h4 class="card-title">Add new student</h4>

      <div class="form-group">
        <label for="name">Name</label>
        <!-- TODOne v-model newStudentName -->
        <!--                add a modifier called trim to v-model: will trim white space from the beginning and end of any typed input-->
        <input id="name" class="form-control" v-model.trim="newStudentName">
      </div>
      <div class="form-group">
        <label for="starID">Star ID</label>
        <!-- TODOne v-model newStarID -->
        <input id="starID" class="form-control" v-model.trim="newStarID">
      </div>
      <!-- TODOne v-on:click event handler -->
      <!--            add activity to the button on the page with v-on:click-->
      <!--            then need to add the addStudent method in the let app Vue.createApp section below-->
      <button class="btn btn-primary" v-on:click="addStudent">Add</button>
    </div>
  </div>
</template>

<script>
export default {
  //create the component here:
  //whatever is created here "export" will be available to another
  //  javascript will read this file and "export default" means whatever is created
  //  here will be available to that other file.
  name: 'NewStudentForm',
  //list events ThisStudentForm.vue might emit:
  //emitting student-added event from way below
  emits: ['student-added'], //document the array of events this component emits
  data() {
    return {
      newStudentName: '',
      newStarID: '',
      errors: []
    }
  },
  methods: { //copied addStudent() from (WSS week9) vue-week-2-starter student_sign_in.html
    addStudent() { //this will tell App.vue component to manage the list of students
      this.errors = []
      if (!this.newStudentName) {
        this.errors.push('Student name must be input.')
      }

      if (!this.newStarID) {
        this.errors.push('StarId must be entered.')
      }

      if (this.errors.length === 0) {
        let student = { name: this.newStudentName, starID: this.newStarID, present: false }//    **add new student object onto this.students (the array that is Vue data)

      // Removed this.students.push(student) because this component isn't going to do this work.

      //TODO emit message to parent (App.vue) with new student,
      //app.vue will decide what to do with the new student.
      //will add to a list of students, that's not happening in
      //this NewStudentForm.vue

      //  vvv this is the message that will be sent to app.vue
        this.$emit('student-added', student)
      //  student = new student object created above when the errors === 0
      //  ^^^this.$emit(event/message-sent, argument/data)
      //  head to app.vue to prepare it to receive this message
      this.newStudentName = ''
      this.newStarID = ''
      }
    }
  }
}
</script>

<style scoped>
/*scoped means these component styles will only apply here, not on other components. */

</style>