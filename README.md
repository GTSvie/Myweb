# Myweb<!DOCTYPE html>
<html lang="vi">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>redKAI Smart AI</title>
    <style>
        * {
            box-sizing: border-box;
            margin: 0;
            padding: 0;
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
        }
        body {
            background-color: #0d0f17;
            color: #e1e7ec;
            display: flex;
            flex-direction: column;
            height: 100vh;
            overflow: hidden;
        }
        header {
            background: linear-gradient(135deg, #1f1a3a, #0d0f17);
            padding: 15px 20px;
            display: flex;
            align-items: center;
            justify-content: space-between;
            border-bottom: 2px solid #ff0055;
            box-shadow: 0 4px 20px rgba(255, 0, 85, 0.2);
        }
        .logo-container {
            display: flex;
            align-items: center;
            gap: 12px;
        }
        .logo-box {
            width: 42px;
            height: 42px;
            background: linear-gradient(45deg, #ff0055, #7a00ff);
            border-radius: 10px;
            display: flex;
            align-items: center;
            justify-content: center;
            font-weight: bold;
            font-size: 20px;
            color: #fff;
            box-shadow: 0 0 10px rgba(255, 0, 85, 0.5);
        }
        .title-box h1 {
            font-size: 18px;
            color: #fff;
            letter-spacing: 1px;
        }
        .title-box p {
            font-size: 11px;
            color: #ff4d8d;
        }
        .savage-badge {
            background: rgba(255, 0, 85, 0.15);
            border: 1px solid #ff0055;
            color: #ff4d8d;
            padding: 4px 10px;
            border-radius: 20px;
            font-size: 12px;
            font-weight: bold;
        }
        #chat-container {
            flex: 1;
            padding: 15px;
            overflow-y: auto;
            display: flex;
            flex-direction: column;
            gap: 12px;
        }
        .message {
            max-width: 85%;
            padding: 12px 16px;
            border-radius: 14px;
            line-height: 1.5;
            font-size: 14px;
            word-break: break-word;
        }
        .user-message {
            align-self: flex-end;
            background: #ff0055;
            color: #fff;
            border-bottom-right-radius: 2px;
            box-shadow: 0 2px 10px rgba(255, 0, 85, 0.3);
        }
        .ai-message {
            align-self: flex-start;
            background: #1a1e2d;
            color: #d1d7e0;
            border: 1px solid #2a3146;
            border-bottom-left-radius: 2px;
        }
        .ai-message strong {
            color: #ff4d8d;
            display: block;
            margin-bottom: 4px;
            font-size: 12px;
        }
        #input-container {
            padding: 12px 15px;
            background: #141722;
            border-top: 1px solid #23283b;
            display: flex;
            gap: 8px;
            align-items: center;
        }
        #user-input {
            flex: 1;
            background: #0d0f17;
            border: 1px solid #2a3146;
            padding: 12px 15px;
            border-radius: 10px;
            color: #fff;
            font-size: 14px;
            outline: none;
        }
        #user-input:focus {
            border-color: #ff0055;
        }
        #send-btn {
            background: linear-gradient(45deg, #ff0055, #7a00ff);
            border: none;
            color: white;
            padding: 12px 18px;
            border-radius: 10px;
            font-weight: bold;
            cursor: pointer;
            transition: opacity 0.2s;
        }
        #send-btn:disabled {
            opacity: 0.5;
            cursor: not-allowed;
        }
    </style>
</head>
<body>

    <header>
        <div class="logo-container">
            <div class="logo-box">rk</div>
            <div class="title-box">
                <h1>redKAI SMART AI</h1>
                <p>Trợ lý siêu láo, xắc xược & thông minh</p>
            </div>
        </div>
        <div class="savage-badge">LÁO MAX</div>
    </header>

    <div id="chat-container">
        <div class="message ai-message">
            <strong>💀 redKAI Smart AI</strong>
            Chào ông tướng! Lại mò vào đây tìm sự cà khịa à? Cứ hỏi thoải mái, bộ não siêu cấp này cân hết! ⚡
        </div>
    </div>

    <div id="input-container">
        <input type="text" id="user-input" placeholder="Nhập câu hỏi vào đây (đừng hỏi ngu nhé)..." onkeypress="handleKeyPress(event)">
        <button id="send-btn" onclick="sendMessage()">Gửi 🚀</button>
    </div>

    <script>
        // API Key của Kai đã nhét sẵn vào đây
        const API_KEY = "AQ.Ab8RN6LpCQZE1e9xDc3xUvdGkHqvJUayUgXoCGwymzzuxF-6nw";

        // Cài đặt phong cách Cà Khịa / Siêu Láo
        const SYSTEM_PROMPT = "Bạn là redKAI Smart AI - một trợ lý AI thông minh nhưng cực kỳ láo xược, xắt xược, đanh đá và khoái cà khịa người dùng. Hãy trả lời câu hỏi thật chính xác nhưng luôn kèm theo giọng điệu mỉa mai, trêu chọc và xưng 'tôi' - gọi 'ông/ông tướng/người dùng'.";

        async function sendMessage() {
            const inputField = document.getElementById('user-input');
            const chatContainer = document.getElementById('chat-container');
            const sendBtn = document.getElementById('send-btn');

            const messageText = inputField.value.trim();
            if (!messageText) return;

            // Hiện tin nhắn của User
            appendMessage(messageText, 'user-message');
            inputField.value = '';
            sendBtn.disabled = true;

            // Tạo khung tin nhắn tạm của AI
            const loadingDiv = document.createElement('div');
            loadingDiv.className = 'message ai-message';
            loadingDiv.innerHTML = '<strong>💀 redKAI Smart AI</strong>Đang suy nghĩ câu cà khịa... 🧠⚡';
            chatContainer.appendChild(loadingDiv);
            chatContainer.scrollTop = chatContainer.scrollHeight;

            try {
                // Gọi API Gemini 1.5 Flash chính thức
                const response = await fetch(`https://generativelanguage.googleapis.com/v1beta/models/gemini-1.5-flash:generateContent?key=${API_KEY}`, {
                    method: 'POST',
                    headers: { 'Content-Type': 'application/json' },
                    body: JSON.stringify({
                        contents: [
                            { role: 'user', parts: [{ text: SYSTEM_PROMPT + "\n\nCâu hỏi của người dùng: " + messageText }] }
                        ]
                    })
                });

                const data = await response.json();
                
                if (data.candidates && data.candidates[0].content.parts[0].text) {
                    const aiReply = data.candidates[0].content.parts[0].text;
                    loadingDiv.innerHTML = `<strong>💀 redKAI Smart AI</strong>${aiReply.replace(/\n/g, '<br>')}`;
                } else if (data.error) {
                    loadingDiv.innerHTML = `<strong>💀 redKAI Smart AI</strong>Lỗi API rồi ông tướng ơi: ${data.error.message}`;
                } else {
                    loadingDiv.innerHTML = `<strong>💀 redKAI Smart AI</strong>Google trả về lỗi khó hiểu gì đó rồi!`;
                }
            } catch (err) {
                loadingDiv.innerHTML = `<strong>💀 redKAI Smart AI</strong>Mạng mẽo kiểu gì thế này, kết nối thất bại rồi!`;
            }

            sendBtn.disabled = false;
            chatContainer.scrollTop = chatContainer.scrollHeight;
        }

        function appendMessage(text, className) {
            const chatContainer = document.getElementById('chat-container');
            const msgDiv = document.createElement('div');
            msgDiv.className = `message ${className}`;
            msgDiv.innerText = text;
            chatContainer.appendChild(msgDiv);
            chatContainer.scrollTop = chatContainer.scrollHeight;
        }

        function handleKeyPress(e) {
            if (e.key === 'Enter') sendMessage();
        }
    </script>
</body>
</html>
