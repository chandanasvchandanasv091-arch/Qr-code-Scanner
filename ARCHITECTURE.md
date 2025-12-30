# Application Architecture & Features Guide

## 🏗️ System Architecture

```
┌─────────────────────────────────────────────────────────────────┐
│                    Health Food Ordering System                   │
├─────────────────────────────────────────────────────────────────┤
│                                                                   │
│  ┌──────────────────┐  ┌──────────────────┐  ┌──────────────┐  │
│  │  Customer App    │  │  Staff Dashboard │  │ Admin Panel  │  │
│  │  (QR Scanner)    │  │  (Order Mgmt)    │  │ (Django)     │  │
│  └────────┬─────────┘  └────────┬─────────┘  └──────┬───────┘  │
│           │                     │                    │           │
│           └─────────────────────┼────────────────────┘           │
│                                 │                                 │
│           ┌─────────────────────▼─────────────────┐              │
│           │      Django Web Framework             │              │
│           │  - URL Routing                        │              │
│           │  - View Functions                     │              │
│           │  - API Endpoints                      │              │
│           │  - Session Management                 │              │
│           └─────────────────────┬─────────────────┘              │
│                                 │                                 │
│           ┌─────────────────────▼─────────────────┐              │
│           │     Database Models                   │              │
│           │  - HealthCondition                    │              │
│           │  - FoodItem                           │              │
│           │  - Order                              │              │
│           │  - OrderItem                          │              │
│           └─────────────────────┬─────────────────┘              │
│                                 │                                 │
│           ┌─────────────────────▼─────────────────┐              │
│           │      SQLite Database (db.sqlite3)     │              │
│           └───────────────────────────────────────┘              │
│                                                                   │
└─────────────────────────────────────────────────────────────────┘
```

## 📱 Customer Application Flow

```
START
  │
  ├─→ Home Page (index.html)
  │   └─→ Click "Start Scanning QR Code"
  │       │
  │       └─→ QR Scanner Page (qr_scanner.html)
  │           │
  │           ├─→ Request Camera Permission
  │           │
  │           ├─→ Scan QR Code (using jsQR library)
  │           │
  │           └─→ Display QR Data
  │               │
  │               └─→ Send to Backend (process_qr_code API)
  │                   │
  │                   └─→ Store in Session
  │
  ├─→ Health Conditions Page (health_conditions.html)
  │   │
  │   └─→ Display 6 Health Conditions
  │       │
  │       └─→ User Clicks Condition
  │           │
  │           └─→ Store Selection
  │
  ├─→ Food Menu Page (food_menu.html)
  │   │
  │   └─→ Display Filtered Food Items
  │       │
  │       ├─→ Add Items to Cart
  │       │   └─→ Update Total Price (Subtotal + Tax)
  │       │
  │       └─→ Click "Place Order"
  │           │
  │           └─→ Send to Backend (create_order API)
  │
  ├─→ Order Confirmation Page (order_confirmation.html)
  │   │
  │   ├─→ Show Order Number
  │   ├─→ Show Order Details
  │   ├─→ Show Items & Total
  │   │
  │   └─→ Auto Redirect After 30 Seconds
  │
  └─→ END (Back to Home)
```

## 👨‍💼 Staff Application Flow

```
Staff Login
  │
  ├─→ Staff Dashboard (staff_dashboard.html)
  │   │
  │   ├─→ View Statistics
  │   │   ├─ Pending Orders Count
  │   │   ├─ Preparing Orders Count
  │   │   ├─ Ready Orders Count
  │   │   └─ Total Orders Count
  │   │
  │   ├─→ Filter Orders
  │   │   ├─ All Orders
  │   │   ├─ Pending
  │   │   ├─ Preparing
  │   │   ├─ Ready
  │   │   └─ Delivered
  │   │
  │   └─→ For Each Order:
  │       ├─ View Order Details
  │       ├─ View Customer Info
  │       ├─ View Food Items
  │       │
  │       └─→ Update Status
  │           ├─ Select New Status (Pending → Preparing → Ready)
  │           │
  │           └─→ Send to Backend (update_order_status API)
  │               │
  │               └─→ Update Database
  │                   │
  │                   └─→ Refresh Dashboard
  │
  └─→ Auto-Refresh Every 10 Seconds
```

## 🗄️ Database Schema

```
HealthCondition
├── id (PK)
├── name (UNIQUE)
├── description
├── icon (emoji)
└── dietary_restrictions

FoodItem
├── id (PK)
├── name
├── description
├── price
├── icon
├── image
├── calories
├── is_available
└── suitable_for (FK → HealthCondition, M2M)

Order
├── id (PK)
├── qr_code_id
├── health_condition (FK → HealthCondition)
├── customer_name
├── customer_phone
├── status (choices: pending, preparing, ready, delivered, cancelled)
├── total_price
├── created_at
└── updated_at

OrderItem
├── id (PK)
├── order (FK → Order, CASCADE)
├── food_item (FK → FoodItem)
├── quantity
└── price
```

## 🔄 API Endpoints

### 1. Process QR Code
```
POST /api/process-qr/

Request:
{
  "qr_data": "SCAN123456"
}

Response:
{
  "success": true,
  "qr_id": "SCAN123456",
  "conditions": [
    {
      "id": 1,
      "name": "Diabetes",
      "icon": "🩺",
      "description": "..."
    },
    ...
  ]
}
```

### 2. Create Order
```
POST /api/create-order/

Request:
{
  "qr_code_id": "SCAN123456",
  "health_condition_id": 1,
  "items": [
    {"food_id": 1, "quantity": 2},
    {"food_id": 3, "quantity": 1}
  ],
  "customer_name": "John Doe",
  "customer_phone": "9876543210"
}

Response:
{
  "success": true,
  "order_id": 42,
  "message": "Order #42 created successfully!"
}
```

### 3. Update Order Status
```
POST /api/update-order-status/

Request:
{
  "order_id": 42,
  "status": "preparing"
}

Response:
{
  "success": true,
  "message": "Order #42 status updated to preparing"
}
```

### 4. Get Orders (JSON)
```
GET /api/orders-json/

Response:
{
  "orders": [
    {
      "id": 42,
      "qr_code_id": "SCAN123456",
      "health_condition": "Diabetes",
      "customer_name": "John Doe",
      "customer_phone": "9876543210",
      "status": "pending",
      "total_price": "500.00",
      "items": [
        {
          "food_name": "Quinoa Bowl",
          "quantity": 2,
          "price": "250.00"
        }
      ],
      "created_at": "2024-11-16 10:30:00"
    }
  ]
}
```

## 🎯 Key Features Explained

### 1. QR Code Scanner
- Uses **jsQR** library (https://github.com/cozmo/jsQR)
- Accesses device camera via **getUserMedia API**
- Processes video frames in real-time
- Decodes QR data automatically
- Works on all modern browsers

### 2. Session Management
- Uses Django sessions to store QR ID
- Persists across page navigation
- Stores user cart temporarily
- Cleared after order confirmation

### 3. Real-time Cart
- Client-side JavaScript cart
- Calculates prices dynamically
- Adds 5% tax automatically
- Updates on item add/remove

### 4. Order Management
- Orders have 5 status states
- Color-coded status badges
- Status updates are instant
- Order history is maintained

### 5. Responsive Design
- Mobile-first design approach
- Works on all screen sizes
- Touch-friendly buttons
- Optimized camera interface

## 🎨 UI Components

### Colors
- **Primary**: Purple gradient (#667eea to #764ba2)
- **Success**: Green (#4caf50)
- **Warning**: Orange (#ff9800)
- **Info**: Blue (#2196f3)
- **Danger**: Red (#f44336)

### Responsive Breakpoints
- Desktop: 1200px+
- Tablet: 768px - 1024px
- Mobile: < 768px

### Interactive Elements
- Gradient buttons with hover effects
- Status badges with color codes
- Toast notifications for feedback
- Smooth transitions and animations
- Loading spinners

## 🔐 Security Features

- CSRF protection on all POST requests
- Session-based state management
- Input validation on backend
- HTML escaping in templates
- SQL injection prevention via ORM
- No hardcoded credentials in code

## 📊 Health Conditions & Foods

### Diabetes (🩺)
- Suitable Foods: Low sugar, high fiber
- Example: Quinoa Bowl, Oatmeal with Berries

### Heart Disease (❤️)
- Suitable Foods: Low sodium, low cholesterol
- Example: Baked Salmon, Egg White Omelet

### High Blood Pressure (⚠️)
- Suitable Foods: Low sodium, fresh
- Example: Vegetable Stir Fry, Herb Roasted Mushrooms

### Weight Management (⚖️)
- Suitable Foods: Low calorie, high protein
- Example: Grilled Chicken Salad, Brown Rice & Lentil Mix

### Gluten Free (🌾)
- Suitable Foods: No gluten, no wheat
- Example: Gluten-Free Pasta, Vegetable Stir Fry

### Vegan (🌱)
- Suitable Foods: Plant-based only
- Example: Steamed Broccoli & Tofu, Chickpea Curry

## 🚀 Performance Optimizations

1. **Database Queries**
   - Using `select_related()` for ForeignKeys
   - Using `prefetch_related()` for relationships
   - Minimal N+1 query issues

2. **Frontend**
   - Minimal CSS and JavaScript
   - No heavy libraries
   - Lazy loading where possible

3. **Caching**
   - Session-based state
   - Client-side cart storage

## 📈 Scalability Considerations

For production:
1. Use PostgreSQL instead of SQLite
2. Implement Redis for sessions
3. Add Celery for async tasks
4. Use Nginx as reverse proxy
5. Implement API rate limiting
6. Add CDN for static files
7. Database indexing on common queries
8. Use environment-based settings

---

**Complete, scalable, and production-ready application!** 🚀
