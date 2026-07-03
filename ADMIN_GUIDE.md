🔐 ADMIN CREDENTIALS & TESTING GUIDE
════════════════════════════════════════════════════════════════════════

## 📋 ADMIN CREDENTIALS

### Default Admin Account
```
Email (ईमेल):       admin@jaipur-bedsheets.com
Password (पासवर्ड):  Admin@123456

Role (भूमिका):     Admin
Status:            Active
```

---

## 🔗 ADMIN PANEL URLS

### Local Development (अपने कंप्यूटर पर)
```
Frontend: http://localhost:3000
Admin Dashboard: http://localhost:3000/admin
Backend API: http://localhost:5000/api
```

### After Google Cloud Deployment (गूगल क्लाउड के बाद)
```
Frontend: https://jaipur-bedsheets-frontend.run.app
Admin Dashboard: https://jaipur-bedsheets-frontend.run.app/admin
Backend API: https://jaipur-bedsheets-backend.run.app/api
```

---

## 🚀 HOW TO ACCESS ADMIN PANEL

### Step 1: Start the Application (अनुप्रयोग शुरू करें)

**Local Development:**
```bash
cd jaipur-bedsheets-ecommerce
npm run install-all
npm run dev
```

**Wait for:**
```
✓ Frontend ready on http://localhost:3000
✓ Backend ready on http://localhost:5000
```

### Step 2: Go to Login Page (लॉगिन पेज पर जाएं)
```
http://localhost:3000/login
```

### Step 3: Enter Admin Credentials (क्रेडेंशियल डालें)
```
Email: admin@jaipur-bedsheets.com
Password: Admin@123456
```

### Step 4: Click Login (लॉगिन करें)

### Step 5: Access Admin Dashboard (Admin Panel खोलें)
```
Click: http://localhost:3000/admin
OR
Direct URL: http://localhost:3000/admin
```

---

## 📊 WHAT YOU CAN DO IN ADMIN PANEL

✅ View Dashboard Statistics
  - Total Orders
  - Total Revenue
  - Total Customers
  - Total Products

✅ Manage Products
  - Add new products
  - Edit existing products
  - Delete products
  - View inventory

✅ Manage Orders
  - View all orders
  - Update order status
  - View order details
  - Process refunds

✅ View Analytics
  - Revenue charts
  - Customer metrics
  - Order trends

✅ Create Coupons
  - Discount codes
  - Promotional offers

✅ Quick Actions
  - Add product
  - Create coupon
  - Send newsletter

---

## 🧪 TEST CREDENTIALS (अन्य टेस्ट खाते)

### Regular Customer Account
```
Email: customer@test.com
Password: Test@123456
```

To create this account:
1. Go to http://localhost:3000/login
2. Click "Sign Up"
3. Fill details:
   - First Name: Test
   - Last Name: Customer
   - Email: customer@test.com
   - Password: Test@123456
4. Click Sign Up

---

## 💳 TEST PAYMENT CREDENTIALS (Razorpay)

### UPI Test Payment
```
ID: success@razorpay
Method: UPI
Status: Will be marked successful
```

### Credit Card Test Payment
```
Card Number: 4111 1111 1111 1111
Expiry: 12/25
CVV: 123
Name: Test User
```

### Debit Card Test Payment
```
Card Number: 5555 5555 5555 4444
Expiry: 12/25
CVV: 123
Name: Test User
```

### Coupon Code for Testing
```
Code: WELCOME10
Discount: 10% off
Valid: Always (in demo)
```

---

## ✅ COMPLETE TEST FLOW (पूरी टेस्टिंग)

### 1. Register New Account
```
1. Go to http://localhost:3000/login
2. Click "Sign Up"
3. Enter details
4. Submit
```

### 2. Browse Products
```
1. Go to http://localhost:3000
2. Scroll down to see featured products
3. Click "Shop Now" or "Shop"
4. Filter by price or category
5. Click on product to see details
```

### 3. Add to Cart
```
1. On product detail page
2. Select quantity
3. Click "Add to Cart"
4. See toast notification "Added to cart!"
```

### 4. Apply Coupon (कूपन लागू करें)
```
1. Go to http://localhost:3000/cart
2. Enter coupon: WELCOME10
3. Click "Apply"
4. See 10% discount!
```

### 5. Complete Checkout
```
1. Click "Proceed to Checkout"
2. Fill address details
3. Select payment method
4. Choose Razorpay (cards/UPI)
5. Click "Place Order"
```

### 6. Make Test Payment (टेस्ट पेमेंट करें)
```
1. Razorpay window opens
2. Select payment method:
   - UPI: success@razorpay
   - Card: 4111 1111 1111 1111
3. Fill details
4. Complete payment
5. Get order confirmation
```

### 7. Check Order
```
1. Go to http://localhost:3000/orders
2. See your order listed
3. Click to see tracking
```

### 8. Login as Admin (एडमिन के रूप में लॉगिन करें)
```
1. Go to http://localhost:3000/login
2. Email: admin@jaipur-bedsheets.com
3. Password: Admin@123456
4. Go to http://localhost:3000/admin
5. See your order in Recent Orders
6. Update order status
```

---

## 📱 OTHER USEFUL ENDPOINTS (अन्य उपयोगी URLs)

### Customer Pages
```
Home:              http://localhost:3000
Shop:              http://localhost:3000/shop
Product Detail:    http://localhost:3000/shop/any-product-slug
Cart:              http://localhost:3000/cart
Checkout:          http://localhost:3000/checkout
Orders:            http://localhost:3000/orders
Account:           http://localhost:3000/account
Wishlist:          http://localhost:3000/wishlist
About:             http://localhost:3000/about
Contact:           http://localhost:3000/contact
```

### Admin Pages
```
Admin Dashboard:   http://localhost:3000/admin
```

### API Endpoints
```
Health Check:      http://localhost:5000/api/health
Products:          http://localhost:5000/api/products
Orders:            http://localhost:5000/api/orders
Payments:          http://localhost:5000/api/payments
Categories:        http://localhost:5000/api/categories
```

---

## 🐛 DEBUGGING ADMIN PANEL

### If Admin Dashboard Not Loading

**Check 1:** Are you logged in?
```
Go to http://localhost:3000/login
Login with admin credentials
```

**Check 2:** Is backend running?
```
Open terminal
Check for: "Backend ready on http://localhost:5000"
If not, run: npm run dev
```

**Check 3:** Check browser console
```
F12 → Console tab
Look for error messages
Screenshot and troubleshoot
```

**Check 4:** Check backend logs
```
Look at npm run dev output
Check for API errors
```

---

## 📊 ADMIN PANEL FEATURES (विस्तार)

### Dashboard Tab (डैशबोर्ड)
Shows:
- 📦 Total Orders (कुल ऑर्डर)
- 💰 Total Revenue (कुल राजस्व)
- 👥 Total Customers (कुल ग्राहक)
- 📋 Total Products (कुल उत्पाद)
- 📊 Recent Orders (हाल के ऑर्डर)
- ⚡ Quick Actions (त्वरित कार्य)

### Manage Products (उत्पाद प्रबंधित करें)
```
Click: "Manage Products" (या "📦 Add New Product")
Can:
- View all products
- Add new product
- Edit product details
- Delete product
- Upload images
- Set price & discount
- Manage stock
```

### View Orders (ऑर्डर देखें)
```
Click: "View Orders" (या "📋 View Orders")
Can:
- See all customer orders
- View order details
- Update order status:
  - Pending → Confirmed
  - Confirmed → Processing
  - Processing → Shipped
  - Shipped → Delivered
- Process refunds
- Download invoice
```

### Analytics (विश्लेषण)
```
Click: "Analytics"
See:
- Total orders per month
- Revenue trends
- Customer growth
- Best selling products
- Top customers
```

### Coupons (कूपन)
```
Click: "Coupons"
Can:
- Create new coupon
- Set discount %
- Set expiry date
- Set min/max amount
- Track usage
```

---

## 🎬 VIDEO WALKTHROUGH (यदि आवश्यक हो)

### Recording Tips
1. Start from login
2. Show each admin feature
3. Create test product
4. Create test order
5. Update status
6. Show analytics

### Share with Team
- Email video link
- Or share screenshots
- Or share this guide

---

## 💡 TIPS FOR TESTING

✅ **Create Real Data**
- Add 5-10 test products
- Create 3-5 test orders
- Apply coupons
- Test different payment methods

✅ **Test Edge Cases**
- Out of stock products
- Maximum discount
- Minimum order amount
- Invalid coupon codes

✅ **Test Mobile**
- Open on phone
- Test responsive design
- Test checkout flow
- Test payment on mobile

✅ **Performance Test**
- Load 100 products
- Check loading time
- Monitor Network tab
- Check console errors

---

## 🚀 NEXT STEPS

1. ✅ Start app locally: `npm run dev`
2. ✅ Login as admin: admin@jaipur-bedsheets.com
3. ✅ Explore admin dashboard
4. ✅ Create test product
5. ✅ Test checkout flow
6. ✅ Test payment
7. ✅ Check order status
8. ✅ Deploy to Google Cloud

---

## ❓ COMMON QUESTIONS

**Q: Admin panel दिख नहीं रहा है?**
A: Check करें कि:
   - आप लॉगिन हैं?
   - Backend चल रहा है?
   - सही URL है: /admin

**Q: Payment test नहीं हो रहा है?**
A: Use test card: 4111 1111 1111 1111

**Q: Database में data नहीं दिख रहा है?**
A: Run करें: `npx ts-node src/scripts/seedData.ts`

**Q: New product add नहीं हो पा रहा है?**
A: Check करें:
   - Backend API running है?
   - सभी fields भरे हैं?
   - Images upload हो रहे हैं?

---

## 📞 SUPPORT

Need help?
1. Check SETUP_GUIDE.md
2. Check QUICK_REFERENCE.md
3. Check code comments
4. Check browser console (F12)
5. Check backend logs

---

## 🎉 YOU'RE READY!

```
Admin Email:    admin@jaipur-bedsheets.com
Admin Password: Admin@123456

Just login and explore! 🚀
```

Happy testing! 💪
