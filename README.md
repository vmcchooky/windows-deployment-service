# Windows Deployment Services (WDS) trên Windows Server 2022

![Windows Server](https://shields.io)
![VMware](https://shields.io)
![Status](https://shields.io)

Đồ án cuối kỳ môn **Quản trị hệ thống mạng** - Khoa Công nghệ thông tin, Trường Đại học Tôn Đức Thắng (TDTU).

## 📝 Giới thiệu
Dự án tập trung vào việc triển khai và cấu hình **Windows Deployment Services (WDS)** để tự động hóa quá trình cài đặt hệ điều hành Windows qua mạng LAN. Giải pháp này giúp quản trị viên không cần dùng USB/DVD vật lý, tiết kiệm thời gian và giảm thiểu sai sót khi triển khai số lượng lớn máy trạm.

## 🛠 Môi trường triển khai
*   **Phần mềm ảo hóa:** VMware® Workstation 17 Pro.
*   **Hệ điều hành máy chủ:** Windows Server 2022 (Domain Controller).
*   **Hệ điều hành triển khai:** Windows 10 Pro.
*   **Dịch vụ đi kèm:** 
    * Active Directory Domain Services (AD DS)
    * Domain Name System (DNS)
    * Dynamic Host Configuration Protocol (DHCP)

## 🚀 Các tính năng chính
- [x] **PXE Boot:** Khởi động máy khách từ card mạng qua giao thức PXE.
- [x] **Image Management:** Quản lý tập trung Boot Images và Install Images (.wim).
- [x] **Automation:** Hỗ trợ cài đặt tự động và tự động join domain sau khi cài đặt.
- [x] **Dual Configuration:** Triển khai qua cả giao diện đồ họa (GUI) và dòng lệnh (PowerShell).

## 📖 Tóm tắt quy trình thực hiện
1.  **Cấu hình hạ tầng:** Thiết lập IP tĩnh, cài đặt AD DS, DNS và DHCP Server.
2.  **Chuẩn bị lưu trữ:** Định dạng ổ đĩa NTFS để lưu trữ tệp image có dung lượng lớn (>4GB).
3.  **Cấu hình WDS:**
    *   Tích hợp WDS với Active Directory.
    *   Thiết lập chế độ phản hồi PXE (Respond to all clients).
    *   Thêm `boot.wim` và `install.wim` từ tệp ISO gốc của Windows.
4.  **Triển khai:** 
    *   Cấu hình DHCP Options (60, 66, 67).
    *   Khởi động máy Client qua mạng, nhấn F12 để gửi Request ID.
    *   Phê duyệt (Approve) thiết bị trên Server và tiến hành cài đặt.

## 💻 Đoạn mã PowerShell tiêu biểu
Để cài đặt nhanh vai trò WDS, nhóm đã sử dụng lệnh:
```powershell
# Cài đặt vai trò WDS và công cụ quản lý
Install-WindowsFeature -Name WDS -IncludeManagementTools

# Kiểm tra trạng thái dịch vụ
Get-WindowsFeature -Name WDS
```

## ⚠️ Lưu ý kỹ thuật
*   **Firewall:** Cần tắt tường lửa trên máy chủ hoặc mở các port liên quan để Client có thể kết nối.
*   **Network:** Server và Client phải cùng kết nối vào một phân đoạn mạng (Custom VMnet) để nhận diện được DHCP và WDS.

## 👥 Tác giả
*   **Nguyễn Hải Đăng** - 52200274
*   **Võ Mạnh Cường** - 52200319
*   **Giảng viên hướng dẫn:** ThS. Lê Viết Thanh

---
*Dự án thực hiện tại TP. Hồ Chí Minh – 2024*
