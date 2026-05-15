<div align="center">
<br/>

<h1>
  <img src="https://readme-typing-svg.demolab.com?font=Sora&weight=800&size=36&pause=1000&color=E6EDF3&center=true&vCenter=true&width=600&lines=Reqres+API+Test+Suite" alt="Title"/>
</h1>

<p>
  <img src="https://readme-typing-svg.demolab.com?font=JetBrains+Mono&weight=400&size=15&pause=1000&color=8B949E&center=true&vCenter=true&width=620&lines=postman+%2B+Newman+%7C+Advanced+Auth+%7C+HTML+Reports" alt="Subtitle"/>
</p>

<br/>

<p>
  <img src="https://img.shields.io/badge/Newman-v6.x-FF6C37?style=for-the-badge&logo=postman&logoColor=white" alt="Newman"/>
  <img src="https://img.shields.io/badge/Node.js-18%2B-339933?style=for-the-badge&logo=nodedotjs&logoColor=white" alt="Node.js"/>
  <img src="https://img.shields.io/badge/Postman-Collection_v2.1-FF6C37?style=for-the-badge&logo=postman&logoColor=white" alt="Postman"/>
  <img src="https://img.shields.io/badge/License-MIT-blue?style=for-the-badge" alt="License"/>
</p>

<p>
  <img src="https://img.shields.io/badge/Tests-Passing-2ea44f?style=for-the-badge&logo=github-actions&logoColor=white" alt="Tests"/>
  <img src="https://img.shields.io/badge/Coverage-REST_API-0078D4?style=for-the-badge&logo=azuredevops&logoColor=white" alt="Coverage"/>
  <img src="https://img.shields.io/badge/Report-HTMLExtra-6f42c1?style=for-the-badge&logo=html5&logoColor=white" alt="Report"/>
  <img src="https://img.shields.io/badge/Maintained-Yes-success?style=for-the-badge" alt="Maintained"/>
</p>

<br/>
<p>
  <img src="https://skillicons.dev/icons?i=postman,nodejs,js,npm&theme=dark" />
</p>

<br/>

</div>

---

## Table of Contents

<div align="center">

| Section | Description |
|---|---|
| [Project Overview](#-project-overview) | Goals, scope, and framework design |
| [Architecture](#-test-suite-architecture) | Test folders and scenario breakdown |
| [Execution Reports](#-execution-summary) | Dashboard, metrics, and result screenshots |
| [Installation](#-installation) | Prerequisites and environment setup |
| [Running Tests](#-running-tests) | CLI execution and report generation |
| [Author](#-author) | Maintainer information |

</div>

---

## Project Overview

<table>
<tr>
<td>

This repository contains a production-grade API automation test suite built against the advanced **[Reqres.in](https://reqres.in) Custom Projects & Collections API**. The framework is engineered using **Postman** for request modeling, test scripting, and environment management — and executed via **Newman** to produce rich, interactive HTML reports with `newman-reporter-htmlextra`.

The suite covers complex, real-world API validations:

- **Advanced Authentication Flow** — Magic Link request, token verification, and dynamic Session Token generation.
- **Secure Collections API** — Header-based API key authorization and environment scoping (`X-Reqres-Env`).
- **Data Integrity Verification** — Asserting exact field values (e.g., `name`, `role`, `department`) against expected datasets.
- **Negative Testing** — Deliberate bad-request scenarios asserting correct `4xx` error behavior (Invalid API keys, Expired Tokens, Wrong Endpoints).

</td>
</tr>
</table>

---

## Test Suite Architecture

### Q3 — Authentication (Magic Link & Verify)

<table>
<tr><td><b>Method</b></td><td><code>POST</code></td></tr>
<tr><td><b>Endpoints</b></td><td><code>/api/app-users/login</code> & <code>/api/app-users/verify</code></td></tr>
<tr><td><b>Objective</b></td><td>Request a magic link, verify the token, and capture the session token.</td></tr>
</table>

Simulates a passwordless login flow. The test scripts:
- Assert `200 OK` HTTP status for both requests.
- Verify the system successfully sends the magic link.
- Dynamically extract and store the `session_token` into the environment for downstream authenticated requests.

---

### Q4 — GET User Records from Collection

<table>
<tr><td><b>Method</b></td><td><code>GET</code></td></tr>
<tr><td><b>Endpoint</b></td><td><code>/api/collections/api-testing-users/records</code></td></tr>
<tr><td><b>Objective</b></td><td>Retrieve records using API keys and validate exact field-level data integrity.</td></tr>
</table>

Fetches records from a custom secured collection. The test scripts:
- Assert `200 OK` HTTP status and validate response length.
- Strictly verify custom fields like `name`, `email`, and `role`.
- Validate response times are within acceptable limits.

---

### Q5 — Full Profile Update (PUT)

<table>
<tr><td><b>Method</b></td><td><code>PUT</code></td></tr>
<tr><td><b>Endpoint</b></td><td><code>/api/collections/api-testing-users/records/{recordId}</code></td></tr>
<tr><td><b>Objective</b></td><td>Perform a full resource replacement and assert updated fields.</td></tr>
</table>

Replaces the entire record payload. The test scripts:
- Assert `200 OK` HTTP status.
- Verify updated fields (`name`, `role`, `department`, `status`) are reflected.
- Validate the system-generated `updated_at` timestamp.

---

### Q6 — Partial Field Update (PATCH)

<table>
<tr><td><b>Method</b></td><td><code>PATCH</code></td></tr>
<tr><td><b>Endpoint</b></td><td><code>/api/collections/api-testing-users/records/{recordId}</code></td></tr>
<tr><td><b>Objective</b></td><td>Modify a single field in isolation without overwriting the full resource.</td></tr>
</table>

Targeted update of specific fields. The test scripts:
- Assert `200 OK` HTTP status.
- Verify the targeted `status` field strictly reflects the new value ("Active").
- Confirm only the intended field was sent in the request body.

---

### Q8 — Negative Testing & Error Handling

<table>
<tr><td><b>Methods</b></td><td><code>GET</code>, <code>POST</code></td></tr>
<tr><td><b>Objective</b></td><td>Validate the API's error-handling robustness under deliberately malformed inputs.</td></tr>
</table>

A dedicated QA segment for **negative testing**:

| Scenario | Expected Status | Assertion |
|---|---|---|
| Request with Invalid/Missing API Key | `403 Forbidden` | Access denied validation |
| Request to a non-existent Collection | `404 Not Found` | Endpoint error validation |
| Verifying an already Used/Expired Token | `400 Bad Request` | Error message validation |

---
## Execution Summary

<div align="center">

### Dashboard Overview

<img src="https://github.com/user-attachments/assets/4b74f801-e306-434f-a1dc-54dd728bd52e" alt="Dashboard Summary" width="85%"/>

<br/><br/>

### Total Requests Breakdown

<img src="https://github.com/user-attachments/assets/4dd54b29-5e4e-40b0-94b7-24a245ed6bf8" alt="Total Requests" width="85%"/>

<br/><br/>

### Skipped Tests

<img src="https://github.com/user-attachments/assets/d8bcdf21-8f28-4ace-aef8-d561e91a62a2" alt="Skipped Tests" width="85%"/>

<br/><br/>

### Failed Tests

<img src="https://github.com/user-attachments/assets/582b776e-95fe-409c-bae0-6263a7f1d745" alt="Failed Tests" width="85%"/>

</div>

---

## Project Structure

```
reqres-api-automation-suite/
├── ReqRes Automation Collection.postman_collection.json  # Core Postman collection
├── ReqRes Environment.postman_environment.json           # Mandatory environment variables
├── newman/                                               # Auto-generated report output
│   └── test-report.html
└── README.md
```
---

## Installation

### Prerequisites

<p>
  <img src="https://img.shields.io/badge/Node.js-%3E%3D18.0.0-339933?style=flat-square&logo=nodedotjs&logoColor=white"/>
  <img src="https://img.shields.io/badge/npm-%3E%3D9.0.0-CB3837?style=flat-square&logo=npm&logoColor=white"/>
  <img src="https://img.shields.io/badge/Git-latest-F05032?style=flat-square&logo=git&logoColor=white"/>
</p>

Ensure **Node.js v18+** and **npm** are installed on your machine before proceeding.

---

### Step 1 — Install Newman and HTML Reporter Globally

```bash
npm install -g newman
npm install -g newman-reporter-htmlextra
```

---

### Step 2 — Clone the Repository

```bash
git clone https://github.com/TheSourav-001/reqres-api-automation-suite.git
cd reqres-api-automation-suite
```

---

## Running Tests

### Execute Full Test Suite with Environment Variables and HTML Report

```bash
newman run "ReqRes Automation Collection.postman_collection.json" \
  -e "ReqRes Environment.postman_environment.json" \
  -r htmlextra,cli \
  --reporter-htmlextra-export "./newman/test-report.html"
```

---

> **Note:**  
> If the magic login token expires, manually replace the token value inside the **Verify Token** request body before re-running the collection.
---

## Author

<div align="center">

<br/>

<img src="https://readme-typing-svg.demolab.com?font=Sora&weight=700&size=18&pause=1000&color=58A6FF&center=true&vCenter=true&width=400&lines=Sourav+Dipto+Apu" alt="Author"/>

<br/>

<p>
  <img src="https://img.shields.io/badge/Role-QA Enthusiast-0078D4?style=for-the-badge&logo=azuredevops&logoColor=white" alt="Role"/>
</p>

<p>
  <a href="https://github.com/TheSourav-001">
    <img src="https://img.shields.io/badge/GitHub-Follow-181717?style=for-the-badge&logo=github&logoColor=white" alt="GitHub"/>
  </a>
  <a href="https://linkedin.com/in/yourprofile">
    <img src="https://img.shields.io/badge/LinkedIn-Connect-0A66C2?style=for-the-badge&logo=linkedin&logoColor=white" alt="LinkedIn"/>
  </a>
</p>

</div>

---

<div align="center">

<br/>

<sub>Built with precision and maintained with care.</sub>

<br/>

<img src="https://img.shields.io/badge/API%20Under%20Test-Reqres.in-orange?style=flat-square" alt="Reqres"/>
<img src="https://img.shields.io/badge/Framework-Newman_HTMLExtra-6f42c1?style=flat-square" alt="Framework"/>
<img src="https://img.shields.io/badge/Standard-REST_API_Testing-2ea44f?style=flat-square" alt="Standard"/>

<br/><br/>

</div>
