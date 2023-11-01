# KYC API Documentation

## Base URL

The base URL is https://kyc.gtriip.com.

## Endpoints require no authentication
We currently don't have any endpoint which doesn't require authentication.

## Endpoints require authentication
For authentication purposes, an API key is required in the request header with the field name `x-api-key`. Please make sure to include your issued API key in the POST request headers."
### OCR API
* [Passport Recognition API](./OCR/PassportRecognitionAPI.md): POST /passport/
* [ID Recognition API](./OCR/IDRecognitionAPI.md): POST /id/

### FR API
* [FR Liveness Check API](./FR/FaceLivenessAPI.md): POST /liveness/
* [FR Comparing API](./FR/FaceCompareAPI.md): POST /compare/