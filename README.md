# SEQTA API Documentation

This directory contains OpenAPI/Swagger documentation for the SEQTA Learn API.

## Files

- `seqta-api-openapi.yaml` - OpenAPI 3.0.3 specification for the SEQTA API

## Overview

The SEQTA API provides access to student information system functionality through REST endpoints. The API supports three user types:

- **Student** - Access to student-specific data (timetables, assessments, resources)
- **Teacher** - Access to teacher-specific data (classes, grading, resources)
- **Parent** - Access to parent-specific data (child's progress, notifications)

## Base URLs

All endpoints are prefixed with the school domain and user type:

- Student: `https://<school-domain>/seqta/student/`
- Teacher: `https://<school-domain>/seqta/teacher/`
- Parent: `https://<school-domain>/seqta/parent/`

**Note:** Replace `<school-domain>` with your school's actual domain name.

## Authentication

The API uses session-based authentication. Users must:

1. Log in through the web interface at `https://<school-domain>/seqta/`
2. Maintain an active session cookie (`sessionid`)
3. Include the session cookie in all API requests

## Viewing the Documentation

### Using Swagger UI

1. Install Swagger UI:
   ```bash
   npm install -g swagger-ui-serve
   ```

2. Serve the documentation:
   ```bash
   swagger-ui-serve seqta-api-openapi.yaml
   ```

3. Open your browser to `http://localhost:3000`

### Using Online Swagger Editor

1. Go to https://editor.swagger.io/
2. Click "File" → "Import file"
3. Upload `seqta-api-openapi.yaml`

### Using Redoc

1. Install Redoc CLI:
   ```bash
   npm install -g redoc-cli
   ```

2. Generate HTML documentation:
   ```bash
   redoc-cli bundle seqta-api-openapi.yaml -o seqta-api-docs.html
   ```

3. Open `seqta-api-docs.html` in your browser

## API Endpoints

### Files
- `GET /load/file` - Load files and resources by UUID

### Dashboard
- `GET /dashboard` - Get dashboard data and recent activities

### Timetable
- `GET /timetable` - Get class timetable for a period

### Assessments
- `GET /assessments` - List assessments and assignments
- `GET /assessments/{assessmentId}` - Get assessment details

### Resources
- `GET /resources` - Get learning resources and materials

### Notifications
- `GET /notifications` - Get notifications and announcements

### Profile
- `GET /profile` - Get user profile information

## Example Usage

### Using cURL

```bash
# Get dashboard data (requires active session cookie)
curl -X GET "https://<school-domain>/seqta/student/dashboard" \
  -H "Cookie: sessionid=your-session-id-here"
```

### Using Python (requests)

```python
import requests

# Set up session with authentication
session = requests.Session()
session.cookies.set('sessionid', 'your-session-id-here')

# Make API request
base_url = "https://<school-domain>/seqta/student"
response = session.get(f"{base_url}/dashboard")

if response.status_code == 200:
    data = response.json()
    print(data)
```

### Using JavaScript (fetch)

```javascript
// Set up fetch with credentials
const baseURL = 'https://<school-domain>/seqta/student';

fetch(`${baseURL}/dashboard`, {
  credentials: 'include', // Include cookies
  headers: {
    'Content-Type': 'application/json'
  }
})
  .then(response => response.json())
  .then(data => console.log(data))
  .catch(error => console.error('Error:', error));
```

## Response Formats

Most endpoints return JSON responses. The file loading endpoint (`/load/file`) may return:

- JSON (for metadata)
- PDF (for documents)
- Images (PNG, JPEG)
- HTML (for web content)

## Error Handling

All endpoints may return the following error responses:

- `400 Bad Request` - Invalid parameters or malformed request
- `401 Unauthorized` - Authentication required or session expired
- `404 Not Found` - The requested resource does not exist
- `500 Internal Server Error` - Server error

Error responses follow this format:

```json
{
  "error": "ERROR_CODE",
  "message": "Human-readable error message",
  "details": {}
}
```

## Notes

- All domain names have been obfuscated to use `<school-domain>` placeholder
- This documentation is based on reverse-engineered API patterns
- Some endpoints may require additional parameters not documented here
- API behavior may vary between different SEQTA installations
- Always ensure you have proper authorization before accessing student data

## Contributing

If you discover additional endpoints or have corrections, please update the OpenAPI specification file.

## License

This documentation is provided as-is for educational and integration purposes.

