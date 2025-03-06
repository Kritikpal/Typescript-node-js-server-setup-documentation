# Express.js + TypeScript + WebSocket Setup

## 1️⃣ Initialize the Project
Run the following commands in your terminal:

```sh
mkdir express-ts-app
cd express-ts-app
npm init -y
```

This creates a basic `package.json` file.

---

## 2️⃣ Install Dependencies
Run this to install **Express and TypeScript-related dependencies**:

```sh
npm install express ws cors dotenv
npm install --save-dev typescript ts-node @types/node @types/express @types/ws
```

### Dependencies Explained:
- `express` → Web framework for Node.js  
- `ws` → WebSocket library  
- `cors` → Enable Cross-Origin Resource Sharing  
- `dotenv` → Load environment variables  
- `typescript` → TypeScript compiler  
- `ts-node` → Run TypeScript files without compiling  
- `@types/node` & `@types/express` → Type definitions  

---

## 3️⃣ Initialize TypeScript
Run this command to generate a `tsconfig.json` file:

```sh
npx tsc --init
```

Now, update `tsconfig.json`:

```json
{
  "compilerOptions": {
    "outDir": "./dist",
    "rootDir": "./src",
    "moduleResolution": "node",
    "esModuleInterop": true,
    "strict": true
  }
}
```

This ensures clean **compilation and TypeScript support**.

---

## 4️⃣ Create `src/server.ts`

Create a new directory:

```sh
mkdir src
touch src/server.ts
```

Now, add the **Express server code**:

### `src/server.ts`

```ts
import express from "express";
import { WebSocketServer } from "ws";
import cors from "cors";
import dotenv from "dotenv";

dotenv.config();
const app = express();
const PORT = process.env.PORT || 5000;

// Enable CORS
app.use(cors());
app.use(express.json());

// HTTP Route
app.get("/", (req, res) => {
  res.send("Express + TypeScript Server is Running 🚀");
});

// Start WebSocket Server
const server = app.listen(PORT, () => {
  console.log(`⚡ Server is running on http://localhost:${PORT}`);
});

const wss = new WebSocketServer({ server });

wss.on("connection", (ws) => {
  console.log("New WebSocket connection");

  ws.send(JSON.stringify({ message: "Hello from WebSocket server!" }));

  ws.on("message", (data) => {
    console.log("Received:", data.toString());
    ws.send(JSON.stringify({ message: "Message received!" }));
  });

  ws.on("close", () => console.log("WebSocket disconnected"));
});
```

---

## 5️⃣ Add Start Scripts
Modify `package.json`:

```json
"scripts": {
  "dev": "ts-node src/server.ts",
  "build": "tsc",
  "start": "node dist/server.js"
}
```

---

## 6️⃣ Run the Server
For **development mode (hot-reloading)**:

```sh
npm run dev
```

For **production mode** (build & run):

```sh
npm run build
npm start
```

---

## 🎉 Done!
Your **Express.js + TypeScript + WebSocket** server is now running! 🚀

