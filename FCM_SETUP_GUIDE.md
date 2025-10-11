# 🔥 FCM Push Notification Setup Guide

## ✅ What's Already Implemented

### Backend (Django)

- ✅ FCM models and database tables created
- ✅ FCM service with push notification functionality
- ✅ API endpoints for token registration
- ✅ Admin integration to send notifications when food is added
- ✅ Database migrations applied

### Frontend (React Native)

- ✅ FCM service created
- ✅ Token registration and management
- ✅ Message handlers for foreground/background notifications
- ✅ App.js integration

## 🔧 What You Need to Do

### Step 1: Get FCM Server Key

1. Go to [Firebase Console](https://console.firebase.google.com/)
2. Select your project (or create one if you haven't)
3. Go to **Project Settings** (gear icon)
4. Click on **Cloud Messaging** tab
5. Copy the **Server Key** (it looks like: `AAAA...`)

### Step 2: Update Django Settings

Edit `leftoverlink-backend/leftoverlink/settings.py`:

```python
# FCM Settings
FCM_SERVER_KEY = "YOUR_ACTUAL_FCM_SERVER_KEY_HERE"  # Replace with your server key
```

### Step 3: Test the Implementation

#### Test Backend:

1. Start your Django server: `python manage.py runserver`
2. Go to Django admin: `http://127.0.0.1:8000/admin/`
3. Add a new food item
4. Check the console for push notification logs

#### Test Frontend:

1. Run your React Native app on a **real device** (not simulator)
2. Login as a customer
3. Check the console for FCM token registration
4. Add food from admin panel
5. Customer should receive push notification

## 📱 How It Works

### When Admin Adds Food:

```
Admin adds food → Django detects new food → Sends FCM to all customers → Customers get notification
```

### Notification Content:

- **Title:** "🍕 New Food Available!"
- **Body:** "{Food Name} is now available for just {Price}"
- **Data:** Food ID, name, price, quantity

## 🧪 Testing Endpoints

### Register FCM Token:

```bash
POST http://127.0.0.1:8000/api/fcm/register/
Headers: Authorization: Bearer YOUR_JWT_TOKEN
Body: {
  "fcm_token": "device_fcm_token",
  "device_type": "android"
}
```

### Send Test Notification:

```bash
POST http://127.0.0.1:8000/api/fcm/test/
Headers: Authorization: Bearer YOUR_JWT_TOKEN
Body: {
  "title": "Test Title",
  "body": "Test message"
}
```

## 🔍 Troubleshooting

### Common Issues:

1. **"FCM_SERVER_KEY not configured"**

   - Make sure you've added the server key to settings.py

2. **"No active customer devices found"**

   - Make sure customers have logged in and registered their FCM tokens

3. **Notifications not appearing**

   - Test on real device (not simulator)
   - Check notification permissions
   - Verify FCM token registration

4. **"FCM service not initialized"**
   - Check if FCM_SERVER_KEY is properly set
   - Restart Django server after adding the key

### Debug Steps:

1. Check Django console for FCM logs
2. Check React Native console for token registration
3. Verify Firebase project configuration
4. Test with a simple notification first

## 🎯 Next Steps

Once basic notifications work, you can:

1. Add more notification types (order updates, promotions, etc.)
2. Implement notification categories
3. Add rich notifications with images
4. Create notification history
5. Add notification preferences

## 📞 Support

If you encounter issues:

1. Check the console logs
2. Verify Firebase project setup
3. Ensure you're testing on a real device
4. Check network connectivity

The implementation is ready - just add your FCM server key and test!
