# Entryway: Indian Tourism Site Booking

A full-stack MERN web app for browsing Indian tourist sites and booking entry tickets. Visitors can browse and search sites by category and add tickets to a cart. Admins manage categories and sites, including photos, ticket prices and opening hours.

## Features

**Visitors**
- Browse tourist sites, filter by category and price, and page through results
- Search by keyword, view site details and see related sites
- Add tickets to a cart
- Register, log in, reset a password with a security answer, and manage a profile and bookings from a user dashboard

**Admins**
- Admin dashboard behind role-based route protection
- Create, update and delete categories
- Create, update and delete sites with name, description, address, ticket price, open and close times, and a photo
- View registered users

## Tech stack

| Layer | Tech |
|---|---|
| Frontend | React 18, React Router v6, Ant Design, Axios, Context API (auth, cart, search), react-hot-toast |
| Backend | Node.js, Express (ES modules), express-formidable for photo uploads, morgan |
| Database | MongoDB with Mongoose |
| Auth | JWT + bcrypt, `requireSignIn` / `isAdmin` middleware |

## API overview (`/api/v1`)

| Area | Endpoints |
|---|---|
| Auth | `POST /auth/register`, `POST /auth/login`, `POST /auth/forgot-password`, `GET /auth/user-auth`, `GET /auth/admin-auth`, `PUT /auth/profile` |
| Category | `POST /category/create-category`, `PUT /category/update-category/:id`, `GET /category/get-category`, `GET /category/single-category/:slug`, `DELETE /category/delete-category/:id` |
| Site | `POST /site/create-site`, `PUT /site/update-site/:pid`, `GET /site/get-site`, `GET /site/get-site/:slug`, `GET /site/site-photo/:pid`, `DELETE /site/delete-site/:pid`, `POST /site/site-filters`, `GET /site/site-count`, `GET /site/site-list/:page`, `GET /site/search/:keyword`, `GET /site/related-sites/:pid/:cid`, `GET /site/site-category/:slug` |

Create, update and category-delete routes need a signed-in admin. `DELETE /site/delete-site/:pid` isn't protected yet; see Known issues below.

## Project structure

```
server.js         # Express entry point
config/db.js      # MongoDB connection
Controllers/      # auth, category, site
models/           # User, Category, Site schemas
routes/           # auth, category, site routers
middlewares/      # JWT + admin checks
helpers/          # password hashing
client/           # React frontend (Create React App)
```

## Getting started

**Prerequisites:** Node.js and a local MongoDB running on `mongodb://localhost:27017`. The connection string is set in `config/db.js`.

1. Create a `.env` file in the project root:

   ```env
   PORT=8080
   DEV_MODE=development
   JWT_SECRET=your_secret
   ```

2. Create `client/.env`:

   ```env
   REACT_APP_API=http://localhost:8080
   ```

3. Install and run:

   ```bash
   npm install
   npm install --prefix client
   npm run dev        # runs the API (nodemon) and the React client together
   ```

The API runs on http://localhost:8080 and the client on http://localhost:3000. The client proxies API requests to port 8080.

## Known issues

- `DELETE /api/v1/site/delete-site/:pid` is missing the `requireSignIn` and `isAdmin` middleware.
- The MongoDB URI is hardcoded in `config/db.js` and should come from `.env`.
- Checkout and payment (Razorpay/Braintree packages are installed) and the bookings page are not finished yet.
