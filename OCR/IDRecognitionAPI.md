# ID Recognition API
This is the API for ID recognition. The ID types we currently support are:
* Singapore
  * `SGN` NRIC for locals and PRs
  * `SGP` Work Permit, Employment Pass, S Pass
* Hong Kong
  * `HKI` Hong Kong ID (Both old and new)
* Macau
  * `MOO` Old Macau ID
  * `MON` New Macau ID
* China
  * `EEP` Exit-Entry Permit for Hong Kong and Macau
* Indonesia
  * `KTP` Kartu Tanda Penduduk (Indonesian ID Card)
* Korea
  * `KRD` Korean Driving License
  * `KRI` Korean ID Card
* Australia
  * `AUD` Driving license (Victoria, New South Wales, Queensland)
* Vietnam
  * `VNM`, `VNO`, `VNN` Vietnam ID - All versions
  * `VND` Driving License

We are still adding more IDs to this list.

## Request Fields
| Request Field Name      | Required | Type          | Remarks |
|-------------------------|----------|---------------|---------|
| `image_front` or `image_front_bytes` | YES      | File or Bytes | You must provide one of these options, depending on your requirements. For `image_front`, we accept image files directly, while for `image_front_bytes`, we expect a **base64-encoded** image. Please refer to the example notebook and Postman for detailed use cases of this field. |
| `front_only`                         | NO       | Boolean       | This is not required, and it's set to `False` by default. Please set this to `True` if you only need one side of ID to get recognised. Please note that for `SGP`, you cannot set this option to `True` because only front side doesn't have enough information for `SGP`.|
| `image_back` or `image_back_bytes`   | NO       | File or Bytes | You may provide one of these two options, depending on your requirements. For `image_back`, we accept image files directly, while for `image_back_bytes`, we expect a **base64-encoded** image. Please note that if you set `front_only` to `False`, this field is **required**.| 
| `skip_face_detect`                   | NO       | List of selections       | This field is optional, the default value is an empty list. This field let you decide which types of the IDs should be ignored for face detection. You should provide a list of selections in 'SGN', 'SGP', 'VNM', 'VNO', 'VNN', 'VND', 'KRI', 'KRD', 'MOO', 'MON', 'HKI', 'EEP', 'AUD', 'KTP' for the types of ID that you wish to allow the face check to be bypassed. |
| `country_code`                       | NO       | List of selections       | This field is optional, the default value is a list: `['SG', 'VN', 'KR', 'MO', 'ID', 'AU']`. You may select in this list for your use case, so that other kinds of IDs won't be checked. Please refer to the example notebook and Postman for detailed use cases of this field.|

## Response Fields
| Response Field Name 	| Type    	| Remarks                                                                                                                                                    	|
|---------------------	|---------	|------------------------------------------------------------------------------------------------------------------------------------------------------------	|
| `success`           	| Boolean 	| Whether the request is successfully finished.                                                                                                              	|
| `result`            	| JSON    	| *Only for successful requests* The dictionary of recognition results, with detailed fields included in [this section](#id-recognition-results).            	|
| `error_message`     	| String  	| *Only for failed requests* Details of the encountered error.                                                                                               	|
| `error_code`        	| String  	| *Only for failed requests* The error code for easier reference. Detailed matching can be found in [this section](#list-of-error-messages-and-error-codes). 	|


## ID recognition results
In common, all the IDs have these three fields:
| Field Name        	| Remarks                                                                       	|
|-------------------	|-------------------------------------------------------------------------------	|
| `error_count`     	| How many important fields are producing result that is not in desired format. 	|
| `max_error_count` 	| The maximum allowed error count for the current type of ID.                   	|
| `document_type`   	| The document type as inferred by the engine.                                  	|

For the specific fields for different types of ID, please check in their own sections:
[HKI](#hong-kong-id-hki), [MOO&MON](#macau-ids-moo-and-mon), [SGN](#singapore-nric-sgn), [SGP](#singapore-employment-pass--work-pass--s-pass-sgp), [EEP](#exit-entry-pass-for-visiting-hong-kong-and-macau-eep), [KRI](#korean-ids-kri), [KRD](#korean-driving-licenses-krd), [VNO](#vietnam-old-id-vno), [VNM, VNN](#vietnam-ids-before-and-after-the-update-vnm-vnn), [AUD](#australian-driving-license-aud), [KTP](#indonesian-id-ktp).
### Hong Kong ID `HKI`
| Field Name           	| Remarks                                                	|
|----------------------	|--------------------------------------------------------	|
| `chinese_name`       	| Chinese name of the ID holder.                         	|
| `telex_code`         	| Telex code of the ID holder.                           	|
| `english_name`       	| *(Important field)* English name of the ID holder.     	|
| `first_name`         	| First name of the ID holder.                           	|
| `last_name`          	| Last name of the ID holder.                            	|
| `date_of_birth`      	| *(Important field)* Date of birth of the ID holder.    	|
| `first_issue_date`   	| Date of first issue of the ID.                         	|
| `current_issue_date` 	| Date of current issue of the ID.                       	|
| `number`             	| *(Important field)* ID number.                         	|
| `sex`                	| Gender of the passport holder.                         	|
| `nationality`        	| Nationality of the ID card holder, defaulted to `HKG`. 	|
| `birth_location`     	| Place of birth of the ID holder.                       	|

### Macau IDs `MOO` and `MON`
| Field Name           	| Remarks                                              	|
|----------------------	|------------------------------------------------------	|
| `chinese_name`       	| Chinese name of the ID holder.                       	|
| `telex_code`         	| Telex code of the ID holder.                         	|
| `english_name`       	| *(Important field)* English name of the ID holder.   	|
| `first_name`         	| First name of the ID holder.                         	|
| `last_name`          	| Last name of the ID holder.                          	|
| `date_of_birth`      	| *(Important field)* Date of birth of the ID holder.  	|
| `height`             	| Height of the ID holder.                             	|
| `validity_date`      	| *(Important field)* Validity date of the ID.         	|
| `first_issue_date`   	| *(Important field)* Date of first issue of the ID.   	|
| `current_issue_date` 	| *(Important field)* Date of current issue of the ID. 	|
| `number`             	| *(Important field)* ID number.                       	|
| `nationality`        	| Nationality of the ID holder (default is 'MAC').     	|
| `expired`            	| Indicates if the ID is expired.                      	|
| `sex`                	| Gender of the ID holder.                             	|
| `birth_location`     	| Place of birth of the ID holder.                     	|

### Singapore NRIC `SGN`
* Front side:
  | Field Name         	| Remarks                                             	|
  |--------------------	|-----------------------------------------------------	|
  | `local_name`       	| Ethnic name of the ID holder.                       	|
  | `english_name`     	| *(Important field)* English name of the ID holder.  	|
  | `first_name`       	| First name of the ID holder.                        	|
  | `last_name`        	| Last name of the ID holder.                         	|
  | `date_of_birth`    	| *(Important field)* Date of birth of the ID holder. 	|
  | `number`           	| *(Important field)* ID number.                      	|
  | `race`             	| Race of the ID holder.                              	|
  | `country_of_birth` 	| Country of birth of the ID holder.                  	|
  | `sex`              	| *(Important field)* Gender of the ID holder.        	|
* Back side:
  | Field Name       	| Remarks                                       	|
  |------------------	|-----------------------------------------------	|
  | `number`         	| *(Important field)* ID number.                	|
  | `nationality`    	| Nationality of the ID holder.                 	|
  | `date_of_issue`  	| *(Important field)* Date of issue of the ID.  	|
  | `address`        	| Address of the ID holder.                     	|
  | `date_of_update` 	| *(Important field)* Date of update of the ID. 	|
  | `blood_type`     	| Blood type of the ID holder.                  	|

### Singapore Employment Pass / Work Pass / S Pass `SGP`
* Front side:
  | Field Name      	| Remarks                                            	|
  |-----------------	|----------------------------------------------------	|
  | `english_name`  	| *(Important field)* English name of the ID holder. 	|
  | `first_name`    	| First name of the ID holder.                       	|
  | `last_name`     	| Last name of the ID holder.                        	|
  | `permit_number` 	| *(Important field)* Permit number.                 	|
  | `sector`        	| Sector of the ID holder.                           	|
  | `card_number`   	| *(Important field)* Card number.                   	|
  | `employer_name` 	| Employer name.                                     	|
  | `permit_type`   	| *(Important field)* Type of permit.                	|
* Back side:
  | Field Name      	| Remarks                                             	|
  |-----------------	|-----------------------------------------------------	|
  | `date_of_birth` 	| *(Important field)* Date of birth of the ID holder. 	|
  | `number`        	| *(Important field)* ID number.                      	|
  | `nationality`   	| Nationality of the ID holder.                       	|
  | `date_of_issue` 	| Date of issue of the ID.                            	|
  | `sex`           	| Gender of the ID holder.                            	|
  | `english_name`  	| English name of the ID holder.                      	|

### Exit Entry Pass for visiting Hong Kong and Macau `EEP`
| Field Name          	| Remarks                                                    	|
|---------------------	|------------------------------------------------------------	|
| `chinese_name`      	| Chinese name of the ID holder.                             	|
| `english_name`      	| *(Important field)* English name of the ID holder.         	|
| `first_name`        	| First name of the ID holder.                               	|
| `last_name`         	| Last name of the ID holder.                                	|
| `date_of_birth`     	| *(Important field)* Date of birth of the ID holder.        	|
| `sex`               	| Gender of the ID holder.                                   	|
| `valid_since`       	| *(Important field)* Validity start date of the ID.         	|
| `valid_until`       	| *(Important field)* Validity end date of the ID.           	|
| `expired`           	| Indicates if the ID has expired.                           	|
| `issuing_authority` 	| Issuing authority of the ID.                               	|
| `issuing_location`  	| Issuing location of the ID.                                	|
| `mrz`               	| *(Important field)* Machine Readable Zone (MRZ) of the ID. 	|
| `nationality`       	| Nationality of the ID holder (default: CHN).               	|
| `number`            	| *(Important field)* ID number.                             	|

### Korean IDs `KRI`
| Field Name          	| Remarks                                                    	|
|---------------------	|------------------------------------------------------------	|
| `chinese_name`      	| Chinese name of the ID holder.                             	|
| `english_name`      	| *(Important field)* English name of the ID holder.         	|
| `first_name`        	| First name of the ID holder.                               	|
| `last_name`         	| Last name of the ID holder.                                	|
| `date_of_birth`     	| *(Important field)* Date of birth of the ID holder.        	|
| `sex`               	| Gender of the ID holder.                                   	|
| `valid_since`       	| *(Important field)* Validity start date of the ID.         	|
| `valid_until`       	| *(Important field)* Validity end date of the ID.           	|
| `expired`           	| Indicates if the ID has expired.                           	|
| `issuing_authority` 	| Issuing authority of the ID.                               	|
| `issuing_location`  	| Issuing location of the ID.                                	|
| `mrz`               	| *(Important field)* Machine Readable Zone (MRZ) of the ID. 	|
| `nationality`       	| Nationality of the ID holder (default: CHN).               	|
| `number`            	| *(Important field)* ID number.                             	|

### Korean Driving Licenses `KRD`
| Field Name              | Remarks                                   |
| ----------------------- | ----------------------------------------- |
| korean_name             | *(Important field)* Korean name of the ID holder. |
| first_name              | First name of the ID holder. |
| last_name               | Last name of the ID holder.  |
| license_type            | License type.              |
| home_address            | Home address of the ID holder. |
| number                  | *(Important field)* ID number.                   |
| license_number          | License number.            |
| serial_number           | Serial number for the license.             |
| date_of_birth           | *(Important field)* Date of birth of the ID holder. |
| sex                     | Gender of the ID holder.      |
| restrictions            | License restrictions.       |
| renewal_period          | License renewal period.     |
| nationality             | Nationality of the ID holder. |
| place_of_birth          | Place of birth of the ID holder. |
| differentiating_number  | Differentiating number extracted from the ID number.      |
| issuing_date            | *(Important field)* Date of issuance of the ID.  |
| issuing_authority       | Issuing authority of the ID. |

### Vietnam Old ID `VNO`
| Field Name   | Remarks                                           |
| ------------  | ------------------------------------------------- |
| full_name     | Full name of the ID holder.         |
| first_name    | First name of the ID holder.        |
| last_name     | Last name of the ID holder.         |
| date_of_birth | *(Important field)* Date of birth of the ID holder.     |
| number        | *(Important field)* ID number.                   |
| nationality   | Nationality of the ID holder.      |
| home_town     | Hometown of the ID holder.         |
| residence     | Residence address of the ID holder.|

### Vietnam IDs, before and after the update `VNM`, `VNN`
| Field Name    | Remarks                                           |
| -------------  | ------------------------------------------------- |
| full_name      | *(Important field)* Full name of the ID holder.         |
| first_name     | First name of the ID holder.        |
| last_name      | Last name of the ID holder.         |
| date_of_birth  | *(Important field)* Date of birth of the ID holder.     |
| home_town      | Hometown of the ID holder.         |
| number         | *(Important field)* ID number.                         |
| sex            | Gender of the ID holder.           |
| nationality    | Nationality of the ID holder.      |
| residence      | Residence address of the ID holder.|
| valid_until    | Validity date of the ID.           |
| expired        | Indicates whether the ID has expired.            |

### Vietnam Driving License `VND`
| Field Name      | Remarks                                           |
| ---------------- | ------------------------------------------------- |
| full_name        | *(Important field)* Full name of the ID holder.         |
| first_name       | First name of the ID holder.        |
| last_name        | Last name of the ID holder.         |
| date_of_birth    | *(Important field)* Date of birth of the ID holder.     |
| nationality      | Nationality of the ID holder.      |
| number           | *(Important field)* ID number.                         |
| residence        | Residence address of the ID holder.|
| license_class    | Class of the license.              |
| valid_until      | *(Important field)* Validity date of the ID.           |
| expired          | Indicates whether the ID has expired.            |
| sex              | Gender of the ID holder.           |

### Australian Driving License `AUD`
| Field Name      | Remarks                                           |
| ---------------- | ------------------------------------------------- |
| full_name        | *(Important field)* Full name of the ID holder.         |
| first_name       | *(Important field)* First name of the ID holder.        |
| last_name        | *(Important field)* Last name of the ID holder.         |
| card_number      | *(Important field)* Card number.                       |
| number           | *(Important field)* ID number.                         |
| date_of_birth    | *(Important field)* Date of birth of the ID holder.     |
| residence        | Residence address of the ID holder.|
| effective_since  | Date when the ID becomes effective.|
| valid_until      | Validity date of the ID.           |
| state            | State information engine parsed from the ID.|
| height           | Height of the ID holder.           |
| license_class    | Class of the license.              |
| conditions       | Additional conditions on the ID.   |
| sex              | Gender of the ID holder.           |
| expired          | Indicates whether the ID has expired.            |

### Indonesian ID `KTP`
| Field Name        | Remarks                                          |
| ------------------ | ------------------------------------------------ |
| number            | *(Important field)* NIK number.                        |
| name              | *(Important field)* Name of the ID holder.            |
| place_of_birth    | Place of birth of the ID holder.  |
| date_of_birth     | *(Important field)* Date of birth of the ID holder.   |
| sex               | Gender of the ID holder.          |
| blood_type        | Blood type of the ID holder.       |
| address           | Address of the ID holder.         |
| religion          | Religion of the ID holder.        |
| marital_status    | Marital status of the ID holder.  |
| occupation        | Occupation of the ID holder.      |
| citizenship       | Citizenship of the ID holder.     |
| valid_until       | *(Important field)* Validity date of the ID.          |
| expired           | Indicates whether the ID has expired.           |
| place_of_issue    | Place of issue of the ID.         |
| date_of_issue     | Date of issue of the ID.          |

## List of Error Messages and Error Codes

The following is a comprehensive list of potential error codes returned by the engine, along with their respective error messages and explanations:

| Error Code | Error Message                                        | Remarks                                                    |
|------------|-----------------------------------------------------|------------------------------------------------------------|
| `001`        | API key is either missing or incorrect.            | Please check the provided API key.            |
| `002`        | Error reading image, please check the request format. | This error occurs when the image fails to be recognized in the expected format.|
| `003`        | Image file size is too large.                      | The engine has a 10MB limit for image files; ensure they do not exceed this size.|
| `004`        | Request is not in the correct format.              | This error indicates an issue with the request format or missing key fields.|
| `005`        | Server internal error, please try again.           | In the case of an internal server error, please contact us if the issue persists.|
| `021`        | Failed to match to any type of ID supported, the recognition failed. | This implies that the recognition result does not match any of the available templates in the selection, or it contains an excessive number of errors for the recognized ID types. In such cases, you may want to advise the user to retry the process or consider using a supported ID instead. |