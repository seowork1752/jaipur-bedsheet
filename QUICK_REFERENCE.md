# 🚀 Developer Quick Reference Guide

## Getting Started (Copy-Paste Commands)

### 1. First-Time Setup

```bash
# Clone the project
git clone <your-repo-url>
cd jaipur-bedsheets-ecommerce

# Install all dependencies
npm run install-all

# Copy environment file
cp backend/.env.example backend/.env

# Edit backend/.env with:
# - MONGODB_URI (get from MongoDB Atlas)
# - RAZORPAY_KEY_ID (get from Razorpay Dashboard)
# - RAZORPAY_KEY_SECRET (get from Razorpay Dashboard)

# Start development servers
npm run dev
```

### 2. Verify Everything Works

```bash
# Frontend should be at: http://localhost:3000
# Backend API should be at: http://localhost:5000
# Health check: http://localhost:5000/api/health
```

---

## Common Development Tasks

### Add a New Page (Frontend)

```typescript
// Create: frontend/src/pages/new-page.tsx
import React from 'react';

export default function NewPage() {
  return (
    <div className="min-h-screen bg-white">
      <div className="max-w-7xl mx-auto px-4 py-12">
        <h1 className="text-4xl font-display font-bold text-primary">
          New Page
        </h1>
      </div>
    </div>
  );
}
```

### Add a New API Endpoint (Backend)

```typescript
// 1. Create controller in: backend/src/controllers/newController.ts
export const newAction = async (req: AuthRequest, res: Response) => {
  try {
    // Your logic here
    return sendSuccess(res, 'Success message', data);
  } catch (error: any) {
    return sendError(res, 'Error message', 500, error);
  }
};

// 2. Create route in: backend/src/routes/new.ts
import { Router } from 'express';
import { newAction } from '../controllers/newController';
import { protectRoute } from '../middleware/auth';

const router = Router();
router.post('/', protectRoute, newAction);
export default router;

// 3. Add to server.ts
import newRoutes from './routes/new';
app.use('/api/new', newRoutes);
```

### Use State Management (Frontend)

```typescript
import { useCartStore, useUserStore } from '@/store';

export default function Component() {
  // User store
  const user = useUserStore((state) => state.user);
  const setUser = useUserStore((state) => state.setUser);
  const logout = useUserStore((state) => state.logout);
  
  // Cart store
  const items = useCartStore((state) => state.items);
  const addItem = useCartStore((state) => state.addItem);
  const removeItem = useCartStore((state) => state.removeItem);
  
  return (
    // Use state in JSX
  );
}
```

### Make API Calls (Frontend)

```typescript
import { apiClient } from '@/utils/api';

// GET request
const response = await apiClient.get('/products');

// POST request
const response = await apiClient.post('/orders', {
  items: [...],
  shippingAddress: {...}
});

// With error handling
try {
  const response = await apiClient.get('/api/endpoint');
  console.log(response.data);
} catch (error) {
  console.error(error.response?.data?.message);
}
```

### Generate Demo Data

```bash
# Run seed script to create sample products
cd backend
npm run dev  # In another terminal
npx ts-node src/scripts/seedData.ts
```

---

## File Structure Quick Navigation

```
🏠 Home Page              → frontend/src/pages/index.tsx
🛍️  Shop Page             → frontend/src/pages/shop/index.tsx
📦 Product Details        → frontend/src/pages/shop/[slug].tsx
🛒 Cart                   → frontend/src/pages/cart.tsx
💳 Checkout              → frontend/src/pages/checkout.tsx
🔐 Login/Register        → frontend/src/pages/login.tsx
📋 Orders                → frontend/src/pages/orders/index.tsx
🎨 Layout                → frontend/src/components/Layout.tsx
🔌 API Client            → frontend/src/utils/api.ts
📊 State Management      → frontend/src/store/index.ts

🖥️  Server Setup         → backend/src/server.ts
👤 Auth Controller       → backend/src/controllers/authController.ts
📦 Product Controller    → backend/src/controllers/productController.ts
📋 Order Controller      → backend/src/controllers/orderController.ts
💳 Payment Controller    → backend/src/controllers/paymentController.ts
🛒 Cart Controller       → backend/src/controllers/cartController.ts
👥 User Model            → backend/src/models/User.ts
📦 Product Model         → backend/src/models/Product.ts
📋 Order Model           → backend/src/models/Order.ts
🔑 Auth Utils            → backend/src/utils/auth.ts
```

---

## Useful Commands

### Frontend Development
```bash
cd frontend

# Start dev server
npm run dev

# Build for production
npm run build

# Start production build
npm start

# Type checking
npm run type-check
```

### Backend Development
```bash
cd backend

# Start dev server (with auto-reload)
npm run dev

# Build for production
npm run build

# Run seed script
npx ts-node src/scripts/seedData.ts
```

### Root Level
```bash
# Install all dependencies at once
npm run install-all

# Start both frontend and backend
npm run dev

# Build both
npm run build
```

---

## Database Quick Tips

### Connect to MongoDB Atlas
1. Create cluster at mongodb.com/cloud/atlas
2. Create database user
3. Get connection string
4. Add to `backend/.env` as `MONGODB_URI`

### Common MongoDB Queries
```javascript
// View all collections
show collections

// Query users
db.users.find()

// Query products
db.products.find().limit(5)

// Count documents
db.products.countDocuments()

// Create index
db.products.createIndex({ slug: 1 })
```

---

## Testing Credentials

### Test Razorpay Payments
- **UPI**: success@razorpay
- **Card**: 4111 1111 1111 1111 | 12/25 | 123
- **Debit**: 5555 5555 5555 4444 | 12/25 | 123

### Test Coupon Code
- **WELCOME10** - 10% discount

### Test Admin Login
- **Email**: admin@jaipur-bedsheets.com
- **Password**: Admin@123

---

## Environment Variables

### Backend (.env)
```env
NODE_ENV=development
PORT=5000
MONGODB_URI=mongodb+srv://...
JWT_SECRET=your_secret_key_min_32_chars
RAZORPAY_KEY_ID=rzp_test_XXXXX
RAZORPAY_KEY_SECRET=rzp_test_secret
FRONTEND_URL=http://localhost:3000
```

### Frontend (.env.local)
```env
NEXT_PUBLIC_API_URL=http://localhost:5000/api
NEXT_PUBLIC_RAZORPAY_KEY=rzp_test_XXXXX
```

---

## CSS/Styling Quick Reference

### Tailwind Classes Used
```
Colors:       text-primary, bg-primary, border-primary
             text-accent, bg-accent
             text-maroon, bg-maroon
             text-mustard, bg-mustard

Spacing:      px-4, py-6, pt-12, pb-8
             gap-4, space-y-3, space-x-2

Responsive:   hidden md:flex, grid-cols-1 lg:grid-cols-3
             text-lg md:text-2xl

Effects:     shadow-luxury, rounded-lg, hover:shadow-lg
            transition-colors, opacity-50
```

### Custom Utilities
```css
.card              /* Card styling */
.card-product      /* Product card styling */
.btn-primary       /* Primary button */
.flex-center       /* Centered flex container */
.container-full    /* Full width container */
.section-padding   /* Standard section padding */
```

---

## Git Workflow

```bash
# Create feature branch
git checkout -b feature/new-feature

# Make changes and commit
git add .
git commit -m "Add new feature"

# Push to remote
git push origin feature/new-feature

# Create Pull Request on GitHub
```

---

## Debugging Tips

### Frontend Issues
```typescript
// Check Redux/Zustand state
import { useCartStore } from '@/store';
const state = useCartStore((s) => s); // See all state in console

// API debugging
// Network tab in DevTools to see requests
// Check browser console for errors
```

### Backend Issues
```bash
# Check logs
npm run dev  # See all logs in terminal

# Debug specific request
# Add console.log() in controller
# Restart server (ctrl+c, npm run dev)

# Check database
# Use MongoDB Compass or Atlas UI
```

---

## Performance Tips

1. **Images**: Use Next.js Image component
2. **Code**: Implement lazy loading
3. **Database**: Use indexes for frequently queried fields
4. **API**: Implement pagination for lists
5. **Caching**: Use React Query or SWR (optional enhancement)

---

## Security Checklist

- ✅ Never commit .env files
- ✅ Use HTTPS in production
- ✅ Keep dependencies updated
- ✅ Validate all user inputs
- ✅ Hash passwords with bcrypt
- ✅ Use JWT for authentication
- ✅ Implement CORS properly
- ✅ Set secure cookie flags

---

## Deployment Quick Steps

### Deploy Frontend (Vercel)
```bash
npm i -g vercel
cd frontend
vercel
# Follow prompts, set environment variables in dashboard
```

### Deploy Backend (Railway)
```bash
npm i -g @railway/cli
cd backend
railway up
# Set environment variables in dashboard
```

---

## Useful Links

- **MongoDB Docs**: docs.mongodb.com
- **Razorpay Docs**: razorpay.com/docs
- **Next.js Docs**: nextjs.org/docs
- **Express Docs**: expressjs.com
- **Tailwind Docs**: tailwindcss.com/docs
- **TypeScript Docs**: typescriptlang.org

---

## When Something Goes Wrong

1. **Check error message** - Read the full error in console
2. **Check .env file** - Make sure all variables are set
3. **Restart servers** - Kill and restart npm run dev
4. **Clear cache** - Delete node_modules, npm install
5. **Check database** - Verify MongoDB connection
6. **Check logs** - Look in terminal for details

---

## Need Help?

1. Read **SETUP_GUIDE.md** - Setup instructions
2. Read **ARCHITECTURE.md** - How system works
3. Check inline **code comments** - Understanding the code
4. Check **DEPLOYMENT_CHECKLIST.md** - Before going live

---

**Good luck! You've got this! 🚀**

Made with ❤️ for the Jaipur Bedsheets project
