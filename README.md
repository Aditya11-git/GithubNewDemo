# 🍽️ Restaurant Management System

A full-stack restaurant management application built with React, ASP.NET Core, and SQLServer. Features include menu management, user authentication, shopping cart, order management, and admin dashboard.

## Features

### User Features
- 🔐 **User Authentication** - Register, login with JWT tokens stored in HTTP-only cookies
- 🔑 **Google OAuth** - Quick login with Google account
- 📧 **Email Verification** - Verify email address during registration
- 🔒 **Password Reset** - Forgot password functionality
- 🍕 **Browse Menu** - View dishes by categories with images and prices
- 🔍 **Search & Filter** - Find dishes quickly
- 🛒 **Shopping Cart** - Add/remove items, update quantities
- 📦 **Order Management** - Place orders and track order history
- 👤 **User Profile** - Update profile information and profile picture

### Admin Features
- 🍔 **Dish Management** - Create, update, and delete dishes (image upload supported in local mode)
- 📑 **Category Management** - Organize menu items by categories
- 📋 **Order Management** - View and manage all customer orders

## Tech Stack

### Frontend
- **React 18** - UI library
- **Redux Toolkit** - State management
- **React Router v6** - Navigation
- **Tailwind CSS** - Styling
- **Axios** - HTTP client
- **React Hook Form** - Form validation
- **React Icons** - Icon library
- **React Google OAuth** - Google authentication

### Backend
- **ASP.NET Core 8** - Web API framework
- **Entity Framework Core** - ORM
- **SQLServer** - Database
- **JWT** - Authentication

### DevOps
- **Docker** - Containerization
- **Docker Compose** - Multi-container orchestration

## Prerequisites

Before you begin, ensure you have the following installed:

- [Node.js](https://nodejs.org/) (v18 or higher)
- [.NET 8 SDK](https://dotnet.microsoft.com/download)
- [SQL Server](https://www.microsoft.com/sql-server/sql-server-downloads) (Express or Developer Edition)
- [Git](https://git-scm.com/)
- [Docker Desktop](https://www.docker.com/products/docker-desktop) (optional, for containerization)

## Getting Started
### Option 1: Run Without Docker (Recommended for Development)

#### 1. Clone the Repository

```bash
git clone https://github.com/Adityaa134/Restaurant-Project.git
cd Restaurant-Project
```

#### 2. Backend Setup

🔧 Configure appsettings.json (Local Only)
Fill values directly in appsettings.json:
```json
{
  "ConnectionStrings": {
    "DefaultConnection": "Server=localhost;Database=RestaurentDB;User Id=sa;Password=YourStrongPassword123;Encrypt=False;TrustServerCertificate=True;"
  },
  "Jwt": {
    "Key": "jwt-secret-key",
    "Issuer": "https://localhost:7219",
    "Audience": "http://localhost:5173"
  },
  "SMTPConfig": {
      "Username": "your-username",
      "SenderDisplayName": "RestaurentApp",
      "SenderAddress": "",
      "Port": "port-number",
      "Password": "your-password",
      "Host": "host"
  },
  "AllowedOrigins": {
      "OriginName": "origin-name"
  }
}
```

**⚠️ Note:** Adjust connection string based on your local SQL Server setup.

Do not commit secrets in appsettings.json.
In production, it is recommended to use Azure Key Vault for secret management.

✅ Uploading images to `wwwroot/images` works correctly in local mode

##### Run Migrations

```bash
cd src/Restaurant.WebAPI
dotnet restore
dotnet ef database update
dotnet run
```

#### Access the application
- Backend API: `https://localhost:7219`
- Swagger: `https://localhost:7219/swagger`

#### 3. Frontend Setup

##### Install Dependencies

```bash
cd src/frontend/RestaurantFrontend
npm install
```

### Google OAuth Setup (Required for Google Login)

This project uses Google OAuth for authentication.

To obtain a Google Client ID:

1. Go to https://console.cloud.google.com/
2. Create a new project (or select an existing one)
3. Navigate to **APIs & Services → Credentials**
4. Create an **OAuth Client ID**
5. Choose **Web Application**
6. Add the following:
   - Authorized JavaScript origin: `http://localhost:5173`
   - Authorized redirect URI: `http://localhost:5173`
7. Copy the generated **Client ID**

Create a .env file inside `src/frontend/RestaurantFrontend` and add:
```env
VITE_CLIENT_ID=your_google_client_id_here
```

##### Run Frontend Development Server

```bash
cd src/frontend/RestaurantFrontend
npm run dev
```

Frontend will run on `http://localhost:5173`

**✅ You can now access the application at `http://localhost:5173`**

### Option 2: Run With Docker

**⚠️ Note:** When running with Docker, you won't have direct access to `wwwroot/Images` folder for uploaded images. Use Option 1 for development if you need to manage uploaded files.

#### Prerequisites
- Docker Desktop installed and running

#### 1. Clone the Repository
```bash
git clone https://github.com/Adityaa134/Restaurant-Project.git
cd Restaurant-Project
```

#### 2. Backend Setup


📁 Environment Variables Setup


Create a .env file in the same folder as docker-compose.yml.
Example .env
```
# Database
CONNECTIONSTRINGS__DEFAULTCONNECTION=Server=sqlserver;Database=RestaurantDB;User Id=sa;Password=YourStrongPassword123;Encrypt=False;TrustServerCertificate=True
SA_PASSWORD=YourStrongPassword123

# JWT
JWT__KEY=this is a secret key for a jwt token
JWT__ISSUER=https://localhost:7219
JWT__AUDIENCE=http://localhost:5173

# SMTP
SMTPCONFIG__USERNAME=your_smtp_username
SMTPCONFIG__SENDERDISPLAYNAME=Restaurant App
SMTPCONFIG__SENDERADDRESS=no-reply@restaurant.com
SMTPCONFIG__PORT=587
SMTPCONFIG__PASSWORD=your_smtp_password
SMTPCONFIG__HOST=smtp.yourprovider.com

# CORS
ALLOWEDORIGINS__ORIGINNAME=http://localhost:5173
```

🔁 How configuration works


appsettings.json contains empty values:

```Json
"Jwt": {
  "Key": "",
  "Issuer": "",
  "Audience": ""
},
"SMTPConfig": {
    "Username": "",
    "SenderDisplayName": "",
    "SenderAddress": "",
    "Port": "",
    "Password": "",
    "Host": ""
},
"AllowedOrigins": {
    "OriginName": ""
}
```
docker-compose.yml maps environment variables:


  - Jwt__Key=${JWT__KEY}
  - Jwt__Issuer=${JWT__ISSUER}
  - Jwt__Audience=${JWT__AUDIENCE}
  - SMTPConfig__Username=${SMTPCONFIG__USERNAME}
  - SMTPConfig__SenderDisplayName=${SMTPCONFIG__SENDERDISPLAYNAME}
  - SMTPConfig__SenderAddress=${SMTPCONFIG__SENDERADDRESS}
  - SMTPConfig__Port=${SMTPCONFIG__PORT}
  - SMTPConfig__Password=${SMTPCONFIG__PASSWORD}
  - SMTPConfig__Host=${SMTPCONFIG__HOST}
  - AllowedOrigins__OriginName=${ALLOWEDORIGINS__ORIGINNAME}


Docker Compose reads values from .env.


ASP.NET Core automatically overrides appsettings.json using environment variables.

#### 3. Run with Docker Compose

```bash
# From the root directory
docker-compose up -d
```

This will start:
- SQL Server database on port `1433`
- Backend API on port `8080`

#### Access the application
- Backend API: `http://localhost:8080`
- Swagger: `http://localhost:8080/swagger`

#### Stop containers

```bash
docker-compose down

# To remove volumes (database data)
docker-compose down -v
```

## 📂 Project Structure

```
RestaurantSolution/
├── src/
│   ├── Restaurant.WebAPI/          # ASP.NET Core Web API
│   │   ├── Controllers/            # API Controllers
│   │   ├── Models/                 # Data models
│   │   ├── Services/               # Business logic
│   │   ├── Data/                   # Database context
│   │   └── appsettings.json        # Configuration
│   │
│   ├── Restaurant.Core/            # Domain models
│   ├── Restaurant.Infrastructure/  # Data access layer
│   │
│   └── frontend/
│       └── RestaurantFrontend/     # React application
│           ├── src/
│           │   ├── components/     # React components
│           │   ├── pages/          # Page components
│           │   ├── features/       # Redux slices
│           │   ├── services/       # API services
│           │   └── store/          # Redux store
│           ├── public/             # Static files
│           └── package.json
│
├── docker-compose.yml              # Docker compose configuration
├── .gitignore
└── README.md
```

## API Documentation

API documentation is available via Swagger UI when running the backend:

```
https://localhost:7219/swagger
```

### Main Endpoints

#### Authentication
- `POST /api/Account/register` - Register new user
- `POST /api/Account/login` - User login
- `POST /api/Account/google-login` - Google OAuth login
- `POST /api/Account/refresh-token` - Refresh JWT token
- `POST /api/Account/logout` - Logout user

#### Dishes
- `GET /api/Dishes` - Get all dishes
- `GET /api/Dishes/{id}` - Get dish by ID
- `POST /api/Dishes` - Add new dish (Admin)
- `PUT /api/Dishes/{id}` - Update dish (Admin)
- `DELETE /api/Dishes/{id}` - Delete dish (Admin)

#### Cart
- `GET /api/Cart` - Get user's cart
- `POST /api/Cart` - Add item to cart
- `PUT /api/Cart` - Update cart item quantity
- `DELETE /api/Cart/{id}` - Remove item from cart

#### Orders
- `GET /api/Orders` - Get user's orders
- `POST /api/Orders` - Place new order
- `GET /api/Orders/{id}` - Get order details

## Contributing

Contributions are welcome! Please follow these steps:

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/AmazingFeature`)
3. Commit your changes (`git commit -m 'Add some AmazingFeature'`)
4. Push to the branch (`git push origin feature/AmazingFeature`)
5. Open a Pull Request

## Author

**Aditya Gupta**

- GitHub: [@Adityaa134](https://github.com/Adityaa134)
- LinkedIn: [Your LinkedIn](https://linkedin.com/in/YOUR_PROFILE)
- Email: adityagupta9966@gmail.com

## Acknowledgments

- [React Documentation](https://reactjs.org/)
- [ASP.NET Core Documentation](https://docs.microsoft.com/aspnet/core)
- [Tailwind CSS](https://tailwindcss.com/)
- Icons by [React Icons](https://react-icons.github.io/react-icons/)

⭐ **If you like this project, please give it a star!** ⭐
