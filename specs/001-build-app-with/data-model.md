# Data Model: Condo Management Application

**Date**: 2024-12-19  
**Feature**: Condo Management Web Application  
**Database**: SQLite with SQLx

## Entity Definitions

### Resident
Represents a condo resident with personal information and associated vehicles.

**Fields**:
- `id`: INTEGER PRIMARY KEY (auto-increment)
- `name`: TEXT NOT NULL (resident's full name)
- `address`: TEXT NOT NULL (unit number and building address)
- `phone`: TEXT (optional contact number)
- `email`: TEXT (optional email address)
- `created_at`: DATETIME DEFAULT CURRENT_TIMESTAMP
- `updated_at`: DATETIME DEFAULT CURRENT_TIMESTAMP

**Validation Rules**:
- Name must be non-empty and at least 2 characters
- Address must be non-empty and contain unit number
- Phone must be valid format if provided (optional)
- Email must be valid format if provided (optional)

**Relationships**:
- One-to-many with Vehicle (a resident can have multiple vehicles)
- One-to-many with Guest (a resident can register multiple guests)

### Vehicle
Represents a car owned by a resident.

**Fields**:
- `id`: INTEGER PRIMARY KEY (auto-increment)
- `resident_id`: INTEGER NOT NULL (foreign key to Resident)
- `license_plate`: TEXT NOT NULL (vehicle license plate)
- `make`: TEXT (vehicle manufacturer)
- `model`: TEXT (vehicle model)
- `color`: TEXT (vehicle color)
- `year`: INTEGER (vehicle year)
- `created_at`: DATETIME DEFAULT CURRENT_TIMESTAMP

**Validation Rules**:
- License plate must be unique within the same resident
- License plate must be non-empty and valid format
- Make and model are optional but recommended
- Year must be valid if provided (1900-2030)

**Relationships**:
- Many-to-one with Resident (belongs to one resident)

### Guest
Represents a temporary visitor with personal information.

**Fields**:
- `id`: INTEGER PRIMARY KEY (auto-increment)
- `resident_id`: INTEGER NOT NULL (foreign key to Resident)
- `name`: TEXT NOT NULL (guest's full name)
- `phone`: TEXT (optional contact number)
- `valid_until`: DATETIME NOT NULL (permit expiration time)
- `created_at`: DATETIME DEFAULT CURRENT_TIMESTAMP
- `updated_at`: DATETIME DEFAULT CURRENT_TIMESTAMP

**Validation Rules**:
- Name must be non-empty and at least 2 characters
- Phone must be valid format if provided (optional)
- Valid_until must be in the future
- Valid_until defaults to 12 hours from creation

**Relationships**:
- Many-to-one with Resident (registered by one resident)
- One-to-many with GuestVehicle (can have multiple vehicles)

### GuestVehicle
Represents a vehicle belonging to a guest.

**Fields**:
- `id`: INTEGER PRIMARY KEY (auto-increment)
- `guest_id`: INTEGER NOT NULL (foreign key to Guest)
- `license_plate`: TEXT NOT NULL (vehicle license plate)
- `make`: TEXT (vehicle manufacturer)
- `model`: TEXT (vehicle model)
- `color`: TEXT (vehicle color)
- `year`: INTEGER (vehicle year)
- `created_at`: DATETIME DEFAULT CURRENT_TIMESTAMP

**Validation Rules**:
- License plate must be unique within the same guest
- License plate must be non-empty and valid format
- Make and model are optional but recommended
- Year must be valid if provided (1900-2030)

**Relationships**:
- Many-to-one with Guest (belongs to one guest)

### ParkingPermit
Represents a generated parking permit for a guest.

**Fields**:
- `id`: INTEGER PRIMARY KEY (auto-increment)
- `guest_id`: INTEGER NOT NULL (foreign key to Guest)
- `permit_number`: TEXT NOT NULL (unique permit identifier)
- `valid_from`: DATETIME NOT NULL (permit start time)
- `valid_until`: DATETIME NOT NULL (permit end time)
- `printed_at`: DATETIME (when permit was printed)
- `created_at`: DATETIME DEFAULT CURRENT_TIMESTAMP

**Validation Rules**:
- Permit number must be unique across all permits
- Valid_from must be in the past or present
- Valid_until must be in the future
- Valid_until must be after valid_from

**Relationships**:
- Many-to-one with Guest (belongs to one guest)

## Database Schema

```sql
-- Residents table
CREATE TABLE residents (
    id INTEGER PRIMARY KEY AUTOINCREMENT,
    name TEXT NOT NULL CHECK(length(name) >= 2),
    address TEXT NOT NULL CHECK(length(address) > 0),
    phone TEXT CHECK(phone IS NULL OR length(phone) > 0),
    email TEXT CHECK(email IS NULL OR email LIKE '%@%'),
    created_at DATETIME DEFAULT CURRENT_TIMESTAMP,
    updated_at DATETIME DEFAULT CURRENT_TIMESTAMP
);

-- Vehicles table
CREATE TABLE vehicles (
    id INTEGER PRIMARY KEY AUTOINCREMENT,
    resident_id INTEGER NOT NULL,
    license_plate TEXT NOT NULL CHECK(length(license_plate) > 0),
    make TEXT,
    model TEXT,
    color TEXT,
    year INTEGER CHECK(year IS NULL OR (year >= 1900 AND year <= 2030)),
    created_at DATETIME DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (resident_id) REFERENCES residents(id) ON DELETE CASCADE,
    UNIQUE(resident_id, license_plate)
);

-- Guests table
CREATE TABLE guests (
    id INTEGER PRIMARY KEY AUTOINCREMENT,
    resident_id INTEGER NOT NULL,
    name TEXT NOT NULL CHECK(length(name) >= 2),
    phone TEXT CHECK(phone IS NULL OR length(phone) > 0),
    valid_until DATETIME NOT NULL CHECK(valid_until > datetime('now')),
    created_at DATETIME DEFAULT CURRENT_TIMESTAMP,
    updated_at DATETIME DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (resident_id) REFERENCES residents(id) ON DELETE CASCADE
);

-- Guest vehicles table
CREATE TABLE guest_vehicles (
    id INTEGER PRIMARY KEY AUTOINCREMENT,
    guest_id INTEGER NOT NULL,
    license_plate TEXT NOT NULL CHECK(length(license_plate) > 0),
    make TEXT,
    model TEXT,
    color TEXT,
    year INTEGER CHECK(year IS NULL OR (year >= 1900 AND year <= 2030)),
    created_at DATETIME DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (guest_id) REFERENCES guests(id) ON DELETE CASCADE,
    UNIQUE(guest_id, license_plate)
);

-- Parking permits table
CREATE TABLE parking_permits (
    id INTEGER PRIMARY KEY AUTOINCREMENT,
    guest_id INTEGER NOT NULL,
    permit_number TEXT NOT NULL UNIQUE,
    valid_from DATETIME NOT NULL,
    valid_until DATETIME NOT NULL CHECK(valid_until > valid_from),
    printed_at DATETIME,
    created_at DATETIME DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (guest_id) REFERENCES guests(id) ON DELETE CASCADE
);

-- Indexes for performance
CREATE INDEX idx_vehicles_resident_id ON vehicles(resident_id);
CREATE INDEX idx_guests_resident_id ON guests(resident_id);
CREATE INDEX idx_guests_valid_until ON guests(valid_until);
CREATE INDEX idx_guest_vehicles_guest_id ON guest_vehicles(guest_id);
CREATE INDEX idx_parking_permits_guest_id ON parking_permits(guest_id);
CREATE INDEX idx_parking_permits_permit_number ON parking_permits(permit_number);
```

## State Transitions

### Resident Lifecycle
1. **Created**: New resident record created with basic information
2. **Updated**: Resident information modified (name, address, contact)
3. **Deleted**: Resident and all associated data removed (cascade delete)

### Guest Lifecycle
1. **Registered**: Guest created by resident with validity period
2. **Active**: Guest within validity period, can generate permits
3. **Expired**: Guest past validity period, cannot generate new permits
4. **Deleted**: Guest record removed (cascade delete to vehicles and permits)

### Vehicle Lifecycle
1. **Added**: Vehicle added to resident or guest
2. **Updated**: Vehicle information modified
3. **Removed**: Vehicle deleted from resident or guest

### Parking Permit Lifecycle
1. **Generated**: Permit created for active guest
2. **Printed**: Permit marked as printed with timestamp
3. **Expired**: Permit past valid_until date
4. **Deleted**: Permit removed (when guest is deleted)

## Data Validation Rules

### Cross-Entity Validation
- Guest validity period cannot exceed 7 days from creation
- Parking permit validity must be within guest validity period
- Vehicle license plates must be unique within the same resident/guest
- Permit numbers must be globally unique

### Business Rules
- Residents can have unlimited vehicles
- Guests can have unlimited vehicles
- No limit on number of guests per resident
- Guest records are kept permanently (no automatic cleanup)
- Parking permits can only be generated for active (non-expired) guests

## Migration Strategy

### Initial Migration
1. Create all tables with proper constraints
2. Create indexes for performance
3. Insert sample data for testing

### Future Migrations
1. Add new fields with default values
2. Modify constraints carefully to avoid data loss
3. Update indexes as needed for performance
4. Test migrations on copy of production data

## Performance Considerations

### Query Optimization
- Use indexes on frequently queried fields
- Limit result sets with pagination
- Use prepared statements for repeated queries
- Avoid N+1 queries with proper joins

### Data Growth
- SQLite can handle millions of records efficiently
- Consider archiving old permits if needed
- Monitor database file size and performance
- Regular VACUUM operations for optimization

## Security Considerations

### Data Protection
- Input validation at application level
- SQL injection prevention with parameterized queries
- Sensitive data encryption if required
- Regular backups of database file

### Access Control
- No authentication required (as per requirements)
- Input sanitization for all user inputs
- Rate limiting for API endpoints
- Audit logging for data changes
