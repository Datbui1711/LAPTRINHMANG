# 💬 Chat Application với Python Asyncio

Ứng dụng chat real-time với hai phiên bản:
1. **Console Chat** - Chat qua terminal
2. **Web Chat** - Giao diện web hiện đại với WebSocket

## 🌟 Tính năng

### Console Version
- ✅ Xử lý nhiều client kết nối đồng thời
- ✅ Broadcast tin nhắn đến tất cả client
- ✅ Nickname cho mỗi người dùng
- ✅ Hiển thị timestamp cho mỗi tin nhắn
- ✅ Thông báo khi có người tham gia/rời khỏi
- ✅ Các lệnh hữu ích (`/users`, `/quit`)
- ✅ Xử lý lỗi và ngắt kết nối gracefully

### Web Version
- ✅ Giao diện web đẹp, responsive
- ✅ Real-time messaging với WebSocket
- ✅ Typing indicator (hiển thị đang gõ)
- ✅ Online users list với avatar
- ✅ Message history
- ✅ Emoji picker 😊
- ✅ Toast notifications
- ✅ Smooth animations

## 📋 Yêu cầu

- Python 3.7 trở lên
- aiohttp (cho web version): `pip install aiohttp`

## 🚀 Cách sử dụng

### Phiên bản Web (Khuyến nghị) 🌐

#### 1. Cài đặt dependencies

```bash
pip install aiohttp
```

#### 2. Khởi động Web Server

```bash
python chat_web_server.py
```

Server sẽ chạy tại: **http://127.0.0.1:8080**

#### 3. Mở trình duyệt

Truy cập: **http://127.0.0.1:8080**

- Nhập tên của bạn
- Bắt đầu chat với giao diện đẹp mắt!
- Mở nhiều tab/cửa sổ để test chat với nhiều người

### Phiên bản Console 💻

#### 1. Khởi động Console Server

```bash
python chat_server.py
```

Server sẽ chạy trên `127.0.0.1:8888`

#### 2. Kết nối Client

Mở terminal khác và chạy:

```bash
python chat_client.py
```

Bạn có thể mở nhiều terminal để chạy nhiều client.

#### 3. Chat

- Nhập nickname khi được yêu cầu
- Gõ tin nhắn và Enter để gửi
- Sử dụng các lệnh:
  - `/users` - Xem danh sách người dùng online
  - `/quit` hoặc `/exit` - Thoát khỏi chat

## 🏗️ Kiến trúc

### Web Chat Server (`chat_web_server.py`)

- **WebChatServer class**: Quản lý WebSocket connections
  - `handle_websocket()`: Xử lý từng WebSocket connection
  - `broadcast_message()`: Gửi tin nhắn đến tất cả clients
  - `broadcast_user_list()`: Update danh sách users
  - `message_history`: Lưu 50 tin nhắn gần nhất
  
- **Web Framework**: Sử dụng `aiohttp` cho HTTP server và WebSocket
- **Real-time**: WebSocket cho two-way communication
- **Static Files**: Serve HTML/CSS/JS từ thư mục `static/`

### Web Client (HTML/CSS/JS)

- **index.html**: Cấu trúc UI với login và chat screens
- **style.css**: Modern styling với gradients, animations
- **app.js**: WebSocket client logic, DOM manipulation
  - Login flow
  - Send/receive messages
  - User list management
  - Typing indicators
  - Emoji picker

### Console Chat Server (`chat_server.py`)

- **ChatServer class**: Quản lý tất cả kết nối client
  - `handle_client()`: Xử lý từng client connection
  - `broadcast()`: Gửi tin nhắn đến tất cả client
  - `remove_client()`: Xóa client khi ngắt kết nối
  
- **Bất đồng bộ**: Sử dụng `asyncio.start_server()` để lắng nghe kết nối
- **Thread-safe**: Sử dụng `asyncio.Lock()` để bảo vệ shared state

### Console Chat Client (`chat_client.py`)

- **ChatClient class**: Quản lý kết nối đến server
  - `receive_messages()`: Task nhận tin nhắn từ server
  - `send_messages()`: Task gửi input của user đến server
  - `connect()`: Kết nối và điều phối các task

- **Concurrent I/O**: Chạy đồng thời 2 tasks (nhận và gửi)

## 📝 Screenshots & Demo

### Web Chat Interface

```
┌─────────────────────────────────────────────────────┐
│  Login Screen                                       │
│  ┌───────────────────────────────────────────┐    │
│  │            💬                              │    │
│  │         Chat App                           │    │
│  │  Kết nối và trò chuyện thời gian thực    │    │
│  │                                            │    │
│  │  Nhập tên của bạn                         │    │
│  │  ┌──────────────────────────────────┐    │    │
│  │  │ Ví dụ: Huy Võ                    │    │    │
│  │  └──────────────────────────────────┘    │    │
│  │                                            │    │
│  │  [ Tham gia Chat → ]                      │    │
│  │                                            │    │
│  │  ⚡ Real-time  🔒 An toàn  🌐 Multi-user  │    │
│  └───────────────────────────────────────────┘    │
└─────────────────────────────────────────────────────┘

┌──────────────────────────────────────────────────────────────┐
│  Chat Screen                                                 │
│  ┌─────────────┬──────────────────────────────────────────┐ │
│  │ 💬 Chat App │  Phòng Chat Chung                       │ │
│  │ 🟢 Huy Võ   │  Alice đang nhập...                     │ │
│  │─────────────│──────────────────────────────────────────│ │
│  │ 👥 Online 3 │  [A] Alice    10:30  Xin chào!         │ │
│  │             │  [H] Huy Võ   10:31  Chào Alice!       │ │
│  │  • Alice    │  [B] Bob      10:32  Hi mọi người 👋   │ │
│  │  • Huy Võ   │                                         │ │
│  │  • Bob      │                                         │ │
│  │             │                                         │ │
│  │             │──────────────────────────────────────────│ │
│  │             │  😊 [Nhập tin nhắn của bạn...    ] 📤  │ │
│  │ [Thoát 🚪]  │                                         │ │
│  └─────────────┴──────────────────────────────────────────┘ │
└──────────────────────────────────────────────────────────────┘
```

### Console Chat Example

```
Terminal 1 (Server):
$ python chat_server.py
2025-12-12 10:00:00 - INFO - 🚀 Chat Server đang chạy trên 127.0.0.1:8888

Terminal 2 (Web):
$ python chat_web_server.py
2025-12-12 10:00:00 - INFO - 🚀 Khởi động Web Chat Server...
Running on http://127.0.0.1:8080
```

## 🔧 Tùy chỉnh

### Web Server
Thay đổi host và port trong `chat_web_server.py`:

```python
web.run_app(app, host='0.0.0.0', port=8080)
```

### Console Server
Thay đổi trong code:

```python
# Trong chat_server.py
server = ChatServer(host='0.0.0.0', port=9999)

# Trong chat_client.py
client = ChatClient(host='192.168.1.100', port=9999)
```

### Styling
Chỉnh sửa `static/style.css` để thay đổi:
- Colors: Sửa biến CSS trong `:root`
- Layout: Thay đổi flexbox/grid
- Animations: Tùy chỉnh `@keyframes`

## 🛡️ Xử lý lỗi

- ✅ Xử lý client ngắt kết nối đột ngột
- ✅ Xử lý lỗi network
- ✅ Cleanup resources khi server shutdown
- ✅ Logging đầy đủ để debug

## 📚 Kiến thức áp dụng

- **Asyncio**: Coroutines, tasks, event loop
- **Network Programming**: TCP sockets
- **Concurrent Programming**: Xử lý nhiều kết nối đồng thời
- **Error Handling**: Try/except/finally patterns
- **Resource Management**: Context managers, cleanup

## 📁 Cấu trúc Project

```
chat-app/
├── chat_server.py          # Console TCP server
├── chat_client.py          # Console TCP client
├── chat_web_server.py      # Web server với WebSocket
├── static/
│   ├── index.html          # Web UI
│   ├── style.css           # Styling
│   └── app.js              # Client-side logic
└── README.md
```

## 🎯 Mở rộng thêm

Các tính năng có thể thêm:
- ✨ Private messages giữa các user
- 🏠 Room/channel system  
- 🔐 Authentication với JWT
- 💾 Persistent message storage (database)
- 📎 File transfer & image sharing
- 🔍 Search messages
- 📱 Mobile responsive improvements
- 🌙 Dark/light theme toggle
- 🔔 Desktop notifications
- 📊 Admin dashboard
- 🎤 Voice messages
- 📹 Video chat integration
