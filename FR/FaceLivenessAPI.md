# Liveness Check API
This API is designed to passively assess the liveness of a selfie captured by the KYC end user. It takes the selfie image as input and provides a result indicating whether the user is a genuine person or if the image is a printout or displayed on a screen.

- **URL:** `/liveness/`
- **Allowed method:** `POST`
- **Require authentication:** YES

## Request Fields
The fields needed in the POST request are listed as follows:

| Request Field Name      | Required | Type          | Remarks |
|-------------------------|----------|---------------|---------|
| `image` or `image_bytes`| YES      | File or Bytes | You must provide one of these options, depending on your requirements. For `image`, we accept image files directly, while for `image_bytes`, we expect a **base64-encoded** image. Please refer to the example notebook and Postman for detailed use cases of this field.

## Response Fields

| Response Field Name | Type     | Remarks                                            |
|----------------------|----------|----------------------------------------------------|
| `success`              | Boolean  | Whether the request is successfully finished.   |
| `result`               | JSON     | *Only for successful requests* The dictionary of liveness results, with detailed fields included in [this section](#liveness-check-result-dictionary). |
| `error_message`        | String   | *Only for failed requests* Details of the encountered error. |
| `error_code`           | String   | *Only for failed requests* The error code for easier reference. Detailed matching can be found in [this section](#list-of-error-messages-and-error-codes).

## Liveness Check Result Dictionary
The result fields of a liveness check request are formatted as follows:

| Field Name | Type    | Remarks |
|------------|---------|---------|
| `liveness`   | Boolean | Indicates whether the face is considered to be from a live person, instead of [printed attack](https://paperswithcode.com/task/face-anti-spoofing#:~:text=Print%20attack%3A%20The%20attacker%20uses%20someone%E2%80%99s%20photo.%20The%20image%20is%20printed%20or%20displayed%20on%20a%20digital%20device.) or [replay attacks](https://paperswithcode.com/task/face-anti-spoofing#:~:text=Replay/video%20attack%3A%20A%20more%20sophisticated%20way%20to%20trick%20the%20system%2C%20which%20usually%20requires%20a%20looped%20video%20of%20a%20victim%E2%80%99s%20face.%20This%20approach%20ensures%20behaviour%20and%20facial%20movements%20to%20look%20more%20%E2%80%98natural%E2%80%99%20compared%20to%20holding%20someone%E2%80%99s%20photo).|
| `score`      | Float   | A value from 0 to 1, indicating how confident the system is for the result.

## List of Error Messages and Error Codes

The following is a comprehensive list of potential error codes returned by the engine, along with their respective error messages and explanations:

| Error Code | Error Message                                        | Remarks                                                    |
|------------|-----------------------------------------------------|------------------------------------------------------------|
| `001`        | API key is either missing or incorrect.            | Please check the provided API key.            |
| `002`        | Error reading image, please check the request format. | This error occurs when the image fails to be recognized in the expected format.|
| `003`        | Image file size is too large.                      | The engine has a 10MB limit for image files; ensure they do not exceed this size.|
| `004`        | Request is not in the correct format.              | This error indicates an issue with the request format or missing key fields.|
| `005`        | Server internal error, please try again.           | In the case of an internal server error, please contact us if the issue persists.|
| `051`        | No face, or more than one face is detected in the selfie. | We strictly require there to be only one face in the selfie image, so we would know which face should we check liveness for. Please ask the KYC end user to take this image in a clean background with no other face in the image to avoid this error. If the image has no face inside, it should also result in a failed request.|
| `052`        | Face mask is detected, please remove the face mask and try again. | This is a KYC concern, as we do not wish to allow the KYC process to be done with a face mask on. So if a mask is detected, the request would fail with this error.
