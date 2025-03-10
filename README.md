# plan-de-finanza.
Bienvenido a Plan de Finanzas – Tu Mejor Opción para Invertir y Generar Ingresos Pasivos
Bienvenido a Plan de Finanzas – Tu Mejor Opción para Invertir y Generar Ingresos Pasivos
En Plan de Finanzas, te ofrecemos la oportunidad de invertir de manera segura y obtener rendimientos diarios con planos diseñados para ajustarse a cualquier tipo de inversionista. Nuestra plataforma busca garantizar estabilidad a largo plazo, permitiéndote recuperar el 100% de tu inversión y seguir generando ingresos sin la necesidad de nuevas inversiones.

📌 Cómo funciona
🔹 Selecciona el Plan VIP que mejor se adapta a tu inversión.
🔹 Genera ingresos diarios según la comisión establecida para cada plan.
🔹 Una vez que recuperas el 100% de tu inversión , seguirás recibiendo pagos semanales todos los miércoles, sin necesidad de volver a invertir .
🔹 Cuanto mayor sea tu inversión, mayores serán tus ganancias en comisiones a largo plazo.

💎 Planes de Inversión VIP
Plan VIP	Inversión Inicial	Comisión Diaria (%)
VIP 1	$7	2,2%
VIP 2	$9	3,1%
VIP 3	$15	4,1%
VIP 4	$20	4,4%
VIP 5	$25	5,1%
VIP 6	$35	7,6%
VIP 7	$45	8.0%
VIP 8	$80	15%
VIP 9	$100	25%
VIP 10	$200	35%
VIP 11	$500	50%
VIP 12	$1,000	80%
VIP 13	$2,000	100%
VIP 14	$5,000	100%
VIP 15	$10,000	100%
📢 ¡Recupera tu inversión y sigue ganando sin necesidad de volver a invertir!

💰 Beneficios Exclusivos
✔ 80% de reembolso garantizado al finalizar tu inversión.
✔ Pagos semanales todos los miércoles de $10 hasta $100 o más , según el plan en el que te encuentres.
✔ No es obligatorio reinvertir, pero mientras más inviertas, mayores serán tus comisiones .
✔ Seguridad y estabilidad: Nuestra plataforma está diseñada para durar años en línea , maximizando el potencial de ingresos.

🚀 Únete Hoy y Empieza a Ganar
No dejes pasar esta oportunidad única de inversión. Escoge tu Plan VIP , multiplica tu dinero y disfruta de pagos semanales garantizados.

📩 Contáctanos ahora y empieza a construir tu futuro financiero con Plan de Finanzas.











Buscar

Analiza


ChatGPT puede cometer errores. Comprueba la información 
const express = require("express");
const cors = require("cors");
const mongoose = require("mongoose");
const dotenv = require("dotenv");
const jwt = require("jsonwebtoken");

dotenv.config();
const app = express();
app.use(express.json());
app.use(cors());

// Conectar a MongoDB
mongoose.connect(process.env.MONGO_URI, {
  useNewUrlParser: true,
  useUnifiedTopology: true,
});

// Modelo de usuario
const UserSchema = new mongoose.Schema({
  username: String,
  email: String,
  password: String,
  balance: { type: Number, default: 0 },
});
const User = mongoose.model("User", UserSchema);

// Registro de usuario
app.post("/register", async (req, res) => {
  const { username, email, password } = req.body;
  const newUser = new User({ username, email, password });
  await newUser.save();
  res.json({ message: "Usuario registrado con éxito" });
});

// Inicio de sesión
app.post("/login", async (req, res) => {
  const { email, password } = req.body;
  const user = await User.findOne({ email, password });
  if (!user) return res.status(401).json({ message: "Credenciales inválidas" });

  const token = jwt.sign({ id: user._id }, process.env.JWT_SECRET);
  res.json({ token, user });
});

// Obtener saldo del usuario
app.get("/balance", async (req, res) => {
  const user = await User.findById(req.user.id);
  res.json({ balance: user.balance });
});

// Completar tarea (recompensa)
app.post("/complete-task", async (req, res) => {
  const user = await User.findById(req.user.id);
  user.balance += 0.01; // Agregar recompensa
  await user.save();
  res.json({ message: "Tarea completada", balance: user.balance });
});

// Retiro de fondos (PayPal o Bitcoin)
app.post("/withdraw", async (req, res) => {
  const { method, address, amount } = req.body;
  if (method !== "PayPal" && method !== "Bitcoin")
    return res.status(400).json({ message: "Método inválido" });

  // Aquí iría la integración con la API de PayPal o Coinbase
  res.json({ message: "Retiro procesado con éxito" });
});

// Iniciar servidor
app.listen(5000, () => console.log("Servidor corriendo en puerto 5000"));
