# MongoDB Database Guide

This document describes the MongoDB setup and schema for the Task Manager backend.

## Database Setup

The backend uses MongoDB with Mongoose for data persistence.

### Environment

Add the following to `backend/.env`:

```env
MONGO_URI=your_mongodb_connection_string
JWT_SECRET=your_jwt_secret
PORT=5000
```

`MONGO_URI` should point to a MongoDB database instance. Example:

```env
MONGO_URI=mongodb://localhost:27017/task-manager
```

## Collections

The application uses two primary collections:

- `users`
- `tasks`

### `users`

Each user document stores account and authentication data.

#### Schema

- `_id`: ObjectId
- `name`: String, required
- `email`: String, required, unique
- `password`: String, hashed with bcrypt
- `createdAt`: Date
- `updatedAt`: Date

#### Example Document

```json
{
  "_id": "64abcd1234ef5678abcd9012",
  "name": "Aisha Khan",
  "email": "aisha@example.com",
  "password": "$2a$10$V...",
  "createdAt": "2026-06-15T12:00:00.000Z",
  "updatedAt": "2026-06-15T12:00:00.000Z"
}
```

### `tasks`

Each task document belongs to a user and contains task details.

#### Schema

- `_id`: ObjectId
- `user`: ObjectId reference to `users._id`
- `title`: String, required
- `description`: String
- `priority`: String, one of `Low`, `Medium`, `High`
- `dueDate`: Date
- `completed`: Boolean
- `createdAt`: Date
- `updatedAt`: Date

#### Example Document

```json
{
  "_id": "64abcd4567ef8901abcd2345",
  "user": "64abcd1234ef5678abcd9012",
  "title": "Finish project proposal",
  "description": "Write the final draft and send to the team.",
  "priority": "High",
  "dueDate": "2026-06-20T00:00:00.000Z",
  "completed": false,
  "createdAt": "2026-06-15T12:30:00.000Z",
  "updatedAt": "2026-06-15T12:30:00.000Z"
}
```

## Data Access Pattern

- `users` are created during registration.
- `tasks` are always created with a `user` reference.
- All task endpoints filter by `req.user._id` to enforce ownership.
- JWT middleware validates the token and loads the user object before task access.

## Indexes and Performance

Mongoose automatically creates a unique index on `email` for the `users` collection because the field is declared `unique`.

For production, add the following indexes manually if needed:

```js
db.users.createIndex({ email: 1 }, { unique: true });
db.tasks.createIndex({ user: 1 });
db.tasks.createIndex({ user: 1, completed: 1 });
```

## Notes

- Passwords are never stored in plain text.
- The app expects MongoDB to be running before the backend starts.
- If using a hosted MongoDB service, include credentials in the `MONGO_URI` securely.
