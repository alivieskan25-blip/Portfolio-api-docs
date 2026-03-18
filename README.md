# API Documentation Portfolio

![Markdown](https://img.shields.io/badge/Markdown-000000?style=for-the-badge&logo=markdown&logoColor=white)
![Git](https://img.shields.io/badge/Git-F05032?style=for-the-badge&logo=git&logoColor=white)
![GitHub](https://img.shields.io/badge/GitHub-181717?style=for-the-badge&logo=github&logoColor=white)
![Postman](https://img.shields.io/badge/Postman-FF6C37?style=for-the-badge&logo=postman&logoColor=white)
![JSON](https://img.shields.io/badge/JSON-000000?style=for-the-badge&logo=json&logoColor=white)
![REST API](https://img.shields.io/badge/REST_API-005571?style=for-the-badge&logo=swagger&logoColor=white)
![OpenAPI](https://img.shields.io/badge/OpenAPI-3.0.3-green?style=for-the-badge&logo=swagger&logoColor=white)

![Status](https://img.shields.io/badge/Status-Active-success?style=for-the-badge)
![Version](https://img.shields.io/badge/Version-1.3.0-blue?style=for-the-badge) 
![License](https://img.shields.io/badge/License-MIT-yellow?style=for-the-badge)

This repository contains samples of API documentation created for learning and portfolio purposes. It demonstrates technical writing skills for REST APIs using modern tools and best practices.

## 📚 Contents

* [Project Description](#project-description)
* [Documentation Structure](#documentation-structure)
* [OpenAPI Specification](#openapi-specification)
* [Technology Stack](#technology-stack)
* [Author](#author)
* [What's Next](#whats-next)

## 📖 Project Description

This repository includes documentation samples for two different APIs:

* **[Petstore API](https://petstore.swagger.io)** — a popular learning API that demonstrates all CRUD operations. Perfect for understanding basic REST concepts.
* **[GitHub REST API](https://docs.github.com/en/rest)** — a real-world API used by millions of developers. All examples are tested with real requests (using my own repositories and tokens).

The goal is to show the ability to document various types of endpoints, work with different data structures, and handle both simple and complex API scenarios. The collection now includes endpoints for retrieving data, creating resources, updating repositories, and listing pull requests.

## 📂 Documentation Structure

### 📁 Petstore API
**File:** [`petstore-api.md`](petstore-api.md)

Documentation for the Swagger Petstore API — a classic learning API.  
**Endpoints covered (all CRUD operations):**
* `GET /pet/{petId}` — get pet by ID
* `GET /pet/findByStatus` — search pets by status (`available`, `pending`, `sold`)
* `POST /pet` — add a new pet to the store
* `PUT /pet` — update an existing pet
* `DELETE /pet/{petId}` — delete a pet by ID

### 📁 GitHub API
**File:** [`github-api.md`](github-api.md)

Documentation for the real GitHub REST API. All examples are tested with live data.

#### GET endpoints (retrieving data):
| Endpoint | Description |
|----------|-------------|
| `GET /users/{username}` | Get public information about a GitHub user |
| `GET /users/{username}/repos` | List public repositories for a user |
| `GET /repos/{owner}/{repo}/commits` | List commits in a repository |
| `GET /users/{username}/starred` | List repositories starred by a user |
| `GET /repos/{owner}/{repo}/pulls` | List pull requests in a repository |

#### POST endpoints (creating data):
| Endpoint | Description |
|----------|-------------|
| `POST /repos/{owner}/{repo}/issues` | Create a new issue in a repository (tested on my own repo) |

#### PATCH endpoints (updating data):
| Endpoint | Description |
|----------|-------------|
| `PATCH /repos/{owner}/{repo}` | Update repository settings (name, description, visibility, etc.) |

## 📖 OpenAPI Specification

In addition to the human-readable Markdown documentation, this repository includes an **OpenAPI 3.0.3** specification for the GitHub API endpoints.

**File:** [`openapi.yaml`](openapi.yaml)

This machine-readable specification allows you to:
- Generate interactive API documentation using tools like Swagger UI or Stoplight
- Import into API testing tools (Postman, Insomnia)
- Validate API requests and responses
- Generate client SDKs in various programming languages

### 🔍 Currently documented endpoints in OpenAPI:

- `GET /users/{username}` — Get user information
- `GET /users/{username}/repos` — List user repositories
- `GET /users/{username}/starred` — List repositories starred by a user

### 👀 Viewing the OpenAPI spec

You can view this specification online:

1. **Option 1:** Copy the contents of [`openapi.yaml`](openapi.yaml) and paste into [Swagger Editor](https://editor.swagger.io/)
2. **Option 2:** (Coming soon) View on GitHub Pages

## 🛠 Technology Stack

* **Markdown** — for documentation design
* **Git** — for version control
* **GitHub** — for hosting and collaboration
* **Postman** — for API testing and generating real examples
* **JSON** — for API response examples
* **OpenAPI/Swagger** — for machine-readable API specification

## 👤 Author

**Alexander Subbotin**  
Linguistics student at PSU (Perm State University), currently on academic mobility at ADA University (Baku).  
Learning API documentation and technical writing.

* **GitHub:** [alivieskan25-blip](https://github.com/alivieskan25-blip)
* **Languages:** English (B2), Spanish (A2), Russian (native)

## 🚀 What's Next

I'm continuously improving my skills. Planned additions:
- Add more endpoints to the OpenAPI specification
- Document more complex GitHub endpoints (search API, organization endpoints)
- Create documentation for a public API in Spanish
- Set up GitHub Pages to automatically publish the OpenAPI documentation
- Write a blog post about transitioning from linguistics to technical writing

---

*Last updated: March 2026*