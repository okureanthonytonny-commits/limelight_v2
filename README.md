Limelight v2

Limelight v2 is the backend service for an e-commerce platform built with FastAPI. It provides a RESTful API for user authentication (via Google OAuth), product catalog management, shopping cart operations, and order processing.

✨ Features

· 🔐 Google OAuth Authentication – Secure, session-based login with Google, using HttpOnly cookies for enhanced security.
· 🛍️ Product Management – Full CRUD operations with soft delete, allowing admins to restore deleted items.
· 🛒 Shopping Cart – Add, update, and remove items with real-time stock validation.
· 📦 Order Processing – Create orders from cart items with price snapshots and status tracking.
· 👑 Role-Based Access Control – Distinguish between customer and admin roles for secure endpoint access.
· 🧪 Comprehensive Testing – Module-level tests using pytest with an in-memory SQLite database.

🛠️ Tech Stack

Component Technology
Backend FastAPI (Python 3.12), SQLModel (ORM), Pydantic v2
Database PostgreSQL (production), SQLite (testing)
Migrations Alembic
Authentication Google OAuth, session cookies (HttpOnly)
Testing pytest (module-level tests)
Deployment Render (backend), Vercel (frontend – planned)
Container Docker (development)

📂 Project Structure

```
backend/
├── app/
│   ├── core/                 # Core configuration & shared utilities
│   │   ├── config.py         # Pydantic settings (env vars)
│   │   ├── database.py       # Database session & engine
│   │   └── dependencies.py   # Auth & admin dependency injections
│   ├── modules/              # Feature modules (each is self-contained)
│   │   ├── auth/             # User, session, OAuth
│   │   ├── products/         # Product CRUD, soft delete
│   │   ├── cart/             # Cart items, stock validation
│   │   └── orders/           # Order creation, status transitions
│   ├── main.py               # FastAPI app entry point
│   └── conftest.py           # pytest configuration
├── alembic/                  # Database migration scripts
├── .env                      # Environment variables (not committed)
├── alembic.ini               # Alembic configuration
└── .conda                    # Conda environment name
```

Each module follows a consistent pattern with models.py, schemas.py, services.py, router.py, and a tests/ subdirectory.

🚀 Getting Started

Prerequisites

· Python 3.12+
· PostgreSQL (or SQLite for development/testing)
· Google OAuth credentials (Client ID & Secret)

1. Clone the Repository

```bash
git clone https://github.com/okureanthonytonny-commits/limelight_v2.git
cd limelight_v2/backend
```

2. Set Up a Virtual Environment

```bash
python -m venv venv
source venv/bin/activate      # On Windows: venv\Scripts\activate
```

3. Install Dependencies

```bash
pip install -r requirements.txt
```

Note: If requirements.txt is not present, install the core packages:

```bash
pip install fastapi sqlmodel psycopg2-binary alembic authlib pydantic-settings starlette uvicorn
```

4. Configure Environment Variables

Create a .env file in the backend/ directory with the following variables:

```env
DATABASE_URL=postgresql://user:password@localhost:5432/limelight_v2
GOOGLE_CLIENT_ID=your_google_client_id
GOOGLE_CLIENT_SECRET=your_google_client_secret
SECRET_KEY=your_secret_key_here
OAUTH_REDIRECT_URI=http://localhost:8000/auth/callback
```

All configuration is managed via Pydantic's BaseSettings.

5. Run Database Migrations

```bash
alembic upgrade head
```

6. Start the Development Server

```bash
uvicorn app.main:app --reload
```

The API will be available at http://localhost:8000. You can view the interactive API docs at http://localhost:8000/docs.

🧪 Running Tests

Run all module tests from the backend/ directory:

```bash
pytest app/modules/ -v
```

Tests are isolated using an in-memory SQLite database.

🗄️ Database Schema

Key models (SQLModel) include:

· User – id, email, name, oauth_provider, oauth_id, role
· Product – id, name, description, price, stock, image_url, deleted_at (soft delete)
· CartItem – id, user_id, product_id, quantity
· Order – id, user_id, status (pending, etc.)
· OrderItem – id, order_id, product_id, quantity, price_snapshot

Design Note: Models do not contain cross-module relationships (e.g., no products relationship in OrderItem). Data enrichment is handled explicitly in routers to avoid circular imports.

🚢 Deployment

The backend is designed for deployment on Render with a PostgreSQL database. A docker-compose.yml is available for local development with a containerized database.

For production, ensure you:

· Set secure=True for the session cookie
· Use HTTPS
· Configure proper CORS origins in main.py

🤝 Contributing

1. Fork the repository
2. Create a feature branch (git checkout -b feature/amazing-feature)
3. Commit your changes (git commit -m 'Add some amazing feature')
4. Push to the branch (git push origin feature/amazing-feature)
5. Open a Pull Request

📄 License

This project is licensed under the MIT License – see the LICENSE file for details (if applicable).

🙏 Acknowledgements

· Built with FastAPI
· Authentication powered by Authlib & Google OAuth
· ORM via SQLModel
