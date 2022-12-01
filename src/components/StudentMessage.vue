<template>
  <div>

<!--    copied from: vue-week-2-starter-student-sign-in (just above script)-->
<!--    <div v-if="mostRecentStudent.name">-->
<!--    simplify:-->
    <div v-show="student.name">
      <div class="alert alert-success" v-if="student.present">
        Welcome, {{ student.name }}
      </div>
      <div v-else class="alert alert-info">
        <!--^^doesn't need a condition after it, v-else will happen if v-if doesn't.-->
        Goodbye, {{ student.name }}. See you later!
      </div>
    </div>
  </div>
</template>

<script>
export default {

  name: 'StudentMessage',
//  StudentMessage will be given a student to display a message to (welcome or goodbye)
  props: {
    student: Object
  //  now go to app.vue to add a new data in app.vue
  }
}
</script>

<style scoped>
/*scoped means these component styles will only apply here, not on other components. */

</style>