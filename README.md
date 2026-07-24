# OpenWrt Xiaomi Mi Router 3 Pro – GitHub Builder

Project này build firmware OpenWrt trực tiếp trên máy chủ GitHub. Máy Windows chỉ cần trình duyệt, không cần cài Ubuntu, WSL, Python hay công cụ biên dịch.

## Firmware mặc định

- Thiết bị: Xiaomi Mi Router 3 Pro
- Target: `ramips/mt7621`
- Profile: `xiaomi_mi-router-3-pro`
- Phiên bản mặc định: OpenWrt `24.10.2`
- File đầu ra: `...squashfs-sysupgrade.bin`

## Thành phần chính

- LuCI HTTPS và giao diện tiếng Việt
- WireGuard
- OpenVPN
- Policy-Based Routing
- SQM
- DDNS
- Watchcat
- vnStat
- Tailscale có thể bật/tắt khi build
- irqbalance, htop, nano, curl và tcpdump-mini

SQM, PBR, OpenVPN, Tailscale và Watchcat không tự chạy trước khi được cấu hình. Software flow offloading được bật; hardware flow offloading bị tắt để tránh xung đột với SQM/PBR.

## Cách sử dụng trên Windows

1. Tạo tài khoản GitHub và đăng nhập.
2. Tạo repository mới, chọn **Private** cũng được.
3. Giải nén ZIP này.
4. Trong repository, chọn **Add file → Upload files** rồi kéo toàn bộ nội dung đã giải nén vào. Phải thấy thư mục `.github/workflows` trong repository.
5. Mở tab **Actions**.
6. Chọn workflow **Build OpenWrt R3P VPN Stable**.
7. Chọn **Run workflow**.
8. Giữ `24.10.2`; chọn `true` hoặc `false` cho Tailscale; bấm **Run workflow**.
9. Chờ khoảng 5–15 phút. Khi dấu tích xanh xuất hiện, mở lần chạy đó.
10. Ở cuối trang, tải Artifact có tên `OpenWrt-R3P-24.10.2-VPN-Stable`.
11. Giải nén Artifact để lấy file `sysupgrade.bin` và `SHA256SUMS`.

## Flash trên router đang chạy OpenWrt 24.10.2

Vào **System → Backup / Flash Firmware → Flash new firmware image**.

- Chọn file kết thúc bằng `xiaomi_mi-router-3-pro-squashfs-sysupgrade.bin`.
- Lần đầu dùng bản tùy biến này nên bỏ chọn **Keep settings**.
- Không chọn **Force upgrade**.
- Không rút điện trong quá trình flash.

Breed vẫn nằm ở vùng bootloader riêng; sysupgrade thông thường không ghi đè Breed. Tuy vậy, luôn giữ bản sao FULL FLASH/EEPROM đã backup trước đó.

## Tùy chỉnh package

Mở `packages.txt`, mỗi dòng là một package. Xóa package không cần hoặc thêm package hợp lệ trong kho đúng phiên bản OpenWrt. Nếu một package không tồn tại, workflow sẽ dừng và log sẽ ghi rõ tên package gây lỗi.

## Lưu ý về Tailscale

Tailscale làm firmware lớn hơn. Nếu build báo vượt kích thước hoặc bạn không dùng Tailscale, chạy lại workflow với `include_tailscale=false`.
