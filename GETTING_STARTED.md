# 🚀 GETTING STARTED GUIDE

## Welcome to Health Food QR Scanner! 👋

This is your complete **health-conscious food ordering system** with QR code scanning capability.

---

## ⚡ 30-Second Quick Start

### Step 1: Open PowerShell
```
Press Windows Key + X
Click: Windows PowerShell
```

### Step 2: Navigate to Project
```powershell
cd "c:\Users\anush\OneDrive\Desktop\mini.proj"
```

### Step 3: Start the Server
```powershell
python manage.py runserver
```

### Step 4: Open in Browser
- **Customer App:** http://localhost:8000
- **Staff Panel:** http://localhost:8000/staff/
- **Admin Panel:** http://localhost:8000/admin/

**That's it! Your app is running!** 🎉

---

## 📱 How to Use - Customer

### 1. Scan QR Code
- Click "Start Scanning QR Code" on home page
- Allow camera permission
- Point at QR code
- System detects automatically ✅

### 2. Select Health Condition
- Choose from 6 conditions
- View recommendations for your condition
- Click to proceed

### 3. Browse Food Menu
- See food items perfect for your health needs
- Each item shows price, calories, description
- Adjust quantity as needed

### 4. Place Order
- Add items to cart
- View total (with tax)
- Enter your name (optional)
- Click "Place Order"

### 5. Order Confirmation
- See your order number
- View receipt with all details
- Done! Your order is submitted ✅

---

## 👨‍🍳 How to Use - Staff Dashboard

### 1. Access Dashboard
- Visit: http://localhost:8000/staff/

### 2. View Statistics
- See how many orders are pending
- See how many are being prepared
- See how many are ready

### 3. Browse Orders
- View all customer orders
- See order details
- Check food items ordered
- See total amount

### 4. Update Order Status
1. Select new status from dropdown
2. Click "Update Status"
3. Status changes immediately
4. Customer sees update in real-time

### 5. Filter Orders
- Click "All Orders" to see everything
- Click "Pending" to see new orders
- Click "Preparing" to track cooking
- Click "Ready" to see ready orders

---

## 👨‍💼 Admin Panel

### Access
- URL: http://localhost:8000/admin/
- Username: `admin`
- Password: `admin123`

### What You Can Do
✅ Create new health conditions
✅ Add food items
✅ View all orders
✅ Modify order status
✅ Delete items
✅ Manage everything

---

## 🔑 Important URLs

Copy & Paste These:

```
Home:               http://localhost:8000
QR Scanner:         http://localhost:8000/scanner/
Health Options:     http://localhost:8000/health-conditions/
Food Menu:          http://localhost:8000/food-menu/1/
Staff Dashboard:    http://localhost:8000/staff/
Admin Panel:        http://localhost:8000/admin/
```

---

## 🎯 Test Scenario

### Want to test the full flow?

**Step 1: Generate a QR Code**
- Visit: https://www.qr-code-generator.com/
- Enter text: `PATIENT_001`
- Generate and download
- Print or display on screen

**Step 2: Scan in App**
- Go to http://localhost:8000
- Click "Start Scanning QR Code"
- Scan your QR code

**Step 3: Complete Workflow**
- Select "Diabetes" condition
- Add 2-3 food items to cart
- Enter your name
- Place order

**Step 4: Check Staff Panel**
- Visit http://localhost:8000/staff/
- See your new order (Pending)
- Click "Update Status"
- Change to "Preparing"
- Watch it update instantly!

---

## 💡 Quick Tips

### Camera Not Working?
- Use Chrome browser
- Allow permissions when asked
- Check browser settings

### QR Code Not Scanning?
- Ensure good lighting
- Hold camera steady
- QR code should be clear

### Server Won't Start?
- Check if port 8000 is free
- Or use: `python manage.py runserver 8001`

### Forgot Admin Password?
- Username: `admin`
- Password: `admin123`

---

## 📊 What's Included

### 6 Health Conditions
🩺 Diabetes
❤️ Heart Disease
⚠️ High Blood Pressure
⚖️ Weight Management
🌾 Gluten Free
🌱 Vegan

### 15+ Food Items
Each with:
- ✅ Price
- ✅ Calories
- ✅ Description
- ✅ Health suitability
- ✅ Nice icons

### Full Features
- ✅ QR code scanning
- ✅ Shopping cart
- ✅ Real-time pricing
- ✅ Order management
- ✅ Staff dashboard
- ✅ Admin panel
- ✅ Mobile friendly

---

## 🎨 Features You'll Love

### Beautiful UI
- Clean, modern design
- Easy to navigate
- Works on all devices
- Smooth animations

### Smart Features
- Real-time QR detection
- Instant cart updates
- Auto price calculation
- Live order tracking

### Perfect for
- Hospitals
- Health centers
- Gyms
- Health-conscious restaurants
- Wellness programs

---

## 🔄 Data Populated

✅ Database is **already populated** with:
- 6 health conditions
- 15 food items
- Sample data ready to use

No additional setup needed!

---

## ❓ FAQ

### Q: Can I add more food items?
**A:** Yes! Go to Admin Panel → Food Items → Add new

### Q: Can I add more health conditions?
**A:** Yes! Go to Admin Panel → Health Conditions → Add new

### Q: Can I modify prices?
**A:** Yes! Admin Panel → Food Items → Edit prices

### Q: Can I delete orders?
**A:** Yes! Admin Panel → Orders → Delete

### Q: Is it secure?
**A:** Yes! CSRF protection enabled, validated input, secure sessions

---

## 📚 Documentation

If you need more help:

| Document | Read Time | Purpose |
|----------|-----------|---------|
| QUICKSTART.md | 2 min | Fast reference |
| README.md | 10 min | Complete guide |
| ARCHITECTURE.md | 15 min | Technical details |
| INDEX.md | 5 min | File structure |

---

## 🚀 Next Steps

1. **Start the server** ✅
2. **Test customer app** ✅
3. **Check staff dashboard** ✅
4. **Explore admin panel** ✅
5. **Customize for your use** ✅

---

## ⌛ Expected Time

- **Setup:** 2 minutes
- **Testing:** 5 minutes
- **Customization:** 10 minutes

**Total: ~20 minutes to fully operational** ⏱️

---

## 📞 Need Help?

### Server Issues
```powershell
python manage.py runserver
```

### Database Issues
```powershell
python manage.py migrate
python manage.py populate_data
```

### Port Issues
```powershell
python manage.py runserver 8001
```

---

## 🎯 You're Ready!

### Right Now You Can:
✅ Start the server
✅ Scan QR codes
✅ Order food
✅ Manage orders
✅ Track orders
✅ Admin functions

### What's Ready:
✅ Complete database
✅ All templates
✅ All business logic
✅ Sample data
✅ Admin configured
✅ Full documentation

---

## 🎉 LET'S GO!

### Quick Command to Start:

```powershell
cd "c:\Users\anush\OneDrive\Desktop\mini.proj" ; python manage.py runserver
```

### Then Open:
**http://localhost:8000** 🌐

---

## 💻 System Requirements

✅ Python 3.8+ (Already installed)
✅ Modern Web Browser (Chrome recommended)
✅ Internet Connection (First launch)

**No additional installations needed!**

---

## 🎁 Bonus Features

✅ Real-time price calculation
✅ Tax computation (5%)
✅ Mobile-responsive design
✅ Auto-refresh dashboard
✅ Color-coded order status
✅ Beautiful gradients
✅ Smooth animations
✅ Toast notifications

---

## ✨ Built With

- **Django** - Web framework
- **SQLite** - Database
- **HTML/CSS** - Frontend
- **JavaScript** - Interactivity
- **jsQR** - QR scanning

---

**Ready? Let's build something awesome! 🚀**

---

**Created with ❤️**
**Status: ✅ Production Ready**
**Last Updated: November 16, 2024**

