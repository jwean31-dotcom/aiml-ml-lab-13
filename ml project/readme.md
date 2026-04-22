import cv2
import mediapipe as mp
import numpy as np
import time

# Initialize MediaPipe FaceMesh
mp_face_mesh = mp.solutions.face_mesh
face_mesh = mp_face_mesh.FaceMesh(max_num_faces=1)
mp_draw = mp.solutions.drawing_utils

# Thresholds
EAR_THRESHOLD = 0.22      # Eye Aspect Ratio
MAR_THRESHOLD = 0.7       # Mouth Aspect Ratio
PERCLOS_THRESHOLD = 0.4   # 40%

# Indices for landmarks
LEFT_EYE = [33, 160, 158, 133, 153, 144]
RIGHT_EYE = [362, 385, 387, 263, 373, 380]
MOUTH = [61, 291, 39, 181, 0, 17, 267, 269]

def euclidean_dist(p1, p2):
    return np.linalg.norm(np.array(p1) - np.array(p2))

def eye_aspect_ratio(eye):
    A = euclidean_dist(eye[1], eye[5])
    B = euclidean_dist(eye[2], eye[4])
    C = euclidean_dist(eye[0], eye[3])
    ear = (A + B) / (2.0 * C)
    return ear

def mouth_aspect_ratio(mouth):
    A = euclidean_dist(mouth[2], mouth[6])
    B = euclidean_dist(mouth[3], mouth[5])
    C = euclidean_dist(mouth[0], mouth[4])
    mar = (A + B) / (2.0 * C)
    return mar























    import { Link } from "react-router-dom";
import { motion } from "framer-motion";
import {
  ArrowRight, Brain, Eye, Activity, Shield,
  GraduationCap, Truck, Bus, AlertTriangle,
  Camera
} from "lucide-react";
import { Button } from "@/components/ui/button";
import SiteHeader from "@/components/SiteHeader";
import SiteFooter from "@/components/SiteFooter";

const useCases = [
  {
    icon: Truck,
    title: "Industrial Fleets",
    desc: "Truck long-haul drivers prevent fatigue-related accidents."
  },
  {
    icon: Bus,
    title: "School Bus Safety",
    desc: "Monitor school bus drivers to keep students safe."
  },
  {
    icon: GraduationCap,
    title: "Driver Training",
    desc: "Driving schools can train students with fatigue detection."
  }
];
















const express = require("express");
const cors = require("cors");
const app = express();

app.use(cors());
app.use(express.json());

// In-memory driver status
let driverStatus = "Awake";
let logs = [];

// API to receive detection metrics from frontend
app.post("/detect", (req, res) => {
  const { ear, mar, perclos } = req.body;

  // Decision Logic
  if (ear < 0.22) {
    driverStatus = "Drowsy";
  }

  if (mar > 0.7) {
    driverStatus = "Yawning";
  }

  if (perclos > 0.4) {
    driverStatus = "Sleeping";
  }

  logs.push({
    timestamp: new Date(),
    ear,
    mar,
    perclos,
    status: driverStatus
  });

  res.json({ status: driverStatus });
});
