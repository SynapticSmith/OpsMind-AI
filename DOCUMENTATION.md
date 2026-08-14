# OpsMind AI - Comprehensive Documentation

Welcome to the OpsMind AI documentation! This guide is written for beginners and will take you step-by-step through setting up, running, and using the application from scratch.

## Table of Contents
1. [Introduction](#introduction)
2. [Prerequisites](#prerequisites)
3. [Setting Up the Backend](#setting-up-the-backend)
4. [Setting Up the Frontend](#setting-up-the-frontend)
5. [How to Use the Application](#how-to-use-the-application)
6. [Architecture & API Overview](#architecture--api-overview)

---

## Introduction

OpsMind AI is an AI-powered knowledge management system. It allows you to upload Standard Operating Procedure (SOP) documents (PDFs) and ask questions about them in natural language. The system retrieves accurate answers strictly derived from your uploaded documents.

---

## Prerequisites

Before starting, make sure you have the following installed on your computer:

1. **Node.js**: The environment needed to run the JavaScript code. Download and install it from [nodejs.org](https://nodejs.org/). (We recommend the LTS version).
2. **Git**: To clone the repository if you haven't already. Download from [git-scm.com](https://git-scm.com/).
3. **MongoDB Atlas Account**: A cloud database to store user accounts and document data. You can sign up for free at [mongodb.com/cloud/atlas](https://www.mongodb.com/cloud/atlas).
4. **Google Gemini API Key**: The AI model used to process the text. Get a free API key from [Google AI Studio](https://aistudio.google.com/). *(Note: The application uses the OpenAI SDK configured to point to Gemini's endpoints).*

---

## Setting Up the Backend

The backend is built with Node.js, Express, and MongoDB. It handles user authentication, PDF uploads, text extraction, and AI queries.

### 1. Open your terminal and navigate to the Backend folder
```bash
cd Backend
```

### 2. Install dependencies
Run the following command to download all required packages:
```bash
npm install
```

### 3. Configure Environment Variables
The backend needs a connection to your database and your AI API key.
1. In the `Backend` folder, create a new file named `.env` (if it does not exist) or edit the existing one.
2. Open `.env` in a text editor and add the following template:

```env
# MongoDB Connection String (Replace <username>, <password>, and cluster URL with your actual MongoDB Atlas details)
MONGO_URI=mongodb+srv://<username>:<password>@cluster0.example.mongodb.net/?retryWrites=true&w=majority&appName=Cluster0

# Google Gemini API Key (Replace with your actual key from Google AI Studio)
OPENAI_API_KEY=your_google_gemini_api_key

# Port for the backend server (Optional, defaults to 3000)
PORT=3000
```

**How to get the `MONGO_URI`:**
- Log in to MongoDB Atlas.
- Create a new Database Cluster (the free tier works fine).
- Go to "Database Access" and create a database user with a password.
- Go to "Network Access" and allow IP address `0.0.0.0/0` (so you can connect from anywhere).
- Click "Connect" on your cluster, choose "Drivers", and copy the connection string. Replace `<password>` with the password you created.

### 4. Start the Backend Server
Run the following command:
```bash
npm start
```
You should see a message in the terminal saying:
```text
MongoDB connected
Server running on port 3000
```
Keep this terminal window open!

---

## Setting Up the Frontend

The frontend is a user interface built with React and Vite.

### 1. Open a *new* terminal window
Keep your backend terminal running and open a fresh terminal.

### 2. Navigate to the Frontend folder
```bash
cd sop-agent-frontend
```

### 3. Install dependencies
Run this command to install the necessary packages for the frontend:
```bash
npm install
```

### 4. Start the Frontend Server
Run the following command:
```bash
npm run dev
```
The terminal will provide a local URL, typically `http://localhost:5173`. Open this URL in your web browser.

---

## How to Use the Application

Once both the backend and frontend are running, follow these steps to use OpsMind AI:

### 1. Register an Account
- When you open `http://localhost:5173` in your browser, you will see the **Register** screen.
- Enter an email and a password.
- Submit the form to create your account.

### 2. Log In
- After registering, you will be directed to the **Login** screen.
- Enter the email and password you just created to log in.

### 3. Upload an SOP (PDF)
- After logging in, you will be taken to the Chat/Upload interface.
- Use the **Upload Panel** to select a PDF file from your computer.
- Click upload. The system will read the PDF, break it into chunks, and securely store it in the database.

### 4. Chat and Ask Questions
- Use the **Chat Window** to ask questions about the document you uploaded.
- For example, if you uploaded a company policy manual, you can type: *"What is the policy on remote work?"*
- The AI will analyze the uploaded documents and provide a direct answer, completely based on the PDF content.

---

## Architecture & API Overview

If you're a developer wanting to connect other applications, here is a quick overview of the Backend REST API (running on `http://localhost:3000`):

### Authentication
- `POST /api/auth/register`: Expects `{ "email": "...", "password": "..." }`. Creates a new user.
- `POST /api/auth/login`: Expects `{ "email": "...", "password": "..." }`. Authenticates a user.

### Files & Queries
- `POST /api/file`: Accepts a `multipart/form-data` upload with a file field named `pdf`. Uploads the document to MongoDB GridFS, extracts text, chunks it, creates embeddings via Gemini, and stores them in the database.
- `POST /api/query`: Expects `{ "question": "..." }`. Converts your question into an embedding, performs a vector similarity search against the stored PDF chunks, and returns the most relevant text chunks as the answer.

---

**Happy Knowledge Managing!** If you encounter any issues, ensure both servers (Frontend and Backend) are running simultaneously in separate terminal windows.
