# TC — Sales Order — Performance

- **Module**: Sales Order
- **Section**: Load Time, Response Time, Concurrency
- **Test Type**: Automated (k6)
- **TC Range**: TC-SO-101 – TC-SO-105
- **Reference**: [Specs](/specs/ERP-DES-SAL-Order.md)

---

## Automated Tests

| TC ID     | Test Case                                                      | Precondition                        | Steps                                                                                                                            | Expected Result                                                            | Priority |
| :-------- | :------------------------------------------------------------- | :---------------------------------- | :------------------------------------------------------------------------------------------------------------------------------- | :------------------------------------------------------------------------- | :------- |
| TC-SO-101 | Sales Order list page loads within 2 seconds with 1000 orders  | 1000 Sales Orders exist             | 1. Simulate navigating to the Sales Order list<br/>2. Start timer on request<br/>3. Measure until list fully rendered            | Page loads and renders within 2 seconds                                    | High     |
| TC-SO-102 | Filter results return within 1 second                          | 1000 orders; filter applied via API | 1. Send GET to Sales Order list API with Stage = "Approved"<br/>2. Measure time from request to response                         | API responds with filtered results within 1 second                         | High     |
| TC-SO-103 | Order with 50 line items saves within 3 seconds                | A Sales Order payload with 50 lines | 1. Build a POST/PUT payload with 50 line items<br/>2. Send the request<br/>3. Measure time to response                           | Order saves successfully within 3 seconds                                  | High     |
| TC-SO-104 | 100 concurrent users can view the list without timeout         | System running under test env       | 1. Simulate 100 virtual users with k6<br/>2. All users hit the Sales Order list simultaneously<br/>3. Measure response and errors | All requests succeed; response time < 3 seconds; error rate = 0%           | Medium   |
| TC-SO-105 | Export of 500 orders completes within 5 seconds                | 500 Sales Orders exist              | 1. Trigger the export action with 500 records<br/>2. Measure time until file download starts                                     | Export file ready within 5 seconds                                         | Medium   |

---

```js
import http from 'k6/http'
import { check, sleep } from 'k6'

// ─────────────────────────────────────────────
// TC-SO-101: List Page Load (1000 orders)
// ─────────────────────────────────────────────
export const options = {
  vus: 1,
  duration: '10s',
}

export default function () {
  const url = 'https://app.happyerp.com/api/sales-orders?pageSize=20&page=1'

  const res = http.get(url, {
    headers: { Authorization: `Bearer ${__ENV.TOKEN}` },
  })

  check(res, {
    'TC-SO-101 | status is 200': (r) => r.status === 200,
    'TC-SO-101 | response time < 2000ms': (r) => r.timings.duration < 2000,
  })

  sleep(1)
}

// ─────────────────────────────────────────────
// TC-SO-102: Filter Response < 1s
// ─────────────────────────────────────────────
export function filterTest() {
  const url = 'https://app.happyerp.com/api/sales-orders?stage=Approved'

  const res = http.get(url, {
    headers: { Authorization: `Bearer ${__ENV.TOKEN}` },
  })

  check(res, {
    'TC-SO-102 | status is 200': (r) => r.status === 200,
    'TC-SO-102 | response time < 1000ms': (r) => r.timings.duration < 1000,
  })

  sleep(1)
}

// ─────────────────────────────────────────────
// TC-SO-104: 100 Concurrent Users
// ─────────────────────────────────────────────
export const concurrentOptions = {
  vus: 100,
  duration: '30s',
}

export function concurrentTest() {
  const res = http.get('https://app.happyerp.com/api/sales-orders', {
    headers: { Authorization: `Bearer ${__ENV.TOKEN}` },
  })

  check(res, {
    'TC-SO-104 | status is 200': (r) => r.status === 200,
    'TC-SO-104 | response time < 3000ms': (r) => r.timings.duration < 3000,
  })

  sleep(1)
}
```
