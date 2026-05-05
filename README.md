# Employee Management System

A Vue 3 + Axios + Bootstrap employee management app with CRUD operations backed by MockAPI.

## Features

- Add, view, update, and delete employee records
- Responsive Bootstrap UI
- Axios-powered API integration
- Clean component-based Vue structure

## Employee Fields

- Employee ID
- Name
- Designation
- Department
- Salary

## Setup

1. Install dependencies:

```bash
npm install
```

2. The app is configured to use `https://69f78f71dd0c226688edd0e6.mockapi.io/Employees` directly in [src/App.vue](src/App.vue).

3. Run the app:

```bash
npm run dev
```

## MockAPI Resource

Create a resource named `employees` with the fields above. The app expects the endpoint to support standard CORS requests.