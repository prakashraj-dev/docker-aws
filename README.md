 docker-aws — Real-time Collaborative Code Editor
A real-time collaborative code editor built with React, Node.js, Socket.IO, Yjs, and Monaco Editor — containerized with Docker and deployable on AWS EC2.
Multiple users can join a session and type code together in real time — just like Google Docs but for code!

🚀 Live Demo

Open two browser tabs with different usernames and type together in real time!

http://localhost:3000?username=YourName

🏗️ Project Structure
docker-aws/
├── dockerfile              ← Multi-stage Docker build
├── .dockerignore
├── .gitignore
├── Backend/
│   ├── server.js           ← Express + Socket.IO + Yjs sync server
│   └── package.json
└── Frontend/
    ├── index.html
    ├── vite.config.js      ← Vite + React + Tailwind CSS v4
    ├── package.json
    └── src/
        ├── main.jsx
        └── app/
            ├── App.jsx     ← Monaco Editor + Yjs + Tailwind UI
            └── App.css

🛠️ Tech Stack
LayerTechnologyFrontendReact 19, Vite, Tailwind CSS v4Code EditorMonaco Editor (@monaco-editor/react)Real-time SyncYjs (CRDT), y-monaco, y-socket.ioBackendNode.js, Express 5, Socket.IOContainerDocker (multi-stage build)DeploymentAWS EC2

⚙️ How It Works
[Stage 1 - frontend-builder]
  node:20-alpine
  → npm install + npm run build → produces /app/dist

[Stage 2 - backend runner]
  node:20-alpine
  → npm install
  → Copy dist/ from Stage 1 → /app/public
  → node server.js  (serves everything on port 3000)

The Express backend serves the React build as static files
Socket.IO handles real-time WebSocket connections between users
Yjs is a CRDT library that merges edits from multiple users conflict-free
y-monaco binds the Yjs document to the Monaco editor
One single Docker container handles everything on port 3000


🖥️ Run Locally (Without Docker)
Prerequisites

Node.js v18+ → https://nodejs.org
npm

Step 1 — Backend
bashcd Backend
npm install
node server.js
# ✅ Server running on http://localhost:3000
Step 2 — Frontend
bashcd Frontend
npm install
npm run dev
# ✅ Running on http://localhost:5173
Step 3 — Open in Browser
http://localhost:5173?username=YourName
Open a second tab:
http://localhost:5173?username=John

🐳 Run with Docker
Prerequisites

Docker Desktop → https://www.docker.com/products/docker-desktop

Build the image
bashdocker build -t docker-aws-app .
Run the container
bashdocker run -p 3000:3000 docker-aws-app
Open in browser
http://localhost:3000?username=YourName
