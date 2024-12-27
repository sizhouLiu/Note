# chatgpt_embed_demo Api doc

----

### 1. Get Sheet Status

**Endpoint**: `/api/get-sheet-status`  
**Method**: `GET`, `OPTIONS`  
**CORS**: Enabled  

**Description**: Retrieves the current status from Redis.

**Response**:
- Returns the status stored in Redis.

---

### 2. Translate Markdown

**Endpoint**: `/api/translate-markdown`  
**Method**: `POST`, `OPTIONS`  
**CORS**: Enabled  

**Description**: Translates Markdown text using an asynchronous job.

**Request Body**:
- `data` (object, required): The Markdown content to be translated.

**Response**:
- **202 Accepted**: 
  - `job_id` (string): The ID of the translation job.
- **400 Bad Request**: 
  - `status`: "failed"
  - `message`: "No data provided"

---

### 3. Check Job Status

**Endpoint**: `/api/check-job-status/<job_id>`  
**Method**: `GET`  

**Description**: Checks the status of a background job using its job ID.

**Path Parameters**:
- `job_id` (string, required): The ID of the job whose status is being checked.

**Response**:
- **200 OK**: 
  - `status` (string): The current status of the job (`pending`, `completed`, `failed`, etc.).
  - `result` (object, optional): The result of the job if completed successfully.
  - `error` (string, optional): Error message if the job failed.

**Example**:
- Request: `GET /api/check-job-status/12345`
- Response:
  ```json
  {
      "status": "completed",
      "result": "Job result data"
  }

### 4. Get Page JSON

**Endpoint**: `/api/get-pagejson`
**Method**: `GET`, `OPTIONS`
**CORS**: Enabled

**Description**: Fetches JSON data from a specified URL.

**Query Parameters**:

- `url` (string, required): The URL to fetch the JSON data from.

**Response**:

- Returns the JSON data retrieved from the specified URL.

------

### 5. Translate Page JSON

**Endpoint**: `/api/translate-json`
**Method**: `POST`, `OPTIONS`
**CORS**: Enabled

**Description**: Translates the content of a JSON structure.

**Request Body**:

- `url` (string, optional): The URL to fetch JSON data from (if `Original` is not provided).
- `Original` (string, optional): The raw JSON text to translate (if `url` is not provided).
- `Language` (string, required): The target language code for translation.

**Workflow**:

1. Retrieves JSON data from the specified URL or input text.
2. Extracts text components that require translation.
3. Breaks down the text into manageable chunks.
4. Attempts translation, retrying up to three times if a KeyError occurs.
5. Merges translated content back into the original JSON structure.
6. Returns the updated JSON.

**Response**:

- 202 Accepted:
  - `job_id` (string): The ID of the translation job.
- 400 Bad Request:
  - `status`: "failed"
  - `message`: "No data provided"

-----

### 6. Get Thread Pool Size

**Endpoint**: `/thread_pool_size`  
**Method**: `GET`  

**Description**: Retrieves the current size of the thread pool.

**Response**:
- **200 OK**: 
  - `status`: "success"
  - `size` (integer): The current size of the thread pool.
  
- **500 Internal Server Error**: 
  - `status`: "error"
  - `message` (string): Error message describing the issue.

---

### 7. Get Markdown

**Endpoint**: `/api/get-markdown`  
**Method**: `GET`, `OPTIONS`  

**Description**: Fetches and returns Markdown content from a specified URL. The URL can be for a blog or a knowledge base article.

**Query Parameters**:
- `url` (string, required): The URL from which to fetch the Markdown content.

**Response**:
- **200 OK**: 
  - Returns the HTML output converted to Markdown format.
  
- **400 Bad Request**: 
  - Returns an error message if the URL is invalid or unknown.
  
- **500 Internal Server Error**: 
  - Returns an error message if there is an issue fetching the article or processing the content.

**Workflow**:
1. Parses the provided URL.
2. Checks if the URL corresponds to a blog or knowledge base article.
3. If it's a blog URL, fetches the content from specified div classes.
4. If it's a knowledge base article, extracts the article ID and retrieves the title and body.
5. Cleans and formats the HTML content for Markdown before returning.

**Example**:
- **Request**: `GET /api/get-markdown?url=https://example.com/blog/sample-post`
- **Response**: 
  ```html
  <div class="s-blog-header">...</div>
  <div class="s-blog-body s-blog-padding">...</div>

----

### 8.Update Review Prompt

**Description**:
Updates the review prompts content retrieved from a specified Google Sheet and stores it in Redis based on certain conditions. The update process is denied if prompts are currently being updated or have not been marked for an update.

**Workflow**:

1. Retrieves current review prompts and auto-update status from the Google Sheet.
2. Checks if auto-update status is FALSE; if so, returns an error message.
3. If prompts are currently being updated, returns an error message.
4. If conditions are met, iterates through the prompts and updates Redis content.
5. Sets Redis status to "UPDATING" and updates the Google Sheet status.
6. After completion, sets status to "SLEEPING".

**Returns**:

- **Success**: `200 OK` with a message indicating the update is complete.
- **Failure**: `500 Internal Server Error` with an error message.

----

### 9.Get Live Prompt

**Description**:
Retrieves the current live prompt based on the specified user language.

**Parameters**:

- `language` (str): The user's preferred language for the prompt.

**Returns**:

- A JSON response containing the live prompt data for the specified language.

---
