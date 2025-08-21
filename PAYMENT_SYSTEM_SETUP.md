# Payment System Setup Guide

## Overview
This guide explains the complete payment system implementation for the Teja app, including Razorpay integration, order management, and push notifications to agent and admin topics.

## Features Implemented

### 1. Payment Screen (`PaymentScreen`)
- **Location**: `lib/features/payment/screens/payment_screen.dart`
- **Features**:
  - Cart summary with item details and quantities
  - Total amount calculation with discounts
  - Payment method selection (UPI, Card, Cash on Delivery)
  - Razorpay integration for online payments
  - Order placement with payment status tracking

### 2. Payment Methods Supported
- **UPI Payment**: Integrated with Razorpay
- **Debit/Credit Card**: Integrated with Razorpay
- **Cash on Delivery (COD)**: Direct order placement

### 3. Order Management
- **Enhanced OrderModel**: Added payment status, payment method, and payment ID fields
- **Payment Status Tracking**: Pending, Completed, Failed, Refunded
- **Order Status**: Automatic status updates based on payment method

### 4. Push Notifications
- **Customer Notifications**: In-app notifications for order confirmation
- **Agent Topic**: `agent_dailymilk` - Receives new order notifications
- **Admin Topic**: `admin_dailymilk` - Receives new order and status update notifications

## Setup Instructions

### 1. Razorpay Configuration
1. Sign up at [Razorpay Dashboard](https://dashboard.razorpay.com/)
2. Get your API keys (Key ID and Key Secret)
3. Update the key in `PaymentScreen`:
   ```dart
   'key': 'rzp_test_1DP5mmOlF5G5ag', // Replace with your actual key
   ```

### 2. Firebase Cloud Functions Setup
1. Deploy the topic notification functions:
   ```bash
   cd functions
   npm install
   firebase deploy --only functions
   ```

2. The following functions will be deployed:
   - `processTopicNotification`: Processes queued topic notifications
   - `cleanupProcessedNotifications`: Cleans up old notifications
   - `sendTestTopicNotification`: Manual testing endpoint

### 3. Firebase Firestore Collections
The system uses these collections:
- `orders`: Order data with payment information
- `topic_notifications`: Queue for topic notifications
- `notifications`: In-app notifications for users
- `emailQueue`: Email notifications queue

### 4. Agent and Admin App Integration
Ensure your agent and admin apps subscribe to the respective topics:
- **Agent App**: Subscribe to `agent_dailymilk`
- **Admin App**: Subscribe to `admin_dailymilk`

```dart
await FirebaseMessaging.instance.subscribeToTopic('agent_dailymilk');
await FirebaseMessaging.instance.subscribeToTopic('admin_dailymilk');
```

## Usage Flow

### 1. Customer Journey
1. Add items to cart
2. Proceed to checkout
3. Enter delivery address and notes
4. Select payment method on Payment Screen
5. Complete payment (if online) or place COD order
6. Receive order confirmation

### 2. Agent/Admin Notifications
1. Receive push notification when new order is placed
2. Notification includes:
   - Order ID and customer details
   - Payment method and status
   - Delivery address
   - Order items and total amount

### 3. Order Status Updates
- **COD Orders**: Start as "Pending" until agent confirms
- **Online Paid Orders**: Start as "Confirmed" immediately
- Status updates are sent to admin topic for tracking

## Notification Data Structure

### New Order Notification
```json
{
  "title": "New COD Order Received!" / "New Paid Order Received!",
  "body": "Order #12345678 from John Doe - ₹299.00",
  "data": {
    "type": "new_order",
    "orderId": "order_id_here",
    "customerId": "user_id_here",
    "customerName": "John Doe",
    "customerEmail": "john@example.com",
    "customerPhone": "+91xxxxxxxxxx",
    "totalAmount": "299.00",
    "paymentMethod": "UPI/Card/Cash on Delivery",
    "paymentStatus": "completed/pending",
    "orderDate": "2024-01-15T10:30:00.000Z",
    "deliveryAddress": "123 Main St, City",
    "itemCount": "3",
    "orderStatus": "confirmed/pending",
    "items": [...] // Array of order items
  }
}
```

## Testing

### 1. Test Payment Flow
1. Add items to cart
2. Proceed through the payment flow
3. Test all three payment methods
4. Verify notifications are received

### 2. Test Topic Notifications
Use the test function:
```bash
curl -X POST https://your-region-your-project.cloudfunctions.net/sendTestTopicNotification \
  -H "Content-Type: application/json" \
  -d '{
    "topic": "agent_dailymilk",
    "title": "Test Notification",
    "body": "This is a test notification",
    "data": {"test": "true"}
  }'
```

## Security Notes

1. **Razorpay Keys**: Use test keys for development, production keys for live app
2. **Firebase Rules**: Ensure proper security rules for Firestore collections
3. **Topic Security**: Topics are public, ensure sensitive data is not exposed
4. **Payment Validation**: Implement server-side payment verification for production

## Troubleshooting

### Common Issues
1. **Razorpay not opening**: Check if the key is correct and network is available
2. **Notifications not received**: Verify topic subscription and FCM setup
3. **Order not saving**: Check Firestore permissions and network connectivity
4. **Cloud Functions not triggering**: Verify function deployment and Firestore triggers

### Debug Steps
1. Check Flutter console for payment errors
2. Monitor Firebase Functions logs
3. Verify Firestore document creation
4. Test topic subscriptions manually

## Future Enhancements

1. **Payment Gateway Options**: Add more payment providers
2. **Recurring Orders**: Implement subscription-based orders
3. **Refund Management**: Add refund processing
4. **Analytics**: Track payment success rates and popular methods
5. **Wallet Integration**: Add digital wallet support

## Support

For issues or questions:
1. Check Flutter and Razorpay documentation
2. Review Firebase Functions logs
3. Test with smaller data sets first
4. Verify all API keys and configurations












