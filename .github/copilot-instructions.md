# Copilot Instructions for Appointy Codebase

## Overview & Architecture

- **Monorepo Structure:**  
  - `backend/`: Node.js/Express REST API for users, doctors, and admin.
  - `frontend/`: React SPA for end-users (patients).
  - `admin/`: React SPA for admin and doctor dashboards.

- **Core Data Models:**  
  - `userModel.js`, `doctorModel.js`, `appointmentModel.js` in `backend/models/`
  - MongoDB is used for persistence, with Mongoose schemas.

- **Authentication:**  
  - JWT-based for users, doctors, and admin.
  - Tokens are passed in headers (`token`, `dToken`, `aToken`) and validated by custom middlewares (`authUser.js`, `authDoctor.js`, `authAdmin.js`).

- **Media & Payments:**  
  - Images are uploaded to Cloudinary (see `.env` and `userController.js`/`adminController.js`).
  - Payments are handled via Razorpay integration in user flows.

## Developer Workflows

- **Backend:**
  - Start: `npm run dev` (assumes nodemon or similar is set up).
  - Environment: Configure `.env` in `backend/` with MongoDB, Cloudinary, Razorpay, and admin credentials.
  - MongoDB connection logic is in `backend/config/mongodb.js`.
  - API endpoints are grouped by user type in `routes/` and use controller logic from `controllers/`.

- **Frontend/Admin:**
  - Start: `npm start` in `frontend/` or `admin/`.
  - Environment: Set `VITE_BACKEND_URL` in `.env` for API base URL.
  - Uses React Context (`AppContext`, `AdminContext`, `DoctorContext`) for global state and API calls.

## Project-Specific Patterns

- **Context-Driven State:**  
  - All global state (auth, user data, doctors, appointments) is managed via React Context Providers.
  - Example: `AppContext.jsx` in `frontend/src/context/` exposes `token`, `userData`, `doctors`, and utility functions.

- **API Integration:**  
  - All API calls use Axios, with tokens injected via headers.
  - Error and success feedback is handled via `react-toastify`.

- **Slot Booking Logic:**  
  - Appointment slots are generated dynamically in the frontend (`Appointment.jsx`) and validated against `slots_booked` in the doctor model.
  - Double-booking is prevented by backend checks.

- **File Uploads:**  
  - Uses `multer` middleware in backend for handling file uploads (profile images).
  - Images are uploaded to Cloudinary and URLs are stored in MongoDB.

- **Admin/Doctor Panel:**  
  - Role-based routing and UI in `admin/` (see `Navbar.jsx`, `Sidebar.jsx`).
  - Contexts (`AdminContext`, `DoctorContext`) manage dashboard data, appointments, and profile updates.

## Integration Points

- **Cloudinary:**  
  - Used for all image uploads (see `.env` and controllers).
- **Razorpay:**  
  - Used for appointment payments (see payment endpoints in `userController.js`).
- **React Router:**  
  - Used for navigation in both `frontend/` and `admin/`.

## Conventions

- **Token Naming:**  
  - `token` for user, `dToken` for doctor, `aToken` for admin.
- **API Route Prefixes:**  
  - `/api/user/`, `/api/doctor/`, `/api/admin/` for clear separation.
- **Component Patterns:**  
  - Use concise, context-driven components with Tailwind CSS for styling.
  - All forms use controlled components and context state.

## Key Files/Directories

- `backend/controllers/` — All business logic for users, doctors, admin.
- `frontend/src/context/AppContext.jsx` — Main context for user app.
- `admin/src/context/AdminContext.jsx`, `admin/src/context/DoctorContext.jsx` — Contexts for admin/doctor panels.
- `frontend/src/pages/Appointment.jsx` — Slot booking logic.
- `backend/middlewares/` — Auth and file upload middlewares.

---

**Feedback Request:**  
If any workflow, integration, or convention is unclear or missing, please specify so this guide can be further refined for maximum productivity.
