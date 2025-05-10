# Backend Technical and Functional Requirements

## 1. User Authentication & Profile Management

### API Endpoints

| Endpoint                     | Method | Description                               |
|------------------------------|--------|-------------------------------------------|
| /api/auth/signup             | POST   | Register new user via email               |
| /api/auth/social-login       | POST   | Register/login via Google/Facebook        |
| /api/auth/login              | POST   | Authenticate existing user                |
| /api/auth/verify             | POST   | Verify user account                       |
| /api/auth/select-role        | PUT    | Update user as host or guest              |
| /api/users/profile           | GET    | Get user profile details                  |
| /api/users/profile           | PUT    | Update user profile                       |
| /api/users/addresses         | GET    | List user addresses                       |
| /api/users/addresses         | PUT    | Update existing address                   |
| /api/users/addresses         | POST   | Add new address                           |
| /api/users/payment-methods   | GET    | List payment methods                      |
| /api/users/payment-methods   | POST   | Add payment method                        |
| /api/users/payment-methods   | PUT    | Update payment method                     |

### Input/Output Specifications

#### Signup Request

```JSON
{
  "email": "user@example.com",
  "password": "securePassword123",
  "firstname": "John",
  "lastname": "Doe",
  "phone": "+12345678901"
}
```

#### Signup Response

```JSON
{
  "id": "uuid-string",
  "email": "user@example.com",
  "firstname": "John",
  "lastname": "Doe",
  "phone": "+12345678901",
  "is_verified": false,
  "created_at": "2023-09-05T10:30:00Z"
}
```

#### Social Login Request

```JSON
{
  "provider": "google|facebook",
  "access_token": "provider-access-token"
}
```

#### User Profile Response

```JSON
{
  "id": "uuid-string",
  "email": "user@example.com",
  "firstname": "John",
  "lastname": "Doe",
  "phone": "+12345678901",
  "profile_image": "https://example.com/images/profile.jpg",
  "is_verified": true,
  "verified_at": "2023-09-05T11:30:00Z",
  "roles": [
    {
      "id": "role-uuid",
      "name": "host|guest"
    }
  ],
  "addresses": [
    {
      "id": "address-uuid",
      "street": "123 Main St",
      "city": "San Francisco",
      "state": "CA",
      "postal_code": "94105",
      "country": "USA"
    }
  ],
  "payment_methods": [
    {
      "id": "payment-method-uuid",
      "type_of_card": "visa",
      "name_on_card": "John Doe",
      "expiration_date": "2025-12"
    }
  ],
  "created_at": "2023-09-05T10:30:00Z",
  "updated_at": "2023-09-05T11:30:00Z"
}
```

### Validation Rules

#### Email

1. Valid format (user@domain.com)
2. Unique in the system
3. Maximum 255 characters
4. Required field

#### Password

1. Minimum 8 characters
2. At least one uppercase letter, one lowercase letter, one number
3. No common passwords or dictionary words

#### Names

1. firstname and lastname: 2-50 characters each
2. Alphabetic characters, spaces, hyphens, and apostrophes only

#### Phone

1. Valid international format
2. Optional but recommended for verification

#### Payment Methods

1. Valid card number (Luhn algorithm check)
2. Valid expiration date (must be in the future)
3. Tokenization for security

### Performance Criteria

1. Authentication response time: < 250ms (95th percentile)
2. Social login integration response: < 1.5 seconds
3. Concurrent authentication capacity: 500 requests/second
4. Secure password hashing using bcrypt (12+ work factor)
5. JWT token expiration: 1 hour
6. Rate limiting: 10 failed attempts before temporary lockout
7. Adequate request throttling to prevent brute force attacks
8. 99.99% uptime for authentication services

## 2. Property Management

### API Endpoints

| Endpoint                                        | Method | Description                               |
|-------------------------------------------------|--------|-------------------------------------------|
| /api/properties                                 | GET    | List properties (paginated)               |
| /api/properties                                 | POST   | Create new property                       |
| /api/properties/:id                             | GET    | Get property details                      |
| /api/properties/:id                             | PUT    | Update property                           |
| /api/properties/:id                             | DELETE | Deactivate property                       |
| /api/properties/:id/photos                      | POST   | Upload property photos                    |
| /api/properties/:id/photos/:photo_id            | DELETE | Remove photo                              |
| /api/properties/:id/photos/:photo_id/gallary    | GET    | Get property photo gallery                |
| /api/properties/:id/rules                       | GET    | Get property rules                        |
| /api/properties/:id/rules                       | PUT    | Update property rules                     |
| /api/properties/:id/address                     | GET    | Get property address                      |
| /api/properties/:id/address                     | PUT    | Update property address                   |
| /api/properties/:id/amenities                   | GET    | Get property amenities                    |
| /api/properties/:id/amenities                   | PUT    | Update property amenities                 |
| /api/properties/:id/calendar                    | GET    | Get property availability                 |
| /api/properties/:id/calendar                    | PUT    | Update property availability              |
| /api/properties/:id/calendar/:date              | GET    | Get availability for specific date        |
| /api/properties/:id/calendar/:date              | PUT    | Update availability for specific date     |

### Input/Output Specifications

#### Create Property Request

```JSON
{
  "name": "Cozy Downtown Apartment",
  "description": "Beautiful apartment in the heart of the city",
  "property_type": "apartment",
  "price": 125.00,
  "discount": 10.00,
  "is_active": true,
  "bedrooms": 2,
  "bathrooms": 1,
  "max_guests": 4,
  "address": {
    "street": "456 Market St",
    "city": "San Francisco",
    "state": "CA",
    "postal_code": "94105",
    "country": "USA",
    "latitude": 37.7897,
    "longitude": -122.3972
  },
  "amenities": ["wifi", "kitchen", "heating", "airconditioning"],
  "rules": {
    "smoking": false,
    "pets": true,
    "parties": false,
    "check_in_time": "15:00",
    "check_out_time": "11:00",
    "cancellation_policy": "moderate",
    "children": true
  }
}
```

#### Property Response

```JSON
{
  "id": "property-uuid",
  "owner_id": "host-uuid",
  "name": "Cozy Downtown Apartment",
  "description": "Beautiful apartment in the heart of the city",
  "property_type": "apartment",
  "price": 125.00,
  "discount": 10.00,
  "is_active": true,
  "bedrooms": 2,
  "bathrooms": 1,
  "max_guests": 4,
  "total_rating": 4.8,
  "address": {
    "id": "address-uuid",
    "street": "456 Market St",
    "city": "San Francisco",
    "state": "CA",
    "postal_code": "94105",
    "country": "USA",
    "latitude": 37.7897,
    "longitude": -122.3972
  },
  "amenities": [
    {
      "id": "amenity-uuid",
      "name": "wifi",
      "icon": "wifi-icon"
    }
  ],
  "rules": {
    "id": "rules-uuid",
    "smoking": false,
    "pets": true,
    "parties": false,
    "check_in_time": "15:00",
    "check_out_time": "11:00",
    "cancellation_policy": "moderate",
    "children": true
  },
  "photos": [
    {
      "id": "photo-uuid",
      "url": "https://example.com/property-photos/image.jpg",
      "type": "exterior"
    }
  ],
  "created_at": "2023-09-05T10:30:00Z",
  "updated_at": "2023-09-05T11:30:00Z"
}
```

### Validation Rules

#### Property Details

1. Name: 5-100 characters, required
2. Description: 20-2000 characters, required
3. Price: > 0, precision to 2 decimal places
4. Discount: >= 0, <= price
5. Property type: Must be from predefined list
6. Bedrooms/Bathrooms: >= 1
7. Max guests: 1-20

#### Address

1. All fields required (street, city, state, postal_code, country)
2. Valid latitude (-90 to 90) and longitude (-180 to 180)
3. Valid postal code format for the specified country

#### Photos

1. At least 1 photo required before property can be activated
2. Maximum 20 photos per property
3. Supported formats: JPEG, PNG
4. Maximum size: 5MB per image
5. Minimum resolution: 800x600 pixels

#### Rules

1. Check-in time must be before check-out time
2. Cancellation policy must be one of: flexible, moderate, strict

### Performance Criteria

1. Property creation response time: < 3 seconds including photo processing
2. Property listing retrieval: < 300ms (95th percentile)
3. Search response time: < 500ms for standard filters
4. Geolocation-based search: < 800ms for 10-mile radius
5. Property photo upload: < 2 seconds per photo
6. Support for 50 simultaneous property creation requests
7. Image optimization and resizing for different device requirements
8. Cache property details for 5 minutes to reduce database load

## 3. Booking and Payment System

### API Endpoints

| Endpoint                                        | Method | Description                               |
|-------------------------------------------------|--------|-------------------------------------------|
| /api/properties/:id/availability                | GET    | Check property availability               |
| /api/properties/:id/bookings                    | GET    | List property bookings                    |
| /api/properties/:id/bookings                    | POST   | Create new booking                        |
| /api/properties/:id/bookings/:booking_id        | GET    | Get booking details                       |
| /api/properties/:id/bookings/:booking_id        | PUT    | Update booking details                    |
| /api/properties/:id/bookings/:booking_id        | DELETE | Cancel booking                            |
| /api/properties/:id/bookings/:booking_id/payment| POST   | Process payment for booking               |
| /api/properties/:id/bookings/:booking_id/review | POST   | Submit review after stay                  |
| /api/bookings/user/:user_id                     | GET    | List user's bookings                      |
| /api/reviews/property/:property_id              | GET    | Get property reviews                      |

### Input/Output Specifications

#### Create Booking Request

```JSON
{
  "property_id": "property-uuid",
  "date_in": "2023-10-15",
  "date_out": "2023-10-20",
  "total_price": 625.00
}
```

#### Booking Response

```JSON
{
  "id": "booking-uuid",
  "user_id": "guest-uuid",
  "property_id": "property-uuid",
  "date_in": "2023-10-15",
  "date_out": "2023-10-20",
  "status": "pending",
  "total_price": 625.00,
  "created_at": "2023-09-05T14:30:00Z",
  "updated_at": "2023-09-05T14:30:00Z"
}
```

#### Payment Request

```JSON
{
  "booking_id": "booking-uuid",
  "payment_method_id": "payment-method-uuid",
  "amount": 625.00
}
```

#### Payment Response

```JSON
{
  "id": "payment-uuid",
  "booking_id": "booking-uuid",
  "amount": 625.00,
  "status": "completed",
  "payment_method_id": "payment-method-uuid",
  "transaction_id": "gateway-transaction-id",
  "created_at": "2023-09-05T14:35:00Z"
}
```

#### Review Request

```JSON
{
  "property_id": "property-uuid",
  "rating": 5,
  "comment": "Excellent stay! The property was clean and exactly as described."
}
```

### Validation Rules

#### Booking Dates

1. Check-in date must be in the future
2. Check-out date must be after check-in date
3. Booking dates must match available dates in property_calendar
4. Minimum 1 night stay, maximum 30 nights

#### Booking Status

1. Valid statuses: pending, confirmed, cancelled
2. Status transitions must follow proper sequence (e.g., confirmed can't go back to pending)

#### Payment

1. Amount must match booking total_price
2. Valid payment_method_id belonging to the user
3. Transaction must be atomic (succeed entirely or fail entirely)

#### Review

1. Only allowed after confirmed booking with checkout date in the past
2. Rating must be 1-5
3. Comment: 10-500 characters
4. One review per booking

### Performance Criteria

1. Availability check response time: < 200ms
2. Booking creation response time: < 500ms
3. Payment processing response time: < 3 seconds
4. Optimistic locking to prevent double bookings
5. Transactional integrity for booking and payment operations
6. High availability requirement: 99.95% uptime
7. Ability to handle 200 concurrent booking requests
8. Failover mechanism for payment processing
9. Effective caching of availability data (invalidated immediately upon booking)
10. Automated email notifications for booking status changes: < 1 minute delay
