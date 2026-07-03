# 🏗️ Technical Architecture - Jaipur Bedsheets Ecommerce

## System Overview

```
┌─────────────────────────────────────────────────────────────┐
│                    Client (Web & Mobile)                      │
│                                                               │
│  Browser → Next.js Frontend (React + TypeScript)             │
└───────────────────────────┬─────────────────────────────────┘
                            │
                   HTTP/REST API (Axios)
                            │
         ┌──────────────────┴──────────────────┐
         │                                     │
    ┌────▼──────┐                    ┌────────▼────────┐
    │  CDN/Cache │                   │  Express.js API │
    │  (Cloudinary)                  │  (Node.js)      │
    └────┬───────┘                   └────────┬────────┘
         │                                    │
         │         ┌─────────────────────────┼──────────────────┐
         │         │                         │                  │
         │    ┌────▼─────┐          ┌────────▼────────┐   ┌────▼──────┐
         │    │ MongoDB   │          │ Razorpay API    │   │ Email/SMS  │
         │    │ Database  │          │ (Payment)       │   │ Services   │
         │    └──────────┘          └─────────────────┘   └───────────┘
         │
    ┌────▼──────┐
    │  Static   │
    │  Assets   │
    └───────────┘
```

## Frontend Architecture

### Tech Stack
- **Framework**: Next.js 14 (React 18)
- **Language**: TypeScript
- **Styling**: Tailwind CSS
- **State**: Zustand
- **HTTP**: Axios
- **Animations**: Framer Motion
- **UI Components**: Custom + Headless

### Project Structure
```
frontend/
├── src/
│   ├── pages/              # Route pages (auto-routing)
│   │   ├── index.tsx       # Home page
│   │   ├── shop/
│   │   │   ├── index.tsx   # Product listing
│   │   │   └── [slug].tsx  # Product details
│   │   ├── cart.tsx        # Shopping cart
│   │   ├── checkout.tsx    # Checkout page
│   │   ├── orders/
│   │   └── account/
│   ├── components/
│   │   ├── layout/         # Header, Footer, Navigation
│   │   ├── product/        # Product card, gallery
│   │   ├── cart/           # Cart item, cart summary
│   │   ├── checkout/       # Address, payment form
│   │   └── common/         # Button, modal, pagination
│   ├── store/              # Zustand stores
│   │   └── index.ts        # User, Cart, Theme stores
│   ├── utils/              # Helper functions
│   │   └── api.ts          # API client
│   ├── styles/             # Global CSS
│   └── types/              # TypeScript types
├── public/                 # Static assets
├── next.config.js          # Next.js config
└── tailwind.config.js      # Tailwind config

```

### State Management (Zustand)
```typescript
// User Store - Authentication & Profile
useUserStore: {
  user: User | null
  token: string | null
  isAuthenticated: boolean
  setUser(user)
  setToken(token)
  logout()
}

// Cart Store - Shopping Cart
useCartStore: {
  items: CartItem[]
  total: number
  discount: number
  addItem(item)
  removeItem(productId)
  updateQuantity(productId, quantity)
  clearCart()
}

// Theme Store - Dark Mode
useThemeStore: {
  isDarkMode: boolean
  toggleDarkMode()
}
```

### API Integration Pattern
```typescript
// Centralized API client with interceptors
const apiClient = axios.create({
  baseURL: process.env.NEXT_PUBLIC_API_URL
});

// Request interceptor: Add auth token
// Response interceptor: Handle 401 errors
// Auto-redirect to login on token expiry
```

## Backend Architecture

### Tech Stack
- **Runtime**: Node.js 18+
- **Framework**: Express.js 4
- **Language**: TypeScript
- **Database**: MongoDB 7
- **Authentication**: JWT
- **Payment**: Razorpay
- **File Storage**: Cloudinary

### Project Structure
```
backend/
├── src/
│   ├── controllers/        # Business logic
│   │   ├── authController.ts
│   │   ├── productController.ts
│   │   ├── orderController.ts
│   │   ├── paymentController.ts
│   │   └── userController.ts
│   ├── models/             # MongoDB schemas
│   │   ├── User.ts
│   │   ├── Product.ts
│   │   ├── Order.ts
│   │   ├── Cart.ts
│   │   └── Category.ts
│   ├── routes/             # Express routes
│   │   ├── auth.ts
│   │   ├── products.ts
│   │   ├── orders.ts
│   │   ├── payments.ts
│   │   └── users.ts
│   ├── middleware/         # Express middleware
│   │   ├── auth.ts         # JWT verification
│   │   ├── errorHandler.ts # Error handling
│   │   └── validation.ts   # Input validation
│   ├── utils/              # Helper functions
│   │   ├── auth.ts         # JWT, passwords, tokens
│   │   ├── response.ts     # Standardized responses
│   │   └── mailer.ts       # Email service
│   ├── config/             # Configuration
│   │   └── database.ts     # MongoDB connection
│   └── server.ts           # Express app setup
├── dist/                   # Compiled JavaScript
├── .env.example
├── tsconfig.json
└── package.json
```

### Data Models

#### User Model
```
User {
  _id: ObjectId
  email: string (unique, lowercase)
  password: string (bcrypt hashed)
  firstName: string
  lastName: string
  phone: string
  addresses: Array<Address>
  wishlist: Array<ObjectId> → Product
  loyaltyPoints: number
  referralCode: string
  role: 'customer' | 'admin' | 'manager' | 'support'
  status: 'active' | 'inactive' | 'suspended'
  createdAt: Date
  updatedAt: Date
}
```

#### Product Model
```
Product {
  _id: ObjectId
  name: string
  slug: string (unique)
  description: string
  category: ObjectId → Category
  collection: string
  price: number
  discountPrice: number
  stock: number
  images: {
    front: string
    back: string
    folded: string
    closeup: string
    bedroom: string
    gallery: string[]
  }
  variants: Array<Variant>
  specifications: {
    fabric: string
    weave: string
    thread_count: number
    gsm: number
    washcare: string[]
    care_instructions: string
  }
  reviews: Array<Review>
  avgRating: number
  totalReviews: number
  tags: string[]
  status: 'active' | 'inactive' | 'draft'
  featured: boolean
  bestseller: boolean
  trending: boolean
  seo: {
    metaTitle: string
    metaDescription: string
    metaKeywords: string[]
    schema: object
  }
  createdAt: Date
  updatedAt: Date
}
```

#### Order Model
```
Order {
  _id: ObjectId
  orderNumber: string (unique)
  userId: ObjectId → User
  items: Array<OrderItem>
  shippingAddress: Address
  billingAddress: Address
  subtotal: number
  discount: number
  tax: number
  total: number
  paymentMethod: 'upi' | 'card' | 'cod' | ...
  paymentStatus: 'pending' | 'completed' | 'failed'
  paymentId: string
  transactionId: string
  orderStatus: 'pending' | 'confirmed' | 'shipped' | 'delivered'
  trackingNumber: string
  estimatedDeliveryDate: Date
  giftWrap: boolean
  returnStatus: 'not_initiated' | 'initiated' | 'approved'
  refundStatus: 'pending' | 'processed' | 'failed'
  createdAt: Date
  updatedAt: Date
}
```

### API Endpoints Structure
```
/api
├── /auth
│   ├── POST   /register
│   ├── POST   /login
│   ├── GET    /me (Protected)
│   ├── PUT    /profile (Protected)
│   └── POST   /change-password (Protected)
├── /products
│   ├── GET    / (filterable)
│   ├── GET    /:id
│   ├── GET    /slug/:slug
│   ├── POST   / (Admin)
│   ├── PUT    /:id (Admin)
│   └── DELETE /:id (Admin)
├── /orders
│   ├── POST   / (Protected)
│   ├── GET    / (Protected)
│   ├── GET    /:id (Protected)
│   ├── PUT    /:id/cancel (Protected)
│   └── POST   /:id/return (Protected)
├── /payments
│   ├── POST   /razorpay/order/:orderId
│   ├── POST   /razorpay/verify
│   ├── GET    /status/:orderId
│   └── POST   /coupon
├── /cart (Protected)
│   ├── GET    /
│   ├── POST   /add
│   ├── PUT    /update/:productId
│   └── DELETE /remove/:productId
└── /wishlist (Protected)
    ├── GET    /
    ├── POST   /add/:productId
    └── DELETE /remove/:productId
```

### Authentication Flow
```
1. User submits login credentials
2. Server verifies password (bcrypt)
3. Server generates JWT token
4. Token sent to client
5. Client stores token in localStorage
6. Client includes token in Authorization header
7. Server verifies token on each request
8. Token refresh on expiry
9. Logout clears token
```

### Payment Processing Flow
```
1. Order created with status: 'pending'
2. Frontend requests Razorpay order from backend
3. Backend creates Razorpay order via API
4. Frontend opens Razorpay checkout
5. User completes payment
6. Razorpay returns payment details to frontend
7. Frontend submits payment verification to backend
8. Backend verifies signature with Razorpay secret
9. Backend updates order: status = 'confirmed'
10. Order confirmation email sent
11. Webhook for additional updates
```

## Database Design

### MongoDB Indexing Strategy
```javascript
// Users Collection
db.users.createIndex({ email: 1 })              // Login
db.users.createIndex({ phone: 1 })              // Phone lookup
db.users.createIndex({ referralCode: 1 })      // Referral tracking

// Products Collection
db.products.createIndex({ category: 1, status: 1 })     // Category filtering
db.products.createIndex({ featured: 1, status: 1 })     // Featured products
db.products.createIndex({ bestseller: 1, status: 1 })   // Top sellers
db.products.createIndex({ name: "text", description: "text" }) // Full-text search
db.products.createIndex({ slug: 1 })                     // URL slug lookup

// Orders Collection
db.orders.createIndex({ userId: 1, createdAt: -1 })    // User order history
db.orders.createIndex({ paymentStatus: 1 })             // Payment tracking
db.orders.createIndex({ orderStatus: 1 })               // Order status
db.orders.createIndex({ orderNumber: 1 })               // Order lookup

// Cart Collection
db.carts.createIndex({ userId: 1 }, { unique: true })  // One cart per user
```

### Aggregation Pipelines
```javascript
// Product popularity ranking
db.products.aggregate([
  { $match: { status: 'active' } },
  { $lookup: { from: 'orders', localField: '_id', foreignField: 'items.productId', as: 'orders' } },
  { $addFields: { popularity: { $size: '$orders' } } },
  { $sort: { popularity: -1 } },
  { $limit: 10 }
])

// Monthly revenue report
db.orders.aggregate([
  { $match: { paymentStatus: 'completed' } },
  { $group: { _id: { $month: '$createdAt' }, revenue: { $sum: '$total' } } },
  { $sort: { _id: 1 } }
])
```

## Scalability Considerations

### Horizontal Scaling
- Load balancer (Nginx) distributes requests
- Multiple backend instances behind load balancer
- MongoDB replica set for high availability
- Redis for session management across instances
- CDN for static asset delivery

### Caching Strategy
- Browser caching for static assets
- Redis caching for frequently accessed data
- Product list pagination to limit data
- Query result caching for reports

### Performance Optimization
- Database query optimization with indexes
- API response pagination
- Lazy loading of images
- Gzip compression
- Code splitting in frontend
- Image optimization (WebP format)

## Security Architecture

### Authentication & Authorization
```
JWT Token Structure:
{
  userId: "user_id",
  iat: 1234567890,
  exp: 1234654290
}

Protected Routes:
- Require valid JWT in Authorization header
- Token verified using JWT_SECRET
- Token expiry enforced
- Refresh token mechanism implemented
```

### Data Protection
- Passwords hashed with bcrypt (salt rounds: 10)
- Sensitive data encrypted at rest
- SSL/HTTPS for data in transit
- Secrets stored in environment variables
- Database credentials not in code

### API Security
- CORS configured for allowed origins
- Rate limiting on sensitive endpoints
- Input validation on all endpoints
- SQL injection prevention (using MongoDB)
- CSRF tokens for state-changing operations

## Monitoring & Logging

### Application Monitoring
- Error tracking (Sentry)
- Performance monitoring (APM)
- Uptime monitoring
- Real-time alerting

### Logging
- Application logs (Winston/Pino)
- Database query logs
- API request/response logs
- Error stack traces
- Centralized log aggregation (ELK stack)

### Metrics
- Response times
- Error rates
- Database query performance
- Memory usage
- CPU usage
- Request throughput

## Deployment Architecture

### Development
- Local MongoDB
- Local backend server
- Local frontend dev server
- Docker Compose for full stack

### Staging
- MongoDB Atlas staging cluster
- Backend on staging server
- Frontend on Vercel preview
- Production-like configuration
- Full testing before production

### Production
- MongoDB Atlas production cluster (replicated)
- Backend on Railway/Heroku with auto-scaling
- Frontend on Vercel with CDN
- Cloudinary for image hosting
- Backup strategy in place
- Disaster recovery plan

## Technology Decisions & Trade-offs

### Why MongoDB?
- Flexible schema for product variants
- Horizontal scalability
- Good for ecommerce use case
- Atlas provides managed database

### Why Next.js?
- Server-side rendering for SEO
- Static site generation for performance
- Built-in image optimization
- Automatic code splitting
- Great developer experience

### Why Tailwind CSS?
- Rapid development
- Consistent design system
- Small production bundle
- Great for responsive design

### Why Zustand?
- Lightweight state management
- Simple API
- Built-in persistence
- Good for global state

## Future Enhancements

- [ ] GraphQL API layer
- [ ] Real-time notifications (WebSocket)
- [ ] Advanced caching (Redis)
- [ ] Search engine (Elasticsearch)
- [ ] Message queue (RabbitMQ)
- [ ] Microservices architecture
- [ ] Machine learning recommendations
- [ ] Multi-region deployment
- [ ] A/B testing framework
- [ ] Mobile app (React Native)

---

**Last Updated**: 2024
**Architecture Version**: 1.0
**Status**: Production Ready ✅
