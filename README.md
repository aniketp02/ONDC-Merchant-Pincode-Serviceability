# Optimal Storage and retrieval in m*n Sparse Matrix

## Description
Pincode based serviceability allows merchants to define the pincodes where they can deliver their products & services;
In ONDC, definition & verification of pincode based serviceability is separated, i.e. merchants define the pincodes they serve and buyer apps verify whether a particular pincode (of buyer) can be served by any of the available merchants;
Considering there are more than 30K pincodes and at least 100 million merchants (of which about 10% may enable pincode based serviceability), this requires an optimal data structure for storing the pincode serviceability by merchant (i.e. a sparse matrix of 10M*30K) so that verification is near real-time.

## Technologies Used
- **Rust**: for secure and scalable backend. 
- **Redis**: for quick retrieval of merchant serviceability.
- **PostgreSQL**: persistant storage of onboarded merchants.
- **Next.js**: Frontend 

## Setup
Provide instructions on how to set up the project locally, including any necessary installations and configurations.
List the environment variables required for the project to run successfully.

## API Documentation

### Add Merchant
- **Endpoint**: POST /merchant
- **Description**: This endpoint is used to add a new merchant to the platform.
- **Request Body**:
```
json
{
    "name": "Merchant name",
    "id": 0,
    "business_category": "Business Category",
    "contact": {
      "phone_number": "9999999999",
      "email": "email@example.com"
    },
    "pincodes_serviced": ["110008", "110009", "110001"]
}
```
- **Response**:
```
json
{
  "ONDC_merchant_id": "12345",
  "message": "Merchant Information added"
}
```

### Onboard Multiple Merchants

- **Endpoint**: POST /upload_csv
- **Description**: This endpoint is used to upload a CSV file containing merchant data to the system to onboard merchants at scale.

### Get Merchants by Pincode

- **Endpoint**: GET merchant/serviceability?pincodes=<pincodes>
- **Description**: This endpoint retrieves the list of merchants that service the given pincode.
- **Query Parameter**: Comma-separated list of pincodes.
- **Response**:
```
json
{
  "merchants": [
    {
      "id": 12345,
      "name": "Merchant Name"
    },
    {
      "id": 67890,
      "name": "Another Merchant"
    }
  ]
}
```

### Get Merchant Info
- **Endpoint**: GET /merchant/<merchant_id>
- **Description**: This endpoint retrieves the information of a specific merchant.
- **Response**:
```
json
{
  "id": 12345,
  "name": "Merchant Name",
  "business_category": "Business Category",
  "pincodes_serviced": ["110001", "110002"]
}
```

### Get All Merchants
- **Endpoint**: GET /merchants
- **Description**: Retrieves a list of all merchants stored in the system.
- **Response**: JSON response containing an array of merchants with their IDs and names.

### Update Merchant Info

- **Endpoint**: PUT /merchant/<merchant_id>
- **Description**: This endpoint is used to update the information of a specific merchant.
- **Request Body**:
```
json
{
    "name": "Updated Merchant name",
    "business_category": "Updated Business Category",
    "phone_number": "0000000000",
    "email": "example@example.com"
}
```
- **Response**:
```
json
{
  "ONDC_merchant_id": "12345",
  "message": "Merchant Information updated"
}
```

### Add Pincode Serviceability for Merchants
- **Endpoint**: PUT /merchant/serviceability/<merchant_id>
- **Description**: This endpoint adds additional serviceable pincodes for a given merchant
- **Request Body**:
```
json
{
  "pincodes": ["110019", "110008"] #additional pincodes to be serviced
}
```

### Delete Pincode Serviceability for Merchants

- **Endpoint**: DELETE /merchant/serviceability/<merchant_id>
- **Description**: This endpoint deletes the serviceability of merchants for a subset of pincodes.
- **Request Body**:
```
json
{
  "pincodes": ["110001"] #Remove serviceability for the following pincodes
}
```
- **Response**:
```
json
{
  "ONDC_merchant_id": "12345",
  "message": "Merchant Information updated"
}
```

### Delete Merchant

- **Endpoint**: DELETE /merchant/<merchant_id>
- **Description**: This endpoint is used to delete a specific merchant from the system.
- **Response**:
```
json
{
  "ONDC_merchant_id": "12345",
  "message": "Merchant Information Deleted!"
}
```


## Flow of the Project

- **Merchant Onboarding**: Merchants can be added to the system individually using the /merchant endpoint or in bulk using the /upload_csv endpoint.

- **Merchant Information Management**: Once merchants are onboarded, their information can be retrieved, updated, or deleted using various endpoints.

- **Serviceability Query**: The API provides endpoints to query merchants based on their serviceability for specific pincodes.

- **Data Storage**: Merchant information is stored in a PostgreSQL database, while serviceability data is cached in Redis for faster retrieval.

- **Email Notification**: Optionally, email notifications can be sent to merchants upon successful onboarding or updates using the /send_email endpoint.

## Conclusion
The project provides a comprehensive solution for managing merchant data, including adding, updating, and deleting merchants, as well as managing their serviceability for specific pincodes. The use of Redis and PostgreSQL databases ensures efficient data management and retrieval. The project can be further enhanced with additional features, such as authentication and authorization, to provide a more secure and robust solution.