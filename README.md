# Equipment and Library Management System

A role-based web application built with Laravel for managing equipment or library items. Admins manage the inventory, and members can browse items, borrow them and return them. The app tracks due dates and shows overdue loans. It also provides a REST API for items and loans.

> Built as a learning project covering the full development cycle: requirement definition, database design, implementation, testing and delivery.

---

## Features

- **Authentication with two roles:** admin and member
- **Item management (admin):** add, edit, delete items, with categories
- **Search and filter:** find items by name, category or availability
- **Borrow and return:** members borrow available items, and a due date is set automatically
- **Overdue tracking:** admins can see all overdue loans
- **Loan history:** each member can see their own past and current loans
- **REST API:** JSON endpoints for items and loans (see below)
- **Automated tests:** PHPUnit feature tests for borrowing rules, authentication and access control
- **One-command setup:** runs in Docker using Laravel Sail

## Tech Stack

| Area | Technology |
|---|---|
| Backend | PHP, Laravel |
| Database | MySQL |
| Frontend | Blade templates, HTML, CSS, JavaScript |
| Environment | Docker (Laravel Sail) |
| Testing | PHPUnit |
| Version control | Git, GitHub |

## Screenshots

> Add 3 or 4 screenshots to a `docs/screenshots/` folder and update the paths below.

| Login | Item list | Borrow page | Overdue list |
|---|---|---|---|
| ![Login](docs/screenshots/login.png) | ![Items](docs/screenshots/items.png) | ![Borrow](docs/screenshots/borrow.png) | ![Overdue](docs/screenshots/overdue.png) |

## Database Design

Main tables:

| Table | Purpose |
|---|---|
| `users` | Accounts, with a `role` column (`admin` or `member`) |
| `categories` | Item categories |
| `items` | Equipment or books, with a category and availability status |
| `loans` | Borrow records: user, item, borrowed date, due date, returned date |

Relationships:

- A category has many items.
- A user has many loans.
- An item has many loans.

> Add an ER diagram at `docs/er-diagram.png` and link it here.

## Getting Started

### Requirements

- Ubuntu (or any Linux system)
- Docker Engine and Docker Compose plugin
- Git

Check that Docker works without `sudo`:

```bash
docker --version
docker compose version
```

If you get a permission error, add your user to the docker group, then log out and log in again:

```bash
sudo usermod -aG docker $USER
```

### Run the project

```bash
# 1. Clone the repository
git clone https://github.com/ATHARVA-87/YOUR-REPO-NAME.git
cd YOUR-REPO-NAME

# 2. Create the environment file
cp .env.example .env

# 3. Install PHP dependencies (using a temporary Docker container)
docker run --rm \
    -u "$(id -u):$(id -g)" \
    -v "$(pwd):/var/www/html" \
    -w /var/www/html \
    laravelsail/php84-composer:latest \
    composer install --ignore-platform-reqs

# 4. Start the containers
./vendor/bin/sail up -d

# 5. Generate the app key, create tables and add sample data
./vendor/bin/sail artisan key:generate
./vendor/bin/sail artisan migrate --seed
```

The application is now available at **http://localhost**.

> If the `laravelsail/php84-composer` image tag does not exist, check the current PHP version in the Laravel documentation and use the matching tag.

### Sample accounts

Created by the database seeder:

| Role | Email | Password |
|---|---|---|
| Admin | admin@example.com | password |
| Member | member@example.com | password |

> Change these in `database/seeders` if you use different values.

### Stop the project

```bash
./vendor/bin/sail down
```

## Running Tests

```bash
./vendor/bin/sail test
```

The tests cover:

- Only admins can add, edit or delete items
- A member cannot borrow an item that is already on loan
- Returning an item makes it available again
- Overdue loans are detected correctly
- Unauthenticated users cannot access protected pages or API routes

## API Endpoints

Base URL: `http://localhost/api`

| Method | Endpoint | Description | Access |
|---|---|---|---|
| GET | `/items` | List items (supports search and category filter) | Authenticated |
| GET | `/items/{id}` | Get one item | Authenticated |
| POST | `/items` | Create an item | Admin |
| PUT | `/items/{id}` | Update an item | Admin |
| DELETE | `/items/{id}` | Delete an item | Admin |
| GET | `/loans` | List loans (own loans for members, all for admins) | Authenticated |
| POST | `/loans` | Borrow an item | Member |
| PATCH | `/loans/{id}/return` | Return an item | Member or Admin |

> Update this table to match the routes you actually build (`./vendor/bin/sail artisan route:list`).

## Project Structure

```
app/
  Http/Controllers/    Web and API controllers
  Models/              User, Item, Category, Loan
database/
  migrations/          Table definitions
  seeders/             Sample data
resources/views/       Blade templates
routes/
  web.php              Web routes
  api.php              API routes
tests/Feature/         PHPUnit feature tests
docs/                  Screenshots and diagrams
```

## What I Learned

- Designing a relational schema in MySQL and writing migrations
- Building a full application in Laravel (routing, controllers, Eloquent, Blade)
- Role-based access control
- Designing and testing a REST API
- Running a development environment with Docker
- Writing feature tests for business rules

## Future Improvements

- Email reminders for due and overdue loans
- Reservation queue for items that are on loan
- Pagination and CSV export for the admin pages
- API token authentication

## Author

**Atharva Thombare**
B.Tech, Materials Science and Engineering, IIT Mandi
GitHub: [ATHARVA-87](https://github.com/ATHARVA-87)

## License

This project is for learning purposes. You may use the code under the MIT License.
