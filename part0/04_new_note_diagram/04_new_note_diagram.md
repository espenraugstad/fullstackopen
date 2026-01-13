# 0.4 Diagram for creating new notes

In this exercise we will create a diagram showing the chain of events when a user creates a new note on the page from the example app at [https://studies.cs.helsinki.fi/exampleapp/notes](https://studies.cs.helsinki.fi/exampleapp/notes).

## Diagram
```mermaid
sequenceDiagram
    participant browser
    participant server

    browser->>server: POST https://studies.cs.helsinki.fi/exampleapp/new_note
    activate server

    Note right of browser: On the server, the body of the POST request (the note) is stored in an object in an array.
    
    server-->>browser: HTTP/1.1 302 Found, Location /exampleapp/notes 
    deactivate server 

    browser->>server: GET https://studies.cs.helsinki.fi/exampleapp/notes
    activate server
    server-->>browser: HTML document
    deactivate server

    browser->>server: GET https://studies.cs.helsinki.fi/exampleapp/main.css
    activate server
    server-->>browser: the css file
    deactivate server

    browser->>server: GET https://studies.cs.helsinki.fi/exampleapp/main.js
    activate server
    server-->>browser: the JavaScript file
    deactivate server

    Note right of browser: The browser starts executing the JavaScript code that fetches the JSON from the server

    browser->>server: GET https://studies.cs.helsinki.fi/exampleapp/data.json
    activate server
    server-->>browser: [{ "content": "HTML is easy", "date": "2023-1-1" }, ... ]
    deactivate server

    Note right of browser: The browser executes the callback function that renders the notes
```

## Explanation

After a user has typed some content in the input field, and clicks the "Save"-button, a POST request is sent to the server containing the information in the input field (the payload). 

After storing the note on the server, it responds with a 302 code and tell the browser which location to go to. This location is the same page as the note was sent from, so the current page will be reloaded.

When the page is reloaded, the same process as when we first accessed the page will follow as before:

* GET request to the server returns the HTML document which the browser starts to parse.
* GET request to get the CSS file when the browser reads the HTML document and finds it needs the CSS file.
* GET request to get the client side javascript file when the HTML requires it.
* GET request from the client side script to get all the notes in a JSON-file.
* Finally, the client side script renders a bulleted list with all the notes.