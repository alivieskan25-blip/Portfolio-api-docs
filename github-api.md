# GitHub API Documentation

Example of documentation for the GitHub public API.
GitHub API allows you to programmatically get data about users, repositories, commits, and more.

## Contents

* [Base URL and Authorization](#base-url-and-authorization)
* [Endpoints](#endpoints)
  * [GET /users/{username} — Get user information](#get-usersusername--get-user-information)
  * [GET /users/{username}/repos — Get user repositories](#get-usersusernamerepos--get-user-repositories)
  * [GET /repos/{owner}/{repo}/commits — Get repository commits](#get-reposownerrepocommits--get-repository-commits)
  * [GET /users/{username}/starred — Get repositories starred by a user](#get-usersusernamestarred--get-repositories-starred-by-a-user)
  * [POST /repos/{owner}/{repo}/issues — Create an issue](#post-reposownerrepoissues--create-an-issue)
  * [PATCH /repos/{owner}/{repo} — Update a repository](#patch-reposownerrepo--update-a-repository)
  * [GET /repos/{owner}/{repo}/pulls — List pull requests](#get-reposownerrepopulls---lists-pull-requests-in-a-specified-repository)
  * [GET /repos/{owner}/{repo}/contributors - Lists contributors to the specified repository](#get-reposownerrepocontributors---lists-contributors-to-the-specified-repository)
  * [POST /repos/{owner}/{repo}/pulls - Create a pull request](#post-reposownerrepopulls---draft-pull-requests-are-available-in-public-repositories-with-github)
  * [GET /repos/{owner}/{repo}/issues - List issues in a repository](#get-reposownerrepoissues---list-issues-in-a-repository)
  * [GET /user/repos - List repositories for the authenticated user](#get-userrepos---list-repositories-for-the-authenticated-user)
  * [GET /orgs/{org}/repos - List organization repositories ](#get-orgsorgrepos---list-organization-repositories)

## Base URL and Authorization

**Base URL:** `https://api.github.com`

GitHub API requires authorization for most requests to increase rate limits.  
The recommended way is to use a **Personal Access Token**:

1. Go to GitHub Settings → Developer settings → Personal access tokens → Tokens (classic)
2. Generate a new token (at least `public_repo` and `read:user` scopes)
3. Use it in the `Authorization` header: `Authorization: token YOUR_TOKEN`

## 📖 OpenAPI Specification

This API is also described in a machine-readable **OpenAPI 3.0.3** format.  
You can view the specification here: [`openapi.yaml`](openapi.yaml) or explore it in [Swagger Editor](https://editor.swagger.io/).

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

### PATCH /repos/{owner}/{repo} — Update a repository

**URL:** `https://api.github.com/repos/{owner}/{repo}`

**Method:** `PATCH`

**Description:** Updates the settings or properties of a specified repository. You can change the name, description, visibility, archive status, and many other options.

#### Path Parameters

| Parameter | Type   | Required | Description |
|-----------|--------|----------|-------------|
| `owner` | string | Yes | The account owner of the repository |
| `repo` | string | Yes | The name of the repository |

#### Headers

| Header | Value | Required | Description |
|--------|-------|----------|-------------|
| `Accept` | `application/vnd.github+json` | Yes | Tells GitHub to send the response in JSON format [citation:1][citation:4]. |
| `Authorization` | `Bearer YOUR_TOKEN` | **Yes** | Token for authentication. Without it, you cannot update a repository |
| `Content-Type` | `application/json` | Yes | Indicates that the request body is JSON |

#### Request Body Schema

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `name` | string | No | Changing of the repository name |
| `description` | string | No | Brief description of the repository |
| `homepage` | string | No | A URL with more information about the repository |
| `private` | boolean | No | Either `true` to make the repository private or `false` to make it public. Default: `false` |
| `visibility` | string | No | The visibility of the repository |
| `default_branch` | string | No | Updates the default branch for this repository (for example from `master` to `main`) |
| `archived` | boolean | No | Whether to archive this repository. `false` will unarchive a previously archived repository |
| `has_issues` | boolean | No | Either `true` to enable issues for this repository or `false` to disable them |
| `has_wiki` | boolean | No | Either `true` to enable the wiki for this repository or `false` to disable it |
| `allow_merge_commit` | boolean | No | Either `true` to allow merging pull requests with a merge commit, or `false` to prevent merging pull requests with merge commits |
| `delete_branch_on_merge` | boolean | No | Either `true` to allow automatically deleting head branches when pull requests are merged, or `false` to prevent automatic deletion |

#### Request Body Example

```json
{
  "description": "This is my awesome test repository updated via API",
  "homepage": "https://github.com/alivieskan25-blip/test-issues"
}
```

#### cURL Command

```bash
curl -L -X PATCH \
  -H "Accept: application/vnd.github+json" \
  -H "Authorization: Bearer YOUR_TOKEN" \
  -H "Content-Type: application/json" \
  https://api.github.com/repos/alivieskan25-blip/test-issues \
  -d '{"description": "Updated via API", "homepage": "https://github.com/alivieskan25-blip/test-issues"}'
```

#### Response Example (successful, code 200)

```json
{
  "id": 1167654025,
  "name": "test-issues",
  "full_name": "alivieskan25-blip/test-issues",
  "private": false,
  "description": "Repository for test requests",
  "html_url": "https://github.com/alivieskan25-blip/test-issues",
  "created_at": "2026-02-26T14:35:13Z",
  "updated_at": "2026-03-06T18:25:24Z",
  "has_issues": true,
  "has_wiki": true,
  "archived": false,
  "visibility": "public",
  "default_branch": "main",
  "permissions": {
    "admin": true,
    "push": true,
    "pull": true
  }
}
```
#### Response Fields Descriprion

| Field | Type | Description |
|-------|------|-------------|
| `id` | integer | Unique repository identifier |
| `name` | string | Repository name |
| `private` | boolean | Whether repository is private |
| `description` | string | Repository description |
| `html_url` | string | URL to the repository on GitHub |
| `created_at` | string | Repository creation date |
| `updated_at` | string | Last update date |
| `has_issues` | boolean | Whether issues are enabled |
| `has_wiki` | boolean | Whether wiki is enabled |
| `visibility` | string | Repository visibility: "public" or "private" |
| `default_branch` | string | Default branch name |
| `permissions` | object | Current user's permissions |
| `permissions.admin` | boolean | Whether user can admin the repo |
| `permissions.push` | boolean | Whether user can push to the repo |
| `permissions.pull` | boolean | Whether user can pull from the repo |

#### Response Codes

| Code | Description |
|------|-------------|
| 200 | Successful request.Repository is updated |
| 307 | Temporary redirect |
| 403 | 	Forbidden. You don't have permission to update this repository or an invalid token was provided |
| 404 | Not Found. The repository or owner does not exist |
| 422 | Unprocessable Entity. Validation failed (e.g., trying to change to an invalid name) |

### GET /repos/{owner}/{repo}/pulls - Lists pull requests in a specified repository

**URL:** `https://api.github.com/repos/{owner}/{repo}/pulls`

**Method:** `GET`

**Description:** Lists pull requests in a specified repository.

#### Path Parameters

| Parameter | Type   | Required | Description |
|-----------|--------|----------|-------------|
| `owner` | string | Yes | The account owner of the repository |
| `repo` | string | Yes | The name of the repository |

#### Query Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `state` | string | No | Either open, closed, or all to filter by state |
| `head` | string | No | Filter by branch in `user:ref-name` format |
| `base` | string | No | Filter pulls by base branch name. Example: `gh-pages` |
| `sort` | string | No | What to sort results by |
| `direction` | string | No | The direction of the sort. Default: `desc` when sort is created or `sort` is not specified, otherwise `asc` |
| `per_page` | integer | No | The number of results per page (max 100) |
| `page` | integer | No | The page number of the results to fetch |

#### Request examples

`GET https://api.github.com/repos/octocat/Hello-World/pulls` - Lists all open pull-requests(base request)

`GET https://api.github.com/repos/octocat/Hello-World/pulls?state=closed&sort=updated&direction=desc` - Lists closed requests sorted by update date

`GET https://api.github.com/repos/octocat/Hello-World/pulls?head=octocat:feature-branch` - Filters by specified branch

#### Headers

| Header | Value | Required | Description |
|--------|-------|----------|-------------|
| `Accept` | `application/vnd.github+json` | Yes | Response format |
| `Authorization` | `Bearer YOUR_TOKEN` | No | Authentication token |

#### cURL Command

```bash
curl -L \
  -H "Accept: application/vnd.github+json" \
  -H "Authorization: Bearer <YOUR-TOKEN>" \
  -H "X-GitHub-Api-Version: 2022-11-28" \
  https://api.github.com/repos/OWNER/REPO/pulls
```

#### Response Example (successful, code 200)

```json
[
  {
    "url": "https://api.github.com/repos/octocat/Hello-World/pulls/1347",
    "id": 1,
    "number": 1347,
    "state": "open",
    "title": "Amazing new feature",
    "user": {
      "login": "octocat",
      "id": 1,
      "avatar_url": "https://github.com/images/error/octocat_happy.gif"
    },
    "body": "Please pull these awesome changes in!",
    "created_at": "2011-01-26T19:01:12Z",
    "updated_at": "2011-01-26T19:01:12Z",
    "closed_at": null,
    "merged_at": null,
    "merge_commit_sha": "e5bd3914e2e596de",
    "head": {
      "label": "octocat:new-feature",
      "ref": "new-feature",
      "sha": "6dcb09b5b57875f334f61aebed695e2e4193db5e"
    },
    "base": {
      "label": "octocat:main",
      "ref": "main",
      "sha": "6dcb09b5b57875f334f61aebed695e2e4193db5e"
    },
    "draft": false,
    "merged": false,
    "mergeable": true,
    "comments": 10,
    "commits": 3,
    "additions": 100,
    "deletions": 3,
    "changed_files": 5
  }
]
```

#### Response Fields Description

| Field | Type | Description |
|-------|------|-------------|
| `id` | integer | Pull-request unique identifier |
| `number` | integer | Pull-request number in the repository |
| `state` | string | Whether is `open` or `closed` |
| `title` | string | Header |
| `body` | string | Description |
| `user` | object | Pull-request author |
| `created_at` | string | Creation date |
| `updated_at` | string | Last update |
| `closed_at` | string | Closing date (null, if open) |
| `merged_at` | string | Merge date (null, if not merged) |
| `head` | object | Source-branch of changes |
| `head.ref` | string | Source-branch name |
| `head.sha` | string | SHA of last commit |
| `draft` | boolean | Pull-request draft |
| `merged` | boolean | Whether is merged |
| `mergeable` | boolean | Can be it merged |
| `comments` | integer | Comments number |
| `commits` | integer | Commits number |
| `additions` | integer | Number of strings added |
| `deletions` | integer | Number of strings deleted |
| `changed_files` | integer | Number of changed files |

#### Response Codes

| Code | Description |
|------|-------------|
| 200 | Successful request |
| 304 | Not modified |
| 422 | Validation failed, or the endpoint has been spammed |

### GET /repos/{owner}/{repo}/contributors - Lists contributors to the specified repository

**URL:** `https://api.github.com/repos/{owner}/{repo}/contributors`

**Method:** `GET`

**Description:** Lists contributors to the specified repository and sorts them by the number of commits per contributor in descending order

#### Path Parameters

| Parameter | Type   | Required | Description |
|-----------|--------|----------|-------------|
| `owner` | string | Yes | The account owner of the repository |
| `repo` | string | Yes | The name of the repository |

#### Query Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `anon` | string | No | Set to `true` to include anonymous contributors in results |
| `per_page` | integer | No | The number of results per page (max 100) |
| `page` | integer | No | The page number of the results to fetch |

#### Headers

| Header | Value | Required | Description |
|--------|-------|----------|-------------|
| `Accept` | `application/vnd.github+json` | Yes | Response format |
| `Authorization` | `Bearer YOUR_TOKEN` | No | Authentication token |

#### Request Examples

`GET https://api.github.com/repos/rust-lang/rustfmt/contributors`

`GET https://api.github.com/repos/rust-lang/rustfmt/contributors?anon=true`

#### cURL Command

```bash
curl -L \
  -H "Accept: application/vnd.github+json" \
  -H "Authorization: Bearer <YOUR-TOKEN>" \
  -H "X-GitHub-Api-Version: 2026-03-10" \
  https://api.github.com/repos/OWNER/REPO/contributors
```

#### Response Example (successful, code 200)

```json
[
    {
        "login": "bors",
        "id": 3372342,
        "node_id": "MDQ6VXNlcjMzNzIzNDI=",
        "avatar_url": "https://avatars.githubusercontent.com/u/3372342?v=4",
        "gravatar_id": "",
        "url": "https://api.github.com/users/bors",
        "html_url": "https://github.com/bors",
        "followers_url": "https://api.github.com/users/bors/followers",
        "following_url": "https://api.github.com/users/bors/following{/other_user}",
        "gists_url": "https://api.github.com/users/bors/gists{/gist_id}",
        "starred_url": "https://api.github.com/users/bors/starred{/owner}{/repo}",
        "subscriptions_url": "https://api.github.com/users/bors/subscriptions",
        "organizations_url": "https://api.github.com/users/bors/orgs",
        "repos_url": "https://api.github.com/users/bors/repos",
        "events_url": "https://api.github.com/users/bors/events{/privacy}",
        "received_events_url": "https://api.github.com/users/bors/received_events",
        "type": "User",
        "user_view_type": "public",
        "site_admin": false,
        "contributions": 47831
    }
]
```
#### Response Fields Description

| Field | Type | Description |
|-------|------|-------------|
| `login` | string | GitHub username of the contributor |
| `id` | integer | Unique identifier of the user |
| `avatar_url` | string | URL to contributor's avatar |
| `html_url` | string | GitHub profile URL |
| `type` | string | Type of user (User, Bot, etc.) |
| `user_view_type` | string | Whether the user public or not |
| `site_admin` | boolean | Whether the user is admin |
| `contributions` | integer | Number of commits made by this contributor |

#### Response Codes

| Code | Description |
|------|-------------|
| 200 | Successful request |
| 204 | Response if repository is empty |
| 401 | Response if the token is incorrect or absent |
| 403 | Rate limit exceeded (use a token) |
| 404 | Resource not found |

### POST /repos/{owner}/{repo}/pulls - Create a pull request

**URL:** `https://api.github.com/repos/{owner}/{repo}/pulls`

**Method:** `POST`

**Description:** Draft pull requests are available in public repositories with GitHub

#### Path Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `owner` | string | Yes | The account owner of the repository |
| `repo` | string | Yes | The name of the repository |

#### Headers

| Header | Value | Required | Description |
|--------|-------|----------|-------------|
| `Accept` | `application/vnd.github+json` | Yes | Response format |
| `Authorization` | `Bearer YOUR_TOKEN` | Yes | Authentication token |
| `Content-Type` | `application/json` | Yes | Indicates that the request body is JSON |

#### Request Body Schema

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `head` | string | Yes | The name of the branch with updates |
| `base` | string | Yes | The name of the branch you want the changes pulled into |
| `title` | string | **No** | The title of the new pull request. Required unless `issue` is specified |
| `head_repo` | string | **No** | The name of the repository where the changes in the pull request were made. This field is required for cross-repository pull requests if both repositories are owned by the same organization |
| `body` | string | No | The contents of the pull request |
| `maintainer_can_modify` | boolean | No | Indicates whether maintainers can modify the pull request |
| `draft` | boolean | No | Indicates whether the pull request is a draft |
| `issue` | integer | **No** | An issue in the repository to convert to a pull request.Required unless `title` is specified |

#### Request Example

```json
{
    "title": "Amazing new feature",
    "body": "Please pull these awesome changes in!",
    "head": "feature-branch",
    "base": "main"
}
```

#### cURL Command

```bash
curl -X POST \
  -H "Accept: application/vnd.github+json" \
  -H "Authorization: Bearer YOUR_TOKEN" \
  -H "Content-Type: application/json" \
  https://api.github.com/repos/OWNER/REPO/pulls \
  -d '{
    "title": "Amazing new feature",
    "body": "Please pull these awesome changes in!",
    "head": "feature-branch",
    "base": "main"
  }'
```

#### Response Example (successful, code 201)

```json
{
  "url": "https://api.github.com/repos/alivieskan25-blip/Portfolio-api-docs/pulls/2",
  "html_url": "https://github.com/alivieskan25-blip/Portfolio-api-docs/pull/2",
  "number": 2,
  "state": "open",
  "title": "Amazing new feature",
  "body": "Please pull these awesome changes in!",
  "user": {
    "login": "alivieskan25-blip",
    "avatar_url": "https://avatars.githubusercontent.com/u/261747932?v=4"
  },
  "created_at": "2026-03-24T17:24:25Z",
  "updated_at": "2026-03-24T17:24:25Z",
  "draft": false,
  "head": {
    "ref": "feature-branch",
    "sha": "308c8ee3f604739c2fc1d65603bd55c830b00b4c"
  },
  "base": {
    "ref": "main",
    "sha": "4faa510d281b991ae0ebc8cbea396a969648b113"
  },
  "mergeable": null,
  "merged": false,
  "commits": 1,
  "additions": 43,
  "deletions": 1,
  "changed_files": 3
}
```

#### Response Fields Description

| Field | Type | Description |
|-------|------|-------------|
| `url` | string | API URL of the pull request |
| `html_url` | string | HTML URL to view the pull request on GitHub |
| `number` | integer | Number of the pull request |
| `state` | string | Current state: `open`, `closed` or `merged` |
| `title` | string | Pull request title |
| `body` | string | Pull request description |
| `user` | object | Author of the pull request |
| `user.login` | string | Author's GitHub username |
| `user.avatar_url` | string | Author's GitHub avatar |
| `created_at` | string | Creation date (ISO 8601) |
| `updated_at` | string | Last update date |
| `draft` | boolean | Whether the pull request is a draft |
| `head.ref` | string | The branch where changes are implemented |
| `base.ref` | string | The branch where changes are pulled into |
| `merged` | boolean | Whether the pull request has been merged |
| `commits` | integer | Number of the commits in the pull request |
| `additions` | integer | Number of the added lines in the pull request |
| `deletions` | integer | Number of lines deleted |
| `changed_files` | integer | Number of the changed files |

#### Response Codes

| Code | Description |
|------|-------------|
| 201 | Pull request successfully created |
| 401 | Unauthorized — token missing or invalid |
| 403 | Forbidden — token lacks repo scope |
| 404 | Not found — repository doesn't exist |
| 422 | Unprocessable Entity — validation failed |

### GET /repos/{owner}/{repo}/issues - List issues in a repository

**URL:** `https://api.github.com/repos/{owner}/{repo}/issues`

**Method:** `GET`

**Description:** List issues in a repository. Only open issues will be listed.

#### Path Parameters

| Parameter | Type | Required | Descripion |
|-----------|------|----------|------------|
| `owner` | string | Yes | The account owner of the repository |
| `repo` | string | Yes | The name of the repository |

#### Query Parameters

| Parameters | Type | Required | Description | Default |
|------------|------|----------|-------------|---------|
| `state` | string | No | Indicates the state of the issues to return.Can be one of: `open`, `closed`, `all` | `open` |
| `labels` | string | No | Comma-separated list of label names (e.g., `bug,docs`) | — |
| `since` | string | No | Only issues updated after this date (ISO 8601) | — |
| `sort` | string | No | What to sort results by: `created`, `updated`, `comments` | `created` |
| `direction` | string | No | Sort direction: `asc` or `desc` | `desc` |
| `per_page` | integer | No | Results per page (max 100) | `30` |
| `page` | integer | No | Page number | `1` |

#### Headers

| Header | Value | Required | Description |
|--------|-------|----------|-------------|
| `Accept` | `application/vnd.github+json` | Yes | Response format |
| `Authorization` | `Bearer YOUR_TOKEN` | No | Authentication token (for higher rate limits) |

#### Request Examples

`GET https://api.github.com/repos/octocat/Hello-World/issues`

`GET https://api.github.com/repos/octocat/Hello-World/issues?state=closed&labels=bug&since=2026-01-01T00:00:00Z`

#### cURL Command

```bash
curl -L \
  -H "Accept: application/vnd.github+json" \
  -H "Authorization: Bearer YOUR_TOKEN" \
  https://api.github.com/repos/OWNER/REPO/issues?state=open&per_page=10
```

#### Response Example (successful, code 200)

The response contains an array with objects.

```json
[
  {
    "url": "https://api.github.com/repos/octocat/Hello-World/issues/1",
    "id": 1,
    "number": 1,
    "title": "Found a bug",
    "state": "open",
    "user": {
      "login": "octocat",
      "avatar_url": "https://avatars.githubusercontent.com/u/1?v=4"
    },
    "labels": [
      {
        "name": "bug",
        "color": "d73a4a"
      }
    ],
    "created_at": "2026-03-01T12:00:00Z",
    "updated_at": "2026-03-28T14:30:00Z",
    "closed_at": null,
    "body": "When I click the button, nothing happens."
  }
]
```

#### Response Fields Description

| Field | Type | Description |
|-------|------|-------------|
| `id` | integer | Unique issue identifier |
| `number` | integer | Issue number in the repository |
| `title` | string | Issue title |
| `state` | string | Issue state: `open` or `close` |
| `user` | object | Issue's creator |
| `user.login` | string | Creator's name |
| `labels` | array | List of issue's labels |
| `labels[].name` | string | Label name (e.g., "bug") |
| `labels[].color` | string | Label color in hex (e.g., "d73a4a") |
| `created_at` | string | Issue creation date |
| `updated_at` | string | Issue update date |
| `closed_at` | string | Closing date(null if open) |
| `body` | string | Issue description |

#### Response Codes

| Code | Description |
|------|-------------|
| 200 | Successful request |
| 301 | Moved permanently — the repository has been renamed or moved |
| 304 | Not modified |
| 403 | Rate limit exceeded |
| 404 | Repository not found |
| 422 | Validation failed or the endpoint has been spammed |

### GET /user/repos - List repositories for the authenticated user

**URL:** `https://api.github.com/user/repos`

**Method:** `GET`

**Description:** Lists repositories that the authenticated user has explicit permission (`:read`, `:write`, or `:admin`) to access

#### Query Parameters

| Parameter | Type | Required | Description | Default |
|-----------|------|----------|-------------|---------|
| `visibility` | string | No | Repository visibility: `all`, `public`, `private` | `all |
| `affiliation` | string | No | Comma-separated list of affiliations: `owner`, `collaborator`, `organization_member` | - |
| `type` | string | No | Limit results to repositories of the specified type: `all`, `owner`, `public`, `private` or `member` | `all` |
| `sort` | string | No | Sort by: `created`, `updated`, `pushed`, `full_name` | `full_time` |
| `direction` | string | No | Sort direction: `asc` or `desc` | `desc` |
| `per_page` | integer | No | Results per page (max 100) | `30` |
| `page` | integer | No | The page number of the results to fetch | `1` | 

#### Headers

| Header | Value | Required | Description |
|--------|-------|----------|-------------|
| `Accept` | `application/vnd.github+json` | Yes | Response format |
| `Authorization` | `Bearer YOUR-TOKEN` | **Yes** | Authentication token (requires `repo` scope for private repos) |

#### Request Example

`GET https://api.github.com/user/repos`

`GET https://api.github.com/user/repos?visibility=public&sort=updated&direction=desc`

#### cURL Command

```bash
curl -L \
  -H "Accept:application/vnd.github+json" \
  -H "Authorization: Bearer YOUR_TOKEN" \
  "https://api.github.com/user/repos?per_page=10"
```

#### Response Example (successful, code 200)

The response is an array of repository objects (same structure as in GET /users/{username}/repos).

```json
[
  {
    "id": 1167654025,
    "name": "test-issues",
    "full_name": "alivieskan25-blip/test-issues",
    "private": false,
    "html_url": "https://github.com/alivieskan25-blip/test-issues",
    "description": "Repository for test requests",
    "created_at": "2026-02-26T14:35:13Z",
    "updated_at": "2026-04-01T12:00:00Z"
  }
]
```

#### Response Fields Description

See `Repository` schema — the response structure is identical to `GET /users/{username}/repos`.

#### Response Codes

| Code | Description |
|------|-------------|
| 200 | Successful request |
| 304 | Not modified |
| 401 | Unauthorized — token missing or invalid |
| 403 | Forbidden — token lacks required scopes |
| 422 | Validation failed |

### GET /orgs/{org}/repos - List organization repositories

**URL:** `https://api.github.com/orgs/ORG/repos`

**Method:** `GET`

**Description:** Lists repositories for the specified organization

#### Path Parameters

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `org` | string | Yes | The organization name |

#### Query Parameters

| Parameter | Type | Reqiured | Description | Default |
|-----------|------|----------|-------------|---------|
| `type` | string | No | Specifies the types of repositories you want returned.Can be one of: `all`, `public`, `private`, `forks`, `sources`, `member` | `all` |
| `sort` | string | No | Sort by: `created`, `updated`, `pushed`, `full_name` | `created` |
| `direction` | string | No | Sort direction: `asc` or `desc` | `desc` |
| `per_page` | integer | No | Results per page (max 100) | `30` |
| `page` | integer | No | The page number of the results to fetch | `1` |

#### Headers

| Header | Value | Required | Description |
|--------|-------|----------|-------------|
| `Accept` | `application/vnd.github+json` | Yes | Response format |
| `Authorization` | `Bearer YOUR_TOKEN` | No | Authentication token (for higher rate limits) |

#### Request Examples

`GET https://api.github.com/orgs/github/repos`

`GET https://api.github.com/orgs/google/repos?type=public&sort=updated&direction=desc&per_page=10`

#### cURL Command

```bash
curl -L \
  -H "Accept: application/vnd.github+json" \
  -H "Authorization: Bearer YOUR_TOKEN" \
  https://api.github.com/orgs/github/repos?per_page=5&direction=desc"
```

#### Response Example (successful, code 200)

The response is an array of repository objects (same structure as in GET /users/{username}/repos).

```json
[
  {
    "id": 1936771,
    "name": "truth",
    "full_name": "google/truth",
    "private": false,
    "html_url": "https://github.com/google/truth",
    "description": "Fluent assertions for Java and Android",
    "fork": false,
    "created_at": "2011-06-22T18:55:12Z",
    "updated_at": "2026-04-03T06:53:54Z",
    "stargazers_count": 2781,
    "language": "Java",
    "forks_count": 267,
    "open_issues_count": 74,
    "license": {
      "key": "apache-2.0",
      "name": "Apache License 2.0"
    },
    "visibility": "public"
  }
]
```

#### Response Fields Description

See Repository schema — the response structure is identical to `GET /users/{username}/repos`.

#### Response Codes

| 200 | Successful request |
| 304 | Not modified |
| 403 | Forbidden — rate limit exceeded |
| 404 | Not found — organization does not exist |
| 422 | Validation failed |