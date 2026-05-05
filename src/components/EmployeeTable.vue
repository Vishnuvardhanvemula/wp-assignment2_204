<script setup>
defineProps({
  employees: {
    type: Array,
    required: true,
  },
  loading: {
    type: Boolean,
    default: false,
  },
})

defineEmits(['edit', 'delete'])
</script>

<template>
  <div class="card shadow-sm border-0 h-100">
    <div class="card-body p-4">
      <div class="d-flex justify-content-between align-items-center mb-3">
        <div>
          <h2 class="h4 mb-1"><i class="bi bi-card-list text-primary me-2"></i>Employee List</h2>
          <p class="text-muted mb-0">Current records stored in the API.</p>
        </div>
      </div>

      <div v-if="loading" class="text-center py-5">
        <div class="spinner-border text-primary" role="status" aria-hidden="true"></div>
        <p class="mt-3 mb-0 text-muted">Loading employees...</p>
      </div>

      <div v-else-if="employees.length === 0" class="empty-state text-center py-5">
        <i class="bi bi-inbox fs-1 text-muted opacity-50 mb-3 d-block"></i>
        <p class="h5 mb-2">No employee records yet</p>
        <p class="text-muted mb-0">Use the form to create the first employee entry.</p>
      </div>

      <div v-else class="table-responsive">
        <table class="table align-middle table-hover mb-0">
          <thead class="table-light">
            <tr>
              <th>Employee ID</th>
              <th>Name</th>
              <th>Designation</th>
              <th>Department</th>
              <th class="text-end">Salary</th>
              <th class="text-end">Actions</th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="employee in employees" :key="employee.id">
              <td class="fw-semibold">{{ employee.employeeId }}</td>
              <td>{{ employee.name }}</td>
              <td>{{ employee.designation }}</td>
              <td>{{ employee.department }}</td>
              <td class="text-end">{{ Number(employee.salary).toLocaleString() }}</td>
              <td class="text-end">
                <div class="btn-group btn-group-sm shadow-sm" role="group">
                  <button class="btn btn-outline-primary" type="button" @click="$emit('edit', employee)" title="Edit">
                    <i class="bi bi-pencil-square"></i> Edit
                  </button>
                  <button class="btn btn-outline-danger" type="button" @click="$emit('delete', employee)" title="Delete">
                    <i class="bi bi-trash3"></i> Delete
                  </button>
                </div>
              </td>
            </tr>
          </tbody>
        </table>
      </div>
    </div>
  </div>
</template>