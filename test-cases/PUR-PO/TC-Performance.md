# TC — Purchase Order — Performance

- **Module**: Purchase Order
- **Section**: Load, Response Time, Concurrency
- **Test Type**: Automated (k6 / Artillery)
- **Reference**: [UI Doc](/UI/PUR-PO/ERP_UI_PUR_PO.md)

---

## Automated Tests

| TC ID     | Test Case                                                 | Precondition                            | Steps                                                                                                                                                  | Expected Result                                                            | Priority |
| :-------- | :-------------------------------------------------------- | :-------------------------------------- | :----------------------------------------------------------------------------------------------------------------------------------------------------- | :------------------------------------------------------------------------- | :------- |
| TC-PO-096 | PO list page loads within 2 seconds with 1000 POs         | 1000 POs exist in the system            | 1. Simulate a user navigating to the PO list page<br/>2. Start a timer on page request<br/>3. Measure time until the list is fully rendered            | Page loads and renders within 2 seconds                                    | High     |
| TC-PO-097 | Filter results return within 1 second                     | 1000 POs exist; filters applied via API | 1. Send a GET request to the PO list API with Stage = "Approved" filter<br/>2. Measure time from request to response                                   | API responds with filtered results within 1 second                         | High     |
| TC-PO-098 | PO with 50 product lines saves within 3 seconds           | A PO with 50 product lines prepared     | 1. Create a PO payload with 50 product lines, all required fields filled<br/>2. Send a POST/PUT request to save the PO<br/>3. Measure time to response | PO saves successfully within 3 seconds                                     | High     |
| TC-PO-099 | 100 concurrent users can view the PO list without timeout | System running under test environment   | 1. Use k6 or Artillery to simulate 100 virtual users<br/>2. All users navigate to PO list simultaneously<br/>3. Measure response time and error rate   | All requests succeed; response time stays under 3 seconds; error rate = 0% | Medium   |
| TC-PO-100 | Export of 500 POs completes within 5 seconds              | 500 POs exist in the system             | 1. Trigger the export action on the PO list page with 500 records visible<br/>2. Measure time from export click to file download start                 | Export file is ready for download within 5 seconds                         | Medium   |

---

```js
import http from 'k6/http'
import { check, sleep } from 'k6'

// ─────────────────────────────────────────────
// TC-PO-096: PO List Page Load (1000 records)
// ─────────────────────────────────────────────
export const options = {
  vus: 1,
  duration: '10s',
}

export default function () {
  // Arrange
  const url = 'https://app.happyerp.com/api/purchase-orders?pageSize=20&page=1'

  // Act
  const res = http.get(url, {
    headers: { Authorization: `Bearer ${__ENV.TOKEN}` },
  })

  // Assert — TC-PO-096: must load within 2000ms
  check(res, {
    'TC-PO-096 | status is 200': (r) => r.status === 200,
    'TC-PO-096 | response time < 2000ms': (r) => r.timings.duration < 2000,
  })

  sleep(1)
}

// ─────────────────────────────────────────────
// TC-PO-099: 100 Concurrent Users
// ─────────────────────────────────────────────
export const concurrentOptions = {
  vus: 100,
  duration: '30s',
}

export function concurrentTest() {
  // Act
  const res = http.get('https://app.happyerp.com/api/purchase-orders', {
    headers: { Authorization: `Bearer ${__ENV.TOKEN}` },
  })

  // Assert — TC-PO-099: all users must succeed under 3000ms
  check(res, {
    'TC-PO-099 | status is 200': (r) => r.status === 200,
    'TC-PO-099 | response time < 3000ms': (r) => r.timings.duration < 3000,
  })

  sleep(1)
}
```
