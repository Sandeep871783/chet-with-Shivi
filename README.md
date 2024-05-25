
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Chat with Shivi</title>
    <style>
        /* Code Owner: Mr. S */
        body {
            font-family: Arial, sans-serif;
            margin: 0;
            padding: 0;
            display: flex;
            flex-direction: column;
            height: 100vh;
            background-color: #f0f0f0;
        }
        #welcome-container {
            width: 100%;
            height: 200px;
            display: flex;
            align-items: center;
            justify-content: center;
            background-color: #fff;
        }
        #welcome-message {
            font-size: 2em;
            font-weight: bold;
        }
        #login-container {
            display: flex;
            justify-content: center;
            margin: 20px 0;
        }
        #login-button {
            background-color: #4285f4;
            color: white;
            border: none;
            padding: 10px;
            border-radius: 5px;
            cursor: pointer;
        }
        #chat-container {
            flex: 1;
            padding: 20px;
            overflow-y: auto;
            display: none;
            background-color: rgba(255, 255, 255, 0.8); /* Adding a white translucent background to chat */
        }
        #input-container {
            display: flex;
            align-items: center;
            padding: 20px;
            background-color: #fff;
            box-shadow: 0 -1px 6px rgba(0,0,0,0.1);
            display: none;
        }
        #chat-box {
            flex: 1;
            padding: 10px;
            box-sizing: border-box;
            margin-right: 10px;
            border: 1px solid #ccc;
            border-radius: 4px;
        }
        .message {
            margin-bottom: 10px;
            padding: 10px;
            border-radius: 5px;
        }
        .user {
            background-color: #d1e7dd;
            text-align: right;
        }
        .bot {
            background-color: #f8d7da;
            text-align: left;
        }
        #mic-button {
            background-color: #007bff;
            color: white;
            border: none;
            padding: 10px;
            border-radius: 50%;
            cursor: pointer;
        }
        #camera-button {
            background-color: #28a745;
            color: white;
            border: none;
            padding: 10px;
            border-radius: 50%;
            cursor: pointer;
            margin-left: 10px;
        }
        #shivi-logo {
            position: absolute;
            top: 10px;
            left: 10px;
            width: 100px;
        }
        #photo-preview {
            display: none;
            margin: 10px 0;
        }
        #footer {
            padding: 10px;
            background-color: #f1f1f1;
            text-align: center;
            font-size: 14px;
            color: #333;
        }
    </style>
</head>
<body>
    <div id="welcome-container">
        <div id="welcome-message">Welcome to Shivi Chat</div>
    </div>
    <img id="shivi-logo" src="https://images.app.goo.gl/3XxJrLkyQHih9C5H7" alt="Shivi"> <!-- Replace with your own logo link -->
    <div id="login-container">
        <button id="login-button">Login with Google</button>
    </div>
    <div id="chat-container"></div>
    <div id="input-container">
        <input type="text" id="chat-box" placeholder="Type your message here..." />
        <button id="mic-button">🎤</button>
        <button id="camera-button">📸</button>
    </div>
    <audio id="greeting-audio" src="http://sharevideo1.com/v/TWlfTlpWbnMxT3c/?t=ytb&f=co" type="audio/mp3"></audio>
    <img id="photo-preview" src="" alt="Photo Preview">
    <div id="footer">
        Owner: Mr. S | Email: bholasharmasharma123@gmail.com | Address: Mandsaur, Madhya Pradesh
    </div>

    <!-- Firebase SDK -->
    <script type="module">
      // Import the functions you need from the SDKs you need
      import { initializeApp } from "https://www.gstatic.com/firebasejs/10.12.1/firebase-app.js";
      import { getAnalytics } from "https://www.gstatic.com/firebasejs/10.12.1/firebase-analytics.js";
      import { getAuth, signInWithPopup, GoogleAuthProvider } from "https://www.gstatic.com/firebasejs/10.12.1/firebase-auth.js";

      // Your web app's Firebase configuration
      const firebaseConfig = {
        apiKey: "AIzaSyAvVIUZdVNE-nUwxe8Z4njEtvyIvRApOTc",
        authDomain: "chet-with-shivi.firebaseapp.com",
        projectId: "chet-with-shivi",
        storageBucket: "chet-with-shivi.appspot.com",
        messagingSenderId: "875078766698",
        appId: "1:875078766698:web:1a8d178e232f4c4d2efa13",
        measurementId: "G-LMECZBXLTM"
      };

      // Initialize Firebase
      const app = initializeApp(firebaseConfig);
      const analytics = getAnalytics(app);
      const auth = getAuth(app);

      // Google Login
      const loginButton = document.getElementById('login-button');
      loginButton.addEventListener('click', () => {
          const provider = new GoogleAuthProvider();
          signInWithPopup(auth, provider)
              .then((result) => {
                  loginButton.style.display = 'none';
                  document.getElementById('chat-container').style.display = 'block';
                  document.getElementById('input-container').style.display = 'flex';
                  playGreeting();
              })
              .catch((error) => {
                  console.error('Error:', error);
              });
      });

      // Function to play a greeting message
      function playGreeting() {
          document.getElementById('greeting-audio').play();
      }

      const chatBox = document.getElementById('chat-box');
      const chatContainer = document.getElementById('chat-container');
      const micButton = document.getElementById('mic-button');
      const cameraButton = document.getElementById('camera-button');
      const photoPreview = document.getElementById('photo-preview');

      chatBox.addEventListener('keypress', function (e) {
          if (e.key === 'Enter') {
              const userMessage = chatBox.value;
              addMessage(userMessage, 'user');
              chatBox.value = '';

              // Fetch response from ChatGPT API
              fetch('https://api.openai.com/v1/completions', {
                  method: 'POST',
                  headers: {
                      'Content-Type': 'application/json',
                      'Authorization': 'Bearer sk-proj-fwwEaEDFJc9hlZKqQsVDT3BlbkFJEAfZHLuBLrTs0BBPYxlL'
                  },
                  body: JSON.stringify({
                      model: 'text-davinci-003',
                      prompt: userMessage,
                      max_tokens: 150
                  })
              })
              .then(response => response.json())
              .then(data => {
                  const botMessage = data.choices[0].text.trim();
                  addMessage(botMessage + ' 😊', 'bot');
                  speakMessage(botMessage);
              })
              .catch(error => console.error('Error:', error));
          }
      });

      micButton.addEventListener('click', () => {
          const recognition = new (window.SpeechRecognition || window.webkitSpeechRecognition)();
          recognition.lang = 'hi-IN';
          recognition.start();
          recognition.onresult = function(event) {
              const speechResult = event.results[0][0].transcript;
              addMessage(speechResult, 'user');

              // Fetch response from ChatGPT API
              fetch('https://api.openai.com/v1/completions', {
                  method: 'POST',
                  headers: {
                      'Content-Type': 'application/json',
                      'Authorization': 'Bearer sk-proj-fwwEaEDFJc9hlZKqQsVDT3BlbkFJEAfZHLuBLrTs0BBPYxlL'
                  },
                  body: JSON.stringify({
                      model: 'text-davinci-003',
                      prompt: speechResult,
                      max_tokens: 150
                  })
              })
              .then(response => response.json())
              .then(data => {
                  const botMessage = data.choices[0].text.trim();
                  addMessage(botMessage + ' 😊', 'bot');
                  speakMessage(botMessage);
              })
              .catch(error => console.error('Error:', error));
          };
      });

      cameraButton.addEventListener('click', () => {
          const input = document.createElement('input');
          input.type = 'file';
          input.accept = 'image/*';
          input.capture = 'camera';
          input.onchange = function(event) {
              const file = event.target.files[0];
              const reader = new FileReader();
              reader.onload = function(e) {
                  photoPreview.src = e.target.result;
                  photoPreview.style.display = 'block';

                  // Simulate processing the image and generating a response
                  setTimeout(() => {
                      const botMessage = "तस्वीर को प्रोसेस कर लिया गया है। यह बहुत सुंदर है!";
                      addMessage(botMessage + ' 😊', 'bot');
                      speakMessage(botMessage);
                  }, 2000);
              };
              reader.readAsDataURL(file);
          };
          input.click();
      });

      function addMessage(message, sender) {
          const messageDiv = document.createElement('div');
          messageDiv.classList.add('message', sender);
          messageDiv.textContent = message;
          chatContainer.appendChild(messageDiv);
          chatContainer.scrollTop = chatContainer.scrollHeight;
      }

      function speakMessage(message) {
          const speech = new SpeechSynthesisUtterance(message);
          speech.lang = 'hi-IN';
          window.speechSynthesis.speak(speech);
      }
    </script>
</body>
</html>
