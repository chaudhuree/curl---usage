# API Testing from the Command Line: A Practical Guide

This guide provides examples of how to interact with APIs using command-line tools like `curl`, `httpie`, and `wget`. It covers common scenarios such as:

*   Sending GET, POST, PUT, PATCH, and DELETE requests
*   Passing query parameters
*   Including path parameters
*   Sending JSON request bodies (from the command line and from files)
*   Setting headers
*   Authentication (Basic Auth and Bearer Token)

## 1. Tools

*   **`curl`:**  A very powerful and versatile command-line tool for transferring data with URLs. It's available on most systems.

    *   **Installation:**
        *   **Linux (Debian/Ubuntu):** `sudo apt-get install curl`
        *   **Linux (Fedora/CentOS):** `sudo yum install curl`
        *   **macOS:** `brew install curl` (requires Homebrew)
        *   **Windows:**  `curl` is often pre-installed on newer Windows versions.  If not, you can download a binary from [curl.se/windows](https://curl.se/windows/) or install it via Chocolatey (`choco install curl`). Make sure curl is added to your system path after installing.

*   **`httpie`:**  A more user-friendly command-line HTTP client with intuitive syntax and helpful output formatting.  Requires Python.

    *   **Installation:**
        *   **Linux/macOS:** `pip install httpie` or `pip3 install httpie` (depending on your Python setup)
        *   **Windows:** `pip install httpie` or `pip3 install httpie`.  Make sure Python is installed and added to your system PATH.
        *   **macOS:** `brew install httpie`

*   **`wget`:**  Primarily designed for downloading files, but can also be used for basic API interactions.  Generally not recommended for complex API testing involving JSON bodies.

    *   **Installation:**
        *   **Linux (Debian/Ubuntu):** `sudo apt-get install wget`
        *   **Linux (Fedora/CentOS):** `sudo yum install wget`
        *   **macOS:** `brew install wget`
        *   **Windows:** Download a binary from a reputable source (e.g., GnuWin32: [gnuwin32.sourceforge.net/packages/wget.htm](http://gnuwin32.sourceforge.net/packages/wget.htm)) and add it to your system PATH. Alternatively, use Chocolatey: `choco install wget`.

## 2. Basic HTTP Methods

### 2.1 GET

Retrieves data from the server.  Typically used to read resources.

**`curl`:**

```bash
# Linux/macOS
curl https://api.example.com/resource
```

```powershell
# Windows PowerShell
curl https://api.example.com/resource
```

**`httpie`:**

```bash
# Linux/macOS
http https://api.example.com/resource
```

```powershell
# Windows PowerShell
http https://api.example.com/resource
```

**`wget`:**

```bash
# Linux/macOS
wget https://api.example.com/resource -O -  # -O - sends output to stdout
```

```powershell
# Windows PowerShell (less common, outputs to file by default)
wget https://api.example.com/resource
```

### 2.2 POST

Sends data to the server to create a new resource.

**`curl`:**

```bash
# Linux/macOS
curl -X POST -H "Content-Type: application/json" -d '{"key1": "value1", "key2": "value2"}' https://api.example.com/resource
```

```powershell
# Windows PowerShell
curl -X POST -H "Content-Type: application/json" -d '{"key1": "value1", "key2": "value2"}' https://api.example.com/resource
```

**`httpie`:**

```bash
# Linux/macOS
http POST https://api.example.com/resource key1=value1 key2=value2
```

```powershell
# Windows PowerShell
http POST https://api.example.com/resource key1=value1 key2=value2
```

**`wget`:** (Less Ideal)

```bash
# Linux/macOS
wget --post-data='{"key1": "value1", "key2": "value2"}' --header="Content-Type: application/json" https://api.example.com/resource -O -
```

```powershell
# Windows PowerShell (Less Ideal)
wget --post-data='{"key1": "value1", "key2": "value2"}' --header="Content-Type: application/json" https://api.example.com/resource
```

### 2.3 PUT

Updates an existing resource by replacing it entirely.

**`curl`:** (Same as POST, just change `-X POST` to `-X PUT`)

```bash
# Linux/macOS
curl -X PUT -H "Content-Type: application/json" -d '{"key1": "new_value1", "key2": "new_value2"}' https://api.example.com/resource/123
```

```powershell
# Windows PowerShell
curl -X PUT -H "Content-Type: application/json" -d '{"key1": "new_value1", "key2": "new_value2"}' https://api.example.com/resource/123
```

**`httpie`:** (Same as POST, just change `http POST` to `http PUT`)

```bash
# Linux/macOS
http PUT https://api.example.com/resource/123 key1=new_value1 key2=new_value2
```

```powershell
# Windows PowerShell
http PUT https://api.example.com/resource/123 key1=new_value1 key2=new_value2
```

### 2.4 PATCH

Partially updates an existing resource.

**`curl`:** (Same as POST/PUT, change `-X POST` to `-X PATCH`)

```bash
# Linux/macOS
curl -X PATCH -H "Content-Type: application/json" -d '{"key1": "updated_value"}' https://api.example.com/resource/123
```

```powershell
# Windows PowerShell
curl -X PATCH -H "Content-Type: application/json" -d '{"key1": "updated_value"}' https://api.example.com/resource/123
```

**`httpie`:** (Same as POST/PUT, change `http POST` to `http PATCH`)

```bash
# Linux/macOS
http PATCH https://api.example.com/resource/123 key1=updated_value
```

```powershell
# Windows PowerShell
http PATCH https://api.example.com/resource/123 key1=updated_value
```

### 2.5 DELETE

Deletes a resource.

**`curl`:**

```bash
# Linux/macOS
curl -X DELETE https://api.example.com/resource/123
```

```powershell
# Windows PowerShell
curl -X DELETE https://api.example.com/resource/123
```

**`httpie`:**

```bash
# Linux/macOS
http DELETE https://api.example.com/resource/123
```

```powershell
# Windows PowerShell
http DELETE https://api.example.com/resource/123
```

**`wget`:** (Generally not suitable)  `wget` isn't really designed for DELETE requests.

## 3. Parameters

### 3.1 Query Parameters

Appended to the URL after a `?` and separated by `&`.

**`curl`:**

```bash
# Linux/macOS
curl "https://api.example.com/resource?param1=value1&param2=value2"
```

```powershell
# Windows PowerShell
curl "https://api.example.com/resource?param1=value1&param2=value2"
```

**`httpie`:**

```bash
# Linux/macOS
http https://api.example.com/resource param1==value1 param2==value2
```

```powershell
# Windows PowerShell
http https://api.example.com/resource param1==value1 param2==value2
```

**`wget`:**

```bash
# Linux/macOS
wget "https://api.example.com/resource?param1=value1&param2=value2" -O -
```

```powershell
# Windows PowerShell
wget "https://api.example.com/resource?param1=value1&param2=value2"
```

### 3.2 Path Parameters

Part of the URL path itself.  The API documentation will define where these go.

**`curl`:**

```bash
# Linux/macOS
curl https://api.example.com/users/123  # 123 is the path parameter
```

```powershell
# Windows PowerShell
curl https://api.example.com/users/123
```

**`httpie`:**

```bash
# Linux/macOS
http https://api.example.com/users/123
```

```powershell
# Windows PowerShell
http https://api.example.com/users/123
```

**`wget`:**

```bash
# Linux/macOS
wget https://api.example.com/users/123 -O -
```

```powershell
# Windows PowerShell
wget https://api.example.com/users/123
```

## 4. JSON Request Body

Used for sending structured data to the server (POST, PUT, PATCH).

### 4.1 Inline JSON

**`curl`:**

```bash
# Linux/macOS
curl -X POST -H "Content-Type: application/json" -d '{"key1": "value1", "key2": "value2"}' https://api.example.com/resource
```

```powershell
# Windows PowerShell
curl -X POST -H "Content-Type: application/json" -d '{"key1": "value1", "key2": "value2"}' https://api.example.com/resource
```

**Important (PowerShell):**  PowerShell can sometimes interpret double quotes within the JSON string. You might need to escape them: `\"`.

```powershell
# Windows PowerShell (Alternative, escaping double quotes)
curl -X POST -H "Content-Type: application/json" -d '{\"key1\": \"value1\", \"key2\": \"value2\"}' https://api.example.com/resource
```

**`httpie`:**

```bash
# Linux/macOS
http POST https://api.example.com/resource key1=value1 key2=value2
http POST https://api.example.com/resource key1:='{"nested": "value"}' #Complex JSON
```

```powershell
# Windows PowerShell
http POST https://api.example.com/resource key1=value1 key2=value2
http POST https://api.example.com/resource key1:='{"nested": "value"}' #Complex JSON
```

### 4.2 JSON from File

**`curl`:**

1.  Create a file named `data.json` with the following content:

    ```json
    {
      "key1": "value1",
      "key2": "value2"
    }
    ```

2.  Run the following command:

```bash
# Linux/macOS
curl -X POST -H "Content-Type: application/json" -d @data.json https://api.example.com/resource
```

```powershell
# Windows PowerShell
curl -X POST -H "Content-Type: application/json" -d @data.json https://api.example.com/resource
```

**`httpie`:**

```bash
# Linux/macOS
http POST https://api.example.com/resource @data.json
```

```powershell
# Windows PowerShell
http POST https://api.example.com/resource @data.json
```

## 5. Headers

Used to provide additional information in the HTTP request.  Common headers include `Content-Type`, `Authorization`, and custom headers.

**`curl`:**

```bash
# Linux/macOS
curl -H "Content-Type: application/json" -H "X-Custom-Header: value" https://api.example.com/resource
```

```powershell
# Windows PowerShell
curl -H "Content-Type: application/json" -H "X-Custom-Header: value" https://api.example.com/resource
```

**`httpie`:**

```bash
# Linux/macOS
http https://api.example.com/resource Content-Type:application/json X-Custom-Header:value
```

```powershell
# Windows PowerShell
http https://api.example.com/resource Content-Type:application/json X-Custom-Header:value
```

## 6. Authentication

### 6.1 Basic Authentication

Sends the username and password encoded in the `Authorization` header.

**`curl`:**

```bash
# Linux/macOS
curl -u username:password https://api.example.com/protected_resource
```

```powershell
# Windows PowerShell
curl -u username:password https://api.example.com/protected_resource
```

**`httpie`:**

```bash
# Linux/macOS
http --auth username:password https://api.example.com/protected_resource
```

```powershell
# Windows PowerShell
http --auth username:password https://api.example.com/protected_resource
```

### 6.2 Bearer Token Authentication

Sends a token (typically a JWT) in the `Authorization` header.

**`curl`:**

```bash
# Linux/macOS
curl -H "Authorization: Bearer YOUR_TOKEN" https://api.example.com/protected_resource
```

```powershell
# Windows PowerShell
curl -H "Authorization: Bearer YOUR_TOKEN" https://api.example.com/protected_resource
```

**`httpie`:**

```bash
# Linux/macOS
http https://api.example.com/protected_resource "Authorization: Bearer YOUR_TOKEN"
```

```powershell
# Windows PowerShell
http https://api.example.com/protected_resource "Authorization: Bearer YOUR_TOKEN"
```

## 7. Environment Variables

Use environment variables to store sensitive information like API keys and passwords.

**Linux/macOS:**

```bash
export API_KEY="your_api_key"
curl -H "Authorization: Bearer $API_KEY" https://api.example.com/resource
```

**Windows PowerShell:**

```powershell
$env:API_KEY = "your_api_key"
curl -H "Authorization: Bearer $env:API_KEY" https://api.example.com/resource
```

## 8. Verbose Output

Use verbose mode to see the full request and response headers.

**`curl`:**

```bash
# Linux/macOS
curl -v https://api.example.com/resource
```

```powershell
# Windows PowerShell
curl -v https://api.example.com/resource
```

**`httpie`:**

```bash
# Linux/macOS
http -v https://api.example.com/resource
```

```powershell
# Windows PowerShell
http -v https://api.example.com/resource
```

## 9. Important Considerations

*   **API Documentation:** Always consult the API documentation for the specific endpoint you're using.  It contains critical information about URLs, methods, headers, request/response formats, and authentication.
*   **Error Handling:** Check the HTTP status code of the response to identify success or errors.
*   **JSON Validation:** Validate JSON payloads to ensure they are well-formed.
*   **Character Encoding:** Be aware of character encoding, especially when dealing with non-ASCII characters.  UTF-8 is recommended.
*   **Command-Line Differences:** Pay attention to syntax differences between Linux/macOS shells (e.g., Bash, Zsh) and Windows PowerShell.  PowerShell often requires escaping special characters and uses different variable syntax.


