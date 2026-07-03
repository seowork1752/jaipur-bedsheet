🚀 GOOGLE CLOUD DEPLOYMENT GUIDE
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

(हिंदी में गाइड नीचे है | Scroll down for Hindi version)

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
ENGLISH VERSION
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

## OPTION 1: GOOGLE CLOUD RUN (RECOMMENDED - FREE TIER AVAILABLE)

### Step 1: Setup Google Cloud Account
1. Go to https://console.cloud.google.com
2. Sign in with Google account
3. Create a new project: "Jaipur-Bedsheets"
4. Enable billing (free tier: 2M requests/month)

### Step 2: Install Google Cloud CLI
```bash
# Download from: https://cloud.google.com/sdk/docs/install

# For Mac:
curl https://sdk.cloud.google.com | bash
exec -l $SHELL
gcloud init

# For Windows:
# Download installer from Google Cloud website
```

### Step 3: Login to Google Cloud
```bash
gcloud auth login
gcloud config set project jaipur-bedsheets
```

### Step 4: Deploy Frontend to Google Cloud Run (Next.js)

Create `frontend/Dockerfile.prod`:
```dockerfile
FROM node:18-alpine
WORKDIR /app
COPY package*.json ./
RUN npm ci --only=production
COPY . .
RUN npm run build
EXPOSE 3000
CMD ["npm", "start"]
```

Deploy:
```bash
cd frontend
gcloud run deploy jaipur-bedsheets-frontend \
  --source . \
  --platform managed \
  --region asia-south1 \
  --allow-unauthenticated \
  --port 3000
```

### Step 5: Deploy Backend to Google Cloud Run (Express)

Deploy:
```bash
cd backend
gcloud run deploy jaipur-bedsheets-backend \
  --source . \
  --platform managed \
  --region asia-south1 \
  --allow-unauthenticated \
  --port 5000 \
  --set-env-vars \
    NODE_ENV=production,\
    MONGODB_URI=your_mongodb_url,\
    JWT_SECRET=your_secret,\
    RAZORPAY_KEY_ID=your_key,\
    RAZORPAY_KEY_SECRET=your_secret
```

### Step 6: Setup MongoDB Atlas (Free)
1. Go to https://www.mongodb.com/cloud/atlas
2. Sign up (free)
3. Create cluster
4. Create database user
5. Get connection string
6. Add to environment variables

### Step 7: Update Frontend Environment
```bash
# After deployment, get backend URL from Cloud Run
# Edit frontend/.env.production:
NEXT_PUBLIC_API_URL=https://your-backend-url.run.app/api
```

### Step 8: Verify Deployment
- Frontend: https://jaipur-bedsheets-frontend.run.app
- Backend: https://jaipur-bedsheets-backend.run.app/api/health

---

## OPTION 2: VERCEL (FRONTEND) + RAILWAY (BACKEND)

### Frontend on Vercel
```bash
# Install Vercel CLI
npm i -g vercel

# Deploy
cd frontend
vercel --prod

# Follow prompts, set:
# NEXT_PUBLIC_API_URL = your backend URL
```

### Backend on Railway
1. Go to https://railway.app
2. Sign up with GitHub
3. Create new project
4. Connect your GitHub repo
5. Set environment variables
6. Deploy

---

## OPTION 3: AWS (PRODUCTION GRADE)

### Frontend on AWS S3 + CloudFront
```bash
# Build
npm run build

# Deploy to S3
aws s3 sync out/ s3://jaipur-bedsheets --delete
```

### Backend on AWS EC2 or Elastic Beanstalk
```bash
# Install AWS CLI
pip install awsebcli

# Deploy
eb create jaipur-bedsheets-api
eb deploy
```

---

## QUICK COMPARISON

| Platform | Frontend | Backend | Cost | Ease |
|----------|----------|---------|------|------|
| **Google Cloud Run** | ✅ | ✅ | Free tier | Medium |
| **Vercel + Railway** | ✅ | ✅ | Free tier | Easy |
| **AWS** | ✅ | ✅ | Paid | Hard |
| **Heroku + Vercel** | ✅ | ✅ | Paid | Medium |

---

## TROUBLESHOOTING

### Issue: Deployment Failed
```bash
# Check logs
gcloud run logs read jaipur-bedsheets-backend

# Check region availability
gcloud compute regions list
```

### Issue: Environment Variables Not Working
```bash
# Update variables
gcloud run services update jaipur-bedsheets-backend \
  --update-env-vars KEY=VALUE
```

### Issue: CORS Error
```bash
# Add to backend server.ts:
const cors = require('cors');
app.use(cors({
  origin: 'https://your-frontend-url.run.app'
}));
```

---

## MONITORING & LOGS

### View Logs
```bash
# Real-time logs
gcloud run logs read jaipur-bedsheets-backend --follow

# Specific timestamp
gcloud run logs read --limit 50
```

### Set Alerts
1. Go to Cloud Run console
2. Click service
3. Set up alerts for:
   - Errors
   - Latency
   - CPU usage

---

## CUSTOM DOMAIN

### Add Custom Domain
```bash
# Verify domain ownership
gcloud run services update jaipur-bedsheets-backend \
  --region asia-south1
```

Then in domain provider:
1. Add CNAME: `ghs.googlehosted.com`
2. Wait for DNS propagation

---

## SCALING

### Auto-scale
```bash
gcloud run services update jaipur-bedsheets-backend \
  --max-instances 100 \
  --min-instances 1
```

---

## SECURITY CHECKLIST

- [ ] Enable VPC Service Controls
- [ ] Setup IAM roles
- [ ] Enable Cloud Armor
- [ ] Setup Cloud CDN
- [ ] Enable Cloud Logging
- [ ] Setup Backup
- [ ] Enable HTTPS only

---

## COST ESTIMATION (Monthly)

**Google Cloud Run:**
- Compute: ~$5-20
- Database (MongoDB): ~$57
- Storage: ~$5
- Total: ~$65-80/month

**With Free Tier:**
- First 2M requests free
- Could be completely free!

---

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
हिंदी संस्करण (HINDI VERSION)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

## विकल्प 1: गूगल क्लाउड रन (सर्वोत्तम - फ्री टियर उपलब्ध)

### चरण 1: गूगल क्लाउड खाता सेटअप करें
1. https://console.cloud.google.com जाएं
2. अपने गूगल खाते से साइन इन करें
3. नया प्रोजेक्ट बनाएं: "Jaipur-Bedsheets"
4. बिलिंग सक्षम करें (फ्री टियर: 2M requests/माह)

### चरण 2: Google Cloud CLI इंस्टॉल करें
```bash
# Mac के लिए:
curl https://sdk.cloud.google.com | bash
exec -l $SHELL
gcloud init

# Windows के लिए:
# Google Cloud वेबसाइट से डाउनलोड करें
```

### चरण 3: गूगल क्लाउड में लॉगिन करें
```bash
gcloud auth login
gcloud config set project jaipur-bedsheets
```

### चरण 4: फ्रंटएंड को Google Cloud Run पर डिप्लॉय करें

डिप्लॉय करें:
```bash
cd frontend
gcloud run deploy jaipur-bedsheets-frontend \
  --source . \
  --platform managed \
  --region asia-south1 \
  --allow-unauthenticated \
  --port 3000
```

### चरण 5: बैकएंड को Google Cloud Run पर डिप्लॉय करें

डिप्लॉय करें:
```bash
cd backend
gcloud run deploy jaipur-bedsheets-backend \
  --source . \
  --platform managed \
  --region asia-south1 \
  --allow-unauthenticated \
  --port 5000 \
  --set-env-vars \
    NODE_ENV=production,\
    MONGODB_URI=आपका_mongodb_url,\
    JWT_SECRET=आपकी_secret,\
    RAZORPAY_KEY_ID=आपकी_key
```

### चरण 6: MongoDB Atlas सेटअप करें (फ्री)
1. https://www.mongodb.com/cloud/atlas जाएं
2. साइन अप करें (फ्री)
3. क्लस्टर बनाएं
4. डेटाबेस यूजर बनाएं
5. कनेक्शन स्ट्रिंग लें
6. Environment variables में जोड़ें

### चरण 7: फ्रंटएंड Environment अपडेट करें

डिप्लॉयमेंट के बाद:
```bash
# frontend/.env.production:
NEXT_PUBLIC_API_URL=https://आपका-backend-url.run.app/api
```

### चरण 8: डिप्लॉयमेंट सत्यापित करें
- फ्रंटएंड: https://jaipur-bedsheets-frontend.run.app
- बैकएंड: https://jaipur-bedsheets-backend.run.app/api/health

---

## विकल्प 2: Vercel + Railway (आसान)

### फ्रंटएंड (Vercel)
```bash
npm i -g vercel
cd frontend
vercel --prod
```

### बैकएंड (Railway)
1. https://railway.app जाएं
2. GitHub से साइन अप करें
3. नया प्रोजेक्ट बनाएं
4. GitHub repo कनेक्ट करें
5. Environment variables सेट करें
6. डिप्लॉय करें

---

## बेहद सरल तरीका (EASIEST WAY)

### सबसे आसान डिप्लॉयमेंट:

**फ्रंटएंड के लिए:**
```bash
# Step 1: Vercel पर जाएं
https://vercel.com

# Step 2: GitHub से कनेक्ट करें
# Step 3: Repository सिलेक्ट करें
# Step 4: Deploy बटन दबाएं
# बस! 5 मिनट में लाइव!
```

**बैकएंड के लिए:**
```bash
# Step 1: Railway पर जाएं
https://railway.app

# Step 2: GitHub से कनेक्ट करें
# Step 3: Repository सिलेक्ट करें
# Step 4: Environment Variables डालें
# Step 5: Deploy करें
# बस! 5 मिनट में लाइव!
```

---

## मासिक खर्च (Monthly Cost)

**Google Cloud Run:**
- कंप्यूट: ~₹400-1600
- डेटाबेस (MongoDB): ~₹4500
- स्टोरेज: ~₹400
- कुल: ~₹5200-6500/माह

**फ्री टियर के साथ:**
- पहले 2M requests फ्री
- **बिल्कुल फ्री हो सकता है!**

---

## समस्या निवारण

### समस्या: डिप्लॉयमेंट फेल हो रहा है
```bash
gcloud run logs read jaipur-bedsheets-backend
```

### समस्या: Environment Variables काम नहीं कर रहे
```bash
gcloud run services update jaipur-bedsheets-backend \
  --update-env-vars KEY=VALUE
```

### समस्या: CORS Error आ रहा है
Backend में add करें:
```javascript
app.use(cors({
  origin: 'https://आपका-frontend-url.run.app'
}));
```

---

## अगला कदम

✅ ऊपर दिया गया कोई भी तरीका चुनें
✅ 15-30 मिनट में डिप्लॉय हो जाएगा
✅ अपना URL मिल जाएगा
✅ Admin panel एक्सेस करेंगे

---

सवाल? मुझसे पूछो! 🚀
