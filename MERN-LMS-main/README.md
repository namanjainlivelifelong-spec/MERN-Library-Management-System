# MERN-LMS

A full-stack **library management system** built with the MERN stack. An admin manages the book catalogue, members, and the lending workflow; members browse the catalogue and raise borrow/renewal requests that the admin approves.

## Stack

- **Client:** React 18, Vite, Redux Toolkit, React Router, Tailwind CSS + daisyUI, Formik + Yup, Axios, React Toastify
- **Server:** Node.js, Express, MongoDB (Mongoose), JWT, bcrypt
- **Auth:** JWT bearer tokens (7-day expiry), bcrypt-hashed passwords, role-based access (`Admin` / `Member`)

## Features

**Admin**

- Add, edit and delete books
- Register and remove members
- Lend books, receive returns, renew loans
- Approve or deny member borrow/renewal requests
- View all members and borrowings

**Member**

- Browse the catalogue
- Request a book or a loan renewal
- View past borrowings and change password

## Project structure

```
client/                React + Vite SPA
  src/actions/         Redux Toolkit slices (auth, book, borrowing, member, request)
  src/pages/admin/     Admin screens
  src/pages/member/    Member screens
  src/components/      Shared UI + feature modals
server/                Express API
  config/db.js         MongoDB connection
  controllers/         Route handlers
  middleware/          JWT auth + role guards
  models/              Mongoose schemas (User, Book, Borrowing, Request)
  routes/              API route definitions
```

## Data model

- **User** — `name`, `email` (unique), `password` (hashed), `role` (`Admin` | `Member`)
- **Book** — `title`, `author`, `category` (`Non-fiction` | `Helping-book` | `Novel`), `isbn`, `refId`
- **Borrowing** — `userId`, `bookId`, `issueDate`, `deadline`, `fine`, `renewed`
- **Request** — `userId`, `bookId`/`borrowingId`, `requestType` (`Borrow` | `Renew`)

## API

All routes are prefixed with `/api`; protected routes need `Authorization: Bearer <token>`.

| Method | Endpoint | Access | Description |
| --- | --- | --- | --- |
| POST | `/auth` | public | Log in → user + JWT |
| PUT | `/auth/:id` | protected | Change password |
| GET | `/auth/getUser` | protected | Current user |
| GET | `/users` | admin | List members |
| POST | `/users` | admin | Register a member |
| DELETE | `/users/:id` | admin | Remove a member |
| GET | `/books` | protected | List books |
| POST | `/books` | admin | Add a book |
| PUT | `/books/:id` | admin | Edit a book |
| DELETE | `/books/:id` | admin | Delete a book |
| GET | `/borrowings` | admin | List borrowings |
| POST | `/borrowings` | admin | Lend a book |
| PUT | `/borrowings/:id` | admin | Renew a borrowing |
| DELETE | `/borrowings/:id` | admin | Receive a return |
| POST | `/requests` | member | Make a borrow/renewal request |
| GET | `/requests` | admin | List requests |
| PUT | `/requests/:id/approve` | admin | Approve a request |
| DELETE | `/requests/:id/deny` | admin | Deny a request |

## Getting started

**Prerequisites:** Node.js 18+, a MongoDB instance (local or Atlas)

**Server**

```bash
cd server
npm install
```

`server/.env`:

```
PORT=5000
MONGO_URI=mongodb://127.0.0.1:27017/mern-lms
JWT_SECRET=your_jwt_secret
```

```bash
node index.js        # or: npx nodemon index.js
```

**Client**

```bash
cd client
npm install
npm run dev
```

The dev server proxies `/api` to the backend (`client/vite.config.js`).
