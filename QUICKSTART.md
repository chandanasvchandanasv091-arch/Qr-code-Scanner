# Quick Start Guide - Health Food QR Scanner

## ⚡ Quick Start (30 seconds)

### 1. Open PowerShell and navigate to project:
```powershell
cd "c:\Users\anush\OneDrive\Desktop\mini.proj"
```

### 2. Start the server:
```powershell
python manage.py runserver
```

### 3. Open in browser:
- **Customer Interface**: http://localhost:8000
- **Staff Dashboard**: http://localhost:8000/staff/
- **Admin Panel**: http://localhost:8000/admin/

---

## 🔑 Login Credentials

**Admin Panel:**
- Username: `admin`
- Password: `admin123`

---

## 🎯 Test Workflow

### Customer Side:
1. Go to http://localhost:8000
2. Click "Start Scanning QR Code"
3. Grant camera permission
4. Scan a QR code (or generate one using: https://www.qr-code-generator.com/)
5. Select a health condition (e.g., Diabetes)
6. Browse and add food items to cart
7. Place order with customer details

### Staff Side:
1. Go to http://localhost:8000/staff/
2. View all pending orders
3. Update order status using dropdown
4. See real-time order updates

---

## 📱 Test QR Code

Generate a QR code encoding one of these values:
- `PATIENT_001`
- `HOSPITAL_QR`
- `TEST_SCAN_123`

Use: https://www.qr-code-generator.com/

---

## 🛠️ Common Commands

### Create superuser:
```powershell
python manage.py createsuperuser
```

### Reset database:
```powershell
python manage.py migrate qrscanner zero
python manage.py makemigrations
python manage.py migrate
```

### Repopulate data:
```powershell
python manage.py populate_data
```

### Run server on different port:
```powershell
python manage.py runserver 8001
```

---

## 📚 Data Included

### Health Conditions (6):
- 🩺 Diabetes
- ❤️ Heart Disease
- ⚠️ High Blood Pressure
- ⚖️ Weight Management
- 🌾 Gluten Free
- 🌱 Vegan

### Food Items (15+):
- Quinoa Bowl with Veggies
- Grilled Chicken Salad
- Brown Rice & Lentil Mix
- Baked Salmon with Herbs
- Steamed Broccoli & Tofu
- Gluten-Free Pasta
- Oatmeal with Berries
- And more...

---

## 🔗 Key Features

✅ QR Code Scanner (Camera-based)
✅ 6 Health Conditions
✅ 15+ Personalized Food Items
✅ Real-time Shopping Cart
✅ Order Management System
✅ Staff Dashboard
✅ Order Status Tracking
✅ Responsive Design
✅ Admin Panel

---

## 📞 Quick Help

**Port 8000 already in use?**
```powershell
python manage.py runserver 0.0.0.0:9000
```

**Need to stop server?**
Press `Ctrl + C` in terminal

**Database not working?**
```powershell
python manage.py migrate
python manage.py populate_data
```

---

## 🌐 Access Points

| URL | Purpose |
|-----|---------|
| http://localhost:8000 | Home Page |
| http://localhost:8000/scanner/ | QR Scanner |
| http://localhost:8000/staff/ | Staff Dashboard |
| http://localhost:8000/admin/ | Admin Panel |

---

**Everything is ready! Start the server and begin using the application! 🚀**
