# health-monitoring-system
A smart health monitoring system for tracking patient vitals, alerts, and medical data in real time.
```javascript id="9ukw1a"
// ===============================
// HEALTH MONITORING SYSTEM
// FULL STACK SINGLE FILE PROJECT
// ===============================

// -----------------------------------
// BACKEND - Node.js + Express + MongoDB
// File: server.js
// -----------------------------------

const express = require("express");
const mongoose = require("mongoose");
const cors = require("cors");

const app = express();

app.use(cors());
app.use(express.json());

// MongoDB Connection
mongoose.connect("mongodb://127.0.0.1:27017/health-monitoring-system", {
  useNewUrlParser: true,
  useUnifiedTopology: true,
});

// Schema
const patientSchema = new mongoose.Schema({
  patientName: String,
  heartRate: Number,
  temperature: Number,
  oxygenLevel: Number,
});

// Model
const Patient = mongoose.model("Patient", patientSchema);

// GET API
app.get("/api/patients", async (req, res) => {
  const patients = await Patient.find();
  res.json(patients);
});

// POST API
app.post("/api/patients", async (req, res) => {
  const patient = new Patient(req.body);
  await patient.save();
  res.json(patient);
});

// Server
const PORT = 5000;

app.listen(PORT, () => {
  console.log(`Backend running on http://localhost:${PORT}`);
});


// -----------------------------------
// FRONTEND - React App
// File: App.js
// -----------------------------------

import React, { useEffect, useState } from "react";
import axios from "axios";
import "./App.css";

function App() {

  const [patients, setPatients] = useState([]);

  const [formData, setFormData] = useState({
    patientName: "",
    heartRate: "",
    temperature: "",
    oxygenLevel: ""
  });

  // Fetch Data
  const fetchPatients = async () => {
    const response = await axios.get(
      "http://localhost:5000/api/patients"
    );

    setPatients(response.data);
  };

  useEffect(() => {
    fetchPatients();
  }, []);

  // Handle Input
  const handleChange = (e) => {
    setFormData({
      ...formData,
      [e.target.name]: e.target.value
    });
  };

  // Submit Form
  const handleSubmit = async (e) => {
    e.preventDefault();

    await axios.post(
      "http://localhost:5000/api/patients",
      formData
    );

    setFormData({
      patientName: "",
      heartRate: "",
      temperature: "",
      oxygenLevel: ""
    });

    fetchPatients();
  };

  return (
    <div className="container">

      <h1>Health Monitoring System</h1>

      <form onSubmit={handleSubmit}>

        <input
          type="text"
          name="patientName"
          placeholder="Patient Name"
          value={formData.patientName}
          onChange={handleChange}
          required
        />

        <input
          type="number"
          name="heartRate"
          placeholder="Heart Rate"
          value={formData.heartRate}
          onChange={handleChange}
          required
        />

        <input
          type="number"
          name="temperature"
          placeholder="Temperature"
          value={formData.temperature}
          onChange={handleChange}
          required
        />

        <input
          type="number"
          name="oxygenLevel"
          placeholder="Oxygen Level"
          value={formData.oxygenLevel}
          onChange={handleChange}
          required
        />

        <button type="submit">
          Add Patient
        </button>

      </form>

      <table>

        <thead>
          <tr>
            <th>Patient</th>
            <th>Heart Rate</th>
            <th>Temperature</th>
            <th>Oxygen Level</th>
          </tr>
        </thead>

        <tbody>

          {patients.map((patient) => (

            <tr key={patient._id}>
              <td>{patient.patientName}</td>
              <td>{patient.heartRate}</td>
              <td>{patient.temperature}</td>
              <td>{patient.oxygenLevel}%</td>
            </tr>

          ))}

        </tbody>

      </table>

    </div>
  );
}

export default App;


// -----------------------------------
// CSS - App.css
// -----------------------------------

body {
  margin: 0;
  font-family: Arial, sans-serif;
  background-color: #f4f7fc;
}

.container {
  width: 80%;
  margin: auto;
  padding: 20px;
}

h1 {
  text-align: center;
  color: #1565c0;
}

form {
  display: grid;
  gap: 10px;
  margin-bottom: 20px;
}

input {
  padding: 12px;
  border: 1px solid #ccc;
  border-radius: 5px;
}

button {
  padding: 12px;
  background-color: #1565c0;
  color: white;
  border: none;
  border-radius: 5px;
  cursor: pointer;
}

button:hover {
  background-color: #0d47a1;
}

table {
  width: 100%;
  border-collapse: collapse;
  background: white;
}

th,
td {
  border: 1px solid #ddd;
  padding: 12px;
  text-align: center;
}

th {
  background-color: #1565c0;
  color: white;
}


// -----------------------------------
// REQUIRED INSTALLATION COMMANDS
// -----------------------------------

// FRONTEND
// npm install axios

// BACKEND
// npm install express mongoose cors nodemon


// -----------------------------------
// RUN COMMANDS
// -----------------------------------

// START MONGODB
// mongod

// RUN BACKEND
// node server.js

// RUN FRONTEND
// npm start


// -----------------------------------
// FEATURES
// -----------------------------------

// 1. Add Patient Health Details
// 2. Monitor Heart Rate
// 3. Monitor Temperature
// 4. Monitor Oxygen Level
// 5. Store Data in MongoDB
// 6. REST API Integration
// 7. Real-Time Dashboard


// -----------------------------------
// FUTURE IMPROVEMENTS
// -----------------------------------

// - Login Authentication
// - AI Health Prediction
// - IoT Sensor Integration
// - Email Notifications
// - Graph Analytics
// - Admin Dashboard
// - Cloud Deployment
```
