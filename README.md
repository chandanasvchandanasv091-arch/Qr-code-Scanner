# Health Food QR Code Scanner & Ordering System

A comprehensive Django-based web application for scanning QR codes, selecting health conditions, ordering health-conscious food items, and managing orders through a staff dashboard.

## 🌟 Features

### Customer Features
- **QR Code Scanner**: Scan QR codes using mobile device camera (supports all modern browsers)
- **Health Condition Selection**: Choose from 6 different health conditions:
  - Diabetes
  - Heart Disease
  - High Blood Pressure
  - Weight Management
  - Gluten Free
  - Vegan

- **Personalized Food Menu**: View food items suitable for selected health condition
- **Food Ordering**: 
  - Add items to cart with quantity control
  - Real-time price calculation with tax
  - Customer information entry (name, phone)
  - Order confirmation with receipt

### Staff Features
- **Orders Dashboard**: Real-time order management interface
- **Order Statistics**: 
  - Pending orders count
  - Preparing orders count
  - Ready for pickup orders count
  - Total orders

- **Order Status Management**:
  - Update order status (Pending → Preparing → Ready → Delivered)
  - Filter orders by status
  - View order details and items

- **Auto-refresh**: Dashboard auto-refreshes every 10 seconds for real-time updates

## 📋 Prerequisites

- Python 3.8+
- pip (Python package installer)
- Modern web browser with camera access (for QR scanning)
- SQLite3 (included with Python)

## 🚀 Installation & Setup

### Step 1: Clone/Navigate to Project
```bash
cd "c:\Users\anush\OneDrive\Desktop\mini.proj"
```

### Step 2: Install Dependencies
```bash
pip install django pillow requests
```

### Step 3: Run Migrations
```bash
python manage.py makemigrations
python manage.py migrate
```

### Step 4: Populate Database with Sample Data
```bash
python manage.py populate_data
```

This command creates:
- 6 health conditions
- 15 health-conscious food items
- Sample data for testing

### Step 5: Create Superuser (Already Done)
Admin credentials are:
- **Username**: admin
- **Password**: admin123

### Step 6: Start the Development Server
```bash
python manage.py runserver
```

The application will be available at: `http://127.0.0.1:8000/`

## 📱 User Workflows

### Customer Workflow

1. **Access Application**
   - Visit `http://localhost:8000`
   - Click "Start Scanning QR Code"

2. **Scan QR Code**
   - Allow camera access
   - Point camera at QR code
   - System automatically detects and processes QR code

3. **Select Health Condition**
   - View 6 health condition options
   - Each shows icon, description, and dietary guidelines
   - Click to select your condition

4. **Browse Food Menu**
   - View 15+ food items suitable for your condition
   - Each food item shows:
     - Icon and image
     - Description
     - Calories
     - Price
     - Quantity selector

5. **Place Order**
   - Add items to cart with desired quantities
   - Review cart with real-time pricing
   - Enter customer information (optional)
   - Click "Place Order"

6. **Order Confirmation**
   - View order number and receipt
   - See order details and items
   - Total amount with tax calculation

### Staff Workflow

1. **Access Staff Dashboard**
   - Visit `http://localhost:8000/staff/`
   - View all orders and statistics

2. **Monitor Orders**
   - See order count by status
   - View all order details including:
     - Order ID
     - Customer info
     - QR Code ID
     - Health condition
     - Food items ordered
     - Total amount

3. **Update Order Status**
   - Select new status from dropdown
   - Click "Update Status"
   - Status changes appear in real-time

4. **Filter Orders**
   - Click filter buttons to view by status:
     - All Orders
     - Pending
     - Preparing
     - Ready
     - Delivered

5. **Auto-Refresh**
   - Dashboard refreshes automatically every 10 seconds
   - Click "Refresh" button for manual refresh

## 🔑 Key URLs

| URL | Purpose | Access |
|-----|---------|--------|
| `/` | Home page | Public |
| `/scanner/` | QR code scanner | Public |
| `/health-conditions/` | Select health condition | Public |
| `/food-menu/<id>/` | Food menu for condition | Public |
| `/order-confirmation/<id>/` | Order confirmation | Public |
| `/staff/` | Staff dashboard | Public (use in secure env) |
| `/admin/` | Django admin panel | Admin only |

## 🗄️ Database Models

### HealthCondition
```
- name (CharField)
- description (TextField)
- icon (CharField - emoji)
- dietary_restrictions (TextField)
```

### FoodItem
```
- name (CharField)
- description (TextField)
- price (DecimalField)
- icon (CharField - emoji)
- image (ImageField - optional)
- calories (IntegerField)
- suitable_for (ManyToMany - HealthConditions)
- is_available (BooleanField)
```

### Order
```
- qr_code_id (CharField)
- health_condition (ForeignKey - HealthCondition)
- customer_name (CharField)
- customer_phone (CharField)
- status (CharField - choices)
- total_price (DecimalField)
- created_at (DateTimeField)
- updated_at (DateTimeField)
```

### OrderItem
```
- order (ForeignKey - Order)
- food_item (ForeignKey - FoodItem)
- quantity (PositiveIntegerField)
- price (DecimalField)
```

## 🔧 Admin Panel

Access Django admin at: `http://localhost:8000/admin/`

**Credentials:**
- Username: `admin`
- Password: `admin123`

In admin panel you can:
- Create/Edit/Delete health conditions
- Manage food items
- View all orders
- Modify order status
- View order items

## 📊 API Endpoints

### POST `/api/process-qr/`
Processes QR code data and returns health conditions

**Request:**
```json
{
  "qr_data": "SCAN123456"
}
```

### POST `/api/create-order/`
Creates a new order

**Request:**
```json
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
```

### POST `/api/update-order-status/`
Updates order status

**Request:**
```json
{
  "order_id": 1,
  "status": "preparing"
}
```

### GET `/api/orders-json/`
Get all orders in JSON format

**Response:**
```json
{
  "orders": [
    {
      "id": 1,
      "qr_code_id": "SCAN123456",
      "health_condition": "Diabetes",
      "customer_name": "John Doe",
      "status": "pending",
      "total_price": "500.00",
      "items": [...],
      "created_at": "2024-11-16 10:30:00"
    }
  ]
}
```

## 🎨 UI/UX Features

- **Responsive Design**: Works on desktop, tablet, and mobile devices
- **Real-time Camera Access**: HTML5 Geolocation API for QR scanning
- **Beautiful Gradients**: Purple gradient theme throughout
- **Toast Notifications**: User feedback for actions
- **Smooth Animations**: Hover effects and transitions
- **Emoji Icons**: Visual food and health category identifiers
- **Cart System**: Real-time price calculations
- **Status Badges**: Color-coded order status indicators

## 🛡️ Security Notes

For production deployment:
1. Set `DEBUG = False` in settings.py
2. Use a strong `SECRET_KEY`
3. Set `ALLOWED_HOSTS` to your domain
4. Use HTTPS
5. Implement authentication for staff dashboard
6. Use environment variables for sensitive data
7. Enable CSRF protection properly

## 📱 Browser Compatibility

- Chrome/Chromium (recommended for QR scanning)
- Firefox
- Safari (iOS 14.5+)
- Edge
- Any modern browser with:
  - getUserMedia API support
  - ES6 JavaScript support
  - CSS Grid support

## 🐛 Troubleshooting

### Camera Access Issues
- Ensure browser has permission to access camera
- Check if HTTPS is enabled (required by some browsers)
- Try a different browser

### QR Code Not Scanning
- Ensure QR code is in good lighting
- Try scanning from different angles
- Ensure QR code is not too pixelated

### Database Issues
- Run migrations: `python manage.py migrate`
- Check SQLite file is not corrupted
- Clear migrations and remake: `python manage.py migrate qrscanner zero`

### Port Already in Use
```bash
python manage.py runserver 8001
# or specify IP and port
python manage.py runserver 127.0.0.1:9000
```

## 📦 Project Structure

```
mini.proj/
├── manage.py                      # Django management script
├── db.sqlite3                     # Database
├── healthfood_project/            # Main project settings
│   ├── settings.py               # Django settings
│   ├── urls.py                   # URL configuration
│   └── wsgi.py                   # WSGI config
├── qrscanner/                    # Main app
│   ├── models.py                 # Database models
│   ├── views.py                  # View functions
│   ├── urls.py                   # App URLs
│   ├── admin.py                  # Admin configuration
│   ├── management/
│   │   └── commands/
│   │       └── populate_data.py  # Data population command
│   └── templates/
│       └── qrscanner/            # HTML templates
│           ├── index.html
│           ├── qr_scanner.html
│           ├── health_conditions.html
│           ├── food_menu.html
│           ├── order_confirmation.html
│           └── staff_dashboard.html
├── media/                        # User uploaded files
├── staticfiles/                  # Static files (CSS, JS)
└── README.md                     # This file
```

## 🚀 Advanced Features to Add

1. **Email Notifications**: Send order confirmations to customers
2. **SMS Integration**: Real-time order updates via SMS
3. **Payment Gateway**: Integrate Razorpay or Stripe
4. **Delivery Tracking**: GPS-based delivery tracking
5. **Customer Login**: User accounts and order history
6. **Ratings & Reviews**: Customer feedback system
7. **Analytics Dashboard**: Order trends and insights
8. **Inventory Management**: Stock tracking for food items
9. **Multi-language Support**: Support multiple languages
10. **Push Notifications**: Real-time updates to staff phones

## 📝 Sample QR Codes

For testing, you can create QR codes using online tools and encode data like:
- `PATIENT_001`
- `SCAN_12345`
- `HOSPITAL_QR_001`
- Any custom identifier

## 🤝 Support

For issues or questions:
1. Check the troubleshooting section
2. Review Django documentation: https://docs.djangoproject.com/
3. Check jsQR documentation: https://github.com/cozmo/jsQR

## 📄 License

This project is open source and available for educational and commercial use.

## ✨ Credits

Built with:
- Django Framework
- jsQR Library for QR scanning
- SQLite Database
- Bootstrap (CSS framework principles)

---

**Happy Ordering! 🍽️**

For any questions or improvements, feel free to modify and enhance the application!
