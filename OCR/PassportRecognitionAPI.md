# Passport Recognition API
This API is designed to process passport detail recognition requests. It is specifically tailored to read passports with the MRZ (Machine Readable Zone) section and verify the presence of a valid face image in the specified location, in accordance with your requirements.

- **URL:** `/passport/`
- **Allowed method:** `POST`
- **Require authentication:** YES

## Request Headers
| Header Field | Required | Remarks                                  |
|--------------|----------|------------------------------------------|
| x-api-key    | Yes      | API key issued to you/your organization. |
| x-session-token | Yes, if we ask you to provide.  | Session token generated by /session/ endpoint. |

## Request Fields
| Request Field Name      | Required | Type          | Remarks |
|-------------------------|----------|---------------|---------|
| `image` or `image_bytes`| YES      | File or Bytes | You must provide one of these options, depending on your requirements. For `image`, we accept image files directly, while for `image_bytes`, we expect a **base64-encoded** image. Please refer to the example notebook and Postman for detailed use cases of this field. |
| `skip_face_detect`      | NO       | Boolean       | This field is optional and defaults to `False`. You may turn it on if you do not wish to check for a face in the passport image. |
| `verify_liveness`       | NO       | Boolean       | This field is optional and defaults to `False`. You may turn it on if you wish to verify the liveness of document. |

## Response Fields
| Response Field Name | Type     | Remarks                                            |
|----------------------|----------|----------------------------------------------------|
| `success`            | Boolean  | Whether the request is successfully recognized.   |
| `result`             | JSON     | *Only for successful requests and some failed requests, please refer to the error section below.* The dictionary of recognition results, with detailed fields included in [this section](#passport-recognition-result-dictionary). |
| `error_message`      | String   | *Only for failed requests* Details of the encountered error. |
| `error_code`         | String   | *Only for failed requests* The error code for easier reference. Detailed matching can be found in [this section](#list-of-error-messages-and-error-codes). |

## Passport Recognition Result Dictionary
The dictionary content is as listed in the table below. For more detailed information about MRZ fields, please refer to the [Wiki](https://en.wikipedia.org/wiki/Machine-readable_passport).

| Field Name           | Type    | Remarks                                            |
| -------------------- | ------- | -------------------------------------------------- |
| `mrz_type`             | String  | There are three types: "TD1," "TD2," and "TD3". Only "TD3" is for passports, so the recognition would be rejected if the detected mrz_type is not "TD3". For more detailed information about MRZ type, please check the [Wiki](https://en.wikipedia.org/wiki/Machine-readable_passport#:~:text=%22Type%203%22%20is%20typical%20of%20passport%20booklets.%20The%20MRZ%20consists%20of%202%20lines%20%C3%97%2044%20characters.). |
| `valid_score`          | Integer | The validity score for the passport, ranging from 0 to 100. Scores are adjusted based on check digits. |
| `raw_text`             | String  | The raw text of the MRZ, with '<' symbols removed. |
| `type`                 | String  | The document type, always 'passport' in this case. |
| `country`              | String  | The issuing country of the passport, parsed using ISO-3166 standards from the three-character code. |
| `number`               | String  | The passport number. |
| `date_of_birth`        | String  | The date of birth in YYYY-MM-DD format. |
| `expired`              | Boolean | Indicates if the passport is expired. |
| `expiration_date`      | String  | The passport's expiration date in YYYY-MM-DD format. |
| `nationality`          | String  | The nationality of the passport holder, following ISO-3166 standards, with one exception (German passport, represented by a single character, 'D'). |
| `sex`                  | String  | The gender of the passport holder. |
| `names`                | String  | The full name of the passport holder. |
| `first_name`           | String  | The first name of the passport holder (may be empty in some countries). |
| `last_name`            | String  | The last name of the passport holder (may be empty in some cases). |
| `personal_number`      | String  | The personal number associated with the passport. |
| `check_number`         | String  | Check digit for the passport number. |
| `check_date_of_birth`  | String  | Check digit for the date of birth. |
| `check_expiration_date`| String  | Check digit for the expiration date. |
| `check_composite`      | String  | Check digit for composite validation. |
| `check_personal_number` | String  | Check digit for the personal number. |
| `valid_number`         | Boolean | Indicates if the passport number is valid, matching the check digit. |
| `valid_date_of_birth`  | Boolean | Indicates if the date of birth is valid, matching the check digit. |
| `valid_expiration_date`| Boolean | Indicates if the expiration date is valid, matching the check digit. |
| `valid_composite`      | Boolean | Indicates if the composite validation is valid, matching the check digit. |
| `valid_personal_number` | Boolean | Indicates if the personal number is valid, matching the check digit. |
<!-- | `real_confidence` | Float | *Experimental* This is the confidence level we provide, for checking if the passport is a real passport or a photocopy/scan of a passport. The higher this value is, the more possible that it's a real passport. | -->

## List of Error Messages and Error Codes
The following is a comprehensive list of potential error codes returned by the engine, along with their respective error messages and explanations:

### Error Cases: Only Error Information
 In the following cases, only error status and error messages would be returned.
| Error Code | Error Message                                        | Remarks                                                    |
|------------|-----------------------------------------------------|------------------------------------------------------------|
| `001`        | API key is either missing or incorrect.            | Please check the provided API key.            |
| `061`        | Session key is not found, please try again. | Please check if you have included the correct session key in your request. |
| `062`        | Session key limit reached, please try restart. | The limit for the certain API access for this API key has reached. Please request for a new one if you wish to continue requesting this API endpoint. | 
| `002`        | Error reading image, please check the request format. | This error occurs when the image fails to be recognized in the expected format.|
| `003`        | Image file size is too large.                      | The engine has a 10MB limit for image files; ensure they do not exceed this size.|
| `004`        | Request is not in the correct format.              | This error indicates an issue with the request format or missing key fields.|
| `005`        | Server internal error, please try again.           | In the case of an internal server error, please try again and contact us if the issue persists.|
| `006`        | Document liveness verification failed!             | Document liveness verification returned a none `Genuine` status, please use your original passport to take. |
| `011`        | Face image is required but not found in the document. | This error is prompted when `skip_face_detect` is set to false, but no valid face image is found in the document.|
| `012`        | MRZ code is of a different type, please try again. | This error is triggered when the MRZ code recognized is not "TD3," leading to a failed recognition.|
| `013`        | Error reading MRZ code, please try again.          | This error occurs during detection, when we couldn't even see the MRZ in the image.|
| `014`        | Error parsing MRZ code, please ensure there are no obstacles or glare on the bottom code portion. | This error indicates that the MRZ accuracy is too low, either because of altering of MRZ parts, or because of mis-reading of some characters due to glare or stain in the MRZ region. |
| `015`        | Passport specimen detected.                         | *This error is currently not in use.* It is triggered when a specimen instead of a real passport is detected.|
| `016`        | MRZ code check failed, please try again, ensure there are no obstacles or glare on the bottom code portion. | This error occurs during MRZ parsing, we are unable to parse correct information from the MRZ region, often due to stains or glare in the MRZ region.|

### Error Cases: MRZ Results with Error Status
In the following cases, the MRZ result will be returned along with the corresponding error status for further verification.

| **Error Code** | **Error Message**                                   | **Remarks**                                                 |
|----------------|-----------------------------------------------------|-------------------------------------------------------------|
| `081`          | Passport number from VIZ does not match the MRZ.    | VIZ and MRZ mismatch detected. Please verify the passport number field. |
| `082`          | Date of birth from VIZ does not match the MRZ.      | VIZ and MRZ mismatch detected. Please verify the date of birth field. |
| `083`          | Date of expiry from VIZ does not match the MRZ.     | VIZ and MRZ mismatch detected. Please verify the date of expiry field. |
| `084`          | Name from VIZ does not match the MRZ.               | VIZ and MRZ mismatch detected. Please verify the name field. |
| `085`          | Suspected paste-over on the passport photo.         | Please inspect the passport photo to ensure its authenticity. |
| `086`          | Suspected photocopy instead of the original passport. | Please verify that the passport is an original document and not a photocopy. |
| `087`          | Suspected screen recapture of the passport.         | Please verify that the passport is an original document and not a screen-captured image. |
| `088`          | Parsing VIZ failed, please kindly double check manually. | Parsing VIZ failed because of problem in capturing dates, please check manually. |

#### Sample response for this case:
```
{'error_code': '083',
 'error_message': 'Date of expiry from VIZ does not match the MRZ.',
 'result': {'check_composite': '...',
            'check_date_of_birth': '...',
            'check_expiration_date': '...',
            'check_number': '...',
            'check_personal_number': '...',
            'country': 'BGR',
            'date_of_birth': '...',
            'document_type': 'passport',
            'expiration_date': '...',
            'expired': False,
            'first_name': 'FirstName FirstName',
            'last_name': 'LastName',
            'mrz_type': 'TD3',
            'nationality': 'BGR',
            'number': ...,
            'personal_number': ...,
            'raw_text': ...,
            'sex': 'Female',
            'type': 'P<',
            'valid_composite': True,
            'valid_date_of_birth': True,
            'valid_expiration_date': True,
            'valid_number': True,
            'valid_personal_number': True,
            'valid_score': 100},
 'success': False}
```

## Thresholds and Explanations

This API uses four thresholds to ensure accurate and reliable operations. The thresholds are detailed as follows:

### Face Detector Threshold
- **Current Setting**: 0.75  
    - If the confidence score for the detected face is below **0.75**, the face detection will be rejected.
- **Recommended Setting from Model Side**: 0.60  
    - However, this lower threshold has caused issues during the comparison stage, as faces with reflection obstructions might pass. Sticking with a higher threshold is recommended to maintain accuracy and reliability.

### Passport MRZ Accuracy Threshold
- **Current Setting**: 0.62  
- If the MRZ valid score is below **62%** (two validation failed), the recognition will be rejected. This typically indicates a high likelihood of OCR misreading or potential alteration of the MRZ.
#### Validation Checks:
There are five validation checks performed:
1. `valid_number`  
2. `valid_date_of_birth`  
3. `valid_expiration_date`  
4. `valid_personal_number`  
5. `valid_composite`

#### Rejection Criteria:
Validation will be rejected in the following cases:
1. **More than 2 checks failed**.  
2. **If `valid_composite` fails**, and at least one of the following also fails:
   - **valid_number**  
   - **valid_date_of_birth**  
   - **valid_expiration_date**  
    *(This means failures in `valid_personal_number` and `valid_composite` are allowed together.)*  
3. **If only `valid_composite` fails**, but all other validity checks pass.  

### Photo-Pasting Threshold
- **Current Setting**: *Not in use*  
- If the confidence level from the photo-pasting detection model exceeds the threshold, it will be assumed that the photo is pasted onto the passport, leading to recognition rejection.

### Photocopy / Screen Recapture Threshold
- **Current Setting**: *Not in use, evaluating*  
- If the confidence level of the photocopy or screen recapture classification is below the threshold, the passport will be considered potentially not a photocopy or screen recapture.

These thresholds are carefully configured to ensure the integrity and security of the face detection and liveness verification processes.
#
# About Parsing of DOB and DOE in MRZ and VIZ

### In MRZ:
MRZ always contains 6-digit dates, meaning the year is represented with 2 digits. Dates of birth (DOB) and expiry (DOE) are parsed using the following logic:

- **Year Parsing**:
  - For years `00`–`68`: Append `20` in front (e.g., `23` becomes `2023`).
  - For years `69`–`99`: Append `19` in front (e.g., `85` becomes `1985`).

- **Date of Birth (DOB)**:
  - If the parsed DOB is **newer than the current date**, subtract 100 years to adjust to the correct century.

- **Date of Expiry (DOE)**:
  - Parse using the same year rules as above.
  - Valid year range: **1980 to 2080**.
  - If the parsed DOE is lower than **1980**, add 100 years.  
  - If the parsed DOE is higher than **2080**, subtract 100 years.

> **Explanations for DOE**  
> This range is chosen because countries began issuing machine-readable passports in the 1980s.

---

### In VIZ:
The VIZ section contains three dates, which must appear in chronological order: Date of Birth (DOB), Date of Issue (DOI), and Date of Expiry (DOE).

- **4-Digit Years**:
  - If dates use 4-digit years, they are parsed directly.

- **2-Digit Years**:
  - For years `00`–`68`: Append `20` in front (e.g., `23` becomes `2023`).  
  - For years `69`–`99`: Append `19` in front (e.g., `85` becomes `1985`).

- **DOB and DOE Parsing**:
  - If the parsed date is **newer than the current date + 10 years**, subtract 100 years to adjust to the correct century.

> **Why do we handle VIZ differently from MRZ?**  
> Unlike MRZ, where the date positions are fixed, the VIZ section does not enforce specific locations for each date. Therefore, additional logic is needed to ensure correct parsing and sequencing.
