# Extract Text from PowerPoint (.pptx)

## Step 1: Direct Text Extraction from PowerPoint Shapes
```python
from pptx import Presentation

# Load your PowerPoint file
ppt = Presentation("New targets_under evaluation_May12_2025.pptx")

# Loop through each slide and shape to extract text if available
for i, slide in enumerate(ppt.slides):
    print(f"--- Slide {i + 1} ---")
    for shape in slide.shapes:
        if hasattr(shape, "text"):
            print(shape.text)
```

## Step 2: OCR Extraction from Slide Images
```python
import pytesseract
from PIL import Image
import os

# Set the folder where slide images are stored
image_folder = "pptx_images"

# Optional: Dictionary to store OCR output for each image
ocr_results = {}

# Loop through each image file in the folder
for filename in sorted(os.listdir(image_folder)):
    if filename.lower().endswith(".png"):
        img_path = os.path.join(image_folder, filename)
        with Image.open(img_path) as img:
            # Run OCR to extract text
            text = pytesseract.image_to_string(img)
            ocr_results[filename] = text.strip()

            # Print the OCR results
            print(f"--- OCR from {filename} ---")
            print(text.strip())
            print("\n" + "-" * 40 + "\n")
```

## Notes
- Ensure `tesseract` is installed and available in the system PATH.
- For best OCR results, use high-resolution `.png` exports of your slides.
- This script combines both structured text extraction and fallback OCR when slides are image-based.
