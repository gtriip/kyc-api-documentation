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

## Envoking sequence
If you are required to generate session key for your requests, then it shall be in the sequence of Session -> OCR(either id or passport) -> FR (liveness -> compare); Else it shall only involve OCR and FR part in the sequence above.