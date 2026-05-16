# Firestore Database Structure for E-Commerce Application

## Overview
This document describes the complete Firestore database schema for an e-commerce platform supporting Users, Products, Cart, Wishlist, and Orders.

---

## Collections Structure

### 1. **users** Collection
User accounts and profile information.

```json
{
  "users": {
    "userId_123": {
      "email": "user@example.com",
      "displayName": "John Doe",
      "avatar": "https://example.com/avatar.jpg",
      "phone": "+66812345678",
      "createdAt": "2024-01-15T10:30:00Z",
      "updatedAt": "2024-01-20T15:45:00Z",
      "isActive": true,
      "role": "customer",
      "addresses": [
        {
          "id": "addr_001",
          "type": "home",
          "street": "123 Main Street",
          "district": "Watthana",
          "city": "Bangkok",
          "province": "Bangkok",
          "postalCode": "10110",
          "country": "Thailand",
          "isDefault": true
        },
        {
          "id": "addr_002",
          "type": "office",
          "street": "456 Business Park",
          "district": "Chatuchak",
          "city": "Bangkok",
          "province": "Bangkok",
          "postalCode": "10900",
          "country": "Thailand",
          "isDefault": false
        }
      ],
      "preferences": {
        "notificationsEnabled": true,
        "newsletter": true,
        "theme": "light"
      },
      "stats": {
        "totalOrders": 5,
        "totalSpent": 15000,
        "lastOrderDate": "2024-01-20T08:00:00Z"
      }
    }
  }
}
```

**Field Descriptions:**
- `email`: User's email address (unique, used for authentication)
- `displayName`: User's display name
- `avatar`: Profile picture URL
- `phone`: Contact phone number
- `createdAt`: Account creation timestamp
- `updatedAt`: Last profile update timestamp
- `isActive`: Account status (soft delete)
- `role`: User type ("customer", "admin", "vendor")
- `addresses`: Array of shipping/billing addresses with type and default flag
- `preferences`: User settings and notification preferences
- `stats`: Aggregate statistics for quick access

---

### 2. **products** Collection
Product catalog with details and variants.

```json
{
  "products": {
    "prod_001": {
      "name": "Wireless Headphones",
      "description": "High-quality noise-cancelling wireless headphones",
      "sku": "WH-1000XM4",
      "categoryId": "cat_electronics",
      "categoryName": "Electronics",
      "subcategories": ["Audio", "Headphones"],
      "price": 2500,
      "originalPrice": 3500,
      "discount": 29,
      "currency": "THB",
      "inStock": true,
      "stockQuantity": 150,
      "lowStockThreshold": 20,
      "images": [
        {
          "id": "img_001",
          "url": "https://example.com/headphones-1.jpg",
          "alt": "Front view",
          "isPrimary": true
        },
        {
          "id": "img_002",
          "url": "https://example.com/headphones-2.jpg",
          "alt": "Side view",
          "isPrimary": false
        }
      ],
      "variants": [
        {
          "id": "var_001",
          "name": "Color: Black",
          "options": {
            "color": "Black",
            "size": "One Size"
          },
          "sku": "WH-1000XM4-BLK",
          "price": 2500,
          "stock": 100
        },
        {
          "id": "var_002",
          "name": "Color: Silver",
          "options": {
            "color": "Silver",
            "size": "One Size"
          },
          "sku": "WH-1000XM4-SLV",
          "price": 2500,
          "stock": 50
        }
      ],
      "specifications": {
        "brand": "Sony",
        "model": "WH-1000XM4",
        "warranty": "1 year",
        "batteryLife": "30 hours",
        "weight": "250g",
        "connectivity": "Bluetooth 5.0"
      },
      "rating": {
        "average": 4.5,
        "count": 128,
        "distribution": {
          "5": 80,
          "4": 30,
          "3": 12,
          "2": 4,
          "1": 2
        }
      },
      "reviews": [
        {
          "id": "review_001",
          "userId": "user_123",
          "username": "John Doe",
          "rating": 5,
          "title": "Amazing quality!",
          "comment": "Great headphones, excellent sound quality",
          "helpful": 45,
          "createdAt": "2024-01-15T10:30:00Z",
          "verified": true
        }
      ],
      "tags": ["wireless", "noise-cancelling", "premium", "audio"],
      "seoKeywords": ["wireless headphones", "noise cancelling", "Sony"],
      "createdAt": "2023-12-01T08:00:00Z",
      "updatedAt": "2024-01-20T15:45:00Z",
      "isActive": true,
      "vendor": {
        "id": "vendor_001",
        "name": "Electronics Plus",
        "rating": 4.8
      }
    }
  }
}
```

**Field Descriptions:**
- `name`: Product name
- `description`: Detailed product description
- `sku`: Stock Keeping Unit for inventory
- `categoryId`: Reference to category
- `categoryName`: Denormalized category name for quick access
- `price`: Current selling price
- `originalPrice`: Price before discount
- `discount`: Discount percentage
- `inStock`: Stock availability flag
- `stockQuantity`: Total available units
- `lowStockThreshold`: Minimum stock warning level
- `images`: Array of product images with primary flag
- `variants`: Product variants (size, color, etc.)
- `specifications`: Technical details
- `rating`: Aggregated review rating and distribution
- `reviews`: Array of customer reviews (limited for performance)
- `tags`: Search and filtering tags
- `vendor`: Vendor information (denormalized for quick access)

---

### 3. **cart** Subcollection (under users)
Shopping cart items for each user.

```json
{
  "users": {
    "userId_123": {
      "cart": {
        "cartItem_001": {
          "productId": "prod_001",
          "productName": "Wireless Headphones",
          "variantId": "var_001",
          "variantName": "Color: Black",
          "quantity": 2,
          "price": 2500,
          "discount": 29,
          "subtotal": 5000,
          "image": "https://example.com/headphones-1.jpg",
          "sku": "WH-1000XM4-BLK",
          "addedAt": "2024-01-20T10:00:00Z",
          "updatedAt": "2024-01-20T14:30:00Z"
        },
        "cartItem_002": {
          "productId": "prod_002",
          "productName": "Phone Case",
          "variantId": "var_005",
          "variantName": "Material: Silicone",
          "quantity": 1,
          "price": 300,
          "discount": 0,
          "subtotal": 300,
          "image": "https://example.com/case-1.jpg",
          "sku": "PC-SILICON-BLK",
          "addedAt": "2024-01-20T11:15:00Z",
          "updatedAt": "2024-01-20T11:15:00Z"
        }
      }
    }
  }
}
```

**Field Descriptions:**
- `productId`: Reference to product document
- `productName`: Denormalized product name for cart display
- `variantId`: Selected product variant
- `quantity`: Number of units in cart
- `price`: Unit price at time of adding to cart
- `discount`: Discount percentage
- `subtotal`: Total for this item (quantity × price)
- `image`: Product image URL for quick display
- `addedAt`: When item was added to cart
- `updatedAt`: Last modification time

---

### 4. **wishlist** Subcollection (under users)
User's wishlist/saved items.

```json
{
  "users": {
    "userId_123": {
      "wishlist": {
        "wishlistItem_001": {
          "productId": "prod_001",
          "productName": "Wireless Headphones",
          "categoryId": "cat_electronics",
          "price": 2500,
          "discount": 29,
          "image": "https://example.com/headphones-1.jpg",
          "rating": 4.5,
          "inStock": true,
          "vendor": {
            "id": "vendor_001",
            "name": "Electronics Plus"
          },
          "priceHistory": [
            {
              "price": 3500,
              "date": "2023-12-01"
            },
            {
              "price": 2800,
              "date": "2024-01-10"
            },
            {
              "price": 2500,
              "date": "2024-01-15"
            }
          ],
          "addedAt": "2024-01-18T09:00:00Z",
          "notes": "Want this for birthday gift",
          "priority": "high"
        }
      }
    }
  }
}
```

**Field Descriptions:**
- `productId`: Reference to product
- `productName`: Denormalized product name
- `price`: Current price
- `discount`: Current discount
- `image`: Product image
- `priceHistory`: Track price changes for price drop notifications
- `addedAt`: When added to wishlist
- `notes`: User notes/reminders
- `priority`: User's priority level (low, medium, high)

---

### 5. **orders** Collection
Complete order records.

```json
{
  "orders": {
    "order_001": {
      "userId": "userId_123",
      "orderNumber": "ORD-2024-00001",
      "orderDate": "2024-01-20T15:30:00Z",
      "status": "processing",
      "paymentStatus": "paid",
      "fulfillmentStatus": "pending",
      "items": [
        {
          "id": "item_001",
          "productId": "prod_001",
          "productName": "Wireless Headphones",
          "sku": "WH-1000XM4-BLK",
          "variantId": "var_001",
          "variantName": "Color: Black",
          "quantity": 2,
          "unitPrice": 2500,
          "discount": 29,
          "discountAmount": 725,
          "subtotal": 4275,
          "tax": 427.5
        }
      ],
      "summary": {
        "subtotal": 4275,
        "discountTotal": 725,
        "taxTotal": 427.5,
        "shippingCost": 100,
        "total": 5077.5,
        "currency": "THB"
      },
      "shippingAddress": {
        "fullName": "John Doe",
        "phone": "+66812345678",
        "street": "123 Main Street",
        "district": "Watthana",
        "city": "Bangkok",
        "province": "Bangkok",
        "postalCode": "10110",
        "country": "Thailand"
      },
      "billingAddress": {
        "fullName": "John Doe",
        "phone": "+66812345678",
        "street": "123 Main Street",
        "district": "Watthana",
        "city": "Bangkok",
        "province": "Bangkok",
        "postalCode": "10110",
        "country": "Thailand"
      },
      "shippingMethod": {
        "id": "ship_001",
        "name": "Express Delivery (2-3 days)",
        "cost": 100,
        "estimatedDelivery": "2024-01-23T23:59:59Z"
      },
      "paymentMethod": {
        "type": "credit_card",
        "last4": "1234",
        "cardBrand": "Visa",
        "transactionId": "txn_abc123xyz"
      },
      "couponApplied": {
        "code": "SAVE20",
        "discountAmount": 725,
        "expiryDate": "2024-02-28"
      },
      "timeline": [
        {
          "status": "pending",
          "timestamp": "2024-01-20T15:30:00Z",
          "note": "Order created"
        },
        {
          "status": "processing",
          "timestamp": "2024-01-20T16:00:00Z",
          "note": "Payment confirmed"
        },
        {
          "status": "shipped",
          "timestamp": "2024-01-21T10:00:00Z",
          "note": "Order dispatched",
          "trackingNumber": "TRK123456789"
        }
      ],
      "notes": "Please deliver in the afternoon",
      "createdAt": "2024-01-20T15:30:00Z",
      "updatedAt": "2024-01-21T10:00:00Z"
    }
  }
}
```

**Field Descriptions:**
- `orderNumber`: Human-readable order ID
- `status`: Order status (pending, processing, shipped, delivered, cancelled)
- `paymentStatus`: Payment state (unpaid, paid, refunded)
- `fulfillmentStatus`: Fulfillment state (pending, processing, shipped, delivered)
- `items`: Array of ordered products with pricing
- `summary`: Order total breakdown
- `shippingAddress`: Delivery address (denormalized for historical accuracy)
- `billingAddress`: Billing address
- `shippingMethod`: Selected shipping option
- `paymentMethod`: Payment details (securely stored)
- `couponApplied`: Discount/coupon information
- `timeline`: Status change history for tracking
- `notes`: Special delivery instructions

---

### 6. **categories** Collection (Optional)
Product categories for better organization.

```json
{
  "categories": {
    "cat_electronics": {
      "name": "Electronics",
      "slug": "electronics",
      "description": "Electronic devices and gadgets",
      "image": "https://example.com/cat-electronics.jpg",
      "parent": null,
      "subcategories": ["Audio", "Computers", "Mobile"],
      "productCount": 456,
      "isActive": true,
      "displayOrder": 1,
      "createdAt": "2023-12-01T08:00:00Z"
    }
  }
}
```

---

### 7. **reviews** Subcollection (under products - optional)
Separate reviews collection for better scalability.

```json
{
  "products": {
    "prod_001": {
      "reviews": {
        "review_001": {
          "userId": "user_123",
          "username": "John Doe",
          "avatar": "https://example.com/avatar.jpg",
          "rating": 5,
          "title": "Amazing quality!",
          "comment": "Great headphones, excellent sound quality and comfort",
          "helpful": 45,
          "notHelpful": 2,
          "verifiedPurchase": true,
          "createdAt": "2024-01-15T10:30:00Z",
          "updatedAt": "2024-01-15T10:30:00Z"
        }
      }
    }
  }
}
```

---

### 8. **userActivity** Subcollection (under users - optional)
Track user browsing and activity for recommendations.

```json
{
  "users": {
    "userId_123": {
      "userActivity": {
        "activity_001": {
          "type": "viewed_product",
          "productId": "prod_001",
          "productName": "Wireless Headphones",
          "categoryId": "cat_electronics",
          "timestamp": "2024-01-20T15:30:00Z"
        },
        "activity_002": {
          "type": "added_to_cart",
          "productId": "prod_002",
          "productName": "Phone Case",
          "timestamp": "2024-01-20T16:00:00Z"
        }
      }
    }
  }
}
```

---

## Data Relationships & Index Strategy

### Key Relationships:

```
Users
├── Cart Items (Subcollection)
│   └── Reference to Products
├── Wishlist Items (Subcollection)
│   └── Reference to Products
├── Orders (Reference via userId)
│   ├── Order Items
│   │   └── Reference to Products
│   └── Shipping/Billing Addresses (Embedded)
└── User Activity (Subcollection)
    └── Reference to Products

Products
├── Categories (Reference)
├── Variants (Embedded)
├── Reviews (Subcollection or Embedded)
└── Vendor (Embedded/Reference)

Orders
├── User (Reference)
└── Order Items
    └── Reference to Products
```

---

## Recommended Firestore Indexes

```json
{
  "indexes": [
    {
      "collection": "products",
      "fields": [
        {"fieldPath": "categoryId", "order": "ASCENDING"},
        {"fieldPath": "createdAt", "order": "DESCENDING"}
      ]
    },
    {
      "collection": "products",
      "fields": [
        {"fieldPath": "inStock", "order": "ASCENDING"},
        {"fieldPath": "rating.average", "order": "DESCENDING"}
      ]
    },
    {
      "collection": "orders",
      "fields": [
        {"fieldPath": "userId", "order": "ASCENDING"},
        {"fieldPath": "orderDate", "order": "DESCENDING"}
      ]
    },
    {
      "collection": "orders",
      "fields": [
        {"fieldPath": "status", "order": "ASCENDING"},
        {"fieldPath": "orderDate", "order": "DESCENDING"}
      ]
    }
  ]
}
```

---

## Firestore Security Rules

```javascript
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    // Users can only access their own documents
    match /users/{userId} {
      allow read, write: if request.auth.uid == userId;
      
      // Subcollections
      match /{subcollection=**} {
        allow read, write: if request.auth.uid == userId;
      }
    }
    
    // Products are readable by everyone, only admins can write
    match /products/{document=**} {
      allow read: if true;
      allow write: if request.auth.token.admin == true;
    }
    
    // Categories are readable by everyone
    match /categories/{document=**} {
      allow read: if true;
      allow write: if request.auth.token.admin == true;
    }
    
    // Orders - users can read/write their own, admins can read all
    match /orders/{orderId} {
      allow read: if request.auth.uid == resource.data.userId || request.auth.token.admin == true;
      allow write: if request.auth.uid == resource.data.userId;
    }
  }
}
```

---

## Best Practices Implemented

✅ **Denormalization**: Product names, prices, and images stored in cart/orders for historical accuracy and query efficiency

✅ **Subcollections**: Used for user-specific data (cart, wishlist) to enable real-time listeners per user

✅ **References**: Used for relationships that need frequent updates (product references in cart/orders)

✅ **Embedded Objects**: Used for read-together data (addresses, specifications, payment methods)

✅ **Document Size Management**: Large arrays (reviews, history) can be moved to separate subcollections as data grows

✅ **Indexing**: Compound indexes for common queries (category + price, user + date)

✅ **Status Tracking**: Timeline arrays for order state changes without requiring separate collections

✅ **Soft Deletes**: `isActive` fields instead of hard deletes for data recovery

✅ **Timestamps**: Both `createdAt` and `updatedAt` for auditing

✅ **Optimization**: Aggregated stats and ratings stored in product document to avoid expensive aggregation queries

---

## Migration Path for Growth

### Phase 1 (Current)
Keep everything as documented above with products and reviews embedded.

### Phase 2 (Scale Up)
- Move reviews to separate `reviews` subcollection under products
- Move user activity to separate `userActivity` subcollection

### Phase 3 (Enterprise)
- Consider separate `inventory` collection for stock management
- Create `analytics` collection for sales tracking
- Implement `audit_logs` for compliance

