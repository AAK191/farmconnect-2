# FarmConnect 🌾

FarmConnect is a full-stack marketplace platform that directly connects **farmers** with **buyers**, cutting out middlemen. Buyers can browse fresh produce, chat with farmers in real time, and pay securely online; farmers can list products and manage incoming orders.

## Features

- **Authentication** — JWT-based register/login with role support (`farmer` / `buyer`) and optional geolocation for location-based discovery.
- **Product Orders** — Buyers can place orders after payment confirmation, view their order history, and see populated product/farmer details.
- **Real-time Chat** — Buyers and farmers can start a conversation tied to a specific product, exchange messages, and track read/unread status.
- **Secure Payments** — Razorpay integration for order creation and payment signature verification (HMAC SHA-256) to prevent tampering.

## Tech Stack

- **Backend:** Node.js, Express
- **Database:** MongoDB with Mongoose
- **Auth:** JSON Web Tokens (JWT), bcrypt-based password hashing
- **Payments:** Razorpay
- **Real-time messaging:** Socket-based chat persistence

## Project Structure

```
backend/
└── src/
    ├── controllers/
    │   ├── authController.js       # register, login
    │   ├── orderController.js      # createOrder, getMyOrders
    │   ├── chatController.js       # startChat, getMyChats, getChat, saveMessage
    │   └── paymentController.js    # createOrder (Razorpay), verifyPayment
    ├── models/
    │   ├── User.js
    │   ├── Order.js
    │   └── Chat.js
    ├── routes/
    │   ├── authRoutes.js
    │   ├── orderRoutes.js
    │   ├── chatRoutes.js
    │   └── paymentRoutes.js
    └── middleware/
        └── authMiddleware.js
```

## Getting Started

### Prerequisites

- Node.js (v16+)
- MongoDB instance (local or Atlas)
- Razorpay account (test or live keys)

### Installation

```bash
git clone https://github.com/<your-username>/farmconnect.git
cd farmconnect/backend
npm install
```

### Environment Variables

Create a `.env` file in the `backend` directory:

```env
PORT=5000
MONGO_URI=your_mongodb_connection_string
JWT_SECRET=your_jwt_secret
JWT_EXPIRE=7d
RAZORPAY_KEY_ID=your_razorpay_key_id
RAZORPAY_KEY_SECRET=your_razorpay_key_secret
```

### Run the server

```bash
npm run dev   # with nodemon
# or
npm start
```

## API Endpoints

### Auth
| Method | Endpoint             | Description         |
|--------|-----------------------|----------------------|
| POST   | `/api/auth/register`  | Register a new user |
| POST   | `/api/auth/login`     | Log in a user        |

### Orders
| Method | Endpoint          | Description                          | Auth |
|--------|-------------------|--------------------------------------|------|
| POST   | `/api/orders`     | Create an order after payment        | ✅   |
| GET    | `/api/orders/my`  | Get orders for logged-in buyer       | ✅   |

### Chat
| Method | Endpoint                    | Description                              | Auth |
|--------|------------------------------|-------------------------------------------|------|
| POST   | `/api/chat/start`           | Get or create a chat for a product        | ✅   |
| GET    | `/api/chat/my`               | Get all chats for logged-in user          | ✅   |
| GET    | `/api/chat/:chatId`         | Get a single chat with messages           | ✅   |
| POST   | `/api/chat/:chatId/message` | Save a new message in a chat              | ✅   |

### Payments
| Method | Endpoint                | Description                        |
|--------|--------------------------|-------------------------------------|
| POST   | `/api/payments/orders`  | Create a Razorpay order            |
| POST   | `/api/payments/verify`  | Verify Razorpay payment signature  |

All authenticated routes require a `Bearer <token>` in the `Authorization` header.

## Security Notes

- Payment signatures are verified server-side using HMAC SHA-256 over `order_id|payment_id`, never trusting client-supplied verification results.
- Passwords are never returned in API responses; the password field is excluded by default and only selected explicitly during login for comparison.

## Roadmap

- [ ] Farmer product listing & inventory management
- [ ] Order status updates (shipped, delivered, cancelled)
- [ ] Push notifications for new messages and order updates
- [ ] Ratings & reviews for farmers

## Contributing

Contributions are welcome! Please open an issue to discuss any major changes before submitting a pull request.

## License

This project is licensed under the MIT License.

