<template>
  <div>
<!--    copied and pasted the table from vue-week-2-starter-student_sign_in.html-->
    <div class="card student-list m-2 p-2">
      <h4 class="card-title">Student List</h4>

      <div class="edit-table-toggle form-check">
        <input id="edit-table" type="checkbox" class="form-check-input" v-model="editTable">
        <label for="edit-table" class="form-check-label">Edit table?</label>
<!--        go to data (after props) below-->
      </div>

      <div id="student-table">
        <table class="table">
          <tr>
            <th>Name</th>
            <th>StarID</th>
            <th>Present?</th>
<!--            in order to select this whole table row when the edit checkbox is toggled, can show or not show Delete:-->
<!--            <th>Delete</th>-->
            <th v-show="editTable">Delete</th>
          </tr>
<!--          copied this table row to StudentRow.vue, now change tr to student-row:-->
<!--          <tr v-for="student in students" v-bind:class=" { present: student.present, absent: !student.present } ">-->
<!--            <td>{{ student.name }}</td>-->
<!--            <td>{{ student.starID }}</td>-->
<!--&lt;!&ndash;            <td> vvv <input type="checkbox" v-model="student.present" v-on:change="arrivedOrLeft(student)"></td>&ndash;&gt;-->
<!--&lt;!&ndash;            remove v-model (used in the input in StudentTable)-->
<!--                **v-model has 2 way data binding, if the checkbox changes, then whatever is v-modeled also changes  :&ndash;&gt;-->
<!--&lt;!&ndash;            *change v-model to v-bind (one way data binding) (to bind the checked property of a checkbox to student.present.)-->
<!--                *$event.target is the html component that caused this event (checkbox), .checked (is the checkbox checked or not (will be true or false).&ndash;&gt;-->
<!--             <td>-->
<!--               <input type="checkbox" v-bind="student.present" v-on:change="arrivedOrLeft(student, $event.target.checked)">-->
<!--&lt;!&ndash;               after one-way data binding (v-bind), then we need to emit a message to a parent (App.vue)**Head down to methodsVVV&ndash;&gt;-->
<!--             </td>-->
          <StudentRow
              v-for="student in students" v-bind:key="student.name"
              v-bind:student="student"
              v-bind:edit="editTable"
              v-on:student-arrived-or-left="arrivedOrLeft"
              v-on:delete-student="deleteStudent">
            <!-- v-bind make the delete image not visible when the edit checkbox is not checked. -->
<!--            ^^^from studentRow.vue where the deleted student event happened:-->
<!--              v-bind:class=" { present: student.present, absent: !student.present } ">-->
<!--            remove all table data things because they will be in the table rowsvvv-->
<!--            <td>{{ student.name }}</td>-->
<!--            <td>{{ student.starID }}</td>-->
<!--               <td>-->
<!--              <input type="checkbox" v-bind="student.present" v-on:change="arrivedOrLeft(student, $event.target.checked)">&lt;!&ndash;               after one-way data binding (v-bind), then we need to emit a message to a parent (App.vue)**Head down to methodsVVV&ndash;&gt;-->
<!--            </td>-->
          </StudentRow>
        </table>
      </div>
    </div>
  </div>
</template>

<script>

//list all the child components:

import StudentRow from "@/components/StudentRow";

export default {
  //create the component here:
  //whatever is created here "export" will be available to another
  //  javascript will read this file and "export default" means whatever is created
  //  here will be available to that other file.
  name: 'StudentTable', //student table is NOT in charge of getting a list of students or creating a list,
  //App.vue will manage the list (the array of students) and will provide that data to student table using a prop.
  //StudentTable's job will be to display that list of students in a table.

  components: { StudentRow },
    //^^^this will let StudentTable know it has a StudentRow component
  //vvv from the method: arrivedOrLeft(student, present) {this.$emit('student-arrived-or-left', student, present)
  emits: ['student-present', 'delete-student'],
    data() {
      return {
        editTable: false
      }
    },
  //then go to app.vue, student-table.
    props: {
      //this will be the type of data, which is an array.
      students: Array
    },
//  add the arrivedOrLeft method
    methods: {
      // arrivedOrLeft() {
      arrivedOrLeft(student, present) {
        //  what should happen when a box is checked or unchecked? Should studentTable deal with it? NO!!
        //  studentTable's parent (App.vue) will deal with it
        //  TODOn emit message to parent.(after v-binding the checked box above)
        //  present is: is the box checked or not (from above:"arrivedOrLeft(student, $event.target.checked)")
        this.$emit('student-present', student, present)
        //  an event with a name student-arrived-or-left will be emitted, and it will carry with it a
        //    payload (data) with the status of student object (name and starID) and whether they are present
      },
      deleteStudent(student){
        this.$emit('delete-student', student)
        //      ^^^passing argument onto the parent, go to App.vue
      }
    }
}
</script>

<style>
/*scoped means these component styles will only apply here, not on other components. */
/*remove the styles and move them to StudentRow.vue*/
/*.present {*/
/*  color:grey;*/
/*  font-style: italic;*/
/*}*/

/*.absent {*/
/*  color: black;*/
/*  font-weight: bold;*/
/*}*/

#student-table {
  max-height: 500px;
  overflow: scroll;
}

.present-true {
  color: grey;
  font-style: italic;
}

.present-false {
  font-weight: bold;
}

</style>