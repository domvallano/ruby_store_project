# Ruby Store

A test Rails e-commerce application that allows users to browse products, manage their accounts, and subscribe to product notifications.

## Overview

Ruby Store is a full-featured e-commerce platform built with Rails 8.1. Users can browse a catalog of products, create accounts, manage passwords, and subscribe to product notifications. When a product comes back in stock, subscribed users receive email notifications automatically.

### Key Features

- **Product Catalog**: Browse and view products with rich descriptions and featured images
- **User Authentication**: Secure user registration and login with password management
- **Product Subscriptions**: Subscribe to out-of-stock products and receive email notifications when they're back in stock
- **Session Management**: Persistent user sessions with secure cookies
- **Email Notifications**: Automated emails for product restocks and password resets
- **Image Management**: Upload and manage product images with Active Storage
- **Rich Text Support**: Create detailed product descriptions with rich text editing

## Requirements

- **Ruby**: 3.3+ (as specified in Rails 8.1 requirements)
- **Rails**: 8.1.3 or later
- **Database**: SQLite3 (default), configurable for production databases
- **Node.js**: For asset pipeline and JavaScript compilation
- **Bundler**: For gem dependency management

## Installation

### 1. Clone the Repository

```bash
git clone <repository-url>
cd ruby_store_project
```

### 2. Install Dependencies

```bash
bundle install
```

### 3. Setup Database

```bash
# Create database and run migrations
bin/rails db:create
bin/rails db:migrate

# Load seed data (optional)
bin/rails db:seed
```

### 4. Prepare Assets

```bash
bin/rails assets:precompile
```

## Configuration

### Environment Variables

Create a `.env` file in the project root (optional, for local development):

```env
# Database configuration is in config/database.yml
# RAILS_ENV defaults to development
```

### Credentials

The application uses Rails encrypted credentials for sensitive information:

```bash
# Edit encrypted credentials
bin/rails credentials:edit
```

### Email Configuration

Configure email sending in `config/environments/production.rb` or `config/environments/development.rb`:

```ruby
config.action_mailer.delivery_method = :smtp
config.action_mailer.smtp_settings = {
  host: "smtp.example.com",
  port: 587,
  user_name: "your-email@example.com",
  password: "your-password"
}
```

## Running the Application

### Development Server

```bash
bin/rails server
# or using Puma directly
bin/puma
```

The application will be available at `http://localhost:3000`

### Background Jobs

The application uses Solid Queue for background jobs:

```bash
bin/rails jobs:work
```

### Background Cache and Cable

```bash
# Start cache service
bin/solid_cache

# Start cable service
bin/solid_cable
```

## Database Schema

### Models

- **Product**: E-commerce products with inventory tracking
  - `name`: Product name
  - `description`: Rich text product description
  - `featured_image`: Product image (Active Storage)
  - `inventory_count`: Current stock quantity
  - Relationships: `has_many :subscribers`

- **User**: User accounts with secure authentication
  - `email_address`: Unique email for login
  - `password_digest`: Encrypted password (bcrypt)
  - Relationships: `has_many :sessions`

- **Session**: User login sessions
  - `user_id`: Associated user
  - Session management for authentication

- **Subscriber**: Product subscription tracking
  - `product_id`: Subscribed product
  - `email_address`: Subscriber's email
  - Auto-generates unsubscribe tokens

### Automatic Features

- **Product Restocking Notifications**: When a product's inventory changes from 0 to positive, all subscribers receive an email notification
- **Password Recovery**: Users can reset forgotten passwords via email

## Testing

Run the test suite:

```bash
# Run all tests
bin/rails test

# Run specific test file
bin/rails test test/models/product_test.rb

# Run system tests
bin/rails test:system
```

## Code Quality & Security

### Linting & Static Analysis

```bash
# Run RuboCop (code style)
bundle exec rubocop

# Run Brakeman (security audit)
bundle exec brakeman

# Check gems for vulnerabilities
bundle exec bundler-audit
```

### Debugging

The application includes the `debug` gem for interactive debugging:

```ruby
binding.break  # or debugger in code
```

## Deployment

### Docker

The application includes a Dockerfile for containerization:

```bash
# Build Docker image
docker build -t ruby-store .

# Run container
docker run -p 3000:3000 ruby-store
```

### Kamal Deployment

Deploy using Kamal (requires configuration):

```bash
# Setup deployment
kamal setup

# Deploy application
kamal deploy
```

See [deploy.yml](config/deploy.yml) for deployment configuration.

## Project Structure

```
app/
  models/          # Application models (Product, User, etc.)
  controllers/     # Request handlers
  views/          # View templates
  mailers/        # Email templates and logic
  jobs/           # Background jobs
  helpers/        # View helpers
  channels/       # WebSocket channels
  assets/         # Images and stylesheets
  javascript/     # Frontend JavaScript

config/
  routes.rb       # Application routes
  database.yml    # Database configuration
  deploy.yml      # Kamal deployment config
  credentials.yml.enc  # Encrypted secrets

db/
  migrate/        # Database migrations
  schema.rb       # Database schema
  seeds.rb        # Seed data

test/             # Test suite
```

## Key Routes

- `GET /` - Product listing (home page)
- `GET /products/:id` - Product detail page
- `POST /sessions` - User login
- `DELETE /sessions` - User logout
- `GET /passwords/new` - Request password reset
- `POST /passwords` - Create password reset
- `GET /passwords/:token` - Reset password form
- `PATCH /passwords/:token` - Update password
- `POST /products/:id/subscribers` - Subscribe to product
- `GET /unsubscribe?token=:token` - Unsubscribe from product

## Architecture

The application follows Rails conventions and best practices:

- **MVC Pattern**: Separation of models, views, and controllers
- **RESTful API**: Standard HTTP methods for resource operations
- **ActiveRecord ORM**: Object-relational mapping for database interactions
- **Action Cable**: Real-time features (configured but optional to enable)
- **Active Storage**: Cloud-agnostic file storage for product images
- **Solid Queue**: Job queuing for background tasks
- **Solid Cache**: Cache storage backend
- **Turbo & Stimulus**: Frontend interactivity without heavy JavaScript

## Development

### Creating Migrations

```bash
bin/rails generate migration AddPriceToProducts price:decimal
bin/rails db:migrate
```

### Generating Models, Controllers, etc.

```bash
# Generate a model
bin/rails generate model ProductReview

# Generate a controller
bin/rails generate controller Reviews

# Generate full scaffold
bin/rails generate scaffold Review product:references rating:integer
```

## Troubleshooting

### Database Issues

```bash
# Reset database (development only)
bin/rails db:reset

# Drop and recreate
bin/rails db:drop db:create db:migrate
```

### Asset Compilation Issues

```bash
# Clear compiled assets
bin/rails assets:clobber

# Recompile
bin/rails assets:precompile
```

### Dependency Issues

```bash
# Update and reinstall gems
bundle update
bundle install
```

## Contributing

1. Create a feature branch (`git checkout -b feature/amazing-feature`)
2. Commit changes (`git commit -m 'Add amazing feature'`)
3. Push to branch (`git push origin feature/amazing-feature`)
4. Open a Pull Request

## License

[Add appropriate license information here]

## Support

For issues, questions, or suggestions, please open an issue on the project repository.
