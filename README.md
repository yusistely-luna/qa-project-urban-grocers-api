# 🛒 Urban Grocers — API QA

**API Testing · Postman · Manual QA** — TripleTen Bootcamp Project (2026)
> 🎓 Sprint Project

---

## 🎯 Problem

Manual API testing of three core Urban Grocers endpoints — adding products to a kit, creating a new kit, and the "Order and Go" delivery-availability service. The goal was to validate HTTP status codes, error messages, and system behavior against valid, invalid, null, boundary, and wrong-type input, and to surface where the API's input validation didn't match its documented rules.

---

## 📊 Results & Impact

| Metric              | Value        |
| -------------------- | ------------- |
| Test cases executed  | **72**        |
| Passed                | **34 (47%)**  |
| Failed                | **38 (53%)**  |
| Defects reported      | **38**        |
| Endpoints covered     | **3**         |

---

## ⚙️ What I Did

- Designed and executed 72 test cases across three endpoints using Postman, covering equivalence classes, boundary values, null fields, missing fields, and wrong data types
- Tested `POST /api/v1/kits/:id/products` (adding products to a kit — including a 30-item-per-kit limit and duplicate-ID quantity summing)
- Tested `POST /api/v1/kits` (kit creation — name length limits, special characters, non-Latin characters, required fields)
- Tested `POST /order-and-go/v1/delivery` (delivery availability and cost — boundary values for `deliveryTime`, `productsCount`, and `productsWeight`, plus null, negative, decimal, and wrong-type inputs)
- Isolated 38 defects and filed each one in Jira with the exact request/response pair
- Wrote a formal test report with an executive summary, per-endpoint breakdown, and a go/no-go recommendation
- Preserved evidence of the original Jira board and a representative Postman request/response as screenshots, since direct access to the board is no longer available

### Results by Endpoint

| Endpoint                                  | Total | Passed | Failed | Success |
| -------------------------------------------- | ----- | ------ | ------ | ------- |
| Add products to a kit                        | 14    | 7      | 7      | 50%     |
| Create a new kit                              | 12    | 5      | 7      | 42%     |
| "Order and Go" delivery service               | 46    | 22     | 24     | 48%     |

---

## 🐞 Key Defect Patterns

| Pattern                                                              | Example ID(s)         |
| ------------------------------------------------------------------------ | ----------------------- |
| Non-existent product ID accepted instead of returning 404                | S4QG6-1                |
| Malformed JSON body doesn't return a clean 400                           | S4QG6-2                |
| Missing required field still succeeds, or returns 500 instead of 400     | S4QG6-3, S4QG6-4, S4QG6-7 |
| Kit name accepted outside the documented length limits                   | S4QG6-5, S4QG6-6        |
| Special/non-Latin characters accepted in kit name                        | S4QG6-8                |
| "Order and Go" returns 200 OK for missing, null, negative, decimal, or wrong-type `deliveryTime` / `productsCount` / `productsWeight` | S4QG6-15 to S4QG6-37 |
| Duplicate product ID in one request doesn't sum quantities correctly      | S4QG6-38                |

> The original Jira board used to file these defects is no longer accessible. Screenshots of the board and of a representative Postman request/response are included as evidence in the test report.

---

## ✅ Learning

The clearest pattern across 38 defects wasn't any single broken field — it was a systemic gap between the status code the API *should* return and the one it actually does. Nearly every failure fell into the same shape: invalid input got a 200 OK or an unhandled 500 instead of a clean 400 Bad Request. That's a more useful finding for the dev team than 38 unrelated bugs, because it points at one validation layer to fix rather than dozens of individual endpoints. Testing boundary values on three numeric fields at once (`deliveryTime`, `productsCount`, `productsWeight`) also made clear how much a service's business logic can hide behind a single "200 OK" response — the status code alone said nothing about whether the *decision* inside it was correct.

---

## 🛠️ Skills

Manual Testing · API Testing · Postman · REST APIs · Equivalence Partitioning · Boundary Value Analysis · Bug Reporting · Jira · Test Reporting · HTTP Status Code Validation

---

## 📁 Project Structure

- `Yusistely_Luna_Urban_Grocers_Checklist.xlsx` — full 72-case checklist with test data, steps, expected/actual results, and status per test
- `Urban_Grocers_Informe_de_Prueba.docx` — formal test report: executive summary, scope, results by endpoint, key defects, recommendations, and an evidence annex (Jira board and Postman collection screenshots)
