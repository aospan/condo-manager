# Quickstart Guide: Condo Management Application

**Date**: 2024-12-19  
**Feature**: Condo Management Web Application  
**Tech Stack**: Rust backend, HTML/CSS/JS frontend, SQLite database, Docker

## Prerequisites

- Docker and Docker Compose installed
- Modern web browser (Chrome, Firefox, Safari, Edge)
- Git (for cloning the repository)

## Quick Start (5 minutes)

### 1. Clone and Setup
```bash
git clone <repository-url>
cd condo-manager
```

### 2. Start the Application
```bash
docker-compose up --build
```

This will:
- Build the Rust backend with SQLite database
- Set up the web frontend
- Start the application on `http://localhost:3000`

### 3. Access the Application
Open your browser and navigate to `http://localhost:3000`

## User Scenarios Testing

### Scenario 1: Add a New Resident
1. **Given** you are on the main dashboard
2. **When** you click "Add Resident" button
3. **Then** you should see a form with fields for name, address, phone, and email
4. **When** you fill in the required fields (name: "John Doe", address: "Unit 101, 123 Main St")
5. **And** click "Save Resident"
6. **Then** you should see the new resident in the residents list
7. **And** the form should be cleared for the next entry

### Scenario 2: Add Vehicles to a Resident
1. **Given** you have a resident in the system
2. **When** you click on the resident's name in the residents list
3. **Then** you should see the resident details page
4. **When** you click "Add Vehicle" button
5. **And** fill in vehicle details (license plate: "ABC123", make: "Toyota", model: "Camry")
6. **And** click "Save Vehicle"
7. **Then** you should see the vehicle listed under the resident
8. **And** you can add multiple vehicles to the same resident

### Scenario 3: Register a Guest
1. **Given** you have a resident in the system
2. **When** you click "Register Guest" button
3. **Then** you should see a guest registration form
4. **When** you select a resident from the dropdown
5. **And** fill in guest details (name: "Jane Smith", phone: "555-0123")
6. **And** set validity period (default 12 hours)
7. **And** click "Register Guest"
8. **Then** you should see the guest in the guests list
9. **And** the guest should be marked as "Active"

### Scenario 4: Add Vehicle to Guest
1. **Given** you have an active guest in the system
2. **When** you click on the guest's name in the guests list
3. **Then** you should see the guest details page
4. **When** you click "Add Vehicle" button
5. **And** fill in vehicle details (license plate: "XYZ789", make: "Honda", model: "Civic")
6. **And** click "Save Vehicle"
7. **Then** you should see the vehicle listed under the guest

### Scenario 5: Generate Parking Permit
1. **Given** you have an active guest with at least one vehicle
2. **When** you click "Generate Permit" button next to the guest
3. **Then** you should see a permit generation dialog
4. **When** you review the permit details (guest name, vehicle info, validity period)
5. **And** click "Generate Permit"
6. **Then** you should see a success message with permit number
7. **And** the permit should be marked as "Generated"

### Scenario 6: Print Parking Permit
1. **Given** you have a generated parking permit
2. **When** you click "Print Permit" button
3. **Then** you should see a print-friendly version of the permit
4. **When** you use your browser's print function (Ctrl+P or Cmd+P)
5. **Then** the permit should print with proper formatting
6. **And** include all required information (guest name, vehicle details, validity dates)

### Scenario 7: Search and Filter
1. **Given** you have multiple residents and guests in the system
2. **When** you use the search box in the residents list
3. **And** type "John"
4. **Then** you should see only residents with "John" in their name
5. **When** you use the search box in the guests list
6. **And** type "Smith"
7. **Then** you should see only guests with "Smith" in their name
8. **When** you check "Active Only" filter in guests
9. **Then** you should see only non-expired guests

### Scenario 8: Edit and Update Records
1. **Given** you have a resident in the system
2. **When** you click "Edit" button next to the resident
3. **Then** you should see an edit form with current values
4. **When** you change the address to "Unit 102, 123 Main St"
5. **And** click "Update Resident"
6. **Then** you should see the updated information in the residents list
7. **And** the change should be reflected immediately

### Scenario 9: Delete Records
1. **Given** you have a resident with vehicles and guests
2. **When** you click "Delete" button next to the resident
3. **Then** you should see a confirmation dialog
4. **When** you confirm the deletion
5. **Then** the resident and all associated data should be removed
6. **And** you should see a success message

## API Testing (Optional)

### Test API Endpoints
```bash
# List all residents
curl http://localhost:3000/residents

# Create a new resident
curl -X POST http://localhost:3000/residents \
  -H "Content-Type: application/json" \
  -d '{"name": "Test Resident", "address": "Unit 999, Test St"}'

# List all guests
curl http://localhost:3000/guests

# Generate a permit (replace {guest_id} with actual ID)
curl -X POST http://localhost:3000/guests/{guest_id}/permit
```

## Troubleshooting

### Common Issues

**Application won't start:**
- Check if Docker is running
- Ensure port 3000 is not in use
- Check Docker logs: `docker-compose logs`

**Database errors:**
- The SQLite database is created automatically
- Check if the database file has proper permissions
- Restart the application to recreate the database

**Frontend not loading:**
- Check if the backend is running on port 3000
- Clear browser cache and reload
- Check browser console for JavaScript errors

**Print function not working:**
- Ensure you're using a modern browser
- Check if popup blockers are enabled
- Try printing from a different browser

### Reset Application
```bash
# Stop the application
docker-compose down

# Remove all data (this will delete all residents, guests, and permits)
docker-compose down -v

# Restart the application
docker-compose up --build
```

## Performance Expectations

- **Page Load**: < 2 seconds for initial load
- **Search**: < 500ms for filtered results
- **Form Submission**: < 1 second for CRUD operations
- **Print Generation**: < 2 seconds for permit generation
- **Concurrent Users**: Supports 100+ simultaneous users

## Data Persistence

- All data is stored in a SQLite database file
- Data persists between application restarts
- Database file is located in the `data/` directory
- Regular backups are recommended for production use

## Security Notes

- No authentication is required (as per requirements)
- All data is stored locally
- Input validation is performed on both client and server
- SQL injection protection is built-in
- XSS protection through template escaping

## Next Steps

After completing the quickstart scenarios:
1. Review the API documentation at `/api/docs`
2. Explore the source code structure
3. Run the test suite: `docker-compose exec backend cargo test`
4. Check the application logs for any issues
5. Customize the application for your specific needs

## Support

For issues or questions:
1. Check the troubleshooting section above
2. Review the application logs
3. Check the GitHub issues page
4. Contact the development team
