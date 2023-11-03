# Face Compare API
The Face Compare API is designed for comparing two facial images. It takes in two images and returns a comparison result, indicating whether the two faces belong to the same person, along with a score and threshold.

- **URL:** `/compare/`
- **Allowed method:** `POST`
- **Require authentication:** YES

## Request Fields
The required fields for the POST request are listed as follows:

| Request Field Name      | Required | Type          | Remarks |
|-------------------------|----------|---------------|---------|
| `pic1` or `pic1_bytes`  | YES      | File or Bytes | You must provide one of these options, depending on your requirements. For `pic1`, we accept image files directly, while for `pic1_bytes`, we expect a **base64-encoded** image. Please refer to the example notebook and Postman for detailed use cases of this field.
| `pic2` or `pic2_bytes`  | YES      | File or Bytes | You must provide one of these options, depending on your requirements. For `pic2`, we accept image files directly, while for `pic2_bytes`, we expect a **base64-encoded** image. Please refer to the example notebook and Postman for detailed use cases of this field.
| `auto_rotate`  | NO      | Boolean | This field is not required. It defaults to `False`. However, in some cases, photos from KYC end users may not have proper EXIF information for the engine to determine the correct orientation of the image. You can enable this option in case some edge cases fail due to the face being in an incorrect orientation. This allows the engine to automatically try to rotate the image to the correct orientation for accurate comparison, but longer running time may incur for non-matching cases.

## Response Fields
The response fields for the request are listed as follows:

| Response Field Name | Type     | Remarks                                            |
|----------------------|----------|----------------------------------------------------|
| `success`              | Boolean  | Whether the request is successfully finished.   |
| `result`               | JSON     | *Only for successful requests* The dictionary of comparison results, with detailed fields included in [this section](#face-compare-result-dictionary). |
| `error_message`        | String   | *Only for failed requests* Details of the encountered error. |
| `error_code`           | String   | *Only for failed requests* The error code for easier reference. Detailed matching can be found in [this section](#list-of-error-messages-and-error-codes). |

## Face Compare Result Dictionary

The result fields of a face comparison request are formatted as follows:

| Response Field Name | Type     | Remarks                                            |
|----------------------|----------|----------------------------------------------------|
| `is_match`            | Boolean  | Indicates whether the two faces match, based on the threshold with an error rate of 0.01%. |
| `score`               | Float    | A score rounded to two decimal places in the range [0~100], describing how similar the two faces provided are. |
| `one_e3`              | Float    | A threshold score rounded to two decimal places in the range [0~100], showing the minimum requirement of the score when the **False Acceptance Rate** is 0.1%. This bar is the lowest. |
| `one_e4`              | Float    | A threshold score rounded to two decimal places in the range [0~100], showing the minimum requirement of the score when the **False Acceptance Rate** is 0.01%. This bar is higher than one_e3, but lower than one_e4. |
| `one_e5`              | Float    | A threshold score rounded to two decimal places in the range [0~100], showing the minimum requirement of the score when the **False Acceptance Rate** is 0.001%. This bar is the highest.

## List of Error Messages and Error Codes

The following is a comprehensive list of potential error codes returned by the engine, along with their respective error messages and explanations:

| Error Code | Error Message                                        | Remarks                                                    |
|------------|-----------------------------------------------------|------------------------------------------------------------|
| `001`        | API key is either missing or incorrect.            | Please check the provided API key.            |
| `002`        | Error reading image, please check the request format. | This error occurs when the image fails to be recognized in the expected format.|
| `003`        | Image file size is too large.                      | The engine has a 10MB limit for image files; ensure they do not exceed this size.|
| `004`        | Request is not in the correct format.              | This error indicates an issue with the request format or missing key fields.|
| `005`        | Server internal error, please try again.           | In the case of an internal server error, please try again and contact us if the issue persists.|
| `041`        | Compare model returned failed result, face is missing in either one of the images. | If either one of the two images have the face missing, this error would be returned. Note that the case where there are multiple faces in the selfie is handled by [LivenessAPI](./FaceLivenessAPI.md) |