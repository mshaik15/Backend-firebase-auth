# Firebase Authentication API

A comprehensive, production-ready Firebase Authentication API built with Node.js, TypeScript, and Express.js. This system provides secure user authentication with advanced security features, rate limiting, and scalable architecture.

## 🚀 Features

### 🔐 Authentication & Security
- **Firebase Authentication Integration** - Secure user registration, login, and token management
- **JWT Token Verification** - Access tokens in Authorization header with Bearer scheme
- **Refresh Token Management** - HttpOnly, Secure, SameSite cookies for refresh tokens
- **Token Revocation** - Force logout from all devices
- **Rate Limiting** - Protection against brute-force attacks
- **CORS Configuration** - Configurable allowed origins
- **Helmet Security** - Security headers and CSP
- **Input Validation** - Comprehensive request validation with express-validator

### 🏗️ Architecture & Scalability
- **TypeScript** - Type-safe development with comprehensive type definitions
- **Modular Structure** - Clean separation of concerns (routes, services, middleware)
- **Error Handling** - Centralized error handling with user-friendly messages
- **Environment Configuration** - Flexible environment variable management
- **Graceful Shutdown** - Proper server shutdown handling
- **Health Checks** - Built-in health monitoring endpoints

### 📡 API Endpoints
- `POST /auth/register` - User registration with email verification
- `POST /auth/login` - User authentication
- `POST /auth/logout` - User logout with token cleanup
- `POST /auth/refresh-token` - Access token refresh
- `POST /auth/reset-password` - Password reset email
- `POST /auth/verify-email` - Email verification
- `GET /auth/profile` - User profile retrieval
- `DELETE /auth/account` - Account deletion
- `POST /auth/revoke-tokens` - Revoke all user tokens
- `GET /health` - Health check endpoint
- `GET /api` - API documentation

## 🛠️ Installation & Setup

### Prerequisites
- Node.js 18+ and npm 8+
- Firebase project with Authentication enabled
- Firebase Admin SDK service account

### 1. Clone and Install Dependencies
```bash
git clone <repository-url>
cd firebase-auth-api
npm install
```

### 2. Firebase Setup
1. Create a Firebase project at [Firebase Console](https://console.firebase.google.com)
2. Enable Authentication with Email/Password provider
3. Generate Firebase Admin SDK private key:
   - Go to Project Settings → Service Accounts
   - Click "Generate new private key"
   - Download the JSON file

### 3. Environment Configuration
```bash
# Copy environment template
cp .env.example .env

# Edit .env with your Firebase credentials
nano .env
```

**Required Environment Variables:**
```env
# Server
NODE_ENV=development
PORT=3000

# Firebase Client Config
FIREBASE_API_KEY=your_api_key
FIREBASE_AUTH_DOMAIN=your_project.firebaseapp.com
FIREBASE_PROJECT_ID=your_project_id
FIREBASE_STORAGE_BUCKET=your_project.appspot.com
FIREBASE_MESSAGING_SENDER_ID=your_sender_id
FIREBASE_APP_ID=your_app_id

# Firebase Admin (choose one method)
# Method 1: Base64 encoded service account (recommended for production)
FIREBASE_SERVICE_ACCOUNT_BASE64=base64_encoded_service_account

# Method 2: Individual fields
# FIREBASE_CLIENT_EMAIL=service_account_email
# FIREBASE_PRIVATE_KEY="-----BEGIN PRIVATE KEY-----\nkey\n-----END PRIVATE KEY-----\n"

# Security
COOKIE_SECRET=your_secure_random_string
ALLOWED_ORIGINS=http://localhost:3000,http://localhost:5173
```

### 4. Development
```bash
# Start development server with hot reload
npm run dev

# Build for production
npm run build

# Start production server
npm start
```

## 📚 API Usage

### Registration
```bash
curl -X POST http://localhost:3000/auth/register \
  -H "Content-Type: application/json" \
  -d '{
    "email": "user@example.com",
    "password": "SecurePass123",
    "displayName": "John Doe"
  }'
```

### Login
```bash
curl -X POST http://localhost:3000/auth/login \
  -H "Content-Type: application/json" \
  -d '{
    "email": "user@example.com",
    "password": "SecurePass123"
  }'
```

### Access Protected Routes
```bash
curl -X GET http://localhost:3000/auth/profile \
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN"
```

### Refresh Token
```bash
curl -X POST http://localhost:3000/auth/refresh-token \
  -H "Content-Type: application/json" \
  --cookie "refreshToken=YOUR_REFRESH_TOKEN"
```

## 🔒 Security Features

### Token Management
- **Access Tokens**: Short-lived tokens sent in Authorization header
- **Refresh Tokens**: Long-lived tokens stored in HttpOnly cookies
- **Token Rotation**: Automatic refresh token rotation for enhanced security
- **Token Revocation**: Ability to revoke all tokens (logout from all devices)

### Security Headers
- **Helmet.js**: Comprehensive security headers
- **CORS**: Configurable cross-origin resource sharing
- **CSP**: Content Security Policy protection
- **HSTS**: HTTP Strict Transport Security

### Rate Limiting
- **Global Rate Limiting**: 1000 requests per 15 minutes per IP
- **Auth Rate Limiting**: 5 authentication attempts per 15 minutes per IP
- **Trusted IPs**: Bypass rate limiting for specified IPs

### Input Validation
- **Email Validation**: RFC compliant email validation
- **Password Strength**: Minimum requirements with complexity rules
- **Request Sanitization**: XSS and injection attack prevention

## 🏗️ Project Structure

```
src/
├── config/
│   └── firebase.ts           # Firebase Admin SDK configuration
├── middleware/
│   └── auth.middleware.ts    # Authentication middleware
├── routes/
│   └── auth.routes.ts        # Authentication routes
├── services/
│   └── auth.service.ts       # Authentication business logic
├── types/
│   └── auth.types.ts         # TypeScript type definitions
└── server.ts                 # Express server setup
```

## 🧪 Testing

### Run Tests
```bash
# Run all tests
npm test

# Run tests in watch mode
npm run test:watch

# Run tests with coverage
npm run test:coverage
```

### Example Test Cases
```typescript
// Test user registration
describe('POST /auth/register', () => {
  it('should register a new user', async () => {
    const response = await request(app)
      .post('/auth/register')
      .send({
        email: 'test@example.com',
        password: 'Test123!',
        displayName: 'Test User'
      });
    
    expect(response.status).toBe(201);
    expect(response.body.success).toBe(true);
  });
});
```

## 🚀 Deployment

### Vercel Deployment
```bash
# Install Vercel CLI
npm i -g vercel

# Deploy
vercel

# Set environment variables in Vercel dashboard
```

### Docker Deployment
```dockerfile
FROM node:18-alpine
WORKDIR /app
COPY package*.json ./
RUN npm ci --only=production
COPY dist ./dist
EXPOSE 3000
CMD ["node", "dist/server.js"]
```

### Environment Variables for Production
```env
NODE_ENV=production
PORT=3000
FIREBASE_SERVICE_ACCOUNT_BASE64=your_base64_encoded_service_account
COOKIE_SECRET=your_production_secret
ALLOWED_ORIGINS=https://yourdomain.com,https://www.yourdomain.com
```

## 🔧 Configuration Options

### Firebase Admin SDK Setup
Choose one of three methods:

1. **Base64 Encoded (Recommended for Production)**
   ```env
   FIREBASE_SERVICE_ACCOUNT_BASE64=base64_encoded_json
   ```

2. **Individual Environment Variables**
   ```env
   FIREBASE_PROJECT_ID=your_project_id
   FIREBASE_CLIENT_EMAIL=service@project.iam.gserviceaccount.com
   FIREBASE_PRIVATE_KEY="-----BEGIN PRIVATE KEY-----\n...\n-----END PRIVATE KEY-----\n"
   ```

3. **Service Account File Path (Development)**
   ```env
   FIREBASE_SERVICE_ACCOUNT_PATH=./config/service-account.json
   ```

### Rate Limiting Configuration
```typescript
// Custom rate limiting
const customRateLimit = rateLimit({
  windowMs: 15 * 60 * 1000, // 15 minutes
  max: 100, // limit each IP to 100 requests per windowMs
  message: 'Too many requests from this IP'
});
```

## 🌟 Advanced Features

### Role-Based Access Control (RBAC)
```typescript
// Set user roles
await firebaseAdmin.setCustomUserClaims(uid, { role: 'admin' });

// Protect routes with roles
app.get('/admin', verifyAccessToken, requireRole(['admin']), handler);
```

### Custom Claims and Permissions
```typescript
// Set custom permissions
await firebaseAdmin.setCustomUserClaims(uid, {
  role: 'editor',
  permissions: ['read', 'write', 'delete']
});

// Check permissions in middleware
app.get('/sensitive', verifyAccessToken, requirePermission('delete'), handler);
```

### Social Login Integration (Future)
```typescript
// Ready for social providers
const socialProviders = {
  google: 'google.com',
  facebook: 'facebook.com',
  twitter: 'twitter.com',
  github: 'github.com'
};
```

## 🔄 Frontend Integration

### React/Next.js Example
```typescript
// Login function
const login = async (email: string, password: string) => {
  const response = await fetch('/auth/login', {
    method: 'POST',
    headers: { 'Content-Type': 'application/json' },
    credentials: 'include', // Important for cookies
    body: JSON.stringify({ email, password })
  });
  
  const data = await response.json();
  
  if (data.success) {
    // Store access token
    localStorage.setItem('accessToken', data.data.accessToken);
    // Refresh token is automatically stored in HttpOnly cookie
  }
};

// API calls with authentication
const fetchProtectedData = async () => {
  const token = localStorage.getItem('accessToken');
  
  const response = await fetch('/api/protected', {
    headers: {
      'Authorization': `Bearer ${token}`
    },
    credentials: 'include'
  });
  
  if (response.status === 401) {
    // Token expired, try to refresh
    await refreshToken();
    // Retry the request
  }
};

// Token refresh
const refreshToken = async () => {
  const response = await fetch('/auth/refresh-token', {
    method: 'POST',
    credentials: 'include' // Important for refresh token cookie
  });
  
  if (response.ok) {
    const data = await response.json();
    localStorage.setItem('accessToken', data.data.accessToken);
  } else {
    // Refresh failed, redirect to login
    window.location.href = '/login';
  }
};
```

## 🛡️ Security Best Practices

### Implemented Security Measures
1. **HTTPS Only**: Secure cookies only work over HTTPS in production
2. **HttpOnly Cookies**: Refresh tokens cannot be accessed by JavaScript
3. **SameSite Cookies**: CSRF protection
4. **Token Expiration**: Short-lived access tokens
5. **Rate Limiting**: Brute-force attack prevention
6. **Input Validation**: XSS and injection prevention
7. **Security Headers**: Comprehensive protection with Helmet.js

### Additional Recommendations
1. **Monitor Login Attempts**: Track failed login attempts
2. **IP Whitelisting**: Restrict admin access to specific IPs
3. **Two-Factor Authentication**: Add 2FA for enhanced security
4. **Database Encryption**: Encrypt sensitive data at rest
5. **Regular Security Audits**: Keep dependencies updated

## 🐛 Troubleshooting

### Common Issues

1. **Firebase Admin SDK Error**
   ```
   Error: Failed to initialize Firebase Admin SDK
   ```
   - Check service account credentials
   - Verify project ID matches Firebase project
   - Ensure private key format is correct

2. **CORS Errors**
   ```
   Access to fetch blocked by CORS policy
   ```
   - Add your frontend URL to `ALLOWED_ORIGINS`
   - Ensure credentials are included in requests

3. **Token Verification Failed**
   ```
   Error: Invalid or expired token
   ```
   - Check token format in Authorization header
   - Verify Firebase project configuration
   - Ensure token hasn't expired

## 📄 License

MIT License - see LICENSE file for details.

## 🤝 Contributing

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

## 📞 Support

- 📧 Email: your.email@example.com
- 🐛 Issues: [GitHub Issues](https://github.com/yourusername/firebase-auth-api/issues)
- 📖 Documentation: [API Docs](https://your-api-docs-url.com)

---

**Built with ❤️ using Node.js, TypeScript, Firebase, and Express.js**