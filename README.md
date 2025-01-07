<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, user-scalable=no, initial-scale=1.0, maximum-scale=1.0, minimum-scale=1.0">
    <title>ChatGPT Chat</title>
    <link href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/5.15.4/css/all.min.css" rel="stylesheet">
    <style>
        body {
            margin: 0;
            font-family: Arial, sans-serif;
            background-color: #fff;
            color: #000;
            display: flex;
            flex-direction: column;
            height: 100vh;
        }
/* Basic styling for the code block */
.code-block {
    position: relative;
    background-color: #f4f4f4;
    padding: 10px;
    border-radius: 5px;
    margin: 5px 0;
    overflow: auto;
    font-family: monospace;
    white-space: pre-wrap;
    word-wrap: break-word;
}

/* Syntax Highlighting */
.code-block .keyword {
    color: #d73a49; /* Red for keywords like 'function', 'var', etc. */
    font-weight: bold;
}

.code-block .variable {
    color: #6f42c1; /* Purple for variables */
}

.code-block .string {
    color: #032f62; /* Dark blue for strings */
}

.code-block .number {
    color: #005cc5; /* Blue for numbers */
}

.code-block .comment {
    color: #6a737d; /* Gray for comments */
    font-style: italic;
}

.code-block .operator {
    color: #e36209; /* Orange for operators */
}

/* Example custom class for inline code */
.inline-code {
    background-color: #f8f8f8;
    padding: 2px 5px;
    border-radius: 3px;
}

.copy-button, .copy-button-inline {
    background-color: transparent; /* Remove blue background */
    color: #007bff; /* Icon color */
    border: none;
    border-radius: 5px;
    padding: 0; /* Remove extra padding */
    cursor: pointer;
    margin-left: 10px;
    font-size: 18px; /* Adjust icon size */
}

.copy-button:hover, .copy-button-inline:hover {
    color: #0056b3; /* Change icon color on hover */
}


        /* Top Bar */
        .top-bar {
            background-color: #fff;
            padding: 15px 20px;
            display: flex;
            align-items: center;
            justify-content: flex-start; /* Changed to left-align */
            font-size: 1.5em;
            font-weight: bold;
            color: #000;
            border-bottom: 1px solid rgba(0, 0, 0, 0.1);
            box-shadow: 0px 2px 2px rgba(0, 0, 0, 0.1);
            position: fixed;
            top: 0;
            width: 100%;
            z-index: 1000;
        }

        /* Chat Area */
        .chat-container {
            flex: 1;
            display: flex;
            flex-direction: column;
            justify-content: flex-start;
            margin-top: 60px; /* Space for top bar */
            margin-bottom: 60px; /* Space for bottom bar */
            overflow: hidden;
        }

        .chat-content {
            flex: 1;
            overflow-y: auto;
            padding: 20px;
            scroll-behavior: smooth;
        }

        .chat-content::-webkit-scrollbar {
            width: 6px;
        }

        .chat-content::-webkit-scrollbar-thumb {
            background-color: rgba(0, 0, 0, 0.2);
            border-radius: 10px;
        }

        .message {
            margin: 10px 0;
            display: flex;
            align-items: flex-end;
            max-width: 80%; /* Adjusting max width */
        }

        .message.user {
            justify-content: flex-end;
            align-self: flex-end;
            margin-left: auto; /* Ensure user messages align to the right */
        }

        .message.bot {
            justify-content: flex-start;
            align-self: flex-start;
            margin-right: auto; /* Ensure bot messages align to the left */
        }

        .bubble {
            padding: 10px 15px;
            border-radius: 20px;
            word-wrap: break-word;
            background-color: #f0f0f0;
            color: #000;
            box-shadow: 0px 2px 2px rgba(0, 0, 0, 0.1);
            max-width: calc(100% - 50px); /* Space for avatar */
        }

        .message.user .bubble {
            background-color: #007BFF;
            color: #fff;
        }

        .avatar {
            width: 40px;
            height: 40px;
            border-radius: 50%;
            margin: 0 10px;
        }

        .message.user .avatar {
            margin-left: 10px;
        }

        .message.bot .avatar {
            margin-right: 10px;
        }

        /* Loading Animation */
        .loading {
            display: flex;
            align-items: center;
            justify-content: center;
            padding: 10px 15px;
            background-color: #f0f0f0;
            border-radius: 20px;
            max-width: 40%;
            color: #000;
            box-shadow: 0px 2px 2px rgba(0, 0, 0, 0.1);
        }

        .dots {
            display: flex;
            align-items: center;
            gap: 5px;
        }

        .dot {
            width: 8px;
            height: 8px;
            background-color: #000;
            border-radius: 50%;
            animation: blink 1.4s infinite;
        }

        .dot:nth-child(2) {
            animation-delay: 0.2s;
        }

        .dot:nth-child(3) {
            animation-delay: 0.4s;
        }

        @keyframes blink {
            0%, 80%, 100% {
                opacity: 0;
            }
            40% {
                opacity: 1;
            }
        }

        /* Bottom Bar */
        .bottom-bar {
            display: flex;
            align-items: center;
            justify-content: space-between;
            background-color: #fff;
            padding: 10px;
            border-top: 1px solid rgba(0, 0, 0, 0.1);
            box-shadow: 0px -2px 2px rgba(0, 0, 0, 0.1);
            position: fixed;
            bottom: 0;
            left: 0;
            right: 0;
        }

        .bottom-bar input {
            flex: 1;
            padding: 10px 15px;
            border: 1px solid rgba(0, 0, 0, 0.2);
            border-radius: 20px;
            font-size: 1em;
            background-color: #f9f9f9;
            color: #000;
            margin-right: 10px;
            outline: none;
        }

        .bottom-bar input::placeholder {
            color: #aaa;
        }

        .bottom-bar button {
            padding: 10px 20px;
            border: none;
            border-radius: 20px;
            font-size: 1em;
            background-color: #007BFF;
            color: #fff;
            cursor: pointer;
            transition: background-color 0.3s;
        }

        .bottom-bar button:hover {
            background-color: #0056b3;
        }

        .bottom-bar button:active {
            background-color: #004494;
        }

        /* Code Block Styling */
        pre {
            background-color: #f5f5f5;
            padding: 10px;
            border-radius: 8px;
            font-size: 1em;
            white-space: pre-wrap;
            word-wrap: break-word;
            max-width: 100%;
        }
    </style>
</head>
<body>
    <!-- Top Bar -->
    <div class="top-bar">
        ChatGPT
    </div>

    <!-- Chat Area -->
    <div class="chat-container">
        <div id="chat-content" class="chat-content">
            <!-- Messages will appear here -->
        </div>
    </div>

    <!-- Bottom Bar -->
    <form id="chat-form" class="bottom-bar">
        <input id="user-input" type="text" placeholder="Type your message..." autocomplete="off">
        <button type="submit">Send</button>
    </form>

<script>
    const chatContent = document.getElementById("chat-content");
    const chatForm = document.getElementById("chat-form");
    const userInput = document.getElementById("user-input");

    const chatGPTLogo = "https://i.ibb.co/JxcxBzh/67373267.jpg";
    const userLogo = "https://i.ibb.co/ssQNvBC/67373290.jpg";

    // Function to parse markdown and add copy buttons with FontAwesome icons
    function parseMarkdown(text) {
        // Handle code blocks with copy button (remove language identifier for copying)
        text = text.replace(/```([\s\S]*?)```/g, (match, code) => {
            const codeWithoutLanguage = code.split('\n').slice(1).join('\n'); // Remove the language identifier (first line)
            return `
                <pre class="code-block">
                    <code>${escapeHtml(code)}</code>
                    <button class="copy-button" data-code="${encodeURIComponent(codeWithoutLanguage)}">
                        <i class="fa fa-copy"></i>
                    </button>
                </pre>`;
        });

        // Handle inline code
        text = text.replace(/`([^`]+)`/g, (match, code) => {
            return `<code>${escapeHtml(code)}</code>
                    <button class="copy-button-inline" data-code="${encodeURIComponent(code)}">
                        <i class="fa fa-copy"></i>
                    </button>`;
        });

        // Handle bold and italic text
        text = text.replace(/\*\*([^*]+)\*\*/g, '<strong>$1</strong>');
        text = text.replace(/\*([^*]+)\*/g, '<em>$1</em>');

        // Handle newlines
        text = text.replace(/\n/g, '<br/>');

        return text;
    }

    // Escape HTML characters
    function escapeHtml(text) {
        return text
            .replace(/&/g, "&amp;")
            .replace(/</g, "&lt;")
            .replace(/>/g, "&gt;")
            .replace(/"/g, "&quot;")
            .replace(/'/g, "&#039;");
    }

    // Function to copy text to clipboard
    function copyToClipboard(text, button) {
        const textarea = document.createElement('textarea');
        textarea.value = text;
        document.body.appendChild(textarea);
        textarea.select();
        document.execCommand('copy');
        document.body.removeChild(textarea);

        // Change the icon to "checked" to indicate copied
        const icon = button.querySelector('i');
        icon.classList.remove('fa-copy');
        icon.classList.add('fa-check');
        
        // Reset back to copy icon after 1 second
        setTimeout(() => {
            icon.classList.remove('fa-check');
            icon.classList.add('fa-copy');
        }, 1000);
    }

    // Event listener for all copy buttons
    document.addEventListener("click", (event) => {
        const button = event.target.closest('.copy-button, .copy-button-inline');
        if (button) {
            const code = decodeURIComponent(button.getAttribute('data-code'));
            copyToClipboard(code, button);
        }
    });

    // Function to create a message element
    function createMessageElement(message, fromUser, isLoading = false) {
        const messageWrapper = document.createElement("div");
        messageWrapper.classList.add("message", fromUser ? "user" : "bot");

        const avatar = document.createElement("img");
        avatar.src = fromUser ? userLogo : chatGPTLogo;
        avatar.alt = fromUser ? "User" : "ChatGPT";
        avatar.classList.add("avatar");

        const bubble = document.createElement("div");
        if (isLoading) {
            bubble.classList.add("loading");
            bubble.innerHTML = `<div class="dots">
                                    <div class="dot"></div>
                                    <div class="dot"></div>
                                    <div class="dot"></div>
                                </div>`;
        } else {
            bubble.classList.add("bubble");
            bubble.innerHTML = parseMarkdown(message); // Applying markdown parsing here
        }

        messageWrapper.appendChild(fromUser ? bubble : avatar);
        messageWrapper.appendChild(fromUser ? avatar : bubble);

        return messageWrapper;
    }

    // Function to fetch response from API
    async function fetchResponse(userMessage) {
        try {
            const response = await fetch("https://backend.buildpicoapps.com/aero/run/llm-api?pk=v1-Z0FBQUFBQm5IZkJDMlNyYUVUTjIyZVN3UWFNX3BFTU85SWpCM2NUMUk3T2dxejhLSzBhNWNMMXNzZlp3c09BSTR6YW1Sc1BmdGNTVk1GY0liT1RoWDZZX1lNZlZ0Z1dqd3c9PQ==", {
                method: "POST",
                headers: {
                    "Content-Type": "application/json"
                },
                body: JSON.stringify({ prompt: userMessage })
            });

            if (!response.ok) {
                throw new Error('Failed to fetch response');
            }

            const data = await response.json();

            if (data.status === "success") {
                return data.text;
            } else {
                throw new Error('Error in response data');
            }
        } catch (error) {
            console.error("Error:", error);
            return "There was an error. Please try again later.";
        }
    }

    // Handle form submission
    chatForm.addEventListener("submit", async (event) => {
        event.preventDefault();

        const userMessage = userInput.value.trim();
        if (!userMessage) {
            return;
        }

        // Display user message
        chatContent.appendChild(createMessageElement(userMessage, true));
        userInput.value = "";

        // Show loading animation
        const loadingMessage = createMessageElement("", false, true);
        chatContent.appendChild(loadingMessage);
        chatContent.scrollTop = chatContent.scrollHeight;

        // Fetch bot response after a delay
        const botResponse = await fetchResponse(userMessage);

        // Remove loading and show bot response
        chatContent.removeChild(loadingMessage);
        chatContent.appendChild(createMessageElement(botResponse, false));
        chatContent.scrollTop = chatContent.scrollHeight;
    });

    // Initial welcome message from ChatGPT
    window.addEventListener('load', () => {
        setTimeout(() => {
            const welcomeMessage = createMessageElement("How can I help you today?", false);
            chatContent.appendChild(welcomeMessage);
            chatContent.scrollTop = chatContent.scrollHeight;
        }, 100); // 1-second delay before showing the message
    });
</script>


</body>
</html>
