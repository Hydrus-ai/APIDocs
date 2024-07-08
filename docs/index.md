### API Specification for Hydrus.ai

#### Overview
The Hydrus.ai API enables seamless integration for managing and manipulating employee data on the Hydrus data management software platform. This documentation is intended for developers and technical users.

#### Base URLs
- **Development**: `https://api.hydrus.ai/dev/v1`
- **Testing**: `https://api.hydrus.ai/test/v1`
- **Production**: `https://api.hydrus.ai/prod/v1`

#### Authentication
All requests to the API require an API key. To obtain an API key, please contact your account manager or email support at [support@hydrus.ai](mailto:support@hydrus.ai). Include the key in the header of each request as follows:
```
x-api-key: YOUR_API_KEY
```

#### Rate Limiting
While there is no inherent rate limiting set on the Hydrus side, it is recommended to limit requests to 3-5 per second to prevent potential load issues. Each client's data is stored on their own cluster, and excessive requests could overload the cluster.

#### Versioning
The API follows a versioning strategy using URL paths. The current version is v1. Here is an example of a best-in-class versioning strategy:

- **Current Version**: `https://api.hydrus.ai/v1`
- **Future Version**: `https://api.hydrus.ai/v2`

When a new version is released, the previous version will continue to be supported for a period of time, typically six months, before it is deprecated. Clients will be notified in advance of any deprecations.

#### Endpoints

##### 1. Create Employee Record
**Endpoint**: `/create`

**Method**: `POST`

**Description**: Creates a new employee record in the Hydrus system.

**Request Format**:
```json
{
    "GroupId": "domain.com",
    "DataId": "string",
    "DatasheetName": "string",
    "Payload": {
        "effective_date": "YYYY-MM-DD",
        "employee_id": "string",
        "employee_type": "string",
        "department_code": "string",
        "department_name": "string",
        "roll_up_department": "string",
        "extra_field_1": "string",
        "location": "string",
        "ethnicity": "string",
        "gender": "string",
        "gender_identity": "string",
        "hire_date": "YYYY-MM-DD",
        "company_code": "string",
        "company_name": "string",
        "job_title": "string",
        "region": "string",
        "people_manager": "string",
        "job_level": "string",
        "disability": "string",
        "age": "number",
        "age_group": "string",
        "veteran": "string",
        "termination_date": "YYYY-MM-DD",
        "termination_type": "string",
        "reason": "string",
        "generation": "string"
    }
}
```

**Response Format**:
- **Success**:
```json
{
    "RequestType": "Create",
    "GroupId": "domain.com",
    "DataId": "string",
    "DatasheetName": "string",
    "ModifiedItems": 1,
    "Message": "Record created successfully."
}
```

- **Error**:
```json
{
    "RequestType": "Create",
    "GroupId": "domain.com",
    "DataId": "string",
    "DatasheetName": "string",
    "ModifiedItems": 0,
    "Message": "An error occurred while adding the record. Please try again later."
}
```

- **Primary Key Conflict**:
```json
{
    "RequestType": "Create",
    "GroupId": "domain.com",
    "DataId": "string",
    "DatasheetName": "string",
    "ModifiedItems": 0,
    "ExistingPrimaryKeys": "effective_date = YYYY-MM-DD HH:MM:SS",
    "Message": "The data for the following primary keys already exists: effective_date = YYYY-MM-DD HH:MM:SS"
}
```

**cURL Example**:
```bash
curl -X POST \
     -H "Content-Type: application/json" \
     -H "x-api-key: YOUR_API_KEY" \
     -d '{"GroupId": "example.com", "DataId": "12345", "DatasheetName": "Employee Data", "Payload": {"effective_date": "2024-01-01", "employee_id": "E12345", "employee_type": "Full-time", "department_code": "D001", "department_name": "Engineering", "roll_up_department": "Tech", "extra_field_1": "Extra", "location": "HQ", "ethnicity": "Asian", "gender": "Female", "gender_identity": "Non-binary", "hire_date": "2023-06-01", "company_code": "C001", "company_name": "Example Corp", "job_title": "Software Engineer", "region": "US", "people_manager": "No", "job_level": "L3", "disability": "No", "age": 29, "age_group": "Adult", "veteran": "No", "termination_date": "2024-01-01", "termination_type": "Voluntary", "reason": "Personal", "generation": "Millennial"}}' \
     https://api.hydrus.ai/test/v1/create
```

##### 2. Read Employee Record
**Endpoint**: `/read`

**Method**: `GET`

**Description**: Retrieves an employee record from the Hydrus system based on the given filters.

**Request Format**:
```json
{
    "GroupId": "domain.com",
    "DataId": "string",
    "DatasheetName": "string",
    "Payload": {
        "filter": "string"
    }
}
```

**Response Format**:
- **Success**:
```json
{
    "RequestType": "Read",
    "GroupId": "domain.com",
    "DataId": "string",
    "DatasheetName": "string",
    "Payload": [
        {
            "employee_id": "string",
            "name": "string",
            ...
        }
    ]
}
```

- **Error**:
```json
{
    "RequestType": "Read",
    "GroupId": "domain.com",
    "DataId": "string",
    "DatasheetName": "string",
    "Payload": [],
    "Message": "An error occurred while retrieving the record. Please try again later."
}
```

**cURL Example**:
```bash
curl -X GET \
     -H "x-api-key: YOUR_API_KEY" \
     "https://api.hydrus.ai/test/v1/read?GroupId=example.com&DataId=12345&DatasheetName=Employee%20Data&Payload={\"filter\":\"name=John\"}"
```

##### 3. Update Employee Record
**Endpoint**: `/update`

**Method**: `PUT`

**Description**: Updates an existing employee record in the Hydrus system.

**Request Format**:
```json
{
    "GroupId": "domain.com",
    "DataId": "string",
    "DatasheetName": "string",
    "Payload": {
        "$oid": "string",
        "field_name": "new_value"
    }
}
```

**Response Format**:
- **Success**:
```json
{
    "RequestType": "Update",
    "GroupId": "domain.com",
    "DataId": "string",
    "DatasheetName": "string",
    "ModifiedItems": 1,
    "Message": "Record updated successfully."
}
```

- **Error**:
```json
{
    "RequestType": "Update",
    "GroupId": "domain.com",
    "DataId": "string",
    "DatasheetName": "string",
    "ModifiedItems": 0,
    "Message": "An error occurred while updating the record. Please try again later."
}
```

**cURL Example**:
```bash
curl -X PUT \
     -H "Content-Type: application/json" \
     -H "x-api-key: YOUR_API_KEY" \
     -d '{"GroupId": "example.com", "DataId": "12345", "DatasheetName": "Employee Data", "Payload": {"$oid": "12345", "job_title": "Senior Software Engineer"}}' \
     https://api.hydrus.ai/test/v1/update
```

##### 4. Delete Employee Record
**Endpoint**: `/delete`

**Method**: `DELETE`

**Description**: Deletes an employee record from the Hydrus system.

**Request Format**:
```json
{
    "GroupId": "domain.com",
    "DataId": "string",
    "DatasheetName": "string",
    "Payload": {
        "$oid": "string"
    }
}
```

**Response Format**:
- **Success**:
```json
{
    "RequestType": "Delete",
    "GroupId": "domain.com",
    "DataId": "string",
    "DatasheetName": "string",
    "ModifiedItems": 1,
    "Message": "Record deleted successfully."
}
```

- **Error**:
```json
{
    "RequestType": "Delete",
    "GroupId": "domain.com",
    "DataId": "string",
    "DatasheetName": "string",
    "ModifiedItems": 0,
    "Message": "An error occurred while deleting the record. Please try again later

."
}
```

**cURL Example**:
```bash
curl -X DELETE \
     -H "Content-Type: application/json" \
     -H "x-api-key: YOUR_API_KEY" \
     -d '{"GroupId": "example.com", "DataId": "12345", "DatasheetName": "Employee Data", "Payload": {"$oid": "12345"}}' \
     https://api.hydrus.ai/test/v1/delete
```

#### Error Handling
Errors are categorized into system errors and data errors. All errors are logged, and notifications are sent to the appropriate teams.

- **System Errors**: Errors caused by connection failures or server issues. These will result in the process stopping, logging the error, and notifying the system administrator.
- **Data Errors**: Errors caused by invalid or non-existent data. These will result in the process stopping, logging the error, and notifying the relevant business process team.

#### Support and Contact Information
For support, please contact your account manager or reach out to [support@hydrus.ai](mailto:support@hydrus.ai).

#### Testing Sandbox
To access the test environment and develop applications with REST inbound and outbound, please contact Hydrus for access. It is recommended to use the testing sandbox to ensure your applications integrate smoothly.

