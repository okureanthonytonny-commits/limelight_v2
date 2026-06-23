# Limelight v2
Limelight v2 is the backend service for an e-commerce platform built with FastAPI. It provides a RESTful API for user authentication (via Google OAuth), product catalog management, shopping cart operations, and order processing.
## ✨ Features
 * **🔐 Google OAuth Authentication** – Secure, session-based login using HttpOnly cookies for enhanced security.
 * **🛍️ Product Management** – Full CRUD operations with soft delete functionality (allows admins to restore items).
 * **🛒 Shopping Cart** – Add, update, and remove items with real-time stock validation.
 * **📦 Order Processing** – Create orders from cart items with price snapshots and status tracking.
 * **👑 Role-Based Access Control** – Separation of customer and admin roles for secure endpoint access.
 * **🧪 Comprehensive Testing** – Module-level tests using pytest with an in-memory SQLite database.
## 🛠️ Tech Stack
| Component | Technology |
|---|---|
| **Backend** | FastAPI (Python 3.12+), SQLModel (ORM), Pydantic v2 |
| **Database** | PostgreSQL (Production), SQLite (Testing) |
| **Migrations** | Alembic |
| **Authentication** | Google OAuth, session cookies (HttpOnly) |
| **Testing** | pytest |
| **Deployment** | Render (Backend), Vercel (Frontend – Planned) |
| **Container** | Docker (Development) |
## 📂 Project Structure
```text
backend/
├── app/
│   ├── core/                 # Core configuration & shared utilities
│   │   ├── config.py         # Pydantic settings (env vars)
│   │   ├── database.py       # Database session & engine
│   │   └── dependencies.py   # Auth & admin dependency injections
│   ├── modules/              # Feature modules (self-contained)
│   │   ├── auth/             # User, session, OAuth
│   │   ├── products/         # Product CRUD, soft delete
│   │   ├── cart/             # Cart items, stock validation
│   │   └── orders/           # Order creation, status transitions
│   ├── main.py               # FastAPI app entry point
│   └── conftest.py           # pytest configuration
├── alembic/                  # Database migration scripts
├── .env                      # Environment variables (git-ignored)
├── alembic.ini               # Alembic configuration
└── .conda                    # Conda environment settings

```
> **Note:** Each module follows a consistent pattern containing models.py, schemas.py, services.py, router.py, and a tests/ subdirectory.
> 
## 🚀 Getting Started
### Prerequisites
 * Python 3.12+
 * PostgreSQL (or SQLite for development)
 * Google OAuth Credentials (Client ID & Secret)
### Setup Instructions
 1. **Clone the Repository**
   ```bash
   git clone https://github.com/okureanthonytonny-commits/limelight_v2.git
   cd limelight_v2/backend
   
   ```
```

2. **Set Up a Virtual Environment**
   ```bash
   python -m venv venv
   source venv/bin/activate  # On Windows use: venv\Scripts\activate

```
 3. **Install Dependencies**
   ```bash
   
   ```
pip install -r requirements.txt
```
   *If requirements.txt is missing, install the core packages manually:*
   ```bash
   pip install fastapi sqlmodel psycopg2-binary alembic authlib pydantic-settings starlette uvicorn pytest

```
 4. **Configure Environment Variables**
   Create a .env file in the backend/ directory:
   ```env
   DATABASE_URL=postgresql://user:password@localhost:5432/limelight_v2
   GOOGLE_CLIENT_ID=your_google_client_id
   GOOGLE_CLIENT_SECRET=your_google_client_secret
   SECRET_KEY=your_secret_key_here
   OAUTH_REDIRECT_URI=http://localhost:8000/auth/callback
   
   ```
```

5. **Run Database Migrations**
   ```bash
alembic upgrade head

```
 6. **Start the Development Server**
   ```bash
   uvicorn app.main:app --reload
   
   ```
```
   - **API URL:** http://localhost:8000
   - **Interactive Docs:** http://localhost:8000/docs

---

## 🧪 Running Tests

Run all module tests from the backend/ directory:
```bash
pytest app/modules/ -v

```
*Note: Tests are completely isolated and utilize an in-memory SQLite database.*
## 🗄️ Database Schema
Key models implemented via **SQLModel**:
 * **User** – id, email, name, oauth_provider, oauth_id, role
 * **Product** – id, name, description, price, stock, image_url, deleted_at *(soft delete)*
 * **CartItem** – id, user_id, product_id, quantity
 * **Order** – id, user_id, status
 * **OrderItem** – id, order_id, product_id, quantity, price_snapshot
> **Design Note:** Models do not contain cross-module relationships (e.g., no products relationship directly inside OrderItem). Data enrichment is explicitly handled in the routers to avoid circular imports.
> 
## 🚢 Deployment & Production
 * Designed for deployment on **Render** with a **PostgreSQL** database.
 * A docker-compose.yml file is provided for local database containerization.
 * **Production Checklist:**
   * Set secure=True for session cookies.
   * Enforce HTTPS.
   * Configure accurate CORS origins within main.py.
## 🤝 Contributing
 1. Fork the repository.
 2. Create your feature branch (git checkout -b feature/amazing-feature).
 3. Commit your changes (git commit -m 'Add some amazing feature').
 4. Push to the branch (git push origin feature/amazing-feature).
 5. Open a Pull Request.
