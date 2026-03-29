# Photo-squeeze
For compressing photos pdf

import fitz  # PyMuPDF for PDF compression
from PIL import Image
import os

# -------- PDF Compression --------
def compress_pdf(input_path, output_path, dpi=100):
    doc = fitz.open(input_path)
    new_doc = fitz.open()
    for page in doc:
        pix = page.get_pixmap(dpi=dpi)  # lower DPI for compression
        img = Image.frombytes("RGB", [pix.width, pix.height], pix.samples)
        img.save("temp.jpg", quality=50)  # save as compressed image
        new_page = new_doc.new_page(width=pix.width, height=pix.height)
        new_page.insert_image(new_page.rect, filename="temp.jpg")
    new_doc.save(output_path)
    os.remove("temp.jpg")
    print(f"Compressed PDF saved as {output_path}")

# -------- Image Compression --------
def compress_image(input_path, output_path, quality=30):
    img = Image.open(input_path)
    img.save(output_path, optimize=True, quality=quality)
    print(f"Compressed image saved as {output_path}")

# -------- Example Usage --------
compress_pdf("input.pdf", "compressed.pdf")
compress_image("input.jpg", "compressed.jpg")