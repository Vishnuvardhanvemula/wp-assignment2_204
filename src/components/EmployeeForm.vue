<script setup>
defineProps({
  form: {
    type: Object,
    required: true,
  },
  editing: {
    type: Boolean,
    default: false,
  },
  saving: {
    type: Boolean,
    default: false,
  },
})

defineEmits(['update:form', 'submit', 'reset'])
</script>

<template>
  <div class="card form-card shadow-sm border-0 h-100">
    <div class="card-body p-4">
      <h2 class="h4 mb-1">
        <i class="bi text-primary me-2" :class="editing ? 'bi-pencil-square' : 'bi-person-plus-fill'"></i>
        {{ editing ? 'Update Employee' : 'Add Employee' }}
      </h2>
      <p class="text-muted mb-4">Enter the employee details below and save them to MockAPI.</p>

      <form @submit.prevent="$emit('submit')">
        <div class="mb-3">
          <label class="form-label">Employee ID</label>
          <input
            :value="form.employeeId"
            class="form-control"
            type="text"
            placeholder="EMP001"
            required
            @input="$emit('update:form', { ...form, employeeId: $event.target.value })"
          />
        </div>

        <div class="mb-3">
          <label class="form-label">Name</label>
          <input
            :value="form.name"
            class="form-control"
            type="text"
            placeholder="John Doe"
            required
            @input="$emit('update:form', { ...form, name: $event.target.value })"
          />
        </div>

        <div class="mb-3">
          <label class="form-label">Designation</label>
          <input
            :value="form.designation"
            class="form-control"
            type="text"
            placeholder="Software Engineer"
            required
            @input="$emit('update:form', { ...form, designation: $event.target.value })"
          />
        </div>

        <div class="mb-3">
          <label class="form-label">Department</label>
          <input
            :value="form.department"
            class="form-control"
            type="text"
            placeholder="IT"
            required
            @input="$emit('update:form', { ...form, department: $event.target.value })"
          />
        </div>

        <div class="mb-4">
          <label class="form-label">Salary</label>
          <input
            :value="form.salary"
            class="form-control"
            type="number"
            min="0"
            step="0.01"
            placeholder="50000"
            required
            @input="$emit('update:form', { ...form, salary: $event.target.value })"
          />
        </div>

        <div class="d-grid gap-2 d-sm-flex">
          <button class="btn btn-primary px-4 shadow-sm" type="submit" :disabled="saving">
            <i class="bi" :class="saving ? 'bi-hourglass-split' : editing ? 'bi-save' : 'bi-plus-circle'"></i>
            {{ saving ? 'Saving...' : editing ? 'Update Employee' : 'Add Employee' }}
          </button>
          <button class="btn btn-light border px-4 shadow-sm" type="button" @click="$emit('reset')">
            <i class="bi bi-x-circle me-1"></i>Clear
          </button>
        </div>
      </form>
    </div>
  </div>
</template>