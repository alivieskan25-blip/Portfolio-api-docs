# GitHub API Documentation

Example of documentation for the GitHub public API.
GitHub API allows you to programmatically get data about users, repositories, commits, and more.

## Contents

* [Base URL and Authorization](#base-url-and-authorization)
* [Endpoints](#endpoints)
  * [GET /users/{username} — Get user information](#get-usersusername--get-user-information)
  * [GET /users/{username}/repos — Get user repositories](#get-usersusernamerepos--get-user-repositories)
  * [GET /repos/{owner}/{repo}/commits — Get repository commits](#get-reposownerrepocommits--get-repository-commits)

## Base URL and Authorization

**Base URL:** `https://api.github.com`

GitHub API requires authorization for most requests to increase rate limits.  
The recommended way is to use a **Personal Access Token**:

1. Go to GitHub Settings → Developer settings → Personal access tokens → Tokens (classic)
2. Generate a new token (at least `public_repo` and `read:user` scopes)
3. Use it in the `Authorization` header: `Authorization: token YOUR_TOKEN`

---

## Endpoints

### GET /users/{username} — Get user information

**URL:** `https://api.github.com/users/{username}`

**Method:** `GET`

**Description:** Returns public information about a GitHub user.

#### Path Parameters

| Parameter | Type   | Required | Description |
|-----------|--------|----------|-------------|
| username  | string | Yes | GitHub user login |

#### Headers

| Header | Value | Required | Description |
|--------|-------|----------|-------------|
| `Authorization` | `token YOUR_TOKEN` | No | Token for increasing rate limits (recommended) |

#### Request Example

`GET https://api.github.com/users/defunkt`

#### cURL Command

```bash
curl -X GET "https://api.github.com/users/defunkt" -H "Authorization: token YOUR_TOKEN"
```

#### Response Example (successful, code 200)

```json
{
    "login": "defunkt",
    "id": 2,
    "node_id": "MDQ6VXNlcjI=",
    "avatar_url": "https://avatars.githubusercontent.com/u/2?v=4",
    "gravatar_id": "",
    "url": "https://api.github.com/users/defunkt",
    "html_url": "https://github.com/defunkt",
    "followers_url": "https://api.github.com/users/defunkt/followers",
    "following_url": "https://api.github.com/users/defunkt/following{/other_user}",
    "gists_url": "https://api.github.com/users/defunkt/gists{/gist_id}",
    "starred_url": "https://api.github.com/users/defunkt/starred{/owner}{/repo}",
    "subscriptions_url": "https://api.github.com/users/defunkt/subscriptions",
    "organizations_url": "https://api.github.com/users/defunkt/orgs",
    "repos_url": "https://api.github.com/users/defunkt/repos",
    "events_url": "https://api.github.com/users/defunkt/events{/privacy}",
    "received_events_url": "https://api.github.com/users/defunkt/received_events",
    "type": "User",
    "user_view_type": "public",
    "site_admin": false,
    "name": "Chris Wanstrath",
    "company": null,
    "blog": "http://chriswanstrath.com/",
    "location": null,
    "email": null,
    "hireable": null,
    "bio": "🍔",
    "twitter_username": null,
    "public_repos": 107,
    "public_gists": 274,
    "followers": 22629,
    "following": 215,
    "created_at": "2007-10-20T05:24:19Z",
    "updated_at": "2025-08-08T19:18:26Z"
}
```

#### Response Fields Description

| Field | Type | Description |
|-------|------|-------------|
| login | string | User login |
| id | integer | User ID |
| avatar_url | string | Avatar URL |
| name | string | User name(can be null) |
| company | string | User company |
| blog | string | Link to personal blog |
| location | string | Location |
| email | string | Public email |
| bio | string | Brief biography |
| public_repos | integer | Number of public repositories |
| followers | integer | Number of followers |
| following | integer | Number of subscriptions |
| created_at | string | Account creation date (ISO 8601) |
| updated_at | string | Last update date |

#### Response Codes

| Code | Description |
|------|-------------|
| 200 | Successful request |
| 403 | Rate limit exceeded (use a token) |
| 404 | User not found |

### GET /users/{username}/repos - Get a list of user public repositories

**URL:** `https://api.github.com/users/{username}/repos`

**Method:** `GET`

**Description:** Returns a list of public repositories for the specified user

#### Path Parameters

| Parameter | Type   | Required | Description |
|-----------|--------|----------|-------------|
| username  | string | Yes | GitHub user login |

#### Headers

| Header | Value | Required | Description |
|--------|-------|----------|-------------|
| `Authorization` | `token YOUR_TOKEN` | No | Token for increasing rate limits (recommended) |
| `Accept` | `application/vnd.github+json` | Yes | Media type |

#### Request example

`GET https://api.github.com/users/defunkt/repos`

#### cURL Command

```bash
curl -X GET "https://api.github.com/users/defunkt/repos" \  
-H "Accept: application/vnd.github+json" \
-H "Authorization: token YOUR_TOKEN"
```

#### Response example (successful, code 200)

The response is an array of repository objects. Below is an example with two repositories (shortened for readability):

```json
[
  {
    "id": 1861402,
    "name": "ace",
    "full_name": "defunkt/ace",
    "private": false,
    "html_url": "https://github.com/defunkt/ace",
    "description": "Ajax.org Cloud9 Editor",
    "fork": true,
    "created_at": "2011-06-07T18:41:40Z",
    "updated_at": "2025-07-16T04:14:55Z",
    "pushed_at": "2011-11-16T18:37:42Z",
    "stargazers_count": 16,
    "language": "JavaScript",
    "forks_count": 7,
    "open_issues_count": 0,
    "license": {
      "key": "other",
      "name": "Other"
    }
  },
  {
    "id": 3594,
    "name": "acts_as_textiled",
    "full_name": "defunkt/acts_as_textiled",
    "private": false,
    "html_url": "https://github.com/defunkt/acts_as_textiled",
    "description": "Makes your models act as textiled.",
    "fork": false,
    "created_at": "2008-03-12T06:20:18Z",
    "updated_at": "2025-07-16T04:11:55Z",
    "stargazers_count": 115,
    "language": "Ruby",
    "forks_count": 32,
    "open_issues_count": 4
  }
]
```

#### Response Fields Description (for each repository object)

| Field | Type | Description |
|-------|------|-------------|
| id | integer | Unique repository identifier |
| name | string | Repository name |
| full_name | string | Full repository name |
| private | boolean | Whether repository is private |
| html_url | string | GitHub URL of the repository |
| description | string | Repository description (can be null) |
| fork | boolean | Whether this is a forked repostitory |
| created_at | string | Creation date (ISO 8601) |
| updated_at | string | Last update date |
| stargazers_count | integer | Number of stars |
| language | string | Primary programming language |
| forks_count | integer | Number of forks |
| open_issues_count | integer | Number of open issues |
| license | object or null | License information (if any) |
| license.key | string | License key (e.g., "mit") |
| license.name | string | License full name (e.g., "MIT License") |

#### Response Codes

| Code | Description |
|------|-------------|
| 200 | Successful request |
| 403 | Rate limit exceeded (use a token) |
| 404 | User or request not found |

### GET /repos/{owner}/{repo}/commits - Get a list of user commits in public repository

**URL:** `https://api.github.com/repos/{owner}/{repo}/commits`

**Method:** `GET`

**Description:** Returns a list of commits for the specified repository.

#### Path Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| owner | string | Yes | GitHub user login |
| repo | string | Yes | GitHub repository name |

#### Headers

| Header | Value | Required | Description |
|--------|-------|----------|-------------|
| `Accept` | `application/vnd.github+json` | Yes | Media type |
| `Authorization` | `token YOUR_TOKEN` | No | Token for higher rate limits |

#### Request example

`GET https://api.github.com/repos/defunkt/ace/commits`

#### cURL Command 

```bash
curl -X GET "https://api.github.com/repos/defunkt/ace/commits" \
  -H "Accept: application/vnd.github+json" \
  -H "Authorization: token YOUR_TOKEN"
```

#### Response example (successful, code 200)

The response is an array of commit objects. Below is an example with one commit (shortened for readability):

```json
[
  {
    "sha": "6fdb28556235107f40242345b1b3a9313a736cc2",
    "commit": {
      "author": {
        "name": "Fabian Jakobs",
        "email": "fabian.jakobs@web.de",
        "date": "2011-09-06T16:15:17Z"
      },
      "committer": {
        "name": "Fabian Jakobs",
        "email": "fabian.jakobs@web.de",
        "date": "2011-09-06T16:15:17Z"
      },
      "message": "jslint fixes",
      "comment_count": 0
    },
    "url": "https://api.github.com/repos/defunkt/ace/commits/6fdb28556235107f40242345b1b3a9313a736cc2",
    "html_url": "https://github.com/defunkt/ace/commit/6fdb28556235107f40242345b1b3a9313a736cc2",
    "author": {
      "login": "fjakobs",
      "id": 40952,
      "avatar_url": "https://avatars.githubusercontent.com/u/40952?v=4"
    },
    "committer": {
      "login": "fjakobs",
      "id": 40952,
      "avatar_url": "https://avatars.githubusercontent.com/u/40952?v=4"
    },
    "parents": [
      {
        "sha": "a0051237b66a1312f31e6b56cb29f12b20c38527"
      }
    ]
  }
]
```

#### Response Fields Description (for each commit object)

| Field | Type | Description |
|-------|------|-------------|
| sha | string | Commit SHA (unique identifier) |
| commit.author.name | string | Author's name |
| commit.author.email | string | Author's email |
| commit.author.date | string | Commit date (ISO 8601) |
| commit.message | string | Commit message |
| commit.comment_count | integer | Number of comments on the commit |
| author | object | GitHub user object of the author |
| author.login | string | Author's GitHub login |
| author.id | integer | Author's unique identifier |
| author.avatar_url | string | Author's avatar |
| committer | object | GitHub user object of the committer |
| committer.login | string | Committer's GitHub login |
| html_url | string | GitHub URL of the commit |
| parents | array | Array of parent commits (each with sha) |

#### Response Codes

| Code | Description |
|------|-------------|
| 200 | Successful request |
| 403 | Rate limit exceeded (use a token) |
| 404 | Repository not found |

### GET /users/{username}/starred - Get a list of repositories starred by user

**URL:** `https://api.github.com/users/{username}/starred`

**Method:** `GET`

**Description:** Get a list of repositories starred by user

#### Path Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `username`  | string | Yes | GitHub user login |

#### Query parameters

| Parameter | Type | Required | Description | Default |
|-----------|------|----------|-------------|---------|
| `sort` | string | No | The property to sort the results by | `created` |
| `direction` | string | No | The direction to sort the results by | `desc` |
| `per_page` | integer | No | Number of results per page | `30` |
| `page` | integer | No | Page number | `1` |

#### Headers

| Header | Value | Required | Description |
|--------|-------|----------|-------------|
| `Accept` | `application/vnd.github+json` | Yes | Media type |
| `Authorization` | `token YOUR_TOKEN` | No | Token for higher rate limits |

#### Request Example

`GET https://api.github.com/users/defunkt/starred`

#### cURL Command

```bash
curl -L "https://api.github.com/users/defunkt/starred" \
  -H "Accept: application/vnd.github+json" \
  -H "Authorization: Bearer <YOUR-TOKEN>" \
  -H "X-GitHub-Api-Version: 2022-11-28"
```

#### Response Example (successful, code 200)

The response is an array of repository objects. Below is an example of one repository:

```json
{
  "id": 1139261611,
  "name": "clorb",
  "full_name": "probablycorey/clorb",
  "private": false,
  "owner": {
    "login": "probablycorey",
    "id": 596,
    "avatar_url": "https://avatars.githubusercontent.com/u/596?v=4",
    "html_url": "https://github.com/probablycorey"
  },
  "html_url": "https://github.com/probablycorey/clorb",
  "description": null,
  "fork": false,
  "created_at": "2026-01-21T18:23:41Z",
  "updated_at": "2026-02-01T04:24:29Z",
  "stargazers_count": 3,
  "language": "TypeScript",
  "forks_count": 0,
  "open_issues_count": 0,
  "license": null,
  "topics": [],
  "visibility": "public"
}
```
#### Response Fields Description

Each object in the array represents a starred repository and has the following structure:

| Field | Type | Description |
|-------|------|-------------|
| `id` | integer | Unique repository identifier |
| `name` | string | Repository name |
| `full_name` | string | Full repository name (owner/name) |
| `private` | boolean | Whether the repository is private |
| `owner` | object | Information about the repository owner |
| `owner.login` | string | Owner's GitHub username |
| `owner.avatar_url` | string | URL to owner's avatar |
| `owner.html_url` | string | URL to owner's GitHub profile |
| `html_url` | string | URL to the repository on GitHub |
| `description` | string or null | Repository description (can be null) |
| `fork` | boolean | Whether this is a forked repository |
| `created_at` | string | Creation date (ISO 8601 format) |
| `updated_at` | string | Last update date |
| `stargazers_count` | integer | Number of stars |
| `language` | string or null | Primary programming language |
| `forks_count` | integer | Number of forks |
| `open_issues_count` | integer | Number of open issues |
| `license` | object or null | License information (if any) |
| `license.key` | string | License key (e.g., "mit") |
| `license.name` | string | License full name |
| `topics` | array | List of repository topics |
| `visibility` | string | Repository visibility: "public" or "private" |

#### Response Codes

| Code | Description |
|------|-------------|
| 200 | Successful request |
| 304 | Not modified |
| 401 | Unauthorized (invalid token) |
| 403 | Rate limit exceeded |
| 404 | User not found |

### POST /repos/{owner}/{repo}/issues - Create a new issue

**URL:** `https://api.github.com/repos/{owner}/{repo}/issues`

**Method:** `POST`

**Description:** Creates a new issue in the specified repository.

#### Authentication

A personal access token with `repo` scope is required. Without authentication, you cannot create issues.

#### Path Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `owner` | string | Yes | Repository owner (username or organization) |
| `repo` | string | Yes | Repository name |

#### Headers

| Header | Value | Required | Description |
|--------|-------|----------|-------------|
| `Accept` | `application/vnd.github+json` | Yes | Media type |
| `Authorization` | `token YOUR_TOKEN` | **Yes** | Token with `repo` scope |
| `Content-Type` | `application/json` | Yes | Request body format |

#### Request Body Schema

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `title` | string | Yes | Issue's title |
| `body` | string | No | The contents of the issue |
| `assignees` | array of strings | No | Usernames to assign to this issue |
| `milestone` | null or integer | No | The number of the milestone to associate this issue with |
| `labels` | array | No | Labels to associate with this issue |

#### Request Body Example

```json
{
  "title": "Found a bug",
  "body": "The login button doesn't work on mobile devices.",
  "assignees": ["alivieskan25-blip"],
  "labels": ["bug", "mobile"]
}
```

#### cURL Command

```bash
curl -L \
  -X POST \
  -H "Accept: application/vnd.github+json" \
  -H "Authorization: Bearer YOUR_TOKEN" \
  -H "X-GitHub-Api-Version: 2022-11-28" \
  https://api.github.com/repos/alivieskan25-blip/test-issues/issues \
  -d '{
    "title": "Found a bug",
    "body": "The login button doesn'\''t work on mobile devices.",
    "assignees": ["alivieskan25-blip"],
    "labels": ["bug", "mobile"]
  }'
```

#### Response Example (successful, code 201)

```json
{
  "id": 3996101496,
  "number": 2,
  "title": "Found a bug",
  "body": "The login button doesn't work on mobile devices.",
  "state": "open",
  "user": {
    "login": "alivieskan25-blip",
    "id": 261747932,
    "avatar_url": "https://avatars.githubusercontent.com/u/261747932?v=4"
  },
  "assignees": [
    {
      "login": "alivieskan25-blip",
      "id": 261747932,
      "avatar_url": "https://avatars.githubusercontent.com/u/261747932?v=4"
    }
  ],
  "labels": [
    {
      "name": "bug",
      "color": "d73a4a"
    },
    {
      "name": "mobile",
      "color": "ededed"
    }
  ],
  "created_at": "2026-02-26T15:08:37Z",
  "updated_at": "2026-02-26T15:08:37Z",
  "closed_at": null,
  "html_url": "https://github.com/alivieskan25-blip/test-issues/issues/2"
}
```

#### Response Fields Descriprion

| Field | Type | Description |
|-------|------|-------------|
| `id` | integer | Unique issue identifier |
| `number` | integer | Issue number within the repository |
| `title` | string | Issue title |
| `body` | string | Issue body text |
| `state` | string | Issue state:`open`,`closed` |
| `user` | object | Issue's creator |
| `user.login` | string | GitHub user name |
| `user.id` | integer | User unique identifier |
| `user.avatar_url` | string | Creator's avatar URL |
| `assignees` | array | Users assigned to this issue |
| `assignees[].login` | string | Username of assignee |
| `labels` | array | Labels attached to this issue |
| `labels[].color` | string | Label color (hex code) |
| `created_at` | string | Creation date |
| `updated_at` | string | Last update date |
| `html_url` | string | URL to the issue on GitHub |

#### Response Codes

| Code | Description |
|------|-------------|
| 201 | Issue successfully created |
| 400 | Bad request — missing title or invalid data |
| 401 | Unauthorized — token missing or invalid |
| 403 | Forbidden — token lacks repo scope |
| 404 | Not found — repository doesn't exist |
| 410 | Gone — issues are disabled for this repository |
| 422 | Unprocessable Entity — validation failed |