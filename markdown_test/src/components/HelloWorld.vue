<template>
  <div class="test">
    <div v-html="result"></div>
    <!-- <div>{{ result }}</div> -->
  </div>
</template>
<script setup>
import markdownit from 'markdown-it'
import { onMounted, ref } from 'vue';
const md = markdownit({
  html: true,
  // linkify: true,
  // typographer: true
})
const result = ref(null)
const text =  "# **View Continuous Blood Glucose Data** \n\n## **GET /cgms** \nhttps://api.i-sens.com/v1/public/cgms\n\n ### 1. Overview\n - This API allows you to retrieve continuous blood glucose data measured by the sensor and displayed in the CareSens Air app.\n\n ### 2. Request URL and calling method\n - Authorization is performed using an OAuth 2.0 Bearer token in the Request Header.\n<br />\n\n``` code\nGET /v1/public/cgms HTTP/1.1\nHost: api.i-sens.com\nAuthorization: Bearer ${access_token}\n```\n<br />\n\n### 3. Request Header\n\n| Parameter Name | Details | Required |\n| --- | --- | --- |\n| Authorization | Bearer Access Token (access_token) | O |\n<br />\n\n### 4. Request Parameter\n\n| Parameter Name | Type | Details | Example | Required |\n| --- | --- | --- | --- | --- |\n| start | string | Search Start Date | 2022-06-01T00:00:00+09:00 | O |\n| end | string | Search End Date | 2022-06-30T23:59:59+09:00 | O | \n\n* Search query conditions: Up to 3 months\n<br />\n\n### 5. Request Example\n\n```bash\ncurl -X GET \"https://api.i-sens.com/v1/public/cgms?start=${start}&end=${end}\" \\\n-H \"Authorization: Bearer ${access_token}\" \\\n--data-raw \"\"\n``` \n<br />\n\n### 6. Response Parameter\n\n| Parameter name | Type | Details |\n| --- | --- | --- |\n| serial | string | Sensor serial number |\n| seq_number | int | CGM sequence number |\n| event_at | string | CGM measurement time |\n| initial_value | float | Initial value (unit: mg/dL) |\n| value | float | Calibration value (unit: mg/dL) |\n| stage | int | 0: Before calibration, 1: Calibrating, 2: Calibration complete |\n| trend | int | 0: Unknown, 1: rapidly decreasing, 2: decreasing, 3: gradually decreasing, 4: stable, 5: gradually increasing, 6: increasing, 7: rapidly increasing |\n| trend_rate | float | Rate of blood glucose change |\n| error_code | int | Error code |\n| min_max_flag | int | 0: 40~500, 1: Below 40, 2: Above 500 |\n<br />\n    \n\n### 7. Response: Success \n\n```JSON\nHTTP/1.1 200 OK\n[\n     {\n        \"event_at\": \"2023-10-12T21:59:28Z\",\n        \"serial_number\": \"TEST_00126\",\n        \"seq_number\": 3554,\n        \"initial_value\": 157.543533,\n        \"value\": 159.704987,\n        \"trend_rate\": -1.233333,\n        \"trend\": 3,\n\t\"stage\": 2,\n        \"error_code\": 0,\n        \"min_max_flag\": 0\n    },\n    {\n        \"event_at\": \"2023-10-12T22:04:27Z\",\n        \"serial_number\": \"TEST_00126\",\n        \"seq_number\": 3555,\n        \"initial_value\": 153.631485,\n        \"value\": 158.421967,\n        \"trend_rate\": -1.200000,\n        \"trend\": 3,\n\t\"stage\": 2,\n        \"error_code\": 0,\n        \"min_max_flag\": 0\n    },\n    {\n        \"event_at\": \"2023-10-12T22:09:28:27Z\",\n        \"serial_number\": \"TEST_00126\",\n        \"seq_number\": 3556,\n        \"initial_value\": 269,\n        \"value\": 269,\n        \"trend_rate\": -0.866667,\n        \"trend\": -4,\n\t\"stage\": 2,\n        \"error_code\": 0,\n        \"min_max_flag\": 0\n    }\n]\n```\n\n ### 8. Response: Fail, Invalid Access Token. \n\n```JSON\nHTTP/1.1 401 Unauthorized \n{\n      \"code\": \"expired_token\",\n      \"message\": \"The token has expired.\",\n      \"param\": \"access_token\", \n      \"value\": \"\"\t  \n}\n```\n<br />"



onMounted(() => {
  result.value = md.render(text)
})
</script>
<style>
.test {
  border: 1px solid black;
  width: 100%;
  text-align: start;
}
</style>