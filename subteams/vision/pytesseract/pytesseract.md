---
permalink: /vision/pytesseract/
---

# Pytesseract

[Back to Vision Docs](/docs/vision/)

[Pytesseract](https://pypi.org/project/pytesseract/) is a wrapper for Google's [Tesseract Optical Character Recognition Engine](https://github.com/tesseract-ocr/tesseract). Pytesseract allows for the detection of characters in an image. It has support for numerous languages and character sets.

Pytesseract is a wrapper, meaning it doesn't contain much functionality on its own. Essentially, it provides a way to call tesseract's command line tool from Python. Since there is a lack of sufficient documentation on Pytesseract, it is recommended that you look at [tesseract's documentation](https://tesseract-ocr.github.io/) directly and then search for the equivalent Pytesseract function.

The most likely function that you'll be using is `image_to_string()` or `image_to_data()`. Both of these allow you to extract text and other data from an image.

To learn more, check out tesseract's documentation.