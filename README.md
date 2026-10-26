# ♟️ Mobile Chess Application

Dự án phát triển ứng dụng game Cờ vua đa nền tảng trên thiết bị di động (Mobile App) với các tính năng đột phá như sử dụng thẻ bài kỹ năng, tính điểm sinh mệnh (HP) và hệ thống bot thông minh.

## 🛠 Ngăn xếp công nghệ (Tech Stack)
- **Frontend:** React Native
- **Backend:** NestJS
- **Database & Auth:** Supabase (PostgreSQL)
- **Realtime / Socket:** WebSocket + RestAPI
- **Deployment:** Fly.io
- **Containerization:** Docker
- **Bot Engine:** Tích hợp Chess Engine
- **Version Control:** Git (GitHub)

## 👥 Đội ngũ phát triển (Team Members)
- **Nhóm trưởng:** Duy Anh
- **Frontend:** Tuấn Anh
- **Backend:** Biển, Thi
- **Database:** Phương Anh

---

## 🚀 Tính năng nổi bật (Features)

### Chức năng chính:
- Chơi cờ vua cơ bản (Di chuyển, bắt quân, nhập thành, phong cấp...).
- Chế độ chơi: PvP (Người vs Người) & PvE (Người vs Bot theo nhiều cấp độ).
- **Hệ thống Sinh mệnh (HP):** Điểm sinh mệnh ảnh hưởng đến kết quả trận đấu.
- **Thẻ bài chức năng:** Sử dụng thẻ bài để tạo lợi thế chiến thuật với các hiệu ứng đặc biệt.
- **Cửa hàng (Shop):** Mua sắm vật phẩm, skin quân cờ, thẻ bài.

### Yêu cầu phi chức năng:
- **Chịu tải:** Xử lý đồng thời 10.000 người chơi.
- **On time (Uptime):** 99%.
- **Độ trễ (Latency):** < 0.05s cho mỗi thao tác.
- **Bảo mật:** Tiêu chuẩn cao, xác thực JWT, bảo mật Socket.

---

## 📊 Bảng tiến độ và Phân công công việc (Đã hoàn thành)

Dưới đây là danh sách các đầu việc (Issues) mà các thành viên trong nhóm đã hoàn thành qua các giai đoạn (Sprint 1 -> Sprint 4):

| Thành viên | Vai trò | Các đầu việc đã hoàn thành (Sprints 1 - 4) |
| :--- | :--- | :--- |
| **Tuấn Anh** | Frontend | - **Khởi tạo:** Setup React Native chuẩn, env API, Navigation.<br>- **UI Cơ bản:** Màn Home, Login, Register (Skeleton & hoàn thiện), Profile, Theme.<br>- **Bàn cờ & Trận đấu:** Chess Board UI, Render quân cờ, hiệu ứng chọn & di chuyển quân cờ, Turn indicator, thông báo Check/Checkmate, Layout cơ bản màn chơi (thời gian, sinh mệnh, đối thủ). |
| **Biển** | Backend (Game/Socket) | - **Nền tảng:** NestJS structure, Auth/User module, WebSocket structure, Error handling.<br>- **Core Match:** Create/Get match, Make move API, Game state service, Match state machine.<br>- **Realtime & State:** Chuẩn hóa GameService/Gateway, định nghĩa event Socket.<br>- **Nâng cao:** Matchmaking (Ghép trận), xử lý reconnect/disconnect/timeout/resign. Server là nguồn state duy nhất. |
| **Thi** | Backend (API/Engine)| - **Engine Cơ bản:** Board state, Piece model, Move validation, Turn system, Check, Checkmate, Draw.<br>- **Luật cờ vua (Đầy đủ):** Nhập thành (Castling), Bắt tốt qua đường (En passant), Phong cấp, Stalemate, 3 lần lặp lại, luật 50 nước, Serialize/Restore game state.<br>- **API & Test:** JWT REST + WebSocket guard, validation DTO, chuẩn lỗi API. API tạo/xem trận, lưu FEN, move history. API/game-rule tests. |
| **Phương Anh**<br>*(và Leader)* | Database | - **Thiết kế & Khởi tạo:** Bảng profiles, matches, match_players, moves, login_logs, user_settings.<br>- **Mở rộng bảng:** Match status, match result, move sequence, captured pieces.<br>- **Tối ưu:** Migrations, RLS (Row Level Security) Supabase, thiết lập index cơ bản.<br>- **Giao dịch:** Xử lý transaction lưu move, policy đọc lịch sử, seed dữ liệu test. |

---

## ⚙️ Luồng trạng thái trận đấu (Match State Machine)

Hệ thống quản lý trạng thái trận đấu được thiết kế chặt chẽ nhằm xử lý mọi tình huống thực tế (bao gồm cả việc mất kết nối mạng):

```text
WAITING
   │
   ▼
READY
   │
   ▼
PLAYING
   │
   ├──── DISCONNECTED ──▶ RECONNECT ──▶ PLAYING
   │
   ├──── TIMEOUT
   │
   ├──── RESIGN
   │
   └──── CHECKMATE
              │
              ▼
           FINISHED
```
*Trạng thái phụ trợ: `CANCELLED`, `ABANDONED`, `DRAW`.*

**Luồng xử lý nước đi (Move Rule):**
`Client (User A đi cờ)` ➔ `WebSocket` ➔ `Backend` ➔ `Validate (Đúng lượt? Quân tồn tại? Hợp lệ? Check? Card effect? State?)` ➔ `Hợp lệ` ➔ `Update Game State` ➔ `Lưu Move DB` ➔ `Broadcast to Player A & B`.

---

## 📜 Quy định làm việc với Git

Để đảm bảo source code luôn ổn định, toàn team tuân thủ các quy tắc sau:
1. **Pull trước khi tạo code mới:** Luôn cập nhật code mới nhất từ nhánh chính trước khi bắt đầu code.
2. **Push theo tính năng:** Tạo branch mới mang tên tính năng đang làm. Sửa tính năng nào thì commit/push đúng vào branch đó.
3. **Note lỗi:** Mọi lỗi phát sinh phải được note lại rõ ràng.
4. **Note update:** Bất kỳ bản cập nhật nào cũng cần ghi chú (commit message rõ ràng).
5. **Note thay đổi cấu trúc:** Nếu thay đổi cấu trúc thư mục hoặc database, phải thông báo và note lại ngay lập tức.