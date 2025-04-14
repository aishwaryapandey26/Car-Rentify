# Car Rentify API Documentation
## Overview
The **Car Rentify API** serves as the backbone for managing the operations of the Car Rentify
platform, which includes functionalities such as car browsing, booking, managing reservations, and
interacting with car details. The API has been designed to ensure simplicity, scalability, and security,
catering to both customer and admin needs. By employing JWT (JSON Web Token) authentication,
the platform ensures secure API calls. Each endpoint is meticulously crafted to perform specific
operations and is ess...
## Base URL
```
http://localhost:8080/api
```
## Authentication
All API requests must include a valid JWT token in the **Authorization** header. This token ensures
that the request is made by an authenticated user, either as a customer or an admin.
- **Authorization Header Example:**

 ```
 Authorization: Bearer <your-jwt-token>
```
- **Response on Missing/Invalid Token:**
 ```
 401 Unauthorized
 ```
---
## Endpoints
### 1. Get All Cars
**Endpoint:**
```
GET /cars
- **model (optional):** Filter cars by the model name.
- **price_min (optional):** Set a minimum price for the car's daily rental rate.
- **price_max (optional):** Set a maximum price for the car's daily rental rate.
**Example Request:**
```
GET http://localhost:8080/api/cars?availability=true&price_min=30
```
**Response:**
```json
[
 {
 "id": 1,
 "model": "Toyota Corolla",
 "year": 2021,
 "pricePerDay": 40,
 "status": "available",
 "seats": 5,
 "fuelType": "Petrol"
 },
 {
 "id": 2,
 "model": "Ford Focus",
 "year": 2020,
 "pricePerDay": 45,
 "status": "available",
 "seats": 5,
 "fuelType": "Diesel"
```
**Description:**
This endpoint allows customers to retrieve a list of all available cars in the system. The response will
include car details such as model, year, availability status, price per day, number of seats, and fuel
type.
 }
]
```
**Status Codes:**
- `200 OK`: Successfully retrieved the list of cars.
- `500 Internal Server Error`: An unexpected error occurred on the server.
---
### 2. Get Car Details
**Endpoint:**
```
GET /cars/{carId}
```
**Description:**
This endpoint allows you to fetch the details of a specific car using its unique ID. It returns
comprehensive information about the car, such as its model, year, availability, price, and more.
**Path Parameters:**
- **carId (required):** The ID of the car whose details you wish to retrieve.
**Example Request:**
```
GET http://localhost:8080/api/cars/1
```
**Response:**
```json
{
 "id": 1,
 "model": "Toyota Corolla",
 "year": 2021,
 "pricePerDay": 40,
 "status": "available",
 "seats": 5,
 "fuelType": "Petrol",
 "location": "New York"
}
```
**Status Codes:**
- `200 OK`: Successfully retrieved the car details.
- `404 Not Found`: The car with the specified ID was not found.
- `500 Internal Server Error`: An unexpected error occurred on the server.
---
### 3. Book a Car
**Endpoint:**
```
POST /bookings
```
**Description:**
This endpoint enables users to book a car for a specific date range. The user must provide details
{
 "carId": 1,
 "userId": 123,
 "startDate": "2025-04-01",
 "endDate": "2025-04-07",
 "totalPrice": 280
}
```
**Response:**
```json
{
 "bookingId": 456,
 "carId": 1,
 "userId": 123,
 "startDate": "2025-04-01",
 "endDate": "2025-04-07",
 "totalPrice": 280,
 "status": "confirmed"
}
```
**Status Codes:**
- `200 OK`: Booking successfully created.
- `400 Bad Request`: Missing or incorrect fields (e.g., invalid car ID, user ID, or dates).
- `404 Not Found`: The car or user with the given ID was not found.
- `500 Internal Server Error`: An unexpected error occurred on the server.
---
### 4. Get Booking Details
**Endpoint:**
```
GET /bookings/{bookingId}
```
**Description:**
This endpoint allows users to retrieve the details of a specific booking by booking ID. The response
includes details such as the car ID, user ID, booking dates, and booking status.
**Path Parameters:**
- **bookingId (required):** The ID of the booking you wish to retrieve.
- **Example Request:**
```
GET http://localhost:8080/api/bookings/456
```
**Response:**
```json
{
 "bookingId": 456,
 "carId": 1,
 "userId": 123,
 "startDate": "2025-04-01",
 "endDate": "2025-04-07",
 "totalPrice": 280,
 "status": "confirmed"
}
```
**Status Codes:**
- `200 OK`: Successfully retrieved booking details.
- `404 Not Found`: The booking with the specified ID was not found.
- `500 Internal Server Error`: An unexpected error occurred on the server.
---
### 5. Cancel a Booking
**Endpoint:*
```
DELETE /bookings/{bookingId}
```
**Description:**
Allows the user to cancel a previously created booking by specifying the booking ID.
**Path Parameters:**
- **bookingId (required):** The ID of the booking to be canceled.
**Example Request:**
```
DELETE http://localhost:8080/api/bookings/456
```
**Response:**
```json
{
 "message": "Booking successfully cancelled."
}
```
**Status Codes:**
- `200 OK`: Successfully canceled the booking.
- `404 Not Found`: The booking with the specified ID was not found.
- `500 Internal Server Error`: An unexpected error occurred on the server.
---
### 6. Update a Car's Availability Status
**Endpoint:**
```
PUT /cars/{carId}/status
```
**Description:**
This endpoint allows admins to update the availability status of a specific car. This is useful for
managing when cars are available for booking.
**Path Parameters:**
- **carId (required):** The ID of the car whose status needs to be updated.
**Request Body:**
```json
{
 "status": "unavailable"
}
```
**Response:**
```json
{
 "message": "Car status successfully updated.",
 "carId": 1,
 "newStatus": "unavailable"
}
`
such as the car ID, user ID, start and end dates, and the total price for the booking.
**Request Body:**
```json
**Query Parameters:**
- **availability (optional):** Filter cars by availability status (`true` for available, `false` for
unavailable).
```
**Status Codes:**
- `200 OK`: Successfully updated the car's status.
- `400 Bad Request`: Invalid status value provided.
- `404 Not Found`: Car with the specified ID was not found.
- `500 Internal Server Error`: An unexpected error occurred on the server.
---
## Error Handling
The Car Rentify API uses standard HTTP status codes to indicate the success or failure of an API
request. Below are some of the common status codes:
- `200 OK`: The request was successful.
- `201 Created`: A new resource was created (typically used in POST requests).
- `400 Bad Request`: The request is invalid (e.g., missing required parameters).
- `401 Unauthorized`: Authentication failed or the user does not have the required permissions.
- `404 Not Found`: The requested resource could not be found.
- `500 Internal Server Error`: A server-side error occurred
