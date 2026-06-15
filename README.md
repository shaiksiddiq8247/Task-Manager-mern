# Task Manager Frontend

This folder contains the React frontend for the Task Manager application.

## Features

- Responsive login and registration screens
- Protected dashboard route for authenticated users
- Task creation, editing, deletion, and completion toggling
- Search, filter, and sort task list
- Task statistics display
- Toast notifications and loading states

## Setup

1. Install dependencies:

```bash
cd frontend
npm install
```

2. Start the React app:

```bash
npm start
```

## Notes

- The frontend is configured to call the backend API at `/api/`.
- The app stores the authenticated user and token in `localStorage`.
- Use the token in the `Authorization: Bearer <token>` header for API requests.
