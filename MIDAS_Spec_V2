# MIDAS (Market Informed Demand Automation Server) Technical Specification

**Version:** 1.0  
**Date:** April 2025  
**Maintainer:** California Energy Commission (CEC)

---

## Overview

MIDAS is a centralized, RESTful data platform developed by the California Energy Commission to provide public access to time-varying electricity rates, GHG emissions signals, and grid reliability alerts such as CAISO Flex Alerts. It supports automated load shifting, customer energy management systems, and flexible demand programs.

MIDAS enables seamless integration between Load Serving Entities (LSEs), customers, aggregators, and energy service providers by standardizing rate signal formats and providing a secure, real-time API for data exchange.

---

## Purpose

MIDAS fulfills California's Load Management Standards by making LSEs' time-variant rates available in machine-readable formats. It provides transparency, supports automation in energy management, and improves grid flexibility.

---

## System Architecture

MIDAS includes:

- **MIDAS API Server**: Stateless, token-authenticated REST API.
- **MIDAS Database**: Stores rate, GHG, alert, and historical data.
- **MIDAS Web Tool**: Handles user registration and account management.
- **External Integrations**: Fetches GHG data and Flex Alerts.
- **Firewall & Load Balancer**: Secures and distributes traffic.

### Diagram: System Architecture

![System Architecture](images/system-architecture.png)

---

## Data Flow Summary

1. **LSE Uploads**: Upload new rates and holidays via `POST` endpoints.
2. **Validation/Storage**: Validate against schema and store.
3. **External Signals**: Fetch GHG and Flex Alert data.
4. **Client Access**: Authenticated `GET` requests return current or historical data.

### Diagram: API Data Flow

![Data Flow](images/data-flow-diagram.png)

---

## Authentication & Token Workflow

### Steps

1. User registers via `/Registration`.
2. User verifies email.
3. User logs in via `/Token` with Basic Auth.
4. MIDAS issues short-lived token.
5. Token is used in `Authorization: Bearer <token>` for future requests.

### Diagram: Auth Flow

![Authentication Flow](images/authentication-flow.png)

---

## API Overview

All API requests are made over HTTPS to:

```
https://midasapi.energy.ca.gov/api/
```

### Request Format

Use headers:

- `Content-Type: application/json` or `application/xml`
- `Authorization: Bearer <token>`

---

## API Endpoints and Methods

| Endpoint           | Method | Description                                 |
|--------------------|--------|---------------------------------------------|
| `/Registration`    | POST   | Create a new user account                   |
| `/Token`           | GET    | Obtain a temporary access token             |
| `/Holiday`         | GET    | Retrieve list of holidays                   |
| `/Holiday`         | POST   | Upload new holiday entries (LSE-only)       |
| `/ValueData`       | GET    | Retrieve available rates/signals/metadata   |
| `/ValueData`       | POST   | Upload rate and value records (LSE-only)    |
| `/HistoricalList`  | GET    | List historical rates for a utility         |
| `/HistoricalData`  | GET    | Retrieve values over a historical range     |

---

## Common Requests and Responses

### Token Request

```http
GET /api/Token
Authorization: Basic amFuZWRvZTpNeVNlY3JldFBhc3MxMjM=
```

### Rate List

```http
GET /api/ValueData?SignalType=1
Authorization: Bearer <token>
```

### Rate Data by RIN

```http
GET /api/ValueData?ID=USCA-PGPG-TOU4-0000000000&QueryType=alldata
```

### JSON Response Sample

```json
{
  "RateID": "USCA-PGPG-TOU4-0000000000",
  "RateName": "E-1 Tiered Residential",
  "Sector": "Residential",
  "ValueInformation": [
    {
      "ValueName": "Summer On Peak",
      "DateStart": "2025-06-01",
      "DateEnd": "2025-09-30",
      "DayStart": "Monday",
      "TimeStart": "13:00:00",
      "Value": 0.36
    }
  ]
}
```

---

## Lookup Tables

Use `GET /ValueData?LookupTable={name}` to retrieve:

- Country
- State
- Distribution
- Energy
- RateType
- Sector
- EndUse
- Unit
- DayType
- TimeZone

---

## Database Schema

### RateInfo Table

| Field      | Description                 |
|------------|-----------------------------|
| RateID     | Unique rate identifier       |
| RateName   | Human-readable name          |
| Sector     | Customer sector              |
| TimeZone   | Local time reference         |

### Value Table

| Field      | Description                 |
|------------|-----------------------------|
| DateStart  | Start of value period        |
| TimeStart  | Time of day start            |
| Value      | Numeric price or signal      |
| Unit       | Unit of measure              |

### HistoricalData Table

Same schema as Value Table, used for archiving superseded values.

---

## Error Codes and Handling

| Status | Meaning                 |
|--------|-------------------------|
| 200    | OK                      |
| 400    | Bad Request             |
| 401    | Unauthorized            |
| 403    | Forbidden               |
| 404    | Not Found               |
| 500    | Internal Server Error   |

Error messages are returned in plain text or JSON.

---

## Performance & Scaling

- Rate limit enforced per IP/token.
- GHG values cached; Flex Alerts queried live.
- 50,000 value record limit per rate.
- Token TTL: 10 minutes.
- Load-balanced stateless API.

---

## Logging & Monitoring

- API access and errors are logged.
- Failed login attempts are monitored.
- Data uploads include audit trail.

---

## Glossary

| Term | Description                          |
|------|--------------------------------------|
| LSE  | Load Serving Entity                  |
| RIN  | Rate Identification Number           |
| GHG  | Greenhouse Gas                       |
| TOU  | Time-of-Use                          |
| API  | Application Programming Interface    |

---

## Contact

**Email:** midas@energy.ca.gov  
**Web Tool:** Accessible via MIDAS homepage  
**Support:** Mon–Fri, 9am–5pm PT

---

*End of Document*
