# Overview

The **Telex Overview API** provides endpoints to retrieve metrics and summaries for various entities within Telex. This includes retrieving metrics for threads associated with a specific organization over a given number of days. These endpoints ensure efficient handling of data retrieval and integration for organizational insights.


## Get Organization Thread Metrics
- **Endpoint:** `/threads/organisations/{organisationId}/metrics`
- **Method:** `GET`
- **Tags:** Overview
- **Summary:** Retrieves metrics for threads associated with a specific organization over a given number of days.

### Parameters
- **organisationId** (path): Unique identifier of the organization.
- **days** (query): The number of days for which to retrieve metrics.

### Responses
- **200**: Successfully retrieved thread metrics.
```json
{
  "status": "success",
  "status_code": 200,
  "message": "Data retrieved successfully",
  "data": {
    "channel_count_info": {
      "total_success_threads": 16,
      "total_error_threads": 4,
      "total_members": 6,
      "total_resolved_threads": 14
    },
    "channel_metrics": [
      {
        "channel_name": "Default",
        "thread_count": 14,
        "success_count": 12,
        "error_count": 2,
        "other_count": 0
      }
      // More channel metrics...
    ]
  }
}
```
- **400**: Bad request.
- **401**: Unauthorized.
- **403**: Forbidden.
- **422**: Validation error.
- **404**: Organisation not found.
- **500**: Internal server error.

