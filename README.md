<!DOCTYPE html>
<html lang="ko">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>익명 채팅 커뮤니티</title>
    <style>
        body {
            font-family: 'Arial', sans-serif;
            background: linear-gradient(to right, #ff9966, #ff5e62);
            display: flex;
            flex-direction: column;
            align-items: center;
            height: 100vh;
            position: relative;
        }
        .chat-container {
            width: 50%;
            max-width: 600px;
            background: white;
            padding: 20px;
            border-radius: 15px;
            box-shadow: 0 4px 15px rgba(0, 0, 0, 0.2);
            text-align: center;
            margin-top: 50px;
        }
        .chat-box {
            height: 300px;
            overflow-y: auto;
            border: 2px solid #ff5e62;
            padding: 10px;
            margin-bottom: 15px;
            background: #fff;
            border-radius: 10px;
        }
        input {
            width: 75%;
            padding: 12px;
            border: 2px solid #ff5e62;
            border-radius: 8px;
            outline: none;
            font-size: 14px;
        }
        button {
            width: 20%;
            padding: 12px;
            background: #ff5e62;
            color: white;
            border: none;
            border-radius: 8px;
            cursor: pointer;
            font-size: 14px;
            transition: 0.3s;
        }
        button:disabled {
            background: #aaa;
            cursor: not-allowed;
        }
        button:hover:not(:disabled) {
            background: #d64040;
        }
        .server-selection {
            margin-bottom: 15px;
        }
        select {
            padding: 8px;
            border-radius: 5px;
            border: 2px solid #ff5e62;
        }
        .user-id {
            position: absolute;
            top: 10px;
            left: 20px;
            background: rgba(255, 94, 98, 0.9);
            color: white;
            padding: 6px 12px;
            border-radius: 5px;
            font-size: 14px;
            font-weight: bold;
        }
    </style>
</head>
<body>
    <div class="user-id" id="userId">ID: 생성 중...</div>
    <div class="chat-container">
        <h2 style="color:#ff5e62;">익명 채팅 커뮤니티</h2>
        <div class="server-selection">
            <label for="serverSelect">서버 선택: </label>
            <select id="serverSelect" onchange="changeServer()">
                <option value="server1">서버 1</option>
                <option value="server2">서버 2</option>
                <option value="server3">서버 3</option>
                <option value="server4">서버 4</option>
                <option value="server5">서버 5</option>
            </select>
        </div>
        <div class="chat-box" id="chatBox"></div>
        <input type="text" id="messageInput" placeholder="메시지를 입력하세요...">
        <button id="sendButton" onclick="sendMessage()">보내기</button>
    </div>

    <script>
        let userId = localStorage.getItem("userId") || generateUserId();
        document.getElementById("userId").textContent = "ID: " + userId;

        let cooldown = false;

        function generateUserId() {
            const newId = "UID-" + Math.random().toString(36).substr(2, 9);
            localStorage.setItem("userId", newId);
            return newId;
        }

        function changeServer() {
            document.getElementById("chatBox").innerHTML = "";
        }

        function sendMessage() {
            if (cooldown) return;
            
            const input = document.getElementById("messageInput");
            const chatBox = document.getElementById("chatBox");
            const sendButton = document.getElementById("sendButton");
            
            if (input.value.trim() === "") return;
            
            const message = document.createElement("p");
            message.style.color = "#ff5e62";
            message.style.fontWeight = "bold";
            message.textContent = userId + " : " + input.value;
            chatBox.appendChild(message);
            chatBox.scrollTop = chatBox.scrollHeight;
            input.value = "";
            
            cooldown = true;
            sendButton.disabled = true;
            setTimeout(() => {
                cooldown = false;
                sendButton.disabled = false;
            }, 5000);
        }

        function resetServers() {
            document.getElementById("chatBox").innerHTML = "";
            localStorage.removeItem("userId");
        }
        
        setInterval(() => {
            let now = new Date();
            if (now.getHours() === 0 && now.getMinutes() === 0) {
                resetServers();
            }
        }, 60000);
    </script>
</body>
</html>


 
