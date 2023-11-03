# Passport Recognition API
This API is designed to process passport detail recognition requests. It is specifically tailored to read passports with the MRZ (Machine Readable Zone) section and verify the presence of a valid face image in the specified location, in accordance with your requirements.

- **URL:** `/passport/`
- **Allowed method:** `POST`
- **Require authentication:** YES

## Request Fields
| Request Field Name      | Required | Type          | Remarks |
|-------------------------|----------|---------------|---------|
| `image` or `image_bytes`| YES      | File or Bytes | You must provide one of these options, depending on your requirements. For `image`, we accept image files directly, while for `image_bytes`, we expect a **base64-encoded** image. Please refer to the example notebook and Postman for detailed use cases of this field.
| `skip_face_detect`      | NO       | Boolean       | This field is optional and defaults to `False`. You may turn it on if you do not wish to check for a face in the passport image.

## Response Fields
| Response Field Name | Type     | Remarks                                            |
|----------------------|----------|----------------------------------------------------|
| `success`            | Boolean  | Whether the request is successfully recognized.   |
| `result`             | JSON     | *Only for successful requests* The dictionary of recognition results, with detailed fields included in [this section](#passport-recognition-result-dictionary). |
| `error_message`      | String   | *Only for failed requests* Details of the encountered error. |
| `error_code`         | String   | *Only for failed requests* The error code for easier reference. Detailed matching can be found in [this section](#list-of-error-messages-and-error-codes). |

## Passport Recognition Result Dictionary
The dictionary content is as listed in the table below. For more detailed information about MRZ fields, please refer to the [Wiki](https://en.wikipedia.org/wiki/Machine-readable_passport).

| Field Name           | Type    | Remarks                                            |
| -------------------- | ------- | -------------------------------------------------- |
| `mrz_type`             | String  | There are three types: "TD1," "TD2," and "TD3". Only "TD3" is for passports, so the recognition is rejected if the detected mrz_type is not "TD3". For more detailed information about MRZ type, please check the [Wiki](https://en.wikipedia.org/wiki/Machine-readable_passport#:~:text=%22Type%203%22%20is%20typical%20of%20passport%20booklets.%20The%20MRZ%20consists%20of%202%20lines%20%C3%97%2044%20characters.).
| `valid_score`          | Integer | The validity score for the passport, ranging from 0 to 100. Scores are adjusted based on check digits.
| `raw_text`             | String  | The raw text of the MRZ, with '<' symbols removed.
| `type`                 | String  | The document type, always 'passport' in this case.
| `country`              | String  | The issuing country of the passport, parsed using ISO-3166 standards from the three-character code.
| `number`               | String  | The passport number.
| `date_of_birth`        | String  | The date of birth in YYYY-MM-DD format.
| `expired`              | Boolean | Indicates if the passport is expired.
| `expiration_date`      | String  | The passport's expiration date in YYYY-MM-DD format.
| `nationality`          | String  | The nationality of the passport holder, following ISO-3166 standards, with one exception (German passport, represented by a single character, 'D').
| `sex`                  | String  | The gender of the passport holder.
| `names`                | String  | The full name of the passport holder.
| `first_name`           | String  | The first name of the passport holder (may be empty in some countries).
| `last_name`            | String  | The last name of the passport holder (may be empty in some cases).
| `personal_number`      | String  | The personal number associated with the passport.
| `check_number`         | String  | Check digit for the passport number.
| `check_date_of_birth`  | String  | Check digit for the date of birth.
| `check_expiration_date`| String  | Check digit for the expiration date.
| `check_composite`      | String  | Check digit for composite validation.
| `check_personal_number` | String  | Check digit for the personal number.
| `valid_number`         | Boolean | Indicates if the passport number is valid, matching the check digit.
| `valid_date_of_birth`  | Boolean | Indicates if the date of birth is valid, matching the check digit.
| `valid_expiration_date`| Boolean | Indicates if the expiration date is valid, matching the check digit.
| `valid_composite`      | Boolean | Indicates if the composite validation is valid, matching the check digit.
| `valid_personal_number` | Boolean | Indicates if the personal number is valid, matching the check digit.

## List of Error Messages and Error Codes
The following is a comprehensive list of potential error codes returned by the engine, along with their respective error messages and explanations:

| Error Code | Error Message                                        | Remarks                                                    |
|------------|-----------------------------------------------------|------------------------------------------------------------|
| `001`        | API key is either missing or incorrect.            | Please check the provided API key.            |
| `002`        | Error reading image, please check the request format. | This error occurs when the image fails to be recognized in the expected format.|
| `003`        | Image file size is too large.                      | The engine has a 10MB limit for image files; ensure they do not exceed this size.|
| `004`        | Request is not in the correct format.              | This error indicates an issue with the request format or missing key fields.|
| `005`        | Server internal error, please try again.           | In the case of an internal server error, please try again and contact us if the issue persists.|
| `011`        | Face image is required but not found in the document. | This error is prompted when `skip_face_detect` is set to false, but no valid face image is found in the document.|
| `012`        | MRZ code is of a different type, please try again. | This error is triggered when the MRZ code recognized is not "TD3," leading to a failed recognition.|
| `013`        | Error reading MRZ code, please try again.          | This error occurs during the MRZ reading process, often due to offsets caused by stains or other issues.|
| `014`        | Error parsing MRZ code, please ensure there are no obstacles or glare on the bottom code portion. | This error indicates that the model failed to locate the MRZ code.|
| `015`        | Passport specimen detected.                         | *This error is currently not in use.* It is triggered when a specimen instead of a real passport is detected.|
| `016`        | MRZ code check failed, please try again, ensure there are no obstacles or glare on the bottom code portion. | This error occurs during MRZ parsing, often due to stains or glare in the MRZ region.|