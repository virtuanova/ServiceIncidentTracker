# SIT (Support Incident Tracker) - Laravel/Vue.js Migration Guide

## Overview

This migration transforms the legacy PHP SIT application into a modern stack using:
- **Backend**: Laravel with REST API architecture
- **Frontend**: Vue.js 3 with TypeScript SPA
- **Database**: MySQL (backward compatible with existing structure)

## Core Features Migrated

Based on the legacy code analysis, SIT is a support ticket management system with these key features:

### Primary Features
1. **Incident Management** - Create, update, assign, and close support tickets
2. **Contact Management** - Manage customer contacts and their details
3. **Site Management** - Customer sites and locations
4. **Contract Management** - Maintenance contracts and service levels
5. **User Management** - Support engineers and permissions
6. **Dashboard** - Statistics and activity overview

### Secondary Features
1. **Product Catalog** - Software products and versions
2. **Knowledge Base** - Articles and solutions
3. **Billing System** - Time tracking and invoicing
4. **Holiday Management** - Staff holiday tracking
5. **Tasks** - Internal task management
6. **Email Integration** - Automated email notifications
7. **Reporting** - Various reports and statistics

## Database Compatibility

The new Laravel application is designed to work with the existing database structure:

### Key Tables Maintained
- `users` - System users/engineers
- `incidents` - Support tickets
- `contacts` - Customer contacts
- `sites` - Customer sites
- `maintenance` - Service contracts
- `products` - Software products
- `updates` - Incident updates/notes
- `tasks` - Internal tasks

### Laravel Models Created
- `User` - Maps to existing `users` table
- `Incident` - Maps to existing `incidents` table
- `Contact` - Maps to existing `contacts` table
- `Site` - Maps to existing `sites` table
- `Maintenance` - Maps to existing `maintenance` table (contracts)
- `Product` - Maps to existing `products` table

## API Endpoints

### Authentication
- `POST /api/login` - User authentication
- `POST /api/logout` - User logout
- `GET /api/user` - Get current user
- `POST /api/refresh` - Refresh token

### Incidents
- `GET /api/incidents` - List incidents (with filtering)
- `POST /api/incidents` - Create incident
- `GET /api/incidents/{id}` - Get incident details
- `PUT /api/incidents/{id}` - Update incident
- `POST /api/incidents/{id}/close` - Close incident
- `POST /api/incidents/{id}/reassign` - Reassign incident

### Contacts
- `GET /api/contacts` - List contacts
- `POST /api/contacts` - Create contact
- `GET /api/contacts/{id}` - Get contact details
- `PUT /api/contacts/{id}` - Update contact
- `DELETE /api/contacts/{id}` - Delete contact

### Dashboard
- `GET /api/dashboard/stats` - Get dashboard statistics
- `GET /api/dashboard/recent-incidents` - Get recent incidents
- `GET /api/dashboard/my-incidents` - Get user's incidents

## Frontend Structure

### Views Created
1. **LoginView.vue** - Authentication page
2. **DashboardView.vue** - Main dashboard with statistics
3. **IncidentsView.vue** - Incident listing (to be created)
4. **ContactsView.vue** - Contact management (to be created)
5. **SitesView.vue** - Site management (to be created)

### State Management
- **Pinia Store** for authentication state
- **Axios** for API communication with automatic token handling

## Authentication

- Uses Laravel Sanctum for API authentication
- Token-based authentication compatible with SPA
- Automatic token refresh capability
- Route guards for protected pages

## Key Compatibility Notes

1. **Password Hashing**: Laravel uses bcrypt by default. Existing passwords may need migration if using different hashing.

2. **Timestamps**: Legacy app doesn't use Laravel's automatic timestamps. Models configured with `public $timestamps = false;`

3. **Table Prefixes**: Can be configured in Laravel if the existing database uses table prefixes.

4. **Primary Keys**: All models assume `id` as primary key. Adjust if different in legacy database.

## Migration Steps

### 1. Backend Setup
```bash
cd backend
composer install
cp .env.example .env
# Configure database settings in .env
php artisan key:generate
php artisan migrate:fresh --seed  # Only if creating fresh database
```

### 2. Frontend Setup
```bash
cd frontend
npm install
npm run dev
```

### 3. Database Configuration
Update `.env` file in backend:
```
DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_PORT=3306
DB_DATABASE=sit_database
DB_USERNAME=root
DB_PASSWORD=your_password
```

### 4. CORS Configuration
The backend is configured to allow requests from Vue.js development server (localhost:5173).

## Gradual Migration Strategy

1. **Phase 1**: Deploy new API alongside legacy application
2. **Phase 2**: Migrate one module at a time (start with dashboard)
3. **Phase 3**: Gradually replace legacy pages with Vue.js components
4. **Phase 4**: Decommission legacy application

## Additional Features to Implement

1. **Update Management**: Add/edit incident updates
2. **File Attachments**: File upload for incidents
3. **Email Templates**: Notification system
4. **Service Level Management**: SLA tracking
5. **Reporting Module**: Charts and reports
6. **User Permissions**: Role-based access control
7. **Knowledge Base**: Article management
8. **Billing Module**: Time tracking and invoicing

## Security Considerations

1. **Input Validation**: All API endpoints use Laravel validation
2. **SQL Injection**: Eloquent ORM prevents SQL injection
3. **XSS Protection**: Vue.js automatically escapes output
4. **CSRF Protection**: Not needed for API-only backend
5. **Authentication**: Sanctum provides secure token-based auth

## Performance Improvements

1. **API Pagination**: All list endpoints support pagination
2. **Eager Loading**: Related data loaded efficiently
3. **Indexing**: Database indexes can be added without breaking compatibility
4. **Caching**: Laravel caching can be implemented
5. **API Rate Limiting**: Built-in Laravel rate limiting

## Deployment Notes

1. **Web Server**: Both applications can run on same server
2. **Database**: Shared MySQL database
3. **File Storage**: Laravel can use existing file storage structure
4. **Environment**: Separate .env files for different environments

This migration provides a solid foundation for modernizing SIT while maintaining backward compatibility with the existing database structure.