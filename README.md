# 🏨 Hotel Booking & Hospitality Management System

[![Laravel](https://img.shields.io/badge/Laravel_12.x-FF2D20?style=for-the-badge&logo=laravel&logoColor=white)](https://laravel.com)
[![PHP](https://img.shields.io/badge/PHP_8.2+-777BB4?style=for-the-badge&logo=php&logoColor=white)](https://www.php.net/)
[![MySQL](https://img.shields.io/badge/MySQL_8.x-4479A1?style=for-the-badge&logo=mysql&logoColor=white)](https://www.mysql.com/)
[![WebSockets](https://img.shields.io/badge/Real--time-Pusher_WebSockets-00b4d8?style=for-the-badge)]()
[![Status](https://img.shields.io/badge/Status-Production_Ready-success?style=for-the-badge)]()

> **A full-stack Hotel Booking & Hospitality Management platform built with Laravel 12, PHP 8.2+, MySQL, Bootstrap 5, and Pusher WebSockets — featuring real-time date-range room availability checking, add-on service bundling, travel package and car rental bookings, tiered premium subscription discounts, and live customer-staff chat.**

---

## ⚡ Engineering Snapshot (60-Second Overview)

Hotel Booking Management is a multi-vertical hospitality platform unifying room reservations, tour packages, car rentals, and real-time customer support into a single integrated Laravel application.

```
Key Engineering Focus Areas:
• Date-range room availability engine with mathematical booking overlap prevention
• Real-time bi-directional live chat support between guests and hotel staff using Pusher WebSockets
• 3-Tier Role-Based Access Control: Customer → Hotel Manager → System Admin
• Add-on service bundling (Spa, Dining, Gym, Room Service) via relational pivot models
• Multi-vertical reservation system supporting Hotel Rooms, Tour Packages, and Car Rentals
• Tiered premium subscription system with automatic discount calculations during checkout
• Architectural artifacts included: DFD Level 0/1, ERD schema, and Laravel MVC workflow diagrams
```

---

## 📑 Table of Contents

1. [Platform Overview](#-platform-overview)
2. [System Architecture & Visual Diagrams](#-system-architecture--visual-diagrams)
3. [Engineering Decisions & Trade-offs](#-engineering-decisions--trade-offs)
4. [Implementation Status Matrix](#-implementation-status-matrix)
5. [Core Domain Modules](#-core-domain-modules)
6. [Room Booking & Date Overlap Engine](#-room-booking--date-overlap-engine)
7. [Real-Time Live Chat Architecture](#-real-time-live-chat-architecture)
8. [Multi-Vertical Reservation System](#-multi-vertical-reservation-system)
9. [Premium Subscription & Discount Engine](#-premium-subscription--discount-engine)
10. [Role-Based Access Control (RBAC)](#-role-based-access-control-rbac)
11. [Key Engineering Challenges & Solutions](#-key-engineering-challenges--solutions)
12. [Database Schema & Entity Relationships](#-database-schema--entity-relationships)
13. [Installation & Local Setup](#-installation--local-setup)
14. [Author & Contributions](#-author--contributions)

---

## 🏨 Platform Overview

```
┌─────────────────────────────────────────────────────────────────────────┐
│              Hotel Booking & Hospitality Platform                       │
├──────────────────────────┬─────────────────────────┬────────────────────┤
│    Customer Storefront   │     Manager Portal      │    Admin Console   │
├──────────────────────────┼─────────────────────────┼────────────────────┤
│ • Room Date Booking      │ • Room Inventory CRUD   │ • User Management  │
│ • Service Add-on Bundling│ • Booking Status Flow   │ • Manager Roles    │
│ • Travel & Tour Packages │ • Live Chat Inquiries   │ • System Settings  │
│ • Car Rental Booking     │ • Hotel Property Setup  │ • Travel Packages  │
│ • Real-Time Pusher Chat  │ • Service Catalogs      │ • Car Rental Fleet │
│ • Premium Tier Discounts │ • Operational Reports   │ • Premium Plans    │
└──────────────────────────┴─────────────────────────┴────────────────────┘
```

---

## 🏛️ System Architecture & Visual Diagrams

The application is architected following the **Laravel MVC** design pattern, complemented by WebSocket channels for real-time customer engagement:

```mermaid
graph TB
    subgraph ClientLayer["Client Layer"]
        A["Guest / Customer Interface"]
        B["Hotel Manager Dashboard"]
        C["System Admin Console"]
    end

    subgraph SecurityMiddleware["Auth & RBAC Middleware"]
        D["auth (Authentication Guard)"]
        E["role:customer / role:manager / role:admin"]
    end

    subgraph ApplicationLayer["Application Layer (Controllers)"]
        F["BookingController (Date Math & Overlap Engine)"]
        G["ChatController (Pusher Event Dispatcher)"]
        H["RoomController & HotelController"]
        I["TravelBookingController & CarController"]
        J["PremiumController (Subscription Tiers)"]
    end

    subgraph PersistenceLayer["Persistence & Real-Time Channels"]
        K[("MySQL 8.x — Relational Schema")]
        L["Pusher WebSockets — Real-Time Chat Channel"]
        M["Public Storage — Room & Hotel Media"]
    end

    ClientLayer --> SecurityMiddleware --> ApplicationLayer
    ApplicationLayer --> PersistenceLayer
```

> 📐 **Architectural Design Artifacts:** The repository includes complete SVG and Draw.io design diagrams:
> - [`hotel_booking_dfd_lvl0_corrected.svg`](./hotel_booking_dfd_lvl0_corrected.svg) — Data Flow Diagram (Level 0)
> - [`hotel_booking_dfd_lvl1_corrected.svg`](./hotel_booking_dfd_lvl1_corrected.svg) — Data Flow Diagram (Level 1)
> - [`hotel_booking_erd_corrected.svg`](./hotel_booking_erd_corrected.svg) — Entity Relationship Diagram (ERD)
> - [`laravel_mvc_structure.svg`](./laravel_mvc_structure.svg) — Laravel MVC Layered Architecture

---

## ⚖️ Engineering Decisions & Trade-offs

| Architectural Decision | Chosen Approach | Rationale & Trade-offs |
|---|---|---|
| **Real-time Live Chat** | Pusher WebSocket Channels | Avoids continuous client-side HTTP polling, reducing server request load while ensuring sub-second message delivery between guests and staff. |
| **Date-Range Conflict Engine** | SQL Range Overlap Logic | Mathematical overlap check prevents double-booking rooms during concurrent customer reservation requests. |
| **Service Add-ons** | Relational Pivot Model (`BookingServices`) | Decouples add-on charges (spa, dining, airport pickup) from base room rates, preserving itemized billing auditability. |
| **Multi-Vertical Isolation** | Separate Domain Models (`Booking`, `TravelBooking`, `CarBooking`) | Keeps distinct booking lifecycles and metadata structures modular while sharing customer authentication and payment logic. |
| **Role-Based Authorization** | Custom Role Middleware (`role:admin,manager`) | Centralizes access rules at the routing boundary rather than scattering authorization checks throughout controllers. |

---

## 🚦 Implementation Status Matrix

| Feature / Domain | Status | Technical Implementation Details |
|---|:---:|---|
| **Room Booking Engine** | ✅ **Implemented** | Date-range selection, nights calculation, and overlap prevention |
| **Add-on Services Bundling** | ✅ **Implemented** | Multi-service attachment with quantity and price snapshotting |
| **Real-Time Live Chat** | ✅ **Implemented** | Pusher WebSockets with database message persistence and unread flags |
| **3-Tier RBAC System** | ✅ **Implemented** | Role-based middleware protecting Customer, Manager, and Admin routes |
| **Travel & Tour Packages** | ✅ **Implemented** | Package catalog, date scheduling, passenger limits, and bookings |
| **Car Rental Fleet** | ✅ **Implemented** | Vehicle inventory, daily rental rates, driver options, and bookings |
| **Premium Membership Plans** | ✅ **Implemented** | Silver (5%) and Gold (10%) discount tiers automatically applied at checkout |
| **Manager Booking Workflow** | ✅ **Implemented** | Status pipeline (`Pending` → `Confirmed` → `Completed` → `Cancelled`) |
| **Admin System Dashboard** | ✅ **Implemented** | Cross-property metrics, user role management, and fleet configuration |

---

## 🧩 Core Domain Modules

### 1. 📅 Room Booking & Date Overlap Engine
- **Conflict Prevention Query:** Evaluates existing bookings for a requested room against incoming check-in/check-out dates:
  $$\text{Overlap Condition: } (\text{Existing Check-in} < \text{Requested Check-out}) \land (\text{Existing Check-out} > \text{Requested Check-in})$$
- **Dynamic Nights Calculation:** Automatically computes duration of stay and applies tiered room rates.
- **Booking Status State Machine:** `Pending` $\rightarrow$ `Confirmed` $\rightarrow$ `Completed` $\rightarrow$ `Rejected` / `Cancelled`.

### 2. 💬 Real-Time Live Chat Architecture
- **WebSockets Infrastructure:** Powered by Pusher channels with instant message delivery.
- **Message Persistence:** All exchanges between customers and hotel managers are stored in the database (`messages` table).
- **Context-Aware Chat:** Inquiries can link directly to a specific `booking_id` for instant staff reference.

### 3. 🚗 Multi-Vertical Reservation System
- **Hotel & Room Inventory:** Multi-property hotels with room types (Standard, Deluxe, Suite, Presidential) and JSON-casted amenities.
- **Holiday Travel Packages:** Curated tour itineraries with destination highlights, duration, pricing, and group capacity.
- **Car Rental Fleet:** Vehicle model, category (Sedan, SUV, Luxury), daily rate, and rental booking workflow.

### 4. 💎 Premium Subscription & Discount Engine
- **Subscription Tiers:**
  - 🥈 **Silver Tier:** 5% automatic discount on all room and service bookings.
  - 🥇 **Gold Tier:** 10% automatic discount on all room and service bookings.
- **Automated Checkout Deduction:** Validates active subscription status at the checkout boundary and recalculates net payable amounts.

---

## 🔐 Role-Based Access Control (RBAC)

```mermaid
graph TD
    A["System Admin (Super User)"] -->|Manages| B["Hotel Manager"]
    B -->|Manages| C["Hotel Staff"]
    A -->|Manages| D["All Users & Roles"]
    A -->|Configures| E["Cars & Travel Packages"]
    B -->|Manages| F["Rooms, Services & Bookings"]
    B -->|Responds to| G["Live Chat Messages"]
    H["Customer / Guest"] -->|Books| I["Rooms, Tours & Cars"]
    H -->|Chats with| B
```

| Role | Access Permissions |
|---|---|
| **Customer** | Search rooms, select dates, bundle services, book travel/cars, live chat, subscribe to premium |
| **Manager** | Room & service CRUD, booking confirmation/rejection, live chat response, operational reports |
| **Admin** | Full system control, manager account provisioning, user role management, fleet & tour management |

---

## 💡 Key Engineering Challenges & Solutions

### Challenge 1: Preventing Double-Bookings Under Concurrent Traffic
**Problem:** Multiple customers attempting to book the same room for overlapping dates simultaneously could cause double-allocations.

**Solution:** Executed booking creation inside a database transaction with a strict date overlap validation query before committing the reservation record.

---

### Challenge 2: Synchronizing Multi-Party Live Chat
**Problem:** Guests requiring instant support needed a low-latency chat interface without polling the server repeatedly.

**Solution:** Integrated Pusher WebSocket channels to broadcast message events in real time, while simultaneously storing message payloads in MySQL for permanent historical access.

---

### Challenge 3: Itemized Add-on Pricing with Discount Inheritance
**Problem:** Calculating total reservation costs with multiple dynamic add-on services (spa, meals, airport transfer) alongside tiered premium percentage discounts.

**Solution:** Modeled add-ons through a `BookingServices` pivot table that captures unit price snapshots at booking time. The pricing pipeline applies percentage discounts across both room and service line totals predictably.

---

## 🗄️ Database Schema & Entity Relationships

```
hotels
    └── rooms (JSON amenities, pricing, status)
            ├── bookings (check_in, check_out, total, status)
            │       ├── booking_services ── services (Add-ons)
            │       └── messages (Live Chat)
            └── users (roles: customer, manager, admin)
                    └── premium_subscriptions (Silver / Gold tiers)

travel_packages ── travel_bookings
cars ── car_bookings
contact_messages
```

---

## 💻 Tech Stack

| Layer | Technologies |
|---|---|
| **Backend Framework** | Laravel 12.x, PHP 8.2+ (MVC, Eloquent ORM, Middleware) |
| **Database** | MySQL 8.x (InnoDB, Foreign Key Constraints, Transactions) |
| **Real-time Engine** | Pusher WebSockets Channels |
| **Frontend UI** | Blade Templates, Bootstrap 5, Vanilla JavaScript ES6+ |
| **Authentication** | Laravel Breeze / Session-based Auth |
| **Tooling** | Composer, NPM, Vite, Artisan CLI |

---

## 🚀 Installation & Local Setup

### Prerequisites
- PHP `>= 8.2`
- Composer `>= 2.x`
- Node.js & NPM
- MySQL `>= 8.0`
- Pusher Account Credentials (for real-time chat)

### Step-by-Step Setup

1. **Clone the repository:**
   ```bash
   git clone https://github.com/ShakhawatSakib9/Hotel-Booking-Management.git
   cd Hotel-Booking-Management
   ```

2. **Install Dependencies:**
   ```bash
   composer install
   npm install
   ```

3. **Configure Environment:**
   ```bash
   cp .env.example .env
   php artisan key:generate
   ```

4. **Configure Database & Pusher in `.env`:**
   ```env
   DB_CONNECTION=mysql
   DB_HOST=127.0.0.1
   DB_PORT=3306
   DB_DATABASE=hotel_booking
   DB_USERNAME=root
   DB_PASSWORD=

   PUSHER_APP_ID=your_pusher_app_id
   PUSHER_APP_KEY=your_pusher_app_key
   PUSHER_APP_SECRET=your_pusher_app_secret
   PUSHER_APP_CLUSTER=mt1
   ```

5. **Run Migrations & Seeders:**
   ```bash
   php artisan migrate --seed
   ```

6. **Compile Assets & Launch Server:**
   ```bash
   npm run build
   php artisan serve
   ```
   - Application URL: `http://127.0.0.1:8000`

---

## 👨‍💻 Author & Contributions

**Developed by Shakhawat Sakib**  
*Full-Stack Software Engineer · Laravel · PHP · MySQL · WebSockets*

- Portfolio: [github.com/ShakhawatSakib9](https://github.com/ShakhawatSakib9)
- LinkedIn: [linkedin.com/in/shakhawat-sakib](https://linkedin.com)
