. POST /api/auth/register
Input (JSON):

json
Copy code
{
  "first_name": "Alice",
  "last_name": "Smith",
  "email": "alice@example.com",
  "password": "SecurePass123!",
  "role": "guest"
}
Validation Rules:

email: valid format, unique

password: minimum 8 characters, must include uppercase, lowercase, and a number

role: must be guest or host

Output (JSON):

json
Copy code
{
  "message": "Registration successful",
  "token": "<JWT_TOKEN>",
  "user_id": "uuid"
}
2. POST /api/auth/login
Input (JSON):

json
Copy code
{
  "email": "alice@example.com",
  "password": "SecurePass123!"
}
Output:

json
Copy code
{
  "message": "Login successful",
  "token": "<JWT_TOKEN>"
}
⚙️ Performance Criteria
Token generation in under 500ms

Limit login attempts (e.g., 5 per 10 minutes per IP)

✅ Feature 2: Property Management
🔹 Description
Allows hosts to add, update, and delete property listings.

📌 API Endpoints
1. POST /api/properties
Input (JSON):

json
Copy code
{
  "title": "Cozy Loft",
  "description": "Quiet spot near central park.",
  "location": "New York, NY",
  "price_per_night": 120.00,
  "max_guests": 2,
  "amenities": ["WiFi", "Kitchen"],
  "availability": {
    "start_date": "2025-07-01",
    "end_date": "2025-08-31"
  }
}
Validation Rules:

price_per_night: must be > 0

max_guests: must be > 0

Dates: valid ISO format; start_date < end_date

Output:

json
Copy code
{
  "message": "Listing created successfully",
  "property_id": "uuid"
}
2. PUT /api/properties/:id
Update any editable fields (title, description, price, etc.)

3. DELETE /api/properties/:id
Soft delete the listing (status set to inactive)

⚙️ Performance Criteria
Listing fetch latency < 200ms

Search by location/amenities: supports indexing and pagination

✅ Feature 3: Booking System
🔹 Description
Allows guests to book available properties, cancel bookings, and track status.

📌 API Endpoints
1. POST /api/bookings
Input (JSON):

json
Copy code
{
  "property_id": "uuid",
  "start_date": "2025-07-10",
  "end_date": "2025-07-15"
}
Validation Rules:

No date overlaps with existing bookings

Dates must be within property's availability window

Output:

json
Copy code
{
  "message": "Booking created successfully",
  "booking_id": "uuid",
  "status": "pending"
}
2. GET /api/bookings/:id
Returns full booking details if the user is the guest or host

3. DELETE /api/bookings/:id
Cancellation logic:

Within free cancellation window → full refund

After cutoff → apply policy-based penalties
