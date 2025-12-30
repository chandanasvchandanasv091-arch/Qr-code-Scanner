# QR Code System - Quick Reference Card

## 🎯 Main Entry Points

```
HOME PAGE
http://127.0.0.1:8000/
    ↓
    Choose One:
    
    🎥 Camera Scanner (NEW - RECOMMENDED)
    http://127.0.0.1:8000/camera/
    
    📸 Upload QR Image
    http://127.0.0.1:8000/upload-qr/
    
    🧪 Generate Test QR
    http://127.0.0.1:8000/test-qr/
```

---

## 📱 Camera Scanner Workflow

```
START CAMERA
     ↓
SCAN QR CODE
     ↓
QR DETECTED
     ↓
DISPLAY QR DATA + THUMBNAIL
     ↓
CLICK "PROCESS QR"
     ↓
API VALIDATES & EXTRACTS CONDITION ID
     ↓
AUTO-REDIRECT TO /scan/{condition_id}/
     ↓
DISPLAY HEALTH CONDITION & FOOD MENU
     ↓
PLACE ORDER
     ↓
ORDER CONFIRMATION
```

---

## 🔗 Important URLs

### Scanning
- `/camera/` - **NEW** Enhanced camera scanner
- `/scanner/` - Basic scanner
- `/upload-qr/` - Upload image
- `/test-qr/` - Generate test QR

### Processing
- `/api/process-camera-qr/` - **NEW** Process QR data
- `/api/process-qr/` - Original processor

### Main Workflow
- `/scan/<id>/` - **KEY URL** - Health condition + food menu
- `/food-menu/<id>/` - Food items for condition
- `/health-conditions/` - All conditions

### Ordering
- `/api/create-order/` - Create order
- `/order-confirmation/<id>/` - Confirmation
- `/staff/` - Staff dashboard

---

## 📊 QR Code Formats

### What goes IN the QR code:

**Format 1 (URL):**
```
http://127.0.0.1:8000/scan/1/
```
→ System extracts condition_id = 1

**Format 2 (HEALTH):**
```
HEALTH:1:Diabetes
```
→ System extracts condition_id = 1

---

## 🔌 API Endpoints

### POST /api/process-camera-qr/

**Request:**
```json
{
  "qr_data": "http://127.0.0.1:8000/scan/1/"
}
```

**Success Response:**
```json
{
  "success": true,
  "condition_id": 1,
  "redirect": "/scan/1/",
  "message": "QR code scanned successfully"
}
```

**Error Response:**
```json
{
  "error": "Could not extract health condition from QR code",
  "status": "invalid_qr"
}
```

---

## 📝 Create Test QR Code

```
Visit: http://127.0.0.1:8000/test-qr/

Shows:
✓ QR code for each health condition
✓ Contains: http://127.0.0.1:8000/scan/{condition_id}/
✓ Scannable with /camera/
```

---

## 🧪 Testing Checklist

```
☐ Generate test QR from /test-qr/
☐ Visit /camera/
☐ Click "Start Camera"
☐ Show QR to camera
☐ Verify QR detected
☐ Click "Process QR"
☐ Verify redirect to /scan/1/ (or other ID)
☐ Verify food menu displays
☐ Try placing order
☐ Verify order confirmation
```

---

## 🛠️ Backend Processing

### Extract Condition ID from QR:

```python
# Supported formats:
# - http://localhost:8000/scan/1/
# - http://127.0.0.1:8000/scan/1/
# - HEALTH:1:Diabetes
# - /scan/1/

from qrscanner.utils import extract_condition_id_from_qr_data

condition_id = extract_condition_id_from_qr_data(qr_data)
# Returns: 1 (or other ID)
```

---

## 🎨 Frontend - What User Sees

### Camera Scanner Page

```
┌─────────────────────────────────────────┐
│  🔍 QR Code Scanner                    │
│  Scan a health condition QR code       │
├─────────────────────────────────────────┤
│                                        │
│        📷 CAMERA FEED                 │
│        [QR FRAME OVERLAY]             │
│                                        │
├─────────────────────────────────────────┤
│  [Start Camera] [Stop] [Process] [Reset]│
│  [← Back]                             │
└─────────────────────────────────────────┘
```

### After QR Detected

```
┌─────────────────────────────────────────┐
│  ✅ QR Code Detected                    │
│                                        │
│  Detected QR:                         │
│  [Thumbnail Frame]                    │
│                                        │
│  QR Data:                             │
│  http://127.0.0.1:8000/scan/1/        │
│                                        │
│  [Process QR] [Reset]                 │
│  [← Back]                             │
└─────────────────────────────────────────┘
```

### After Redirect

```
┌─────────────────────────────────────────┐
│  🏥 Diabetes (Health Condition)         │
│  - Benefits for diabetes management     │
│  - Heart health support                │
│                                        │
│  Available Food Items:                 │
│  ☐ Grilled Chicken Salad              │
│  ☐ Steamed Vegetables                 │
│  ☐ Brown Rice Bowl                    │
│  ☐ Green Tea                          │
│                                        │
│  [Place Order]                         │
│  [Continue Shopping]                   │
└─────────────────────────────────────────┘
```

---

## 📋 Files Overview

```
qrscanner/
├── views.py
│   ├── qr_camera_scanner() ← NEW
│   ├── process_camera_qr() ← NEW
│   ├── process_camera_qr_image() ← NEW
│   └── scan_qr_phone() [existing]
│
├── urls.py
│   ├── path('camera/', ...) ← NEW
│   ├── path('api/process-camera-qr/', ...) ← NEW
│   └── path('api/process-camera-qr-image/', ...) ← NEW
│
├── utils.py
│   └── Safe pyzbar import [UPDATED]
│
└── templates/qrscanner/
    ├── qr_camera_scanner.html ← NEW
    ├── index.html [UPDATED]
    └── [other existing templates]
```

---

## 💻 Server Commands

### Start Server
```bash
cd C:\Users\umesh\Downloads\mini.proj (1)\mini.proj
py manage.py runserver
```

### Server will be at:
```
http://127.0.0.1:8000/
```

### Stop Server
```
Press Ctrl+Break
```

---

## 🐛 Troubleshooting

| Issue | Solution |
|-------|----------|
| Camera not starting | Check browser permissions |
| QR not detecting | Ensure good lighting, clear QR code |
| Redirect not working | Verify health condition ID exists |
| Page not found | Check URL spelling |
| pyzbar error | Uninstalled - jsQR used instead |
| Session issues | Check cookies enabled |

---

## 🔑 Key Features

✓ Real-time QR detection (jsQR.js)
✓ Beautiful, responsive UI
✓ Mobile-friendly design
✓ Secure API endpoints
✓ Auto-redirect workflow
✓ Multiple QR formats supported
✓ Error handling
✓ Session management

---

## 📚 Full Docs

- See: `QR_SCANNING_SYSTEM.md` for detailed technical docs
- See: `IMPLEMENTATION_GUIDE.md` for complete implementation guide

---

**Last Updated: November 20, 2025**
**Status: ✅ Ready for Production**
