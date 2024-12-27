# ChatGpt-embed-demo

## translate_blog

Project addresses:  

-  prod：https://kodi-web.i.strikingly.com/translate
-  uat：https://kodi-web.uat.strikingly.com/translate

Handle the translation task by using the markdown_translate function to translate the input data into Markdown format, returning error messages in case of exceptions.

### server.py

| Function Name        | Function Description                                         |
| -------------------- | ------------------------------------------------------------ |
| get_markdown         | Handles requests to retrieve Markdown content based on the URL type.  <br />1. Processes Blog URLs to fetch and return specific HTML content. <br />2. Processes Knowledge Base (KB) article URLs to extract article ID, retrieve the title and body, and format them.<br />3. Returns an error message if the URL type is unknown. |
| update_review_prompt | Updates the review prompts content by retrieving data from a Google Sheet and updating Redis. <br />1. Retrieves review prompts and auto-update status from Google Sheet. <br />2. Checks the auto-update status; if FALSE, returns an error message. <br />3. If currently updating, returns an error message. <br />4. Updates content in Redis if conditions are met. <br />5. Sets Redis status to "UPDATING" and updates Google Sheet status. <br />6. After completion, sets status to "SLEEPING." |
| get_live_prompt      | Retrieves live prompts based on the user's language. <br />1. Extracts user language from the request parameters. <br />2. Calls the `get_all_prompt` function with the user's language to fetch the prompts. |

### Translate.py

| Function Name  | Function Description                                         |
| -------------- | ------------------------------------------------------------ |
| get_all_prompt | Retrieves prompt content for blogs and knowledge base based on the user's language. <br />1. Constructs the blog prompt using base and language-specific prompts. <br />2. Constructs the knowledge base prompt using base content. <br />3. Retrieves review prompts from Redis, providing a fallback message if the language is unsupported. <br />4. Returns a dictionary containing blog, KB, and review prompts. |

### async_translation.py

| Function Name          | Function Description                                         |
| ---------------------- | ------------------------------------------------------------ |
| markdown_translate     | Translates a set of Markdown documents into a specified language asynchronously. <br />1. Accepts a dictionary containing Markdown text, glossary, target language, and prompt texts. <br />2. Validates required keys and prepares prompts and temperatures. <br />3. Manages concurrent translations of documents with a semaphore. <br />4. Returns the translated Markdown and prompts or an error message if any exceptions occur. |
| simple_run_in_executor | Executes a given function in a separate thread using an asynchronous event loop. <br />1. Accepts a function and its arguments. <br />2. Optionally allows specifying an event loop. <br />3. Returns the result of the function execution. |
| translate_markdown     | Translates a markdown document into a specified language using the Azure OpenAI API. The function processes the document by parsing it into HTML, extracting context, and formatting a glossary. It attempts to translate the document up to a maximum number of retries if elements are missing, ensuring that all images and links are preserved. If successful, it returns the translated HTML; otherwise, it returns an error message after exhausting retry attempts. Additionally, it performs garbage collection to free resources. |
| get_markdowntext       | Extracts Markdown text from the provided data and splits it into manageable chunks. <br />1. Accepts a dictionary containing the Markdown text. <br />2. Uses `RecursiveCharacterTextSplitter` to create documents with specified chunk size. <br />3. Returns a list of `Document` objects. |
| parse_markdown         | Converts Markdown text to HTML and parses it into a BeautifulSoup object. <br />1. Accepts a string containing Markdown text. <br />2. Converts the Markdown to HTML using the `markdown` library. <br />3. Parses the HTML with BeautifulSoup and returns both the HTML and the soup object. |

### worker.py

| Function Name | Function Description                                         |
| ------------- | ------------------------------------------------------------ |
| translate     | Handle the translation task by using the `markdown_translate` function to translate the input data into Markdown format, returning error messages in case of exceptions. |



## translate_json

Project addresses: 

- prod：https://kodi-web.i.strikingly.com/translateJSON
-  uat：https://kodi-web.uat.strikingly.com/translateJSON

The `translatejson` function is used to translate the JSON file obtained from the site backend, replacing it to generate the corresponding language website template.

### server.py

| Function Name       | Function Description                                         |
| ------------------- | ------------------------------------------------------------ |
| translate_page_json | This endpoint translates JSON content from a provided URL or raw text into a specified target language. |
| get_pagejson        | Handles requests to retrieve Markdown content based on the URL type.  <br />1. Processes Blog URLs to fetch and return specific HTML content <br />2. Processes Knowledge Base (KB) article URLs to extract article ID, retrieve the title and body, and format them.<br />3. Returns an error message if the URL type is unknown. |

### work.py

| Function Name  | Function Description                                         |
| -------------- | ------------------------------------------------------------ |
| translate_json | The translate_json function translates text within a JSON structure, either fetched from a URL or provided as raw input. It extracts text components, performs the translation, and returns the updated JSON with translated content. The function includes error handling with up to three retry attempts for any KeyError encountered during processing. |

### json_translate.py

| Function Name          | Function Description                                         |
| ---------------------- | ------------------------------------------------------------ |
| get_prompt             | Returns a formatted prompt string, substituting the input language for "Hispanic American Spanish" to be more idiomatically correct. It checks if the input language is "Spanish" and changes it accordingly. Finally, it returns the base prompt combined with the specific prompt for the given language. |
| translate_html         | Translates HTML content into the specified language. This function utilizes Azure OpenAI for translation, allowing for temperature settings and a maximum number of retries. It retrieves the system and user messages, then calls the Azure OpenAI chat API for translation. If the translation result is not valid JSON, it retries. If successful, it returns the translated HTML string; if not successful after maximum retries, it returns an empty string. |
| simple_run_in_executor | Runs the specified function in a separate thread using an asynchronous loop. It allows passing a variable number of arguments and keyword arguments and returns the result of the function execution. The function obtains the current running event loop and uses `run_in_executor` to execute the passed function in the specified asynchronous thread, returning the result. |
| json_translate         | Asynchronously translates multiple HTML documents while using a semaphore to limit the number of concurrent tasks. It creates translation tasks for each document and utilizes `await asyncio.gather` with a timeout. If all tasks succeed, it returns a response with a status code of 200 and the translated data; if a timeout or other exception occurs, it returns the corresponding error message. |
| get_json               | Fetches a web page from the specified URL and extracts a JSON object from a `<script>` tag. The function uses BeautifulSoup to parse the webpage, finds `<script>` tags, and extracts JSON data matching a specific pattern. If no matching JSON or `<script>` tag is found, it returns an empty dictionary. |
| re_json                | Extracts the longest nested JSON object from a given string. It takes a string containing JSON objects and returns the longest nested JSON object string if found, otherwise |
| update_component_value | Updates values in a nested dictionary structure based on the provided texts and merged data. It takes the original data structure, a dictionary of locations to be updated, and a dictionary of new values. It raises KeyError`, `TypeError`, and `IndexError` for various issues. Example: updates a nested structure to replace values with new ones based on specified locations. |
| chunk_data_dict        | Splits a dictionary into smaller dictionaries, each within a specified size limit. It takes the original dictionary and the maximum size of each chunk in bytes, returning a list of dictionaries, each being a chunk of the original. Example: `data = {"key1": "value1", "key2": "value2", "key3": "value3"}` and `chunk_size = 50` could result in `[{'key1': 'value1', 'key2': 'value2'}, {'key3': 'value3'}]`. |

# web

### Translatejson.js

| Function Name       | Function Description                                         |
| ------------------- | ------------------------------------------------------------ |
| **API URLs**        | Contains base URLs for API endpoints used for fetching and translating data. |
| **openPopup**       | Opens the popup for editing prompt settings.                 |
| **closePopup**      | Closes the popup window for prompt editing.                  |
| **handleSave**      | Saves the prompt settings and temperatures from the popup.   |
| **handlePreview**   | Fetches the markdown data from a given URL and sets the original text state. Displays an alert if the URL is empty. |
| **handleTranslate** | Sends a request to translate the original text using specified parameters and starts polling for the job status. |
| **pollJobStatus**   | Periodically checks the status of the translation job until it is completed or fails, updating the UI accordingly. |
| **copyToClipboard** | Copies the translated text to the clipboard and alerts the user upon success. |
| **handleLanguage**  | Updates the selected language state based on user input.     |
| **Render**          | Renders the component UI including controls for language selection, previewing, translating, copying, and displaying original and translated text. |

### translate.js

| Function Name             | Function Description                                         |
| ------------------------- | ------------------------------------------------------------ |
| **openPopup()**           | Sets the popup state to open.                                |
| **closePopup()**          | Sets the popup state to closed.                              |
| **handleSave(data)**      | Updates the prompt texts and their temperatures from the provided data. |
| **handlePreview()**       | Fetches the original markdown content from the specified URL, sets it to state, and handles errors if the URL is empty. |
| **handleTranslate()**     | Sends a request to translate the original text, handles loading state, checks for data retrieval, and manages error handling. |
| **pollJobStatus(jobId)**  | Polls the job status every 10 seconds to check if the translation job is completed or failed, updating the state accordingly. |
| **copyToClipboard()**     | Copies the translated text to the clipboard and alerts the user. |
| **handleLanguage(event)** | Updates the selected language based on user input.           |
| **Render**                | Displays the UI, including input for URL, language selection, translate and copy buttons, and text areas for original and translated content. |

### PopupWindow.js

| Function Name         | Function Description                                         |
| --------------------- | ------------------------------------------------------------ |
| **PopupWindow**       | Renders a popup window for editing prompt settings, including blog, knowledge base, and review prompts, along with their temperature settings. |
| **useEffect**         | Updates the content of the text areas with initial values when the popup is opened. |
| **handleSave**        | Collects the content and temperature values from the text areas and calls the `onSave` function to save these settings before closing the popup. |
| **refreshPromptData** | Fetches new prompt data from the provided URL and updates the respective state variables for blog, knowledge base, and review prompts. |
| **Return**            | Returns null if the popup is not open; otherwise, renders the popup overlay with text areas for prompt editing and buttons for saving, refreshing, and closing. |
