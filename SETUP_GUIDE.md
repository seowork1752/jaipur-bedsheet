# Jaipur Bedsheets Ecommerce Platform - Setup Guide

## 🚀 Quick Start (5 minutes)

### Prerequisites
- Node.js 18+ and npm/yarn
- MongoDB Atlas account (free tier available)
- Razorpay account (test keys available)
- Cloudinary account (optional, for image hosting)

### 1. Clone & Install

```bash
# Clone repository
git clone <your-repo-url>
cd jaipur-bedsheets-ecommerce

# Install all dependencies
npm run install-all
```

### 2. Configure Environment Variables

**Backend** - Create `backend/.env`:
```env
NODE_ENV=development
PORT=5000
API_URL=http://localhost:5000
FRONTEND_URL=http://localhost:3000

MONGODB_URI=mongodb+srv://username:password@cluster.mongodb.net/jaipur-bedsheets
DB_TYPE=mongodb

JWT_SECRET=your_super_secret_jwt_key_here_min_32_chars
JWT_EXPIRE=7d

RAZORPAY_KEY_ID=rzp_test_XXXXX
RAZORPAY_KEY_SECRET=your_key_secret

SMTP_HOST=smtp.gmail.com
SMTP_PORT=587
SMTP_USER=your_email@gmail.com
SMTP_PASS=your_app_password
SENDER_EMAIL=noreply@jaipur-bedsheets.com

CLOUDINARY_NAME=your_cloudinary_name
CLOUDINARY_API_KEY=xxx
CLOUDINARY_API_SECRET=xxx

GST_RATE=18
```

**Frontend** - Create `frontend/.env.local`:
```env
NEXT_PUBLIC_API_URL=http://localhost:5000/api
NEXT_PUBLIC_RAZORPAY_KEY=rzp_test_XXXXX
```

### 3. Run Development Servers

```bash
# From root directory
npm run dev

# Or run separately:
# Terminal 1 - Frontend
cd frontend && npm run dev

# Terminal 2 - Backend
cd backend && npm run dev
```

**URLs:**
- Frontend: http://localhost:3000
- Backend API: http://localhost:5000
- API Health: http://localhost:5000/api/health

---

## 📊 Database Setup

### MongoDB Atlas Setup (Free)

1. Go to [MongoDB Atlas](https://www.mongodb.com/cloud/atlas)
2. Create free cluster
3. Create database user with strong password
4. Get connection string
5. Add connection string to `backend/.env` as `MONGODB_URI`

### Database Collections & Indexes

The backend automatically creates collections with proper indexes:

```javascript
// Products
- Indexes: category+status, featured+status, bestseller+status, text search
- Functions: Full-text search, filtering, sorting

// Orders
- Indexes: userId+createdAt, paymentStatus, orderStatus
- Fields: Complete order tracking & payment info

// Users
- Indexes: email, phone, role, status
- Features: Wishlist, addresses, loyalty points, referrals

// Cart
- Indexes: userId (unique)
- Auto-calculation: Subtotal, tax, shipping, discount

// Categories & Coupons
- Ready for admin management
```

---

## 💳 Payment Integration (Razorpay)

### Test Credentials
- Key ID: `rzp_test_XXXXX`
- Key Secret: Available on dashboard

### Test Cards
- **UPI**: `success@razorpay`
- **Credit Card**: `4111 1111 1111 1111` | 12/25 | 123
- **Debit Card**: `5555 5555 5555 4444` | 12/25 | 123
- **Netbanking**: Use any ICICI test account

### Integration Points
```typescript
// Create payment order
POST /api/payments/razorpay/order/{orderId}

// Verify payment
POST /api/payments/razorpay/verify
Body: { orderId, razorpayOrderId, razorpayPaymentId, razorpaySignature }

// Check status
GET /api/payments/status/{orderId}

// Process refund
POST /api/payments/refund/{orderId}

// All payment methods supported:
- UPI
- Credit/Debit Cards
- Net Banking
- Digital Wallets
- EMI
- Cash on Delivery
```

---

## 🏗️ Project Structure

```
jaipur-bedsheets-ecommerce/
├── frontend/
│   ├── src/
│   │   ├── pages/          # Next.js pages
│   │   ├── components/     # Reusable React components
│   │   ├── store/          # Zustand state management
│   │   ├── utils/          # API client, helpers
│   │   ├── styles/         # Global CSS & Tailwind
│   │   └── types/          # TypeScript types
│   ├── next.config.js
│   ├── tailwind.config.js
│   └── package.json
│
├── backend/
│   ├── src/
│   │   ├── controllers/    # Business logic
│   │   ├── models/         # MongoDB schemas
│   │   ├── routes/         # API routes
│   │   ├── middleware/     # Auth, error handling
│   │   ├── utils/          # Helpers, auth
│   │   ├── config/         # Database connection
│   │   └── server.ts       # Express setup
│   ├── .env.example
│   └── package.json
│
└── README.md
```

---

## 🔐 Security Checklist

- [ ] Change all default passwords
- [ ] Set strong `JWT_SECRET` (min 32 chars)
- [ ] Enable HTTPS in production
- [ ] Configure CORS properly
- [ ] Use environment variables (never hardcode secrets)
- [ ] Enable 2FA for admin accounts
- [ ] Set up rate limiting
- [ ] Validate all user inputs
- [ ] Use Helmet.js for headers
- [ ] Keep dependencies updated

---

## 📦 Build & Deploy

### Build for Production

```bash
# Build both frontend and backend
npm run build

# Or individually
cd frontend && npm run build
cd backend && npm run build
```

### Deploy to Vercel (Frontend)

```bash
# Install Vercel CLI
npm i -g vercel

# Deploy from frontend directory
cd frontend
vercel

# Set environment variables in Vercel dashboard
```

### Deploy to Heroku/Railway (Backend)

```bash
# Using Heroku
heroku create jaipur-bedsheets-api
heroku config:set NODE_ENV=production
heroku config:set JWT_SECRET=...
git push heroku main

# Using Railway.app
railway up
```

### Docker Deployment

**Backend Dockerfile:**
```dockerfile
FROM node:18-alpine
WORKDIR /app
COPY backend . 
RUN npm install
RUN npm run build
EXPOSE 5000
CMD ["npm", "start"]
```

**Frontend Dockerfile:**
```dockerfile
FROM node:18-alpine AS build
WORKDIR /app
COPY frontend .
RUN npm install
RUN npm run build

FROM node:18-alpine
WORKDIR /app
COPY --from=build /app/.next ./.next
COPY --from=build /app/node_modules ./node_modules
EXPOSE 3000
CMD ["npm", "start"]
```

---

## 📱 API Documentation

### Authentication
```
POST /api/auth/register
POST /api/auth/login
GET  /api/auth/me              (Protected)
PUT  /api/auth/profile         (Protected)
POST /api/auth/change-password (Protected)
```

### Products
```
GET  /api/products             (Get all with filters)
GET  /api/products/:id
GET  /api/products/slug/:slug
GET  /api/products/featured
GET  /api/products/bestsellers
POST /api/products             (Admin)
PUT  /api/products/:id         (Admin)
DELETE /api/products/:id       (Admin)
```

### Orders
```
POST /api/orders                    (Protected)
GET  /api/orders                    (Protected)
GET  /api/orders/:orderId           (Protected)
PUT  /api/orders/:orderId/status    (Admin)
POST /api/orders/:orderId/cancel    (Protected)
POST /api/orders/:orderId/return    (Protected)
```

### Payments
```
POST /api/payments/razorpay/order/:orderId
POST /api/payments/razorpay/verify
GET  /api/payments/status/:orderId
POST /api/payments/refund/:orderId
POST /api/payments/coupon
GET  /api/payments/methods
```

### Wishlist
```
GET  /api/wishlist                    (Protected)
POST /api/wishlist/add/:productId     (Protected)
DELETE /api/wishlist/remove/:productId (Protected)
```

### Cart
```
GET    /api/cart                      (Protected)
POST   /api/cart/add                  (Protected)
PUT    /api/cart/update/:productId    (Protected)
DELETE /api/cart/remove/:productId    (Protected)
DELETE /api/cart/clear                (Protected)
```

---

## 🎨 Customization

### Colors
Edit `frontend/tailwind.config.js`:
```javascript
colors: {
  'royal-blue': '#0F3A6B',  // Change to your primary
  'maroon': '#8B2E2E',      // Change to your secondary
  'mustard': '#E8B04B',     // Change to your accent
  // ... more colors
}
```

### Fonts
Change in `frontend/src/styles/globals.css` and `tailwind.config.js`:
```css
@import url('https://fonts.googleapis.com/css2?family=YOUR_FONT');
```

### Logo & Images
Replace images in `frontend/public/`:
- `logo.png`
- `favicon.ico`
- `og-image.png`

---

## 🐛 Troubleshooting

### MongoDB Connection Error
```
Error: connect ECONNREFUSED
→ Check MONGODB_URI in .env
→ Verify MongoDB is running
→ Check IP whitelist in Atlas
```

### Razorpay Key Invalid
```
Error: Razorpay Key not found
→ Get keys from Razorpay Dashboard
→ Copy exact Key ID and Secret
→ Restart server after changing .env
```

### CORS Error
```
Error: Access to XMLHttpRequest blocked by CORS
→ Check FRONTEND_URL in backend .env
→ Verify CORS configuration in server.ts
→ Ensure requests use correct domain
```

### Build Failed
```
→ Clear node_modules: rm -rf node_modules
→ Reinstall: npm install
→ Clear Next.js cache: rm -rf .next
→ Rebuild: npm run build
```

---

## 📈 Performance Optimization

### Frontend
- Images optimized with Next.js Image
- Code splitting automatic
- Lazy loading enabled
- Caching strategies configured
- PWA support available

### Backend
- Database indexes for fast queries
- Connection pooling
- Redis caching ready
- Response compression
- Rate limiting configured

---

## 🚢 Production Checklist

- [ ] All environment variables set correctly
- [ ] Database backups configured
- [ ] SSL certificate installed
- [ ] CDN configured for images
- [ ] Email service tested
- [ ] Payment gateway tested
- [ ] Admin accounts created
- [ ] Demo data loaded
- [ ] Error monitoring setup (Sentry)
- [ ] Analytics configured (GA4)
- [ ] Performance monitored
- [ ] SEO optimized
- [ ] Mobile responsiveness tested
- [ ] Cross-browser testing done
- [ ] Accessibility checked

---

## 📞 Support & Resources

- **MongoDB**: [docs.mongodb.com](https://docs.mongodb.com)
- **Razorpay**: [razorpay.com/docs](https://razorpay.com/docs)
- **Next.js**: [nextjs.org/docs](https://nextjs.org/docs)
- **Express**: [expressjs.com](https://expressjs.com)
- **Tailwind**: [tailwindcss.com/docs](https://tailwindcss.com/docs)

---

## 📄 License

This project is licensed under the MIT License.

---

**Happy Building! 🎉**

For questions or issues, please create an issue in the repository.
