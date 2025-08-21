# DailyMilk Agent - Enhanced Admin Dashboard

A comprehensive Flutter application that serves as an admin dashboard for the DailyMilk delivery system. This application connects with the DailyMilk admin API and Teja application to manage orders, track analytics, and provide real-time updates.

## 🚀 Features

### 🔐 Authentication & Security
- **Secure Login System**: Email/password authentication with token-based sessions
- **Auto-login**: Persistent authentication using SharedPreferences
- **Session Management**: Automatic token refresh and logout functionality

### 📊 Real-time Dashboard
- **Live Analytics**: Real-time order statistics and revenue tracking
- **Order Management**: View, accept, reject, and track orders in real-time
- **Status Tracking**: Monitor orders through NEW → RUNNING → COMPLETED workflow
- **WebSocket Integration**: Real-time updates for new orders and status changes

### 🔗 Teja App Integration
- **Cross-Platform Sync**: Seamless integration with Teja application orders
- **Order Synchronization**: Sync orders from Teja app to admin dashboard
- **Unified Management**: Manage orders from both platforms in one interface

### 📱 Beautiful UI/UX
- **Modern Design**: Material Design 3 with custom theming
- **Responsive Layout**: Optimized for mobile and tablet screens
- **Loading States**: Shimmer effects and skeleton screens
- **Error Handling**: User-friendly error messages and retry mechanisms

### 📈 Analytics & Reporting
- **Dashboard Analytics**: Total orders, revenue, customer metrics
- **Order Analytics**: Detailed order statistics and trends
- **Real-time Charts**: Visual representation of data using fl_chart
- **Performance Metrics**: Track key performance indicators

### 🔔 Notifications
- **Real-time Alerts**: Instant notifications for new orders
- **Status Updates**: Live updates for order status changes
- **Connection Status**: Monitor WebSocket connection health

## 🏗️ Architecture

### Core Components
- **API Service**: HTTP client for backend communication
- **Realtime Service**: WebSocket client for live updates
- **State Management**: Provider pattern for app-wide state
- **Models**: Type-safe data models for orders and analytics

### File Structure
```
lib/
├── core/
│   ├── constants.dart          # App constants and configuration
│   ├── models/
│   │   └── order.dart         # Order and OrderItem models
│   ├── providers/
│   │   └── app_provider.dart  # Main app state management
│   └── services/
│       ├── api_service.dart    # HTTP API client
│       └── realtime_service.dart # WebSocket client
├── features/
│   ├── auth/
│   │   ├── login_page.dart    # Enhanced login page
│   │   └── signup_page.dart   # User registration
│   ├── dashboard/
│   │   ├── enhanced_dashboard_page.dart # Main dashboard
│   │   ├── teja_integration_page.dart  # Teja app integration
│   │   └── order_details_page.dart     # Order details view
│   └── profile/
│       └── profile_page.dart   # User profile management
├── widgets/
│   ├── analytics_card.dart     # Analytics display widgets
│   └── order_card.dart        # Order display widgets
└── main.dart                  # App entry point
```

## 🛠️ Setup & Installation

### Prerequisites
- Flutter SDK (3.8.1 or higher)
- Dart SDK
- Android Studio / VS Code
- Git

### Installation Steps

1. **Clone the repository**
   ```bash
   git clone <repository-url>
   cd dailymilk_agent
   ```

2. **Install dependencies**
   ```bash
   flutter pub get
   ```

3. **Configure API endpoints**
   - Update `lib/core/constants.dart` with your API URLs
   - Set up WebSocket endpoint for real-time features

4. **Run the application**
   ```bash
   flutter run
   ```

## 🔧 Configuration

### API Configuration
Update the following constants in `lib/core/constants.dart`:

```dart
static const String baseUrl = 'https://your-admin-api.com/api/v1';
static const String tejaAppBaseUrl = 'https://your-teja-api.com/api/v1';
static const String websocketUrl = 'wss://your-admin-api.com/ws';
```

### Environment Setup
- **Development**: Use local API endpoints
- **Production**: Use production API endpoints
- **Testing**: Use mock data for testing

## 📱 Usage

### Login
1. Enter your email and password
2. Use "Demo Login" for testing with sample credentials
3. Navigate to the enhanced dashboard

### Dashboard Features
- **Analytics Overview**: View real-time statistics
- **Order Management**: Accept/reject new orders
- **Status Tracking**: Monitor order progress
- **Real-time Updates**: Live order notifications

### Teja Integration
- **View Teja Orders**: Access orders from Teja application
- **Sync Orders**: Transfer orders to admin dashboard
- **Unified Management**: Manage all orders in one place

## 🔌 API Integration

### Admin API Endpoints
- `POST /auth/login` - User authentication
- `GET /orders` - Fetch orders with filters
- `PUT /orders/{id}/status` - Update order status
- `GET /analytics` - Fetch analytics data
- `GET /notifications` - Fetch notifications

### Teja App API Endpoints
- `GET /orders` - Fetch Teja app orders
- `GET /orders/{id}` - Get specific order details
- `POST /orders/sync` - Sync order to admin dashboard

### WebSocket Events
- `new_order` - New order notification
- `order_update` - Order status update
- `new_notification` - New notification
- `ping/pong` - Connection health check

## 🎨 UI Components

### Order Card
- **Status Indicators**: Color-coded order status
- **Customer Information**: Name, phone, address
- **Order Details**: Items, quantities, prices
- **Action Buttons**: Accept/reject for new orders

### Analytics Cards
- **Metric Display**: Key performance indicators
- **Visual Charts**: Mini charts for trends
- **Color Coding**: Different colors for different metrics

### Loading States
- **Shimmer Effects**: Smooth loading animations
- **Skeleton Screens**: Placeholder content
- **Progress Indicators**: Loading spinners

## 🔒 Security Features

### Authentication
- **Token-based Auth**: JWT tokens for API access
- **Secure Storage**: Encrypted local storage
- **Auto-refresh**: Automatic token renewal

### Data Protection
- **Input Validation**: Form validation and sanitization
- **Error Handling**: Secure error messages
- **Network Security**: HTTPS/WSS connections

## 📊 Analytics & Reporting

### Dashboard Metrics
- **Total Orders**: Monthly order count
- **Revenue**: Total revenue tracking
- **New Orders**: Pending approval count
- **Running Orders**: In-progress orders
- **Completed Orders**: Delivered orders
- **Customer Count**: Active customer base

### Real-time Updates
- **Live Statistics**: Real-time metric updates
- **Order Notifications**: Instant new order alerts
- **Status Changes**: Live order status updates

## 🚀 Performance Optimizations

### State Management
- **Provider Pattern**: Efficient state updates
- **Selective Rebuilds**: Only update necessary widgets
- **Memory Management**: Proper disposal of resources

### Network Optimization
- **Caching**: Image and data caching
- **Connection Pooling**: Efficient HTTP connections
- **WebSocket Management**: Automatic reconnection

### UI Performance
- **Lazy Loading**: Load data on demand
- **Image Optimization**: Cached network images
- **Smooth Animations**: 60fps animations

## 🧪 Testing

### Unit Tests
```bash
flutter test
```

### Widget Tests
```bash
flutter test test/widget_test.dart
```

### Integration Tests
```bash
flutter drive --target=test_driver/app.dart
```

## 📦 Dependencies

### Core Dependencies
- `flutter`: UI framework
- `provider`: State management
- `dio`: HTTP client
- `web_socket_channel`: Real-time communication
- `shared_preferences`: Local storage

### UI Dependencies
- `cached_network_image`: Image caching
- `shimmer`: Loading effects
- `fl_chart`: Charts and graphs
- `lottie`: Animations

### Utility Dependencies
- `intl`: Internationalization
- `cupertino_icons`: iOS-style icons

## 🤝 Contributing

1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Add tests if applicable
5. Submit a pull request

## 📄 License

This project is licensed under the MIT License - see the LICENSE file for details.

## 🆘 Support

For support and questions:
- Create an issue in the repository
- Contact the development team
- Check the documentation

## 🔄 Version History

### v1.0.0
- Initial release with basic order management
- Real-time dashboard with analytics
- Teja app integration
- Enhanced UI/UX design
- WebSocket real-time updates

---

**Built with ❤️ for DailyMilk**
