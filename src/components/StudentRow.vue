<template>
<!--  copied tr from StudentTable (remove v-for)-->
<!--  <tr v-for="student in students" v-bind:class=" { present: student.present, absent: !student.present } ">-->
  <tr v-bind:class="{ present: student.present, absent: !student.present }">
    <td>{{ student.name }}</td>
    <td>{{ student.starID }}</td>
    <td>
      <input type="checkbox" v-bind="student.present" v-on:change="arrivedOrLeft(student, $event.target.checked)">
      <!-- after one-way data binding (v-bind), then we need to emit a message to a parent (App.vue)**Head down to methodsVVV-->
    </td>
<!--    this will import the delete icon from the assets folder in this project-->
<!--    add a v-on click element here to make the icon functional.-->
<!--    <td><img v-on:click="deleteStudent" src="@/assets/delete.png"></td>-->
<!--    show or not show this td depending on whether the edit box is toggled:-->
    <td v-show="edit">
      <img class="delete-icon" v-on:click="deleteStudent" src="@/assets/delete.png">
    </td>
  </tr>
</template>

<script>
  export default {
    //name: 'after the name of the file', 'piece of data")
    name: 'StudentRow',
    emits: ['student-arrived-or-left', 'delete-student'],
    props: {
      student: Object,
      edit: Boolean
    },
    methods: {
      arrivedOrLeft(student, present) {
        this.$emit('student-arrived-or-left', student, present)
      },
      //from v-on:click on delete icon (above)
      deleteStudent() {
        if (confirm(`Delete ${this.student.name}?`)) {
          //  needs to tell parent to delete student, but StudentTable.vue can't delete students either,
          //  but StudentTable can its parent (App.vue) and then it can delete the student.
          this.$emit('delete-student', this.student)
        }
      //  go to STudentTable.vue
      }
    }
  }
</script>

<style scoped>
/*removed styles from StudentTable.vue and put them here:*/
.present {
  color:grey;
  font-style: italic;
}

.absent {
  color: black;
  font-weight: bold;
}

.delete-icon {
  height: 20px;
}

</style>