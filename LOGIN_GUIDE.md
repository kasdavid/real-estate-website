# Login & Authentication System Documentation

## Overview
Your Real Estate application now includes a complete login and authentication system with user registration, session management, and protected pages.

## Features

### ✅ Login System
- Email and password authentication
- Remember me functionality
- Form validation with clear error messages
- Loading states during authentication
- Session persistence (localStorage & sessionStorage)
- Auto-populate remembered email

### ✅ Registration System
- Complete user registration form
- Email validation
- Password strength requirements
- Password confirmation
- Multiple account types (Buyer, Seller, Agent)
- Duplicate email prevention
- Auto-login after registration

### ✅ User Management
- User profile storage in localStorage
- Role-based access (buyer, seller, agent)
- User menu in navigation bar with profile
- Logout functionality
- Session persistence across page refreshes

### ✅ Security Features
- Password hashing (demo implementation)
- Email validation
- Password strength validation
- Password visibility toggle
- Secure session management

## Demo Accounts

Test the login system with these demo accounts:

**Account 1 (Buyer)**
- Email: `demo@example.com`
- Password: `password123`

**Account 2 (Agent)**
- Email: `agent@example.com`
- Password: `agent123`

## File Structure

```
Real Estate/
├── login.html                   # Login page
├── register.html                # Registration page
├── js/
│   └── auth.js                 # Authentication module
└── css/
    └── styles.css              # Auth styling
```

## How to Use

### Logging In

1. Click the **"Login"** link in the navigation bar
2. Enter your email and password
3. Optionally check **"Remember me"** to auto-fill email next time
4. Click **"Sign In"**
5. If credentials are correct, you'll be redirected to the **Dashboard**

### Creating an Account

1. Click the **"Create one"** link on the login page
2. Fill in your details:
   - First Name
   - Last Name
   - Email Address
   - Account Type (Buyer, Seller, or Agent)
   - Password (min 8 characters with letters & numbers)
   - Confirm Password
3. Agree to Terms of Service
4. Click **"Create Account"**
5. Your account is created and you're automatically logged in

### Logging Out

1. Click your user avatar/name in the navigation bar
2. Select **"Logout"** from the dropdown menu
3. You'll be redirected to the home page

## Authentication API

### Getting Current User
```javascript
const user = getCurrentUser();
if (user) {
    console.log(user.email, user.role);
}
```

### Check if Logged In
```javascript
if (isUserLoggedIn()) {
    // User is logged in
}
```

### Protect a Page
Add this to pages that require login:
```javascript
// Call on page load
protectPage('login.html');
```

### Logout
```javascript
logout();
```

## User Data Structure

```javascript
{
    id: "user_1703123456",
    email: "user@example.com",
    firstName: "John",
    lastName: "Doe",
    role: "buyer",              // buyer, seller, or agent
    phone: "(555) 123-4567",
    loginTime: "2026-04-26T..."
}
```

## User Database Storage

Users are stored in localStorage under the key `realestate_users`:

```javascript
[
    {
        id: "user_123456",
        email: "user@example.com",
        password: "hash_encoded_password",
        firstName: "John",
        lastName: "Doe",
        role: "buyer",
        phone: "",
        createdAt: "2026-04-26T..."
    }
    // More users...
]
```

## Session Management

### Login Session
- **localStorage**: Persists across browser sessions (can last for months)
- **sessionStorage**: Only for current browser tab (cleared on close)

### Remember Me
- Email saved in `localStorage.rememberEmail`
- Auto-populated on next login page visit
- Cleared if "Remember me" is unchecked

## Validation Rules

### Email
- Must be valid format (user@domain.com)
- Must be unique (not already registered)

### Password
- Minimum 8 characters
- Must contain letters
- Must contain numbers
- Should contain special characters or uppercase (recommended)

### Name
- First and last name required
- Max 50 characters recommended

## Form Validation Errors

The system validates and shows these errors:
- ❌ Empty required fields
- ❌ Invalid email format
- ❌ Password too short
- ❌ Passwords don't match
- ❌ Weak password
- ❌ Email already registered
- ❌ Invalid credentials

## Password Visibility Toggle

Click the 👁️ icon in password fields to toggle visibility:
- Shows password as plain text
- Helps verify correct typing
- Useful for confirming password match

## Auto-Login After Registration

When you register a new account:
1. Account is created and stored
2. You're automatically logged in
3. Redirected to Dashboard

## Pages Requiring Login

These pages should be protected and redirect to login if not authenticated:
- `dashboard.html` - User dashboard
- `add-property.html` - Add new property
- `edit-property.html` - Edit property
- `favorites.html` - Saved favorites

### Enable Page Protection

Add this to the top of protected pages:
```html
<script>
    document.addEventListener('DOMContentLoaded', function() {
        protectPage('login.html');
    });
</script>
```

## User Role-Based Access

Different account types:
- **Buyer**: Browse and save properties
- **Seller**: Add and manage properties
- **Agent**: Manage client properties and listings

Store role in user object and check:
```javascript
const user = getCurrentUser();
if (user.role === 'seller') {
    // Show seller features
}
```

## Updating User Profile

To update user information:
```javascript
const users = JSON.parse(localStorage.getItem('realestate_users'));
const userIndex = users.findIndex(u => u.id === user.id);
if (userIndex !== -1) {
    users[userIndex] = { ...users[userIndex], ...updatedData };
    localStorage.setItem('realestate_users', JSON.stringify(users));
}
```

## Backend Integration (Future)

When moving to a real backend:

1. **Replace password hashing** with proper bcrypt on server
2. **Use API calls** instead of localStorage:
   ```javascript
   const response = await fetch('/api/login', {
       method: 'POST',
       body: JSON.stringify({ email, password })
   });
   ```
3. **Use JWT tokens** instead of user data
4. **Enable HTTPS** for production
5. **Implement refresh tokens** for session management

## Security Best Practices

✅ **Current**
- Email validation
- Password strength checking
- Session management
- Input sanitization

⚠️ **For Production**
- Use HTTPS only
- Implement backend API
- Use proper password hashing (bcrypt)
- Add rate limiting for login attempts
- Implement 2-factor authentication
- Use secure cookies instead of localStorage
- Add password reset functionality
- Log authentication events

## Troubleshooting

### "Invalid email or password" error
- Verify email is correct
- Check password is exactly right
- Ensure caps lock is off
- Try demo accounts first

### "Email already registered"
- Use a different email address
- Or login with existing email

### "Password must contain..."
- Password needs: min 8 chars, letters, numbers
- Good example: `MyPassword123`

### Remember me not working
- Check browser allows localStorage
- Browser privacy mode may block it

### Dropdown menu not showing
- Ensure JavaScript is enabled
- Check browser console for errors

### Redirected to login on protected page
- You're not logged in
- Login first, then navigate to page
- Check localStorage is working

## Testing Checklist

- [ ] Demo login works
- [ ] New user registration works
- [ ] Remember me saves email
- [ ] Password toggle shows/hides text
- [ ] Logout clears session
- [ ] Protected pages redirect to login
- [ ] User menu shows in navbar when logged in
- [ ] Session persists on page refresh
- [ ] Validation shows correct errors
- [ ] Loading states display during auth

## Support

For issues or questions:
1. Check browser console (F12) for errors
2. Verify localStorage is enabled
3. Test with demo accounts first
4. Clear browser cache if issues persist

---

**Last Updated**: April 2026
**Version**: 1.0 Complete Authentication System
