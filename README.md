# Week-1-Day-2-MongoDB-Database-Models.
create the four core models:  User — Super Admin, Vendor, Customer Store — each vendor's store Product — products belonging to a store Order — customer purchases

1. User Model

Create:

backend/models/User.js

const mongoose = require("mongoose");

const userSchema = new mongoose.Schema(
  {
    name: {
      type: String,
      required: true,
      trim: true
    },

    email: {
      type: String,
      required: true,
      unique: true,
      lowercase: true,
      trim: true
    },

    password: {
      type: String,
      required: true
    },

    role: {
      type: String,
      enum: ["superadmin", "vendor", "customer"],
      default: "customer"
    },

    storeId: {
      type: mongoose.Schema.Types.ObjectId,
      ref: "Store",
      default: null
    }
  },
  {
    timestamps: true
  }
);

module.exports = mongoose.model("User", userSchema);


2. Store Model

Create:

backend/models/Store.js

const mongoose = require("mongoose");

const storeSchema = new mongoose.Schema(
  {
    name: {
      type: String,
      required: true,
      trim: true
    },

    description: {
      type: String,
      default: ""
    },

    owner: {
      type: mongoose.Schema.Types.ObjectId,
      ref: "User",
      required: true
    },

    logo: {
      type: String,
      default: ""
    },

    isActive: {
      type: Boolean,
      default: true
    }
  },
  {
    timestamps: true
  }
);

module.exports = mongoose.model("Store", storeSchema);

Example:

Store
 ├── name: "Vaishnavi Fashion"
 ├── owner: Vendor ID
 ├── logo
 └── isActive: true


 3. Product Model

Create:

backend/models/Product.js

const mongoose = require("mongoose");

const productSchema = new mongoose.Schema(
  {
    storeId: {
      type: mongoose.Schema.Types.ObjectId,
      ref: "Store",
      required: true,
      index: true
    },

    name: {
      type: String,
      required: true,
      trim: true
    },

    description: {
      type: String,
      default: ""
    },

    price: {
      type: Number,
      required: true,
      min: 0
    },

    stock: {
      type: Number,
      required: true,
      min: 0,
      default: 0
    },

    category: {
      type: String,
      required: true
    },

    image: {
      type: String,
      default: ""
    },

    variants: [
      {
        name: String,
        value: String,
        price: Number,
        stock: Number
      }
    ]
  },
  {
    timestamps: true
  }
);

module.exports = mongoose.model("Product", productSchema);

4. Order Model

Create:

backend/models/Order.js

const mongoose = require("mongoose");

const orderSchema = new mongoose.Schema(
  {
    customerId: {
      type: mongoose.Schema.Types.ObjectId,
      ref: "User",
      required: true
    },

    storeId: {
      type: mongoose.Schema.Types.ObjectId,
      ref: "Store",
      required: true,
      index: true
    },

    items: [
      {
        productId: {
          type: mongoose.Schema.Types.ObjectId,
          ref: "Product",
          required: true
        },

        name: String,

        quantity: {
          type: Number,
          required: true,
          min: 1
        },

        price: {
          type: Number,
          required: true
        }
      }
    ],

    totalAmount: {
      type: Number,
      required: true
    },

    paymentStatus: {
      type: String,
      enum: ["pending", "paid", "failed", "refunded"],
      default: "pending"
    },

    orderStatus: {
      type: String,
      enum: [
        "placed",
        "processing",
        "shipped",
        "delivered",
        "cancelled"
      ],
      default: "placed"
    },

    stripePaymentId: {
      type: String,
      default: null
    }
  },
  {
    timestamps: true
  }
);

module.exports = mongoose.model("Order", orderSchema);

🔗 Database Relationship

                    USER
                     │
          ┌──────────┼──────────┐
          │          │          │
      SuperAdmin   Vendor    Customer
                     │          │
                     ↓          ↓
                   STORE      ORDERS
                     │          ↑
                     ↓          │
                 PRODUCTS ──────┘

backend/
├── config/
│   └── db.js
│
├── models/
│   ├── User.js       ✅
│   ├── Store.js      ✅
│   ├── Product.js    ✅
│   └── Order.js      ✅
│
├── controllers/
├── middleware/
├── routes/
├── services/
├── utils/
├── .env
├── .gitignore
├── server.js
└── package.json


























