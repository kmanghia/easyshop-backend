# Clothes E-commerce Server

A Node.js back-end server for an e-commerce clothing platform developed by Trần Nghĩa.

## Project Overview

This server provides a complete back-end solution for a clothing e-commerce application with features including:

- User authentication and authorization
- Product management
- Shop management
- Order processing
- Shopping cart functionality
- Coupon system
- Reviews and ratings
- Address management
- Favorites/wishlists
- Chat functionality
- Notifications system

## Technology Stack

- **Node.js** with **Express.js** framework
- **MySQL** database with **Sequelize ORM**
- **JWT** for authentication
- **Socket.IO** for real-time communication
- **Nodemailer** for email notifications
- **Google AI** integration for chat functionality
- **Babel** for modern JavaScript support

## Installation and Setup

1. **Clone the repository**

```bash
git clone <repository-url>
cd Clothes-Server-server
```

2. **Install dependencies**

```bash
npm install
```

3. **Configure database**

- Create a MySQL database named `clothes`
- Update database configuration in `src/config/connectDB.js` if needed

4. **Environment Variables**

Create a `.env` file in the root directory with the following variables:

```
PORT=3000
JWT_SECRET=your_jwt_secret
EMAIL_USER=your_email@example.com
EMAIL_PASS=your_email_password
```

5. **Run database migrations**

```bash
npx sequelize-cli db:migrate
```

6. **Start the development server**

```bash
npm run serve
```

7. **For production**

```bash
npm run build
npm start
```

## API Endpoints

The API is structured around these main resources:

- `/api/auth` - Authentication (login, register, password reset)
- `/api/users` - User management
- `/api/shops` - Shop management
- `/api/products` - Product listings and details
- `/api/categories` - Product categories
- `/api/attributes` - Product attributes (colors, sizes)
- `/api/coupons` - Discount coupons
- `/api/cart` - Shopping cart operations
- `/api/orders` - Order processing
- `/api/address` - User address management
- `/api/reviews` - Product reviews
- `/api/favorites` - User favorite products
- `/api/notifications` - User notifications
- `/api/chat` - Chat functionality

## Database Structure

The database follows a relational model with MySQL and Sequelize ORM. Below is a detailed overview of the database structure.

### Core Entities

#### 👤 Users
- Primary entity for authentication and user management
- Stores user credentials, profile information, and role

#### 🏪 Shops
- Represents seller accounts that can list products
- Connected to users (a shop is owned by a user)

#### 👕 Products
- Central entity for all merchandise
- Connected to shops, categories, and variants

#### 📁 Categories
- Hierarchical structure for product organization
- Supports parent-child relationships

#### 🛒 Orders
- Tracks customer purchases
- Contains order items, status, and payment information

#### 🛍️ Carts
- Temporary storage for items before checkout
- Connected to users and contains cart items

### Supporting Entities

#### 📍 Addresses
- Hierarchical structure with Cities, Districts, and Wards
- Connected to users for shipping and billing

#### 🔄 Product Variants
- Extends products with different options (size, color)
- Manages inventory at the variant level

#### ⭐ Reviews
- User feedback and ratings for products
- Connected to both users and products

#### 🏷️ Coupons
- Discount codes for promotions
- Can be applied to orders

#### ❤️ Favorites
- User's saved/wishlisted products
- Connected to users and products

#### 🔔 Notifications
- System messages for users
- Tracks order updates, promotions, etc.

#### 💬 Chat Messages
- Communication between users and shops
- Support for customer service

### Database Schema Diagram

#### User Management Tables
```
👤 USER
├── id (PK)
├── email
├── password
├── fullname
├── phone
├── role
├── createdAt
└── updatedAt
```

#### Shop Tables
```
🏪 SHOP
├── id (PK)
├── name
├── user_id (FK -> USER.id)
├── description
├── logo
├── banner
├── status
├── createdAt
└── updatedAt
```

#### Product Tables
```
👕 PRODUCT
├── id (PK)
├── name
├── shop_id (FK -> SHOP.id)
├── category_id (FK -> CATEGORY.id)
├── price
├── description
├── stock
├── status
├── createdAt
└── updatedAt

🔄 PRODUCT_VARIANT
├── id (PK)
├── product_id (FK -> PRODUCT.id)
├── color_id (FK -> COLOR.id)
├── size_id (FK -> SIZE.id)
├── stock
├── price
├── sku
├── createdAt
└── updatedAt

🎨 COLOR
├── id (PK)
├── name
├── code
├── createdAt
└── updatedAt

📏 SIZE
├── id (PK)
├── name
├── value
├── createdAt
└── updatedAt

🖼️ PRODUCT_IMAGE
├── id (PK)
├── product_id (FK -> PRODUCT.id)
├── url
├── is_thumbnail
├── createdAt
└── updatedAt
```

#### Category Tables
```
📁 CATEGORY
├── id (PK)
├── name
├── parent_id (FK -> CATEGORY.id)
├── description
├── image
├── createdAt
└── updatedAt
```

#### Review Tables
```
⭐ REVIEW
├── id (PK)
├── product_id (FK -> PRODUCT.id)
├── user_id (FK -> USER.id)
├── rating
├── comment
├── createdAt
└── updatedAt
```

#### Order Tables
```
🛒 ORDER
├── id (PK)
├── user_id (FK -> USER.id)
├── status
├── total
├── payment_method
├── order_date
├── createdAt
└── updatedAt

🏪 ORDER_SHOP
├── id (PK)
├── order_id (FK -> ORDER.id)
├── shop_id (FK -> SHOP.id)
├── subtotal
├── status
├── createdAt
└── updatedAt

📦 ORDER_ITEM
├── id (PK)
├── order_shop_id (FK -> ORDER_SHOP.id)
├── product_id (FK -> PRODUCT.id)
├── variant_id (FK -> PRODUCT_VARIANT.id)
├── quantity
├── price
├── createdAt
└── updatedAt
```

#### Cart Tables
```
🛍️ CART
├── id (PK)
├── user_id (FK -> USER.id)
├── createdAt
└── updatedAt

🏪 CART_SHOP
├── id (PK)
├── cart_id (FK -> CART.id)
├── shop_id (FK -> SHOP.id)
├── createdAt
└── updatedAt

📦 CART_ITEM
├── id (PK)
├── cart_shop_id (FK -> CART_SHOP.id)
├── product_id (FK -> PRODUCT.id)
├── variant_id (FK -> PRODUCT_VARIANT.id)
├── quantity
├── createdAt
└── updatedAt
```

#### Coupon Tables
```
🏷️ COUPON
├── id (PK)
├── shop_id (FK -> SHOP.id)
├── code
├── value
├── type
├── start_date
├── end_date
├── usage_limit
├── used_count
├── createdAt
└── updatedAt

🎟️ USER_COUPON
├── id (PK)
├── user_id (FK -> USER.id)
├── coupon_id (FK -> COUPON.id)
├── is_used
├── used_date
├── createdAt
└── updatedAt
```

#### Address Tables
```
📍 ADDRESS
├── id (PK)
├── user_id (FK -> USER.id)
├── city_id (FK -> CITY.id)
├── district_id (FK -> DISTRICT.id)
├── ward_id (FK -> WARD.id)
├── street
├── phone
├── recipient_name
├── is_default
├── createdAt
└── updatedAt

🏙️ CITY
├── id (PK)
├── name
├── code
├── createdAt
└── updatedAt

🏢 DISTRICT
├── id (PK)
├── city_id (FK -> CITY.id)
├── name
├── code
├── createdAt
└── updatedAt

🏘️ WARD
├── id (PK)
├── district_id (FK -> DISTRICT.id)
├── name
├── code
├── createdAt
└── updatedAt
```

#### Favorite Tables
```
❤️ FAVORITE
├── id (PK)
├── user_id (FK -> USER.id)
├── product_id (FK -> PRODUCT.id)
├── createdAt
└── updatedAt
```

#### Notification Tables
```
🔔 NOTIFICATION
├── id (PK)
├── user_id (FK -> USER.id)
├── title
├── content
├── type
├── is_read
├── createdAt
└── updatedAt
```

#### Chat Tables
```
💬 CHAT
├── id (PK)
├── user_id (FK -> USER.id)
├── shop_id (FK -> SHOP.id)
├── message
├── sender_type
├── is_read
├── createdAt
└── updatedAt

🤖 CHAT_HISTORY
├── id (PK)
├── user_id (FK -> USER.id)
├── query
├── response
├── createdAt
└── updatedAt
```

### Key Relationships

1. **User Relationships:**
   - User owns Shop (1:N)
   - User has Address (1:N)
   - User places Order (1:N)
   - User has Cart (1:1)
   - User writes Review (1:N)
   - User saves Favorite products (1:N)
   - User receives Notification (1:N)
   - User collects Coupons (1:N)
   - User participates in Chat (1:N)

2. **Shop Relationships:**
   - Shop sells Products (1:N)
   - Shop offers Coupons (1:N)
   - Shop fulfills Orders (1:N)
   - Shop communicates via Chat (1:N)

3. **Product Relationships:**
   - Product has Variants (1:N)
   - Product has Images (1:N)
   - Product receives Reviews (1:N)
   - Product can be Favorite (1:N)
   - Product can be in Order (1:N)
   - Product can be in Cart (1:N)

4. **Category Relationships:**
   - Category has Products (1:N)
   - Category has Subcategories (1:N)

5. **Order Relationships:**
   - Order contains Order Shops (1:N)
   - Order Shop includes Order Items (1:N)

6. **Cart Relationships:**
   - Cart contains Cart Shops (1:N)
   - Cart Shop includes Cart Items (1:N)

7. **Address Relationships:**
   - City contains Districts (1:N)
   - District contains Wards (1:N)
   - Address is located in City, District, Ward (N:1)

## Features

- **Authentication** - Secure user registration and login with JWT
- **User Management** - User profiles, preferences, and history
- **Product Management** - CRUD operations for products with variants
- **Shopping Experience** - Cart, wishlist, and checkout processes
- **Order Processing** - Order creation, status tracking, and history
- **Review System** - Product ratings and comments
- **Chat Support** - Real-time communication between users and shops
- **Notifications** - Real-time updates on orders, products, etc.

## License

ISC License - See LICENSE file for details 
