npx create-react-app edu-platform
cd edu-platform
npm start
import React, { useState, useEffect } from "react";
const VideoLessons = () => {
  const [videos, setVideos] = useState([]);
  useEffect(() => {
    fetch("/api/videos") // Backend API dan videolarni olish
      .then((res) => res.json())
      .then((data) => setVideos(data));
  }, []);
  return (
    <div>
      <h2>Video Darsliklar</h2>
      {videos.map((video) => (
        <div key={video._id}>
          <h3>{video.title}</h3>
          <video controls>
            <source src={video.url} type="video/mp4" />
            Brauzeringiz ushbu videoni qo‘llab-quvvatlamaydi.
          </video>
        </div>
      ))}
    </div>
  );
};
export default VideoLessons;
mkdir backend
cd backend
npm init -y
npm install express mongoose cors multer
const express = require("express");
const mongoose = require("mongoose");
const cors = require("cors");
const app = express();
app.use(cors());
app.use(express.json());
mongoose.connect("mongodb://localhost:27017/edu-platform", {
  useNewUrlParser: true,
  useUnifiedTopology: true,
});
const VideoSchema = new mongoose.Schema({
  title: String,
  url: String,
});
const Video = mongoose.model("Video", VideoSchema);
app.get("/api/videos", async (req, res) => {
  const videos = await Video.find();
  res.json(videos);
});
app.listen(5000, () => console.log("Server 5000-portda ishlamoqda"));
const newVideo = new Video({
  title: "Matematika darsi 1",
  url: "/videos/math1.mp4",
});
await newVideo.save();
self.addEventListener("install", (event) => {
  event.waitUntil(
    caches.open("edu-platform-cache").then((cache) => {
      return cache.addAll(["/", "/index.html", "/videos/math1.mp4"]);
    })
  );
});
self.addEventListener("fetch", (event) => {
  event.respondWith(
    caches.match(event.request).then((response) => {
      return response || fetch(event.request);
    })
  );
});
if ("serviceWorker" in navigator) {
  navigator.serviceWorker.register("/service-worker.js")
    .then(() => console.log("Service Worker Ro‘yxatdan O‘tdi"))
    .catch((err) => console.log("Xatolik:", err));
}
