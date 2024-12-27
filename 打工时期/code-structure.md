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