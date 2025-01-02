# Session Key Generator API
This API is designed for generating the session token for session-based API requests. You would only need to generate a session token if we require you to do so. Please confirm with Trevo developers for this function.

- **URL:** `/session/`
- **Allowed method:** `GET`
- **Require authentication:** YES

## Request Headers
| Header Field | Required | Remarks                                  |
|--------------|----------|------------------------------------------|
| x-api-key    | Yes      | API key issued to you/your organization. |

## Response Fields
| Response Field Name | Type     | Remarks                                            |
|----------------------|----------|----------------------------------------------------|
| `success`            | Boolean  | Whether the request is successfully recognized.   |
| `session_token`      | String   | *Only for successful requests* Created session token for your subsequence accesses to the APIs. |
| `error_message`      | String   | *Only for failed requests* Details of the encountered error. |
| `error_code`         | String   | *Only for failed requests* The error code for easier reference. Detailed matching can be found in [this section](#list-of-error-messages-and-error-codes). |

## Notes
* The access count per session token for each API is 3 times. ID&Passport are using the same API count, while liveness and compare result are using different ones.
* The session token generated would be removed upon successful compare result, or after 3 hours.