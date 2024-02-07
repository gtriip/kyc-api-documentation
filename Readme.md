# KYC API Documentation

## Base URL

The base URL is https://kyc.gtriip.com. (https://kyc-qa.gtriip.com for Staging QA environment)

## Endpoints require no authentication
We currently don't have any endpoint which doesn't require authentication.

## Endpoints require authentication
For authentication purposes, an API key is required in the request header with the field name `x-api-key`. Please make sure to include your issued API key in the POST request headers."

### Session API
* [Session Key Generator API](./Session/SessionKeyGenerationAPI.md): GET /session/

### OCR API
* [Passport Recognition API](./OCR/PassportRecognitionAPI.md): POST /passport/
* [ID Recognition API](./OCR/IDRecognitionAPI.md): POST /id/

### FR API
* [FR Liveness Check API](./FR/FaceLivenessAPI.md): POST /liveness/
* [FR Comparing API](./FR/FaceCompareAPI.md): POST /compare/

## Postman URL
The postman collection is here in [Postman Collection](https://api.postman.com/collections/14363995-7fe3a40c-dfbf-40b3-bb4c-e61bb4256b02?access_key=PMAT-01HP16V8N6BHH3M27KP7MQNCJQ). 