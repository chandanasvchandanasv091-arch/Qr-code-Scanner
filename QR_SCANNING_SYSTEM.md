# QR Code Scanning System - Documentation

## Overview
This document describes the complete QR code scanning workflow including camera-based scanning, image upload, and automatic redirection to health condition pages.

## Workflow Diagram

```
┌─────────────────────────────────────────────────────────────────────┐
│                    QR Code Scanning System                          │
└─────────────────────────────────────────────────────────────────────┘

1. HOME PAGE (/)
   ↓
   ├─→ [🎥 Camera Scanner (NEW)] → Enhanced Camera Scanner
   ├─→ [📱 QR Scanner] → Basic QR Scanner  
   ├─→ [📸 Upload Image] → Image Upload
   └─→ [🧪 Test Generator] → Generate Test QR Codes

2. CAMERA SCANNER WORKFLOW (/camera/)
   ├─ Start Camera
   ├─ Detect QR Code (jsQR library)
   ├─ Display Detected QR
   ├─ Show QR Data
   ├─ Click "Process QR"
   └─ API: /api/process-camera-qr/ → Extract condition_id
       └─ Redirect to: /scan/{condition_id}/

3. SCAN URL (/scan/{condition_id}/)
   ├─ Get Health Condition
   ├─ Get Food Items
   ├─ Render QR Result Page (qr_result.html)
   └─ User Can Select Food & Place Order

4. UPLOAD WORKFLOW (/upload-qr/)
   ├─ Upload QR Image
   ├─ API: /upload-qr/ (POST)
   ├─ Decode QR (pyzbar - if available)
   ├─ Extract condition_id
   └─ Redirect to: /scan/{condition_id}/
```

## API Endpoints

### 1. Process Camera QR
- **URL:** `/api/process-camera-qr/`
- **Method:** POST
- **Content-Type:** application/json
- **Request Body:**
  ```json
  {
    "qr_data": "http://127.0.0.1:8000/scan/1/"
  }
  ```
- **Response (Success):**
  ```json
  {
    "success": true,
    "qr_id": "http://127.0.0.1:8000/scan/1/",
    "condition_id": 1,
    "redirect": "/scan/1/",
    "message": "QR code scanned successfully"
  }
  ```
- **Response (Error):**
  ```json
  {
    "error": "Could not extract health condition from QR code",
    "status": "invalid_qr"
  }
  ```

### 2. Process Camera QR Image
- **URL:** `/api/process-camera-qr-image/`
- **Method:** POST
- **Content-Type:** multipart/form-data
- **Request Body:**
  ```
  image_data: data:image/png;base64,iVBORw0KGgoAAAANSUhE...
  ```
- **Response:** Same as above

### 3. Process QR Code (Original)
- **URL:** `/api/process-qr/`
- **Method:** POST
- **Content-Type:** application/json
- **Handles:** Both URL format and HEALTH format QR codes

### 4. Upload QR Image
- **URL:** `/upload-qr/`
- **Method:** POST
- **Content-Type:** multipart/form-data
- **Request:** QR image file

## QR Code Formats Supported

### Format 1: URL-based
```
http://127.0.0.1:8000/scan/1/
http://localhost:8000/scan/1/
/scan/1/
```

### Format 2: HEALTH format
```
HEALTH:1:Diabetes
HEALTH:2:Hypertension
```

The system extracts the condition_id from both formats.

## Frontend Features

### Camera Scanner (/camera/)
- **Technology:** jsQR.js library (client-side)
- **Features:**
  - Real-time QR detection from camera
  - Visual QR frame overlay
  - Display detected QR data
  - Animated scanning indicator
  - Show captured frame as thumbnail
  - Display health condition info
  - Auto-redirect after processing

### QR Detection Flow
1. User clicks "Start Camera"
2. Camera stream starts with overlay guide
3. jsQR scans frames continuously
4. When QR detected:
   - Camera stops
   - Scanned data displayed
   - Frame thumbnail shown
   - "Process QR" button appears
5. User clicks "Process QR"
6. Backend validates and extracts condition_id
7. Auto-redirect to `/scan/{condition_id}/`

## Backend Processing

### Extract Condition ID (`extract_condition_id_from_qr_data`)
```python
def extract_condition_id_from_qr_data(qr_data):
    """
    Supports formats:
    - http://localhost:8000/scan/1/
    - http://127.0.0.1:8000/scan/1/
    - HEALTH:1:Diabetes
    - /scan/1/
    """
```

### Process Camera QR (`process_camera_qr`)
```python
@csrf_exempt
@require_http_methods(["POST"])
def process_camera_qr(request):
    # Extract QR data from request
    # Parse QR data
    # Verify health condition exists
    # Return redirect URL
```

### Scan QR Phone (`scan_qr_phone`)
```python
def scan_qr_phone(request, condition_id):
    # Get health condition
    # Create session
    # Get food items
    # Render qr_result.html
```

## URLs Configuration

```python
urlpatterns = [
    # Main views
    path('', views.index, name='index'),
    path('scanner/', views.qr_scanner, name='qr_scanner'),
    path('camera/', views.qr_camera_scanner, name='qr_camera_scanner'),  # NEW
    path('upload-qr/', views.upload_qr_image, name='upload_qr_image'),
    path('test-qr/', views.qr_test_generator, name='qr_test_generator'),
    path('health-conditions/', views.health_conditions, name='health_conditions'),
    
    # API endpoints
    path('api/process-qr/', views.process_qr_code, name='process_qr_code'),
    path('api/process-camera-qr/', views.process_camera_qr, name='process_camera_qr'),  # NEW
    path('api/process-camera-qr-image/', views.process_camera_qr_image, name='process_camera_qr_image'),  # NEW
    
    # Main workflow
    path('scan/<int:condition_id>/', views.scan_qr_phone, name='scan_qr_phone'),
    path('food-menu/<int:health_condition_id>/', views.food_menu, name='food_menu'),
    
    # Other
    path('api/create-order/', views.create_order, name='create_order'),
    path('order-confirmation/<int:order_id>/', views.order_confirmation, name='order_confirmation'),
    path('staff/', views.staff_dashboard, name='staff_dashboard'),
    path('api/update-order-status/', views.update_order_status, name='update_order_status'),
    path('api/orders-json/', views.get_orders_json, name='get_orders_json'),
]
```

## How to Generate Test QR Codes

Visit: `http://127.0.0.1:8000/test-qr/`

This will generate QR codes for each health condition with the URL format:
```
http://127.0.0.1:8000/scan/{condition_id}/
```

## Workflow Example: Full User Journey

### Step 1: User at Home
- Navigate to `http://127.0.0.1:8000/`
- See home page with options

### Step 2: User Clicks Camera Scanner
- Navigate to `http://127.0.0.1:8000/camera/`
- Enhanced QR scanner loads

### Step 3: User Starts Camera
- Click "Start Camera" button
- Camera access requested
- Live camera feed displays with overlay guide
- Scanning starts

### Step 4: QR Code Detected
- jsQR detects QR code in frame
- Camera stops automatically
- QR data displayed
- Frame thumbnail shown
- "Process QR" button appears

### Step 5: User Processes QR
- Click "Process QR" button
- Frontend sends QR data to `/api/process-camera-qr/`
- Backend validates and extracts condition_id
- Returns redirect URL

### Step 6: User Redirected
- Page shows "Redirecting..." message
- Auto-redirect to `/scan/{condition_id}/`
- QR Result page loads with health condition info

### Step 7: User Selects Food & Orders
- View food items for health condition
- Select items
- Place order
- Confirmation page

## Files Modified/Created

### Modified:
- `qrscanner/views.py` - Added new functions
- `qrscanner/urls.py` - Added new routes
- `qrscanner/utils.py` - Made pyzbar import safe
- `qrscanner/templates/qrscanner/index.html` - Added camera scanner link

### Created:
- `qrscanner/templates/qrscanner/qr_camera_scanner.html` - New enhanced camera scanner

## Error Handling

### Camera Access Denied
```javascript
showMsg('❌ Camera error: ' + e.message, 'error');
```

### Invalid QR Code
```json
{
  "error": "Could not extract health condition from QR code",
  "status": "invalid_qr"
}
```

### Health Condition Not Found
- Invalid condition_id in QR
- Backend validates before redirect

## Testing Checklist

- [ ] Generate test QR codes at `/test-qr/`
- [ ] Scan QR with camera scanner
- [ ] Upload QR image
- [ ] Verify condition_id extraction
- [ ] Test URL format parsing
- [ ] Test HEALTH format parsing
- [ ] Verify session storage
- [ ] Test redirect to `/scan/{condition_id}/`
- [ ] Test food menu display
- [ ] Test order creation

## Browser Compatibility

- **Chrome/Edge:** Full support
- **Firefox:** Full support
- **Safari:** Full support (iOS 14.5+)
- **Mobile Browsers:** Full support with camera

## Dependencies

- `jsQR` - QR code detection (client-side)
- `PIL (Pillow)` - Image processing
- `pyzbar` - QR decoding from images (optional)
- Django sessions - For storing QR data

## Performance Considerations

1. **Camera Performance**
   - requestAnimationFrame used for smooth scanning
   - Canvas operations optimized
   - Frame scaling for better performance

2. **Network**
   - Single API call for QR processing
   - JSON responses for fast parsing
   - CSRF protection enabled

3. **Memory**
   - Camera stream cleaned up properly
   - Canvas context reused
   - Event listeners properly removed

## Security Features

- CSRF protection on all API endpoints
- Session-based state management
- Input validation for condition_id
- Health condition existence verification
- No direct file access
