<template>
  <div class="main">
    <Navbar />
    <div class="table p-4">
      <b-table striped hover :items="employees.data" :fields="fields">
        <template #cell(actions)="row">
          <b-button :to="{ path: '/EmployeeDetail', query: {employeeId: row.item.id}}" size="sm" variant="primary">
            Edit
          </b-button>
          <b-button size="sm" variant="danger">
            Delete
          </b-button>
        </template>
      </b-table>
    </div>
  </div>
</template>

<script>
import { EMPLOYEE_SERVICE } from '@/constants'
export default {
  name: 'IndexPage',
  data: () => ({
    employees: [],
    fields: [
      'Full Name',
      'Birth Place',
      'Birth Date',
      'Phone Number',
      'Actions',
    ],
  }),
  async fetch() {
    this.employees = await fetch(process.env.BASE_URL + EMPLOYEE_SERVICE).then(
      (res) => res.json()
    )
  },
}
</script>
