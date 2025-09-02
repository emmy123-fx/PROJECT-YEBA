# YEBA Design Marketplace - Database Schema

## Overview
This database schema supports a design marketplace platform where designers can sell digital designs to buyers.

## Entity Relationship Diagram

\`\`\`
Users (Base Table)
├── Designers (1:1 with Users where role='designer')
├── Buyers (1:1 with Users where role='buyer')
└── Messages (Many-to-Many self-referencing)

Designers
├── Designs (1:Many)
├── Reviews (1:Many)
└── Withdrawals (1:Many)

Designs
└── Transactions (1:Many)

Buyers → Transactions (1:Many)
Buyers → Reviews (1:Many)
\`\`\`

## Tables

### Users
- **Purpose**: Base authentication and user management
- **Key Fields**: id, name, email, password_hash, role
- **Roles**: buyer, designer, admin

### Designers
- **Purpose**: Extended profile for design sellers
- **Key Fields**: bio, portfolio_link, rating, earnings
- **Relationships**: Inherits from Users, has many Designs

### Buyers  
- **Purpose**: Extended profile for design purchasers
- **Key Fields**: extra_info (JSON for flexible data)
- **Relationships**: Inherits from Users, has many Transactions

### Designs
- **Purpose**: Marketplace products/listings
- **Key Fields**: title, description, category, price, file_url, watermarked_preview_url
- **Relationships**: Belongs to Designer, has many Transactions

### Transactions
- **Purpose**: Purchase records and payment tracking
- **Key Fields**: amount, payment_status, payment_method
- **Relationships**: Links Buyers to Designs

### Reviews
- **Purpose**: Designer ratings and feedback
- **Key Fields**: rating (1-5), comment
- **Relationships**: Links Buyers to Designers

### Messages
- **Purpose**: User-to-user communication
- **Key Fields**: sender_id, receiver_id, message, timestamp
- **Relationships**: Self-referencing Users table

### Withdrawals
- **Purpose**: Designer earnings withdrawal requests
- **Key Fields**: amount, status (pending/approved/rejected)
- **Relationships**: Belongs to Designer

## Key Features

### Automated Calculations
- Designer ratings automatically update when reviews are added/modified
- Designer earnings automatically increase when transactions complete

### Data Integrity
- Foreign key constraints ensure referential integrity
- Check constraints validate data ranges (ratings 1-5, positive amounts)
- Unique constraints prevent duplicate purchases and reviews

### Performance Optimization
- Strategic indexes on frequently queried fields
- Composite indexes for complex queries (conversations, etc.)

### Security Considerations
- Password hashing (implementation required in application layer)
- Role-based access control through user roles
- Cascade deletes to maintain data consistency

## Usage Notes

1. Run migration files in numerical order (001-010)
2. Triggers automatically maintain calculated fields
3. Sample data provided for testing
4. Designed for PostgreSQL but compatible with MySQL with minor adjustments
