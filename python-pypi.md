# 🐍 آموزش کامل ساخت و انتشار پکیج پایتون در PyPI

این راهنما شما را به صورت گام به گام با فرآیند ساخت، بسته‌بندی و انتشار یک پکیج پایتون روی PyPI آشنا می‌کند.

---

## 📁 مرحله ۱: ساختاردهی پروژه

ساختار پیشنهادی برای پروژه به شکل زیر است:

```
your_package/
│
├── src/
│   └── your_package/
│       ├── __init__.py
│       └── module.py
├── tests/
│   └── test_module.py
├── README.md
├── LICENSE
├── pyproject.toml
├── setup.cfg
└── setup.py (اختیاری)
```

---

## ⚙️ مرحله ۲: تنظیم فایل `pyproject.toml`

```toml
[build-system]
requires = ["setuptools>=61.0", "wheel"]
build-backend = "setuptools.build_meta"
```

---

## ⚙️ مرحله ۳: تنظیم فایل `setup.cfg`

```ini
[metadata]
name = your-package-name
version = 0.1.0
author = Your Name
author_email = your@email.com
description = توضیح کوتاه درباره پکیج
long_description = file: README.md
long_description_content_type = text/markdown
url = https://github.com/yourusername/your-package
license = MIT
classifiers =
    Programming Language :: Python :: 3
    License :: OSI Approved :: MIT License
    Operating System :: OS Independent

[options]
package_dir =
    = src
packages = find:
python_requires = >=3.7

[options.packages.find]
where = src
```

---

## 📄 مرحله ۴: فایل `README.md`

```md
# Your Package Name

توضیحاتی در مورد اینکه این پکیج چه کاری انجام می‌دهد.

## نصب

```bash
pip install your-package-name
```


---

## 🧪 مرحله ۵: ساخت محیط مجازی (اختیاری ولی پیشنهاد شده)

```bash
python -m venv venv
source venv/bin/activate  # ویندوز: venv\Scripts\activate
```

---

## 🧰 مرحله ۶: نصب ابزارهای مورد نیاز

```bash
pip install build twine
```

---

## 🏗️ مرحله ۷: ساخت پکیج

```bash
python -m build
```

خروجی پکیج در پوشه `dist/` ظاهر خواهد شد:

```
dist/
├── your_package_name-0.1.0.tar.gz
└── your_package_name-0.1.0-py3-none-any.whl
```

---

## ☁️ مرحله ۸: آپلود به PyPI

### 🔐 روش امن (توصیه‌شده): استفاده از فایل `.pypirc`

۱. فایل `~/.pypirc` را بسازید:

```ini
[distutils]
index-servers =
    pypi

[pypi]
username = __token__
password = pypi-XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX
```

۲. سطح دسترسی فایل را محدود کنید:

```bash
chmod 600 ~/.pypirc
```

۳. آپلود با Twine:

```bash
twine upload dist/*
```
یا
```bash
twine upload --config-file .pypirc dist/*
```
---

### 🧪 آپلود به PyPI آزمایشی (اختیاری)

برای تست قبل از انتشار واقعی:

در `.pypirc`:

```ini
[testpypi]
repository = https://test.pypi.org/legacy/
username = __token__
password = pypi-XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX
```

و اجرای:

```bash
twine upload -r testpypi --config-file .pypirc dist/*
```
یا
```bash
twine upload -r testpypi dist/*
```
---

### ⛔ روش ساده‌تر (کمتر امن)

```bash
twine upload --username __token__ --password pypi-XXXXXXXXXXXXXXXX dist/*
```

---

## ✅ مرحله ۹: بررسی قبل از آپلود (اختیاری)

```bash
twine check dist/*
```

---

## 📦 مرحله ۱۰: نصب و تست پکیج

```bash
pip install your-package-name
```

### نصب نسخه آزمایشی
```bash
pip install --index-url https://test.pypi.org/simple/ --extra-index-url https://pypi.org/simple your-package-name
```


---

## 🔁 مرحله ۱۱: به‌روزرسانی نسخه و انتشار مجدد

هر بار قبل از انتشار جدید، نسخه را در `setup.cfg` افزایش دهید:

```ini
version = 0.1.1
```

سپس مراحل ساخت و آپلود را تکرار کنید:

```bash
python -m build
twine upload dist/*
```

---

## 📚 منابع بیشتر

- [مستندات رسمی PyPI](https://packaging.python.org/)
- [ساخت پکیج با setuptools](https://setuptools.pypa.io/)
- [ابزار Twine](https://twine.readthedocs.io/)

---
---

## ⚙️ مرحله ۱۲: فایل setup.py (اختیاری)

در صورتی که نیاز به فایل `setup.py` دارید، می‌توانید یکی از دو نسخه زیر را استفاده کنید:

### ✅ نسخه ساده (در کنار setup.cfg)

```python
from setuptools import setup

setup()
```

این نسخه کافی است اگر تمام تنظیمات شما در `setup.cfg` قرار داشته باشند.

---

### 🧩 نسخه کامل‌تر (در صورت عدم استفاده از setup.cfg)

```python
from setuptools import setup, find_packages

setup(
    name="your-package-name",
    version="0.1.0",
    author="Your Name",
    author_email="your@email.com",
    description="توضیح کوتاه درباره پکیج",
    long_description=open("README.md", encoding="utf-8").read(),
    long_description_content_type="text/markdown",
    url="https://github.com/yourusername/your-package",
    packages=find_packages(where="src"),
    package_dir={"": "src"},
    classifiers=[
        "Programming Language :: Python :: 3",
        "License :: OSI Approved :: MIT License",
        "Operating System :: OS Independent",
    ],
    python_requires=">=3.7",
    license="MIT",
)
```

> توصیه می‌شود در پروژه‌های مدرن از `setup.cfg` + `pyproject.toml` به همراه `setup.py` ساده استفاده شود.
