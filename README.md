# Resumely --- Resume ATS Analyzer

Resumely is a full-stack resume analysis application that evaluates
resume content against Applicant Tracking System (ATS)-oriented
criteria. Users can provide resume text or upload a supported resume
file, optionally add a target job description, and review structured
feedback to identify strengths and areas for improvement.

## Screenshots

### Resume Analysis
![Resume Analysis](./Screenshot%202026-10-26%20195638.png)

### ATS Results
![ATS Results](./Screenshot%202026-10-26%20232310.png)

### Keyword Matching
![Keyword Matching](./Screenshot%202026-10-26%20232333.png)

## Features

-   **Resume input:** Upload a resume as PDF, DOCX, or TXT, or provide
    resume text.
-   **Job-description alignment:** Optionally compare resume content
    with a target job description.
-   **ATS-style score and verdict:** View a score out of 100 and a
    high-level assessment.
-   **Structured feedback:** Review a summary, strengths, and suggested
    improvements.
-   **Keyword matching:** See matched and missing keywords.
-   **Section-level review:** View scores and notes for evaluated resume
    sections.
-   **Accounts and protected analysis routes:** Signup/login endpoints
    and authenticated analysis endpoints.
-   **Saved analysis history:** Retrieve previous analyses and view an
    individual analysis.

> **Note:** The score and feedback are automated guidance, not a
> guarantee of ATS performance, interview selection, or hiring outcomes.
> Review suggestions carefully before changing a resume.

## Tech Stack

### Frontend

-   React
-   React DOM
-   React Router
-   Redux Toolkit and React Redux
-   esbuild
-   `live-server` for local development

### Backend

-   Node.js
-   Express
-   MongoDB with Mongoose
-   Google Generative AI SDK (Gemini) for structured resume analysis
-   `pdf-parse` for PDF text extraction
-   `mammoth` for DOCX text extraction
-   Multer for file uploads
-   JWT-based authentication and `bcryptjs` password hashing
-   Cloudinary integration for resume file storage
-   Helmet/security middleware, CORS, and rate limiting

## How It Works

1.  The user enters resume text or uploads a resume file.
2.  The user can optionally provide a job description for role-specific
    alignment.
3.  The frontend sends the analysis request to the Node.js/Express API.
4.  The backend extracts text from supported files and requests a
    structured ATS analysis from Gemini.
5.  The analysis response includes a score, verdict, summary, strengths,
    improvements, matched/missing keywords, and section-level notes.
6.  Authenticated users can access their saved analysis history.

## Project Structure

``` text
Resume_ATS_industry_architecture-main/
├── frontEnd/
│   ├── public/
│   └── src/
│       ├── components/       # Analyzer, authentication, dashboard, results UI
│       ├── services/         # Frontend API requests
│       ├── store/            # Redux state management
│       └── App.jsx
└── backEnd/
    ├── config/               # Database, environment, security, storage config
    ├── controllers/          # Authentication and analysis request handlers
    ├── middlewares/          # Authentication, uploads, error handling
    ├── models/               # User and analysis models
    ├── routes/               # REST API routes
    ├── services/             # Analysis, authentication, file storage
    └── index.js
```

## Prerequisites

Install the following before running the project:

-   Node.js and npm
-   MongoDB (local instance or a MongoDB connection URI)
-   A Gemini API key
-   Cloudinary credentials if using the configured cloud file-storage
    functionality

## Setup

### 1. Clone the repository

``` bash
git clone <your-repository-url>
cd Resume_ATS_industry_architecture-main
```

Replace `<your-repository-url>` with the URL of your GitHub repository.

### 2. Configure the backend

``` bash
cd backEnd
npm install
```

Create a `.env` file in the `backEnd/` directory using `.env.example` as
a reference:

``` env
PORT=4000
MONGO_URI=mongodb://127.0.0.1:27017
MONGO_DATABASE=resumely
JWT_SECRET=replace-with-a-long-random-secret
JWT_EXPIRES_IN=7d
CLIENT_ORIGIN=http://localhost:3000,http://127.0.0.1:3000
NODE_ENV=development
GEMINI_API_KEY=your-gemini-api-key
GEMINI_MODEL=gemini-3.5-flash
CLOUDINARY_API_KEY=your-cloudinary-api-key
CLOUDINARY_API_SECRET=your-cloudinary-api-secret
```

Use your own credentials. Do not commit `.env` or API keys to GitHub.
Confirm the Gemini model name supported by your account and API
configuration.

Start the backend:

``` bash
npm run dev
```

The API runs on port `4000` by default. Its root endpoint
(`http://localhost:4000/`) returns a basic service-status response.

### 3. Configure the frontend

Open a second terminal:

``` bash
cd frontEnd
npm install
npm run dev
```

The frontend development server runs on port `3000` by default. When
served on that port, the frontend API client targets the backend on port
`4000`.

### 4. Open the application

Visit:

``` text
http://localhost:3000
```

Make sure MongoDB is running, the backend environment variables are
configured, and both frontend and backend servers are active.

## API Overview

The backend exposes these main routes:

  -----------------------------------------------------------------------
  Method                  Endpoint                Purpose
  ----------------------- ----------------------- -----------------------
  `POST`                  `/api/auth/signup`      Create an account

  `POST`                  `/api/auth/login`       Log in

  `POST`                  `/api/auth/signin`      Login alias

  `POST`                  `/api/analyze`          Analyze resume
                                                  content/file
                                                  (authenticated)

  `GET`                   `/api/analyses`         Retrieve analysis
                                                  history (authenticated)

  `GET`                   `/api/analyses/:id`     Retrieve one saved
                                                  analysis
                                                  (authenticated)
  -----------------------------------------------------------------------

Analysis requests use `multipart/form-data` and can include `text`,
`jobDescription`, and an optional `resume` file. Protected endpoints
require a bearer token.

## Available Scripts

### Frontend (`frontEnd/`)

  -----------------------------------------------------------------------
  Command                             Description
  ----------------------------------- -----------------------------------
  `npm run dev`                       Start the frontend bundle watcher
                                      and local web server

  `npm run build`                     Build the frontend bundle
  -----------------------------------------------------------------------

### Backend (`backEnd/`)

  Command               Description
  --------------------- ---------------------------------------------
  `npm run dev`         Start the API server with automatic restart
  `npm start`           Start the API server
  `npm run seed:demo`   Run the demo-data seeding script

## Security Notes

-   Keep secrets in environment variables and out of source control.
-   Set a strong, unique `JWT_SECRET` before using the application
    beyond local development.
-   Configure `CLIENT_ORIGIN` for the frontend origins you actually
    trust.
-   Resume files contain personal information. Only upload documents you
    have permission to process, and use care when storing or sharing
    analysis results.

## Future Improvements

Potential next steps include expanding automated tests, improving error
and loading states, adding deployment documentation, and refining
accessibility and mobile responsiveness.

## License

No license information is specified here. Add a license file if you
intend to publish the project for reuse.
