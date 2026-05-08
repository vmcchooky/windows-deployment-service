# Windows Deployment Services (WDS) Implementation on Windows Server 2022

[![Windows Server](https://img.shields.io/badge/OS-Windows%20Server%202022-0078D4?style=flat-square&logo=windows-server&logoColor=white)](https://www.microsoft.com/en-us/windows-server)
[![VMware](https://img.shields.io/badge/Platform-VMware%20Workstation-607078?style=flat-square&logo=vmware&logoColor=white)](https://www.vmware.com/)
[![Status](https://img.shields.io/badge/Status-Completed-success?style=flat-square)](https://github.com/)

> **Đồ án cuối kỳ:** Quản trị hệ thống mạng
> **Đơn vị:** Khoa Công nghệ thông tin - Trường Đại học Tôn Đức Thắng (TDTU)

## 📝 Giới thiệu
Dự án tập trung triển khai giải pháp **Windows Deployment Services (WDS)** nhằm tự động hóa quy trình phân phối hệ điều hành Windows qua môi trường mạng LAN. Giải pháp này giúp quản trị viên tối ưu hóa hiệu suất, loại bỏ sự phụ thuộc vào thiết bị lưu trữ vật lý (USB/DVD) và đảm bảo tính đồng nhất cho hạ tầng máy trạm số lượng lớn.



## 🛠 Môi trường triển khai

| Thành phần | Chi tiết cấu hình |
| :--- | :--- |
| **Phần mềm ảo hóa** | VMware® Workstation 17 Pro |
| **Server OS** | Windows Server 2022 (Standard Edition) |
| **Client OS** | Windows 10 Pro (64-bit) |
| **Network Roles** | AD DS, DNS, DHCP, WDS |
| **Storage** | NTFS Partition (Dedicated for Images) |

## 🚀 Các tính năng chính
- [x] **PXE Boot Integration:** Khởi động và nhận diện máy khách qua card mạng (Preboot Execution Environment).
- [x] **Centralized Image Management:** Quản lý tập trung các tệp tin `boot.wim` và `install.wim`.
- [x] **Zero-Touch Deployment (Lite):** Hỗ trợ trả lời tự động và tự động gia nhập tên miền (Join Domain).
- [x] **Hybrid Management:** Linh hoạt cấu hình thông qua GUI hoặc script tự động hóa PowerShell.

## 📖 Quy trình thực hiện tiêu chuẩn

### 1. Chuẩn bị hạ tầng (Infrastructure)
* Thiết lập IP tĩnh (Static IP) cho Domain Controller.
* Triển khai và cấu hình **AD DS, DNS, DHCP**.
* Đảm bảo Scope DHCP hoạt động để cấp phát IP cho máy khách.

### 2. Cấu hình WDS Role
* **Initialize Server:** Chọn chế độ *Integrated with Active Directory*.
* **PXE Response:** Thiết lập *Respond to all client computers (known and unknown)*.
* **Image Selection:**
    * **Boot Image:** Trích xuất từ bộ cài Windows (`\sources\boot.wim`).
    * **Install Image:** Thêm các phiên bản Windows cụ thể từ tệp `install.wim`.

### 3. Tối ưu hóa DHCP cho PXE
Cấu hình các DHCP Options để định hướng Client tìm thấy WDS Server:
* `Option 60`: PXEClient
* `Option 66`: Host name of the boot server (IP của WDS Server)
* `Option 67`: Boot file name (e.g., `boot\x64\pxeboot.com`)

## 💻 Quản trị bằng PowerShell
Sử dụng PowerShell giúp quá trình cài đặt nhanh chóng và chính xác hơn:

```powershell
# 1. Cài đặt vai trò WDS và các công cụ quản lý đi kèm
Install-WindowsFeature -Name WDS -IncludeManagementTools

# 2. Khởi tạo cấu hình WDS cơ bản
WDSUTIL /Initialize-Server /RemInst:"D:\RemoteInstall"

# 3. Kiểm tra trạng thái dịch vụ sau khi cấu hình
Get-Service -Name WDSServer | Select-Object Name, Status, StartType

## ⚠️ Lưu ý kỹ thuật
*   **Firewall:** Cần tắt tường lửa trên máy chủ hoặc mở các port liên quan để Client có thể kết nối.
*   **Network:** Server và Client phải cùng kết nối vào một phân đoạn mạng (Custom VMnet) để nhận diện được DHCP và WDS.

## 👥 Tác giả
*   **Nguyễn Hải Đăng** - 52200274
*   **Võ Mạnh Cường** - 52200319
*   **Giảng viên hướng dẫn:** ThS. Lê Viết Thanh

---
*Dự án thực hiện tại TP. Hồ Chí Minh – 2024*
