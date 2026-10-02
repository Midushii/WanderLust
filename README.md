<div align="center">

# WanderLust ✈️

A full-stack travel stay-listing platform where hosts can publish properties and travellers can explore, search, filter and review destinations around the world.

[![Live Site](https://img.shields.io/badge/Live-wanderlust--wtus.onrender.com-ff385c?style=for-the-badge&logo=render&logoColor=white)](https://wanderlust-wtus.onrender.com/listings)
![Node](https://img.shields.io/badge/Node.js-339933?style=for-the-badge&logo=node.js&logoColor=white)
![Express](https://img.shields.io/badge/Express-4.x-000000?style=for-the-badge&logo=express&logoColor=white)
![MongoDB](https://img.shields.io/badge/MongoDB-Atlas-47A248?style=for-the-badge&logo=mongodb&logoColor=white)
![Bootstrap](https://img.shields.io/badge/Bootstrap-5.3-7952B3?style=for-the-badge&logo=bootstrap&logoColor=white)
![Render](https://img.shields.io/badge/Deployed%20on-Render-46E3B7?style=for-the-badge&logo=render&logoColor=white)

**🌐 [wanderlust-wtus.onrender.com](https://wanderlust-wtus.onrender.com/listings)**

<img src="public/screenshots/1.png" width="860" alt="WanderLust listings page">

<em>The WanderLust listings page</em>

</div>

---

## Table of Contents

1. [Overview](#1-overview)
2. [Problem Statement](#2-problem-statement)
3. [Key Features](#3-key-features)
4. [Application Walkthrough](#4-application-walkthrough)
5. [System Architecture](#5-system-architecture)
6. [Technology Stack](#6-technology-stack)
7. [Database Design](#7-database-design)
8. [Repository Structure](#8-repository-structure)
9. [Developers](#9-developers)

---

## 1. Overview

**WanderLust** is a full-stack web application inspired by Airbnb. Hosts can list their homes, rooms, castles, farms or beachfront stays with photos and pricing, while travellers can browse listings, search by destination, filter by category, and leave star-rated reviews.

The platform is organised around four core modules:

| Module | Purpose |
|---|---|
| 🏠 **Listings** | Create, view, edit and delete property listings with image uploads |
| 🔍 **Explore & Filter** | Search by title or location and filter by 12 travel categories |
| ⭐ **Reviews** | Star ratings and comments on every listing |
| 🔐 **Authentication** | Secure signup, login and owner-only access control |

---

## 2. Problem Statement

Finding and sharing unique places to stay usually means juggling multiple apps, unreliable reviews and cluttered interfaces. Hosts also lack a simple way to publish and manage their own listings.

WanderLust addresses this by providing:

1. **One place to explore** stays across categories such as Mountains, Castles, Beach and Iconic Cities.
2. **A simple hosting flow** where any logged-in user can add a listing with a photo, price and location.
3. **Trustworthy feedback** through star-rated reviews tied to real user accounts.
4. **Safe ownership rules** so that only the owner can edit or delete a listing, and only the author can delete a review.

---

## 3. Key Features

### Listings
- Full **CRUD**: create, view, edit and delete listings
- Each listing has a title, description, image, price, location, country and category
- **Image upload** through Cloudinary (PNG, JPG, JPEG)
- Owner information displayed on every listing
- Reviews are **automatically deleted** when their listing is removed

### Explore & Filter
- **Search bar** matching listing title or location (case-insensitive)
- **12 category filters** with icons: Trending, Rooms, Iconic Cities, Mountains, Castles, Amazing Pools, Camping, Farms, Arctic, Domes, Boats and Beach
- Horizontally scrollable filter bar with arrow controls
- **"Total after taxes" toggle** that reveals the price including 18% GST

### Reviews & Ratings
- Interactive **1–5 star rating** input
- Written comments with form validation
- Reviews show the author's username and star rating
- Only the review author can delete their review

### Accounts & Access
- Signup and login using **Passport.js** (local strategy) with salted and hashed passwords
- Session-based authentication stored in MongoDB
- **Login required** to create listings or post reviews, with automatic redirect back to the original page after login
- **Owner-only** edit and delete permissions

### Experience
- Flash messages for success and error feedback
- Client-side and server-side validation
- Custom error page for invalid routes and failures
- Responsive layout built with Bootstrap 5

---

## 4. Application Walkthrough

### 4.1 Explore Listings

The home page displays every listing as a card with its image, title and price. A search bar and category filter strip sit at the top, and the **Total after taxes** switch shows prices with 18% GST.

<p align="center">
  <img src="public/screenshots/1.png" width="860" alt="Explore listings">
</p>

### 4.2 Listing Details

Each listing page shows the full description, owner, location, price and reviews. Owners see **Edit** and **Delete** buttons.

<p align="center">
  <img src="public/screenshots/2.png" width="860" alt="Listing details">
</p>

### 4.3 Reviews & Ratings

Logged-in users can leave a star rating and comment. All reviews are listed beneath the listing with the author's username.

<p align="center">
  <img src="public/screenshots/3.png" width="860" alt="Reviews and ratings">
</p>

### 4.4 Create a New Listing

Hosts fill in the title, description, category, price, country, location and upload an image. Validation runs on both client and server.

<p align="center">
  <img src="public/screenshots/4.png" width="860" alt="Create new listing">
</p>

### 4.5 Login & Signup

Secure account creation and login, with flash messages for feedback.

<p align="center">
  <img src="public/screenshots/5.png" width="860" alt="Login and signup">
</p>

---

## 5. System Architecture

WanderLust follows the **MVC (Model–View–Controller)** pattern with server-side rendering.

```
┌──────────────────────┐          ┌──────────────────────────────────┐
│       Browser        │          │       Render (Node + Express)    │
│  EJS pages ·         │ ───────► │                                  │
│  Bootstrap · JS      │          │  Routes ─► Middleware ─►         │
└──────────────────────┘          │  Controllers ─► Models ─► Views  │
                                  └───────────────┬──────────────────┘
                                                  │
                         ┌────────────────────────┼───────────────────────┐
                         ▼                        ▼                       ▼
                ┌─────────────────┐     ┌───────────────────┐   ┌────────────────────┐
                │  MongoDB Atlas  │     │    Cloudinary     │   │  Session Store     │
                │ users, listings │     │  listing images   │   │  (connect-mongo)   │
                │ reviews         │     │                   │   │                    │
                └─────────────────┘     └───────────────────┘   └────────────────────┘
```

**Request flow**

1. A request hits an Express route.
2. Middleware checks login status, ownership and validates the input with **Joi**.
3. The controller reads or writes data through **Mongoose** models.
4. Images are uploaded to **Cloudinary** through Multer.
5. An **EJS** view (with the shared boilerplate layout) renders the HTML response.

## 6. Technology Stack

**Frontend**
EJS · EJS-Mate (layouts) · Bootstrap 5.3 · CSS3 · Vanilla JavaScript · Font Awesome · Google Fonts (Plus Jakarta Sans)

**Backend**
Node.js · Express 4 · Passport.js (`passport-local`, `passport-local-mongoose`) · `express-session` · `connect-mongo` · `connect-flash` · `method-override` · `Joi` · `Multer` · `dotenv`

**Database**
MongoDB Atlas with Mongoose ODM

**Services & Hosting**
Render (deployment) · Cloudinary (image storage)

---

## 7. Database Design

Three Mongoose models define the data layer.

| Model | Fields |
|---|---|
| **User** | `email`, `username`, hashed `password` (managed by passport-local-mongoose) |
| **Listing** | `title`, `description`, `image {url, filename}`, `price`, `location`, `country`, `category`, `reviews[]`, `owner` |
| **Review** | `comment`, `rating (1–5)`, `createdAt`, `author` |

**Relationships**

```
User ──< Listing (owner)
Listing ──< Review (reviews[])
User ──< Review (author)
```

When a listing is deleted, a Mongoose post-hook removes all of its reviews, keeping the database clean. Input is validated with **Joi** schemas (`schema.js`) before it reaches the database.

---

## 8. Repository Structure

```
WanderLust/
│
├── app.js                    Express app, sessions, Passport and routes
├── schema.js                 Joi validation schemas
├── middleware.js             isLoggedIn, isOwner, isReviewAuthor, validators
├── cloudConfig.js            Cloudinary + Multer storage setup
│
├── models/
│   ├── listing.js            Listing schema
│   ├── review.js             Review schema
│   └── user.js               User schema (Passport plugin)
│
├── controllers/
│   ├── listings.js           Listing logic (CRUD, search)
│   ├── reviews.js            Review logic
│   └── users.js              Signup, login, logout
│
├── routes/
│   ├── listing.js            /listings routes
│   ├── review.js             /listings/:id/reviews routes
│   └── user.js               /signup, /login, /logout
│
├── views/
│   ├── layouts/boilerplate.ejs
│   ├── includes/             navbar, footer, flash messages
│   ├── listings/             index, show, new, edit
│   ├── users/                login, signup
│   └── error.ejs
│
├── public/
│   ├── css/                  style.css, rating.css (star ratings)
│   ├── js/                   script.js, map.js
│   └── screenshots/          README images
│
├── init/
│   ├── index.js              Database seeding script
│   └── data.js               Sample listings
│
├── utils/
│   ├── wrapAsync.js          Async error wrapper
│   └── ExpressError.js       Custom error class
│
└── package.json
```

---

## 9. Developer

**Built By**

| Name | Role |
|---|---|
| **Midushi Maheshwari** | B.Tech. Electronics and Communication Engineering (AI), IGDTUW |

Contact: [midushi.maheswari@gmail.com](mailto:midushi.maheswari@gmail.com)

---

**[Visit WanderLust →](https://wanderlust-wtus.onrender.com/listings)**
