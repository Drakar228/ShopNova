# ShopNova - Smart Glasses E-Commerce Platform

ShopNova is a full-stack e-commerce application built for the smart glasses market. It's not a generic shopping site; we specialize in one genuinely exciting product category: smart eyewear. We're talking Meta Ray-Ban glasses, Apple Vision Pro, AR glasses from companies like Xreal and Microsoft HoloLens, and audio eyewear from Bose and Amazon.

The whole thing is built with real technologies used in production: Spring Boot, MySQL, Thymeleaf, and Spring Security. It's a working platform, not a mock-up. Users can browse, filter products, manage carts, place orders, and track them. Admins get a full dashboard with business metrics and inventory management.

## What You Can Do

**As a Customer:**
- Browse a catalogue of 20 real smart glasses products across 4 categories
- Filter by category, price range, brand, and sort by rating or price
- View detailed product specs (battery life, display type, connectivity, weight, etc.)
- Add products to a persistent shopping cart
- Create an account with secure BCrypt password hashing
- Check out and place orders with shipping info
- Track your order history with status updates and tracking numbers
- Scan QR codes on product pages to open them on your phone

**As an Admin:**
- See live business metrics: total revenue, orders, registered users, low-stock alerts
- View a revenue chart showing 12 months of sales data
- Manage order statuses (change from Processing to Shipped to Delivered)
- Show/hide products from the shop
- View all users and their purchase history

## Tech Stack

| Technology | Why It's Here |

| **Spring Boot 3.2** | Main framework. Handles routing, dependency injection, and configuration without XML files. |
| **Spring Security 6** | Authentication, authorization, and BCrypt password hashing. |
| **Spring Data JPA / Hibernate** | Maps Java objects to MySQL tables automatically. No manual SQL. |
| **MySQL 8.0** | Real persistent database. Data survives server restarts. Industry standard. |
| **Thymeleaf** | Server-side template engine. Injects live data into HTML pages. |
| **Apache Tomcat 10** | Application server. Runs the application. |
| **QRCode.js** | Generates real scannable QR codes in the browser. |
| **Maven** | Builds the project and manages dependencies. |

## Architecture

The application follows a clean 3-layer MVC architecture:

- **Controller Layer**: Handles HTTP requests. Never contains business logic or database code.
- **Service Layer**: All business logic lives here. Filtering products, calculating cart totals, managing orders.
- **Repository Layer**: Talks to the database. Spring Data JPA turns method signatures into SQL queries automatically.
- **Model Layer**: Plain Java entity classes. Hibernate reads the annotations and creates MySQL tables.
- **Templates**: Thymeleaf HTML files. The server fills in the live data before sending to the browser.

## Features in Action

### Home Page
Landing page with hero section, category guides, featured products, and brand logos. Designed to look premium, not generic.

![Home Page](shopnova_images/image1.png)

### Shop with Filters
Full sidebar with category filter, price range slider, brand filter, and sort options. All server-side filtering, so filtered views are bookmarkable.

![Shop Page]([shopnova_images/image2.png](https://github.com/Drakar228/ShopNova/blob/main/image1.png))

### Product Details
Full specifications table, pricing with discount badges, QR code panel that actually works, and stock-aware Add to Cart button.

![Product Detail](shopnova_images/image3.png)

### Authentication
Clean login and registration pages with BCrypt password hashing. No demo credentials exposed.

![Sign In](shopnova_images/image4.png)

![Registration](shopnova_images/image5.png)

### Shopping Cart
Persistent session-based cart with quantity controls, line totals, and sticky order summary. Shows shipping cost calculation.

![Shopping Cart](shopnova_images/image6.png)

### Order Tracking
Customers can see their complete purchase history with status badges, items purchased, totals, shipping address, and tracking numbers.

![My Orders](shopnova_images/image7.png)

### Admin Dashboard
KPI cards showing live business metrics, revenue chart, order management with status dropdowns, and full product inventory with stock level indicators.

![Admin Dashboard](shopnova_images/image8.png)

## Database Design

Six tables, all created automatically by Hibernate:

| Table | What It Does |

| **users** | Registered accounts with BCrypt hashed passwords and role-based access (USER or ADMIN) |
| **products** | 20 smart glasses products with specs, pricing, stock, and Unsplash image URLs |
| **carts** | One row per user. Links them to their persistent shopping cart. |
| **cart_items** | Individual items in a cart with quantities. Increments quantity instead of creating duplicates. |
| **orders** | Orders with SN-YYYYMMDD-XXXX format numbers, shipping address, status, and tracking. |
| **order_items** | Snapshot of what was in the cart at purchase. Stores unit price so price changes don't affect historical orders. |

## Security

- **URL Access Control**: Spring Security blocks unauthorized access. Regular users can't guess their way into admin pages.
- **Password Hashing**: BCrypt with cost factor 12. Takes 250ms to compute, making brute force attacks expensive.
- **CSRF Protection**: Enabled on all forms.
- **Session Management**: Max one active session per user. Logging in on a second device invalidates the first.

## The Product Catalogue

20 real products across 4 categories:

**Meta Glasses**: Ray-Ban Wayfarer, Headliner, and Standard versions

**VR Headsets**: Quest 3/3S, Apple Vision Pro, Samsung Galaxy XR, PlayStation VR2

**AR Glasses**: Xreal Air 2 Ultra, Google Glass Enterprise 2, Microsoft HoloLens 2, Vuzix Blade, and more

**Audio Glasses**: Bose Frames, Amazon Echo Frames, Huawei EyeWear, Fauna Audio

Every product includes real Unsplash photos, accurate pricing, and genuine technical specs. The price range goes from ~$100 for audio glasses to $3,499 for the Apple Vision Pro, which makes the filtering system actually useful.

## Setup Instructions

You need: MySQL, Java 17, and IntelliJ IDEA.

### Step 1: Install MySQL
Download from mysql.com/downloads. Choose Developer Default setup. Remember your root password.

### Step 2: Create the Database
Open MySQL Workbench. Run this:
```sql
CREATE DATABASE shopnova_db CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
```

### Step 3: Configure the Application
Open `src/main/resources/application.properties`. Update this line:
```
spring.datasource.password=your_mysql_password_here
```

### Step 4: Run It
Open `ShopNovaApplication.java` in IntelliJ. Click the green Run button. Watch the console. On first run, Hibernate creates the tables and the DataSeeder populates the products. When you see "Started ShopNovaApplication", you're ready.

### Step 5: Open in Browser
Go to `http://localhost:8080/shopnova`

### Test Accounts
Admin account: `admin@shopnova.com` / `Admin@123`
Customer account: `sara@example.com` / `User@123`

## Code Quality

The whole codebase follows SOLID principles:

- **Single Responsibility**: Controllers handle HTTP. Repositories handle queries. Services handle logic. No mixing.
- **Open/Closed**: Adding new filters doesn't touch existing filter code.
- **Dependency Inversion**: Everything is injected. Easy to swap implementations.
- **Interface-based**: All repositories are interfaces. Spring Data generates implementations automatically.
- **Defense in Depth**: Security is built in at multiple layers, not just one place.

## What Went Well

Switching from H2 to MySQL was the right call. It forces you to think about real persistence, configuration, and what happens between restarts — all production concerns.

The product catalogue with 20 real products and accurate specs makes every feature feel meaningful. Testing filters with a $299 Meta Ray-Ban next to a $3,499 Apple Vision Pro makes the price slider actually useful.

The QR code feature works end to end. Scan the code on a product page, and it opens on your phone. That's a real-world detail.

Replacing emoji icons with inline SVGs completely changed the look. This looks like a professional product, not assembled from Unicode.

## What Could Be Better

The checkout flow doesn't integrate a real payment processor like Stripe. It accepts any payment details without validation.

The admin revenue chart uses demo data instead of aggregating actual orders from MySQL. That would be a straightforward fix with a custom @Query method.

Product images come from Unsplash URLs directly. Production would self-host or use a CDN with fallback handling.

## Code Structure

```

