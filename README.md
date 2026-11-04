IntelliMeet — AI-Powered Enterprise Meeting & Collaboration Platform

IntelliMeet is a full-stack MERN web application for enterprise collaboration. It provides low-latency peer-to-peer video calls, real-time synchronized chat, screen sharing, and automated AI-generated meeting summaries with action items once a call ends.

	
Live Demo	https://intellimeet-eight.vercel.app
Repository	https://github.com/happy-gupta/AI-Powered-Enterprise-Meeting-and-Collaboration-Platform
Demo Video	https://screenapp.io/app/share/sFX-ZgVndS-pwf8b47BiOAAc
Team
Mohit Kumar
Akshayaram
Tarun Seth
Khushi Gupta
Features
Real-time video calling — peer-to-peer streaming over WebRTC with mic and camera toggles
Screen sharing — share your screen during a call and switch back to the camera automatically when sharing stops
Live synchronized chat — instant messaging with real-time timestamps, powered by Socket.io
AI meeting summarization — when the call ends, the chat transcript is sent to the backend and returned as a structured summary
Automatic action items — key takeaways and next steps extracted from the conversation
Responsive UI — works on desktop and mobile browsers
Tech Stack
Layer	Technology
MongoDB	Database for storing meetings, transcripts, and summaries (Mongoose ODM)
Express.js	REST API server and middleware (CORS, JSON parsing)
React (Vite)	Frontend UI, CSS3 styling
Node.js	Server runtime
Socket.io / Socket.io-client	WebSocket signaling and real-time chat
WebRTC	Peer-to-peer audio/video streaming
AI API	Summary and action-item generation
GitHub Codespaces	Cloud development environment
Vercel	Frontend deployment
Architecture & Workflow
 ┌──────────────┐   WebSocket (signaling + chat)   ┌──────────────────┐
 │  React Client │ ◄──────────────────────────────► │ Node/Express +   │
 │  (Vite)       │                                  │ Socket.io Server │
 └──────┬───────┘                                   └────────┬─────────┘
        │                                                    │
        │  WebRTC (direct peer-to-peer video/audio)          │ POST /api/summarize
        ▼                                                    ▼
 ┌──────────────┐                                   ┌──────────────────┐
 │ Other Client │                                   │  AI Summarizer   │
 └──────────────┘                                   │  + MongoDB       │
                                                    └──────────────────┘
Clients connect to the server through Socket.io, which handles room joining, WebRTC signaling (offer / answer / ICE candidates), and chat broadcasts.
Once signaling completes, video and audio flow directly between peers using WebRTC.
Chat messages are broadcast to everyone in the room in real time with timestamps.
When the call ends, the chat transcript is sent to the /api/summarize REST endpoint.
The endpoint returns structured takeaways and action items, which are displayed to the users (and can be stored in MongoDB).
Project Structure
intellimeet/
├── client/                  # React (Vite) frontend
│   ├── public/
│   ├── src/
│   │   ├── assets/
│   │   ├── App.jsx          # Main call UI, WebRTC + socket logic
│   │   ├── App.css
│   │   ├── index.css
│   │   └── main.jsx
│   ├── index.html
│   ├── vite.config.js
│   └── package.json
│
├── server/                  # Node.js + Express backend
│   ├── server.js            # Express app, Socket.io, /api/summarize
│   ├── .env                 # Environment variables (not committed)
│   └── package.json
│
└── README.md
Getting Started
Prerequisites
Node.js v18 or higher
npm
A MongoDB connection string (local or MongoDB Atlas)
An API key for your AI provider
A modern browser with camera and microphone access
1. Clone the repository
bash
git clone https://github.com/happy-gupta/AI-Powered-Enterprise-Meeting-and-Collaboration-Platform.git
cd AI-Powered-Enterprise-Meeting-and-Collaboration-Platform
2. Set up the backend
bash
cd server
npm install

Create a .env file inside server/:

env
PORT=5000
MONGO_URI=your_mongodb_connection_string
AI_API_KEY=your_ai_provider_api_key

Start the server:

bash
node server.js

The server runs at http://localhost:5000.

3. Set up the frontend

Open a second terminal:

bash
cd client
npm install
npm run dev

The app runs at http://localhost:5173.

4. Try it out
Open the app in two browser tabs (or two devices on the same network).
Join the same room in both.
Allow camera and microphone access.
Chat, toggle your mic/camera, share your screen, then end the call to see the AI summary.

Note: Browsers only allow camera and microphone access on https:// or localhost.

API Reference
POST /api/summarize

Generates a summary and action items from a meeting transcript.

Request body

json
{
  "transcript": [
    { "user": "Mohit", "message": "Let's finalize the UI by Friday.", "time": "10:42 AM" },
    { "user": "Tarun", "message": "I'll handle the backend deployment.", "time": "10:43 AM" }
  ]
}

Response

json
{
  "summary": "The team agreed to finalize the UI and deploy the backend this week.",
  "actionItems": [
    "Finalize the UI by Friday",
    "Deploy the backend"
  ]
}
Socket.io Events
Event	Direction	Description
join-room	Client → Server	Join a meeting room
user-joined	Server → Client	Notify peers that someone joined
offer / answer	Client ↔ Server ↔ Client	WebRTC session negotiation
ice-candidate	Client ↔ Server ↔ Client	Exchange network candidates
send-message	Client → Server	Send a chat message
receive-message	Server → Client	Broadcast a chat message with timestamp
user-left	Server → Client	Notify peers that someone left

Event names may differ slightly from the implementation. Check server/server.js and client/src/App.jsx for the exact names.

Deployment
Frontend: deploy the client/ folder to Vercel. Set the build command to npm run build and the output directory to dist.
Backend: deploy the server/ folder to a Node host such as Render or Railway. Add the environment variables from the .env file in the host's dashboard.
Update the Socket.io server URL in the client to point to your deployed backend.
Future Improvements
User authentication and role-based meeting access
Meeting recording and playback
Live captions and speech-to-text transcription
Multi-participant (SFU-based) video for large meetings
Meeting history dashboard with searchable summaries
Calendar integration
Conclusion

IntelliMeet demonstrates full-stack real-time data synchronization together with automated meeting productivity analysis, combining WebRTC, WebSockets, and AI into a responsive collaboration workflow.

License

This project was developed for academic purposes.
