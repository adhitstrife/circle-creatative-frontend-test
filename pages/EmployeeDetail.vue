<template>
  <div class="main">
    <Navbar />
    <p v-if="$fetchState.pending">Fetching mountains...</p>
    <div class="p-4" v-if="employee.data">
      <b-form clas="p-4" v-if="employee" @submit.prevent="submit">
        <b-form-group id="full_name" label="Name:" label-for="full_name">
          <b-form-input
            id="full_name"
            v-model="form.full_name"
            placeholder="Enter name"
            required
          ></b-form-input>
        </b-form-group>

        <b-form-group
          id="birth_place"
          label="Birth Place:"
          label-for="birth_place"
        >
          <b-form-input
            id="birth_place"
            v-model="form.birth_place"
            placeholder="Ex : Makassar"
            required
          ></b-form-input>
        </b-form-group>

        <b-form-group
          id="birth_date"
          label="Birth Date:"
          label-for="birth_date"
        >
          <b-form-datepicker
            id="birth_date"
            v-model="form.birth_date"
            required
          ></b-form-datepicker>
        </b-form-group>

        <b-form-group id="phone" label="Phone Number:" label-for="phone">
          <b-form-input
            id="phone"
            v-model="form.phone"
            required
          ></b-form-input>
        </b-form-group>

        <b-form-group
          id="department"
          label="Department:"
          label-for="department"
        >
          <b-form-select id="input-3" v-model="form.department_id" required>
            <b-form-select-option value="0"
              > - </b-form-select-option
            >
            <b-form-select-option v-for="department in departments.data" :key="department.id" :value="department.id"
              >{{ department.department_name }}</b-form-select-option
            >
          </b-form-select>
        </b-form-group>

        <b-form-group
          id="department"
          label="Position:"
          label-for="department"
        >
          <b-form-select id="input-3" v-model="form.position_id" required>
            <b-form-select-option value="0"
              > - </b-form-select-option
            >
            <b-form-select-option v-for="position in positions.data" :key="position.id" :value="position.id"
              >{{ position.position_name }}</b-form-select-option
            >
          </b-form-select>
        </b-form-group>

        <b-button type="submit" variant="primary">Submit</b-button>
        <b-button type="reset" variant="danger">Reset</b-button>
      </b-form>
    </div>
  </div>
</template>

<script>
import { EMPLOYEE_SERVICE, DEPARTMENT_SERVICE, POSITION_SERVICE } from '@/constants'
export default {
  name: 'EmployeeDetailPage',
  data() {
    return {
      employee: {},
      departments: [],
      positions: [],
      form: {
        full_name: '',
        birth_place: '',
        birth_date: '',
        phone: '',
        department_id: 0,
        position_id: '',
      },
    }
  },
  created() {},
  async fetch() {
    const employeeId = this.$route.query.employeeId
    this.employee = await fetch(
      process.env.BASE_URL + EMPLOYEE_SERVICE + '/' + employeeId
    ).then((resp) => resp.json())

    this.departments = await fetch(
      process.env.BASE_URL + DEPARTMENT_SERVICE
    ).then((resp) => resp.json())

    this.positions = await fetch(
      process.env.BASE_URL + POSITION_SERVICE
    ).then((resp) => resp.json())

    
    this.employee.data.department === 0 ? this.form.department_id = 0 : this.form.department_id = this.employee.data.department.id
    this.form.full_name = this.employee.data.full_name
    this.form.birth_place = this.employee.data.birth_place
    this.form.birth_date = this.employee.data.birth_date
    this.form.phone = this.employee.data.phone
    this.employee.data.position === 0 ? this.form.position_id = 0 : this.form.position_id = this.employee.data.position.id
    console.log(this.employee.data.phone)
  },
  methods: {
    submit() {
        this.$axios.$put(process.env.BASE_URL + EMPLOYEE_SERVICE + '/' + this.employee.data.id, {
            data: this.form
        }).then((resp) => {
            console.log(resp)
        }).catch((err) => {
            console.log(err)
        })
    },
    reset() {},
  },
}
</script>
