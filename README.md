AventurePassion/
├── server.js
├── package.json
├── .env.example
├── /public
│   ├── index.html
│   ├── css/style.css
│   ├── js/app.js
│   ├── js/socket.js
│   └── images/logo.png
├── /routes
│   ├── auth.js
│   └── chat.js
├── /controllers
│   ├── userController.js
│   ├── chatController.js
│   └── streamController.js
├── /db
│   └── database.sqlite
└── README.txt
1️⃣ package.json
json


{
  "name": "aventurepassion",
  "version": "1.0.0",
  "main": "server.js",
  "type": "module",
  "scripts": {
    "start": "node server.js"
  },
  "dependencies": {
    "express": "^4.21.0",
    "socket.io": "^4.8.1",
    "bcrypt": "^5.1.1",
    "dotenv": "^16.4.0"
  }
}
2️⃣ server.js
js


import express from "express";
import http from "http";
import { Server } from "socket.io";
import path from "path";
import { fileURLToPath } from "url";
import authRoutes from "./routes/auth.js";
import chatRoutes from "./routes/chat.js";
const __filename = fileURLToPath(import.meta.url);
const __dirname = path.dirname(__filename);
const app = express();
const server = http.createServer(app);
const io = new Server(server);
app.use(express.static(path.join(__dirname, "public")));
app.use(express.json());
app.use("/auth", authRoutes);
app.use("/chat", chatRoutes);
io.on("connection", (socket) => {
  console.log("🟢 Nouvel utilisateur connecté :", socket.id);
  socket.on("message", (msg) => io.emit("message", msg));
  socket.on("disconnect", () => console.log("🔴 Utilisateur déconnecté"));
});
app.get("/", (req, res) => {
  res.sendFile(path.join(__dirname, "public", "index.html"));
});
const PORT = process.env.PORT || 3000;
server.listen(PORT, () =>
  console.log(`✅ Serveur en ligne sur http://localhost:${PORT}`)
);
3️⃣ /public/index.html
html


<!DOCTYPE html>
<html lang="fr">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>AventurePassion.com</title>
  <link rel="stylesheet" href="css/style.css" />
</head>
<body>
  <div class="container">
    <img src="images/logo.png" alt="AventurePassion Logo" class="logo">
    <p class="tagline">Plaisir, aventure et passion sans limite</p>
    <button onclick="window.location.href='/auth/login'">Se connecter</button>
  </div>
  <script src="/socket.io/socket.io.js"></script>
  <script src="js/app.js"></script>
</body>
</html>
4️⃣ /public/css/style.css
css


body {
  margin: 0;
  padding: 0;
  background-color: #020018;
  color: #fff;
  font-family: 'Poppins', sans-serif;
  height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
}
.container {
  text-align: center;
}
.logo {
  width: 260px;
  margin-bottom: 20px;
}
.tagline {
  font-size: 1.2rem;
  margin-bottom: 30px;
  color: #7ec8ff;
}
button {
  background: #0078ff;
  border: none;
  padding: 12px 28px;
  border-radius: 6px;
  color: white;
  font-size: 1rem;
  cursor: pointer;
  transition: 0.3s ease;
}
button:hover {
  background: #0090ff;
}
5️⃣ /public/js/app.js
js


const socket = io();
socket.on("connect", () => {
  console.log("Connecté au chat en temps réel !");
});
socket.on("message", (msg) => {
  console.log("Message reçu :", msg);
});
6️⃣ /routes/auth.js
js


import express from "express";
const router = express.Router();
router.get("/login", (req, res) => {
  res.send("<h2>Page de connexion (en construction)</h2>");
});
router.post("/login", (req, res) => {
  res.send({ status: "ok", message: "Connexion réussie" });
});
router.post("/register", (req, res) => {
  res.send({ status: "ok", message: "Inscription réussie" });
});
export default router;
7️⃣ /routes/chat.js
js


import express from "express";
const router = express.Router();
router.get("/", (req, res) => {
  res.send("Bienvenue dans le chat !");
});
export default router;
8️⃣ /controllers/userController.js
js


// ici tu pourras ajouter la logique d'inscription/login avec bcrypt
9️⃣ README.txt


=== AVENTUREPASSION.COM ===
INSTALLATION :
1. Extrais le dossier sur ton ordinateur
2. Ouvre un terminal dans le dossier
3. Lance la commande : npm install
4. Puis : npm start
5. Ouvre http://localhost:3000 dans ton navigateur
STRUCTURE :
- Page d'accueil moderne
- Chat en temps réel (Socket.IO)
- Authentification de base
- Design épuré bleu nuit
