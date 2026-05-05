<script>
import axios from 'axios'
import EmployeeForm from './components/EmployeeForm.vue'
import EmployeeTable from './components/EmployeeTable.vue'

const EMPLOYEES_API_URL = 'https://69f78f71dd0c226688edd0e6.mockapi.io/Employees'

const api = axios.create({
  baseURL: EMPLOYEES_API_URL,
  headers: {
    'Content-Type': 'application/json',
  },
})

function getErrorMessage(error) {
  return error?.response?.data?.message || error?.message || 'Request failed.'
}

export default {
  name: 'App',
  components: {
    EmployeeForm,
    EmployeeTable,
  },
  data() {
    return {
      employees: [],
      loading: false,
      saving: false,
      error: '',
      success: '',
      editingRecordId: null,
      form: {
        employeeId: '',
        name: '',
        designation: '',
        department: '',
        salary: '',
      },
    }
  },
  mounted() {
    this.loadEmployees()
  },
  methods: {
    resetMessages() {
      this.error = ''
      this.success = ''
    },
    clearForm() {
      this.form = {
        employeeId: '',
        name: '',
        designation: '',
        department: '',
        salary: '',
      }
      this.editingRecordId = null
    },
    async loadEmployees() {
      this.loading = true
      this.resetMessages()

      try {
        const { data } = await api.get('')
        this.employees = data
      } catch (requestError) {
        this.error = getErrorMessage(requestError) || 'Failed to load employees.'
      } finally {
        this.loading = false
      }
    },
    startEdit(employee) {
      this.editingRecordId = employee.id
      this.form = {
        employeeId: employee.employeeId ?? '',
        name: employee.name ?? '',
        designation: employee.designation ?? '',
        department: employee.department ?? '',
        salary: employee.salary ?? '',
      }
      this.resetMessages()
    },
    async submitEmployee() {
      this.resetMessages()
      this.saving = true

      const payload = {
        employeeId: this.form.employeeId.trim(),
        name: this.form.name.trim(),
        designation: this.form.designation.trim(),
        department: this.form.department.trim(),
        salary: Number(this.form.salary),
      }

      try {
        if (this.editingRecordId !== null) {
          await api.put(`/${this.editingRecordId}`, payload)
          this.success = 'Employee updated successfully.'
        } else {
          await api.post('', payload)
          this.success = 'Employee added successfully.'
        }

        this.clearForm()
        await this.loadEmployees()
      } catch (requestError) {
        this.error = getErrorMessage(requestError) || 'Unable to save employee record.'
      } finally {
        this.saving = false
      }
    },
    async handleDelete(employee) {
      const confirmed = window.confirm(
        `Delete employee ${employee.name}? This action cannot be undone.`,
      )

      if (!confirmed) {
        return
      }

      this.resetMessages()
      try {
        await api.delete(`/${employee.id}`)
        this.success = 'Employee deleted successfully.'
        if (this.editingRecordId === employee.id) {
          this.clearForm()
        }
        await this.loadEmployees()
      } catch (requestError) {
        this.error = getErrorMessage(requestError) || 'Unable to delete employee.'
      }
    },
  },
}
</script>

<template>
  <main class="page-shell">
    <section class="hero-banner">
      <div class="container py-5">
        <div class="row align-items-center g-4">
          <div class="col-lg-7">
            <p class="eyebrow mb-2">Employee Management System</p>
            <h1 class="display-5 fw-bold text-white mb-3">Manage employee records with Vue.js and MockAPI</h1>
            <p class="lead text-white-75 mb-0">
              Add, update, view, and delete employees from a responsive Bootstrap interface powered by Axios.
            </p>
          </div>
          <div class="col-lg-5">
            <div class="stats-card shadow-lg">
              <div class="d-flex justify-content-between align-items-center mb-3">
                <span class="text-uppercase small text-muted fw-semibold"><i class="bi bi-people-fill me-2"></i>Records</span>
                <span class="badge text-bg-primary rounded-pill"><i class="bi bi-cloud-check me-1"></i>Live API</span>
              </div>
              <div class="display-6 fw-bold">{{ employees.length }}</div>
              <div class="text-muted">employees synced from MockAPI</div>
            </div>
          </div>
        </div>
      </div>
    </section>

    <section class="container pb-5">
      <div v-if="error" class="alert alert-danger shadow-sm">{{ error }}</div>
      <div v-if="success" class="alert alert-success shadow-sm">{{ success }}</div>

      <div class="row g-4">
        <div class="col-lg-4">
          <EmployeeForm
            v-model:form="form"
            :editing="editingRecordId !== null"
            :saving="saving"
            @submit="submitEmployee"
            @reset="clearForm"
          />
        </div>
        <div class="col-lg-8">
          <EmployeeTable
            :employees="employees"
            :loading="loading"
            @edit="startEdit"
            @delete="handleDelete"
          />
        </div>
      </div>
    </section>
  </main>
</template>