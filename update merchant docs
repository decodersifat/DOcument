# Admin Merchant API Documentation

## 1. GET `/merchants/:id` — Get Merchant Full Detail

**Auth:** Admin only (`Bearer <token>`)  
**Method:** `GET`

### Description
Returns comprehensive information about a merchant including personal info, all documents, all payout methods, all stores with full details and per-store performance, and aggregated parcel statistics.

---

### Example Request

```
GET {{baseUrl}}/merchants/a1b2c3d4-e5f6-7890-abcd-ef1234567890
Authorization: Bearer <admin_token>
```

### Example Response

```json
{
  "merchant": {
    "id": "a1b2c3d4-e5f6-7890-abcd-ef1234567890",
    "user_id": "u1234567-89ab-cdef-0123-456789abcdef",
    "full_name": "Karim Ahmed",
    "phone": "+8801712345678",
    "email": "karim@example.com",
    "thana": "Mirpur",
    "district": "Dhaka",
    "full_address": "House 12, Road 5, Block A, Mirpur-10, Dhaka-1216",
    "secondary_number": "01898765432",
    "status": "APPROVED",
    "is_active": true,
    "is_advance_payment_disabled": false,
    "approved_at": "2026-03-15T10:30:00.000Z",
    "created_at": "2026-03-01T08:00:00.000Z",
    "updated_at": "2026-04-20T14:00:00.000Z",

    "documents": {
      "nid": {
        "number": "1234567890123",
        "front_url": "https://s3.amazonaws.com/bucket/nid-front.jpg",
        "back_url": "https://s3.amazonaws.com/bucket/nid-back.jpg",
        "verified": true
      },
      "trade_license": {
        "number": "TL-2026-00456",
        "url": "https://s3.amazonaws.com/bucket/trade-license.pdf",
        "verified": true
      },
      "tin": {
        "number": "TIN-789012",
        "url": "https://s3.amazonaws.com/bucket/tin-cert.pdf",
        "verified": false
      },
      "bin": {
        "number": null,
        "url": null,
        "verified": false
      }
    },

    "payout_methods": [
      {
        "id": "pm-uuid-1",
        "method_type": "BKASH",
        "status": "VERIFIED",
        "is_default": true,
        "bank_name": null,
        "branch_name": null,
        "account_holder_name": null,
        "account_number": null,
        "routing_number": null,
        "bkash_number": "01712345678",
        "bkash_account_holder_name": "Karim Ahmed",
        "bkash_account_type": "PERSONAL",
        "nagad_number": null,
        "nagad_account_holder_name": null,
        "nagad_account_type": null,
        "verified_at": "2026-03-20T12:00:00.000Z",
        "created_at": "2026-03-15T10:30:00.000Z"
      },
      {
        "id": "pm-uuid-2",
        "method_type": "BANK_TRANSFER",
        "status": "PENDING",
        "is_default": false,
        "bank_name": "Dutch-Bangla Bank",
        "branch_name": "Mirpur Branch",
        "account_holder_name": "Karim Ahmed",
        "account_number": "1234567890",
        "routing_number": "090260345",
        "bkash_number": null,
        "bkash_account_holder_name": null,
        "bkash_account_type": null,
        "nagad_number": null,
        "nagad_account_holder_name": null,
        "nagad_account_type": null,
        "verified_at": null,
        "created_at": "2026-04-01T09:00:00.000Z"
      }
    ],

    "store_count": 2,
    "stores": [
      {
        "id": "store-uuid-1",
        "store_code": "KAR001",
        "merchant_id": "a1b2c3d4-e5f6-7890-abcd-ef1234567890",
        "business_name": "Karim Fashion House",
        "business_address": "Shop 12, Mirpur-10 Market",
        "phone_number": "01712345678",
        "email": "shop@karimfashion.com",
        "facebook_page": "https://facebook.com/karimfashion",
        "hub_id": "hub-uuid-1",
        "is_default": true,
        "status": "APPROVED",
        "district": "Dhaka",
        "thana": "Mirpur",
        "area": "Mirpur-10",
        "carrybee_store_id": "cb-123",
        "carrybee_city_id": 1,
        "carrybee_zone_id": 5,
        "carrybee_area_id": 22,
        "is_carrybee_synced": true,
        "carrybee_synced_at": "2026-03-20T12:00:00.000Z",
        "auto_assign_to_carrybee": false,
        "created_at": "2026-03-01T08:00:00.000Z",
        "updated_at": "2026-04-10T11:00:00.000Z",
        "performance": {
          "total_parcels_handled": 245,
          "successfully_delivered": 210,
          "total_returns": 18
        },
        "hub": {
          "id": "hub-uuid-1",
          "hub_code": "DHK001",
          "branch_name": "Dhaka Central Hub",
          "area": "Mirpur",
          "address": "Road 12, Mirpur-2"
        },
        "merchant": { "..." : "merchant summary" }
      },
      {
        "id": "store-uuid-2",
        "store_code": "KAR002",
        "business_name": "Karim Online Store",
        "business_address": "House 5, Road 3, Gulshan-2",
        "phone_number": "01898765432",
        "is_default": false,
        "status": "APPROVED",
        "district": "Dhaka",
        "thana": "Gulshan",
        "area": "Gulshan-2",
        "performance": {
          "total_parcels_handled": 89,
          "successfully_delivered": 72,
          "total_returns": 8
        },
        "hub": {
          "id": "hub-uuid-2",
          "hub_code": "DHK002",
          "branch_name": "Gulshan Hub"
        },
        "...": "other fields"
      }
    ],

    "parcel_stats": {
      "total_parcels": 334,
      "total_delivered": 282,
      "total_returns": 26
    }
  },
  "message": "Merchant retrieved successfully"
}
```

---

## 2. PATCH `/merchants/:id` — Admin Update Merchant

**Auth:** Admin only (`Bearer <token>`)  
**Method:** `PATCH`

### Description
Allows admin to update merchant info individually or in groups. All fields are optional — send only the fields you want to update.

### Patchable Fields

| Field | Type | Description |
|-------|------|-------------|
| `fullAddress` | `string` | Merchant full address |
| `secondaryNumber` | `string` | Secondary phone number |
| `thana` | `string` | Thana / upazila |
| `district` | `string` | District |
| `nid_number` | `string` | NID number (resets verification) |
| `trade_license_number` | `string` | Trade license number (resets verification) |
| `bin_number` | `string` | BIN number (resets verification) |
| `stores` | `array` | Array of store updates (see below) |

### Store Update Object

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `id` | `uuid` | ✅ Yes | Store ID to update |
| `business_name` | `string` | No | Store business name |
| `business_address` | `string` | No | Store address |
| `phone_number` | `string` | No | BD format (01XXXXXXXXX) |
| `email` | `string` | No | Store email |
| `district` | `string` | No | Store district |
| `thana` | `string` | No | Store thana |
| `area` | `string` | No | Store area |
| `facebook_page` | `string` | No | Facebook page URL |

---

### Example 1: Update Only NID Number

```
PATCH {{baseUrl}}/merchants/a1b2c3d4-e5f6-7890-abcd-ef1234567890
Content-Type: application/json
Authorization: Bearer <admin_token>
```

```json
{
  "nid_number": "9876543210123"
}
```

**Response:**

```json
{
  "id": "a1b2c3d4-e5f6-7890-abcd-ef1234567890",
  "message": "Merchant updated successfully"
}
```

---

### Example 2: Update NID + Trade License Together

```json
{
  "nid_number": "9876543210123",
  "trade_license_number": "TL-2026-99999"
}
```

---

### Example 3: Update Only BIN Number

```json
{
  "bin_number": "BIN-2026-00123"
}
```

---

### Example 4: Update Store Info

```json
{
  "stores": [
    {
      "id": "store-uuid-1",
      "business_name": "Karim Fashion House - Updated",
      "phone_number": "01999999999"
    }
  ]
}
```

---

### Example 5: Update Documents + Store Info Together

```json
{
  "nid_number": "9876543210123",
  "trade_license_number": "TL-2026-99999",
  "bin_number": "BIN-2026-00123",
  "stores": [
    {
      "id": "store-uuid-1",
      "business_name": "Karim Fashion House v2",
      "business_address": "New Address, Mirpur-12",
      "email": "newshop@karim.com"
    },
    {
      "id": "store-uuid-2",
      "thana": "Uttara",
      "district": "Dhaka"
    }
  ]
}
```

---

### Example 6: Update Basic Info + Documents

```json
{
  "fullAddress": "House 15, Road 8, Dhanmondi, Dhaka",
  "secondaryNumber": "01611111111",
  "district": "Dhaka",
  "nid_number": "5555555555555"
}
```

---

> [!IMPORTANT]
> When document numbers (nid_number, trade_license_number, bin_number) are updated via PATCH, their verification status is automatically reset to `false`. Admin must re-verify the document through the separate approval endpoints.
