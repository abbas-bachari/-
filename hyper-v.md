# 🔧 **فعال و غیرفعال کردن Hyper-V در ویندوز 11**  

### 📜 در این راهنما، دو اسکریپت ساده برای **فعال‌سازی**  و **غیرفعال‌سازی**  Hyper-V و ویژگی‌های مرتبط در ویندوز ۱۱ ارائه شده است.  

## 🛠 **نحوه استفاده:**  

#### 1️⃣ برنامه **Notepad** را باز کنید.  

#### 2️⃣ کد اسکریپت مورد نظر را کپی و در Notepad قرار دهید.  

#### 3️⃣ از منوی **File >  Save As** را بزنید.  

#### 4️⃣ نام فایل را یکی از موارد زیر قرار دهید:  
   - 🟢 `enable_hyperv.bat` برای **فعال‌سازی**  
   - 🔴 `disable_hyperv.bat` برای **غیرفعال‌سازی**  

#### 5️⃣ در قسمت **Save as type** گزینه ** All Files** را انتخاب کنید.  

#### 6️⃣ فایل را با  راست‌کلیک > **Run as Administrator** اجرا کنید.  

#### 7️⃣ پس از پایان عملیات، **سیستم را ری‌استارت کنید** 
---

## 📌 اسکریپت غیرفعال‌سازی Hyper-V

```bash
@echo off
title 🔻 Disable Hyper-V
echo ================================================
echo 🔻 Disabling Hyper-V and related features...
echo ================================================
echo Please wait...
echo.
dism.exe /Online /Disable-Feature:Microsoft-Hyper-V-All
dism.exe /Online /Disable-Feature:VirtualMachinePlatform
dism.exe /Online /Disable-Feature:Windows-Hypervisor-Platform
bcdedit /set hypervisorlaunchtype off
echo.
echo ✅ Done! Please restart your computer.
echo ================================================
pause
```



## 🚀 اسکریپت فعال‌سازی Hyper-V

```bash
@echo off
title 🚀 Enable Hyper-V
echo ================================================
echo 🚀 Enabling Hyper-V and related features..
echo ================================================
echo Please wait...
echo.
dism.exe /Online /Enable-Feature:Microsoft-Hyper-V-All /All
dism.exe /Online /Enable-Feature:VirtualMachinePlatform /All
dism.exe /Online /Enable-Feature:Windows-Hypervisor-Platform /All
bcdedit /set hypervisorlaunchtype auto
echo.
echo ✅ Done! Please restart your computer.
echo ================================================
pause
```
