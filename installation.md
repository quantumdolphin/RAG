# 🧐 OCR Scanned PDF using Tesseract and PyMuPDF (Python)

This script extracts text from **scanned PDFs** using **OCR (Optical Character Recognition)**. It combines:

* `pytesseract`: Python wrapper for Tesseract OCR
* `PyMuPDF (fitz)`: To render PDF pages as images
* `Pillow`: For image handling

---

## 📦 Environment Setup

### 1. Create Conda Environment

```bash
conda create -n ocr_env python=3.10 -y
conda activate ocr_env
```

### 2. Install Python Dependencies

```bash
pip install pymupdf pillow pytesseract
```

---

## 🔧 Install Tesseract OCR (Windows)

### 🔹 Recommended Version for Windows:

📅 [Download EXE Installer from UB Mannheim](https://github.com/UB-Mannheim/tesseract/wiki)



### ✅ During install:

* Check: **“Add Tesseract to the system PATH”**
* Default install location: `C:\Program Files\Tesseract-OCR\tesseract.exe`

### ❗ If not added to PATH:

Manually add `C:\Program Files\Tesseract-OCR` to your system PATH via **Environment Variables**.

---

## ✅ Verifying Installation

```bash
tesseract --version
```

Should return version info like:

```
tesseract v5.x.x
```

---

## 🧪 Python Script: OCR All Pages of a Scanned PDF

Save this as `ocr_pdf.py`:

```python
import fitz  # PyMuPDF
import pytesseract
from PIL import Image
import io

# Set path to Tesseract executable manually
pytesseract.pytesseract.tesseract_cmd = r"C:\\Program Files\\Tesseract-OCR\\tesseract.exe"

# === CONFIGURATION ===
input_pdf = "C:/path/to/your/70_page_file.pdf"         # <- Change this
output_txt = "C:/path/to/output_text.txt"              # <- Change this

# === OCR PROCESSING ===
doc = fitz.open(input_pdf)
print(f"Total pages: {len(doc)}")

with open(output_txt, "w", encoding="utf-8") as out_file:
    for page_num in range(len(doc)):
        print(f"[INFO] Processing page {page_num + 1}/{len(doc)}")
        page = doc.load_page(page_num)
        pix = page.get_pixmap(dpi=300)
        img = Image.open(io.BytesIO(pix.tobytes("png")))
        text = pytesseract.image_to_string(img)
        out_file.write(f"\n--- Page {page_num + 1} ---\n{text}\n")

print(f"[DONE] All text saved to: {output_txt}")
```

---

## 📂 Output

* All extracted text will be saved into a single `.txt` file
* Each page's text is separated with a header like `--- Page 1 ---`

---

## 🚀 To Run

```bash
python ocr_pdf.py
```

---

## 📌 Tips

* For high accuracy, DPI is set to `300` in `get_pixmap(dpi=300)`
* Works best on clean, high-contrast scanned documents
* You can modify the script to:

  * Output one `.txt` file per page
  * Handle a folder of PDFs
  * Save in `.csv`, `.json`, or database

---

## 🧪 Sample Output

```
--- Page 1 ---
This is a scanned PDF document.
Text is extracted using OCR...

--- Page 2 ---
Another page of content...
```

---

## 🧠 Credits

* [Tesseract OCR](https://github.com/tesseract-ocr/tesseract)
* [PyMuPDF (fitz)](https://pymupdf.readthedocs.io/)
* [Pillow](https://python-pillow.org/)
