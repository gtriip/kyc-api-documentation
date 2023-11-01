# ID Recognition API
This is the API for ID recognition. The ID types we currently support are:
* Singapore
  * NRIC for locals and PRs
  * Work Permit, Employment Pass, S Pass
* Hong Kong
  * Hong Kong ID (Both old and new)
* Macau
  * Old Macau ID
  * New Macau ID
* China
  * Exit-Entry Permit for Hong Kong and Macau
* Indonesia
  * KTP (Kartu Tanda Penduduk, Indonesian ID Card)
* Korea
  * Korean Driving License
  * Korean ID Card
* Australia
  * Driving license (Victoria, New South Wales, Queensland)
* Vietnam
  * ID - All versions
  * Driving License

We are still adding more IDs to this list.

## Request Fields
| Request Field Name      | Required | Type          | Remarks |
|-------------------------|----------|---------------|---------|
| `image_front` or `image_front_bytes`| YES      | File or Bytes | You must provide one of these options, depending on your requirements. For `image_front`, we accept image files directly, while for `image_front_bytes`, we expect a **base64-encoded** image. Please refer to the example notebook and Postman for detailed use cases of this field.
| `skip_face_detect`      | NO       | List of selections       | This field is optional, the default value is an empty list. This field let you decide which types of the IDs should be ignored for face detection. You should provide a list of selections in 