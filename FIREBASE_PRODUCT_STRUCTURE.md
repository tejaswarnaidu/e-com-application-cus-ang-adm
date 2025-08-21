# 📦 **Firebase Product Structure for Deal of the Day**

## 🎯 **Required Fields for Deal of the Day**

Add these fields to your products in Firebase console:

### **For your milk product (EV1729FLn0DSL34cygld):**

```json
{
  "name": "milk",
  "price": 76,
  "category": "Dairy",
  "subcategory": "Milk",
  "description": "fft",
  "imageUrl": "your-cloudinary-url",
  
  // ADD THESE FIELDS:
  "isDiscounted": true,
  "isAvailable": true,
  "originalPrice": 85,
  "stock": 50,
  "unit": "1 liter"
}
```

### **Steps to Add Fields:**

1. **Open Firebase Console**
2. **Go to Firestore Database**
3. **Click on each product document**
4. **Click "Add field" button**
5. **Add these required fields:**
   - `isDiscounted`: boolean → `true`
   - `isAvailable`: boolean → `true` 
   - `originalPrice`: number → `85` (higher than price)
   - `stock`: number → `50`
   - `unit`: string → `"1 liter"`

## 🎉 **Result:**
Once you add these fields, your products will appear in Deal of the Day with discount badges!