<!doctype html>
<html lang="en"> 
<head> 
  <meta charset="UTF-8"> 
  <meta name="viewport" content="width=device-width, initial-scale=1.0"> 
  <title>Authentication</title> 

  <!-- Firebase Authentication --> 
  <script type="module">
        import { initializeApp } from "https://www.gstatic.com/firebasejs/9.6.1/firebase-app.js";
        import { getAuth, signInWithEmailAndPassword, createUserWithEmailAndPassword, sendPasswordResetEmail } from "https://www.gstatic.com/firebasejs/9.6.1/firebase-auth.js";

        const firebaseConfig = {
            apiKey: "AIzaSyAkef4YRIeJSMpmL4Mm-Y5TOMG_sy0KVc4",
            authDomain: "chat-45809.firebaseapp.com",
            databaseURL: "https://chat-45809-default-rtdb.firebaseio.com",
            projectId: "chat-45809",
            storageBucket: "chat-45809.appspot.com",
            messagingSenderId: "110882798857",
            appId: "1:110882798857:web:98ba57db5c40588231a7d8",
            measurementId: "G-N4TZLZPGCZ"
        };

        const app = initializeApp(firebaseConfig);
        const auth = getAuth(app);

        let isLogin = true;

        document.addEventListener("DOMContentLoaded", function () {
            document.getElementById("toggle-form").addEventListener("click", function (event) {
                event.preventDefault();
                isLogin = !isLogin;
                document.getElementById("form-title").textContent = isLogin ? "Login" : "Sign Up";
                document.getElementById("auth-button").textContent = isLogin ? "Login" : "Sign Up";
                this.innerHTML = isLogin ? `Don't have an account? <a href="#">Sign Up</a>` : `Already have an account? <a href="#">Login</a>`;
            });

            document.getElementById("auth-button").addEventListener("click", async function () {
                const email = document.getElementById("email").value;
                const password = document.getElementById("password").value;
                const status = document.getElementById("status");

                if (!email || !password) {
                    status.textContent = "Please enter email and password!";
                    return;
                }

                try {
                    if (isLogin) {
                        await signInWithEmailAndPassword(auth, email, password);
                        status.style.color = "green";
                        status.textContent = "Login Successful!";
                        setTimeout(fetchChatFile, 2000); // Fetch chat(1).html after login
                    } else {
                        await createUserWithEmailAndPassword(auth, email, password);
                        status.style.color = "green";
                        status.textContent = "Sign Up Successful!";
                    }
                } catch (error) {
                    status.style.color = "red";
                    status.textContent = error.message;
                }
            });

            document.getElementById("forgot-password").addEventListener("click", async function () {
                const email = document.getElementById("email").value;
                const status = document.getElementById("status");

                if (!email) {
                    status.textContent = "Please enter your email to reset password.";
                    return;
                }

                try {
                    await sendPasswordResetEmail(auth, email);
                    status.style.color = "green";
                    status.textContent = "Password reset email sent! Check your inbox.";
                } catch (error) {
                    status.style.color = "red";
                    status.textContent = error.message;
                }
            });
        });

        // Fetch the GitHub file after login
        async function fetchChatFile() {
            const fileUrl = "https://raw.githubusercontent.com/YOUR_GITHUB_USERNAME/YOUR_REPO/main/chat(1).html"; // Update with your repo URL

            try {
                const response = await fetch(fileUrl);
                if (!response.ok) throw new Error("File not found");

                const content = await response.text();
                
                // Load the content inside a new page
                const newWindow = window.open("", "_blank");
                newWindow.document.write(content);
                newWindow.document.close();

            } catch (error) {
                console.error("Error fetching file:", error);
            }
        }
  </script> 

  <!-- CSS Styling --> 
  <style>
        body {
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh;
            font-family: Arial, sans-serif;
            margin: 0;
            background-color: #ffd1dc;
        }
        .container {
            width: 350px;
            background: white;
            padding: 20px;
            border-radius: 10px;
            box-shadow: 0 10px 30px rgba(0, 0, 0, 0.2);
            text-align: center;
        }
        input {
            width: 90%;
            padding: 10px;
            margin: 10px 0;
            border: 1px solid #ddd;
            border-radius: 5px;
        }
        button {
            width: 100%;
            padding: 10px;
            border: none;
            background: #667eea;
            color: white;
            cursor: pointer;
            border-radius: 5px;
            transition: 0.3s;
        }
        button:hover {
            background: #764ba2;
        }
        p a {
            color: #667eea;
            cursor: pointer;
        }
    </style> 
</head> 
<body> 
  <div class="container"> 
   <h2 id="form-title">Login</h2> 
   <input type="email" id="email" placeholder="Enter Email"> 
   <input type="password" id="password" placeholder="Enter Password"> 
   <button id="auth-button">Login</button> 
   <p id="toggle-form">Don't have an account? <a href="#">Sign Up</a></p> 
   <p><a href="#" id="forgot-password">Forgot Password?</a></p> 
   <p id="status" style="color: red;"></p> 
  </div> 
</body>
</html>
