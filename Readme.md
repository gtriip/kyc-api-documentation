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

## Requirements for images for different purposes
### Passport
* **Size and Resolution**: The passport image should have a minimum resolution of 1280 x 720 pixels (or 1280 x 2160 if vertical) to ensure clarity and legibility.

* **Avoid Covering Details**: Please ensure that fingers or other objects do not accidentally cover essential details such as the face, MRZ (Machine Readable Zone), and passport details during image capture.

* **Avoid Reflection and Glare**: To facilitate accurate detection of passport details, avoid capturing images with reflection, glare, or shadows on the face and MRZ region.

* **Background**: Use a clean and clear background without distracting patterns or textures. If possible, ensure that there is no text present in the background of the passport image, to reach higher OCR accuracy.

* **Cropping**: It's recommended to crop the image to fit the passport biodata page if possible, ensuring that unnecessary background or borders are removed.

### ID
* **Size and Resolution** The ID image should have a minumum resolution of 1280 x 720 pixel (or 1280 x 2160 if vertical) to ensure clarity and legibility.

* **Avoid Covering Details** Please ensure that the face image, ID details, as well as the QR or bar codes are not accidentally covered during the image capture.

* **Background** Use a clean and clear background without distracting patterns or textures. Additionally, ensure that there is strictly no text present in the background of the ID image, as it may interfere with OCR accuracy.

* **Cropping** It's recommended to crop the image to fit the ID edges if possible, ensuring the complex backgroud being removed.

### Face Requirements

* **Size and Resolution:** The face image should have a minimum resolution of 1280 x 720 pixels (or 720 x 1280 if vertical) to ensure clarity and legibility.

* **Blurryness:** The face image must be clear; blurry images may be rejected.

* **Background:** Ensure that the background is clear of other faces. Images with multiple detected faces in a single selfie will be rejected.

## Update Notes:
* 05/08/2024: We have introduced a beta version of document liveness detection. We plan to upgrade this version within a month to enhance its speed and accuracy.