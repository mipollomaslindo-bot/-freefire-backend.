
{
  "name": "freefire-backend",
  "version": "1.0.0",
  "private": true,
  "scripts": {
    "start": "node server.js"
  },
  "engines": {
    "node": ">=20"
  },
  "dependencies": {
    "cors": "^2.8.5",
    "express": "^5.1.0",
    "helmet": "^8.1.0"
  }
}

const express = require("express");
const helmet = require("helmet");
const cors = require("cors");

const app = express();
const port = process.env.PORT || 3000;

app.disable("x-powered-by");
app.use(helmet());
app.use(express.json({ limit: "10kb" }));

// Configura ALLOWED_ORIGIN con el origen exacto
// de tu panel cuando conozcas su URL.
const allowedOrigin = process.env.ALLOWED_ORIGIN;

app.use(cors({
  origin(origin, callback) {
    if (!origin) return callback(null, false);
    if (allowedOrigin && origin === allowedOrigin) {
      return callback(null, true);
    }
    return callback(null, false);
  }
}));

app.get("/health", (req, res) => {
  res.json({
    status: "ok",
    service: "freefire-backend"
  });
});

// Las recargas no están habilitadas todavía.
app.post("/api/v1/topups", (req, res) => {
  res.status(501).json({
    error: "TOPUP_PROVIDER_NOT_CONFIGURED",
    message:
      "Configura primero la API autorizada del proveedor."
  });
});

// La consulta real requiere un proveedor conectado.
app.get("/api/v1/topups/:orderId", (req, res) => {
  res.status(501).json({
    error: "TOPUP_PROVIDER_NOT_CONFIGURED",
    message:
      "No se puede verificar este pedido todavía."
  });
});

app.listen(port, "0.0.0.0", () => {
  console.log(`Backend iniciado en puerto ${port}`);
});

.env
.env.*
!.env.example
node_modules/

