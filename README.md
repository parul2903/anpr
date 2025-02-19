# **Automatic Number Plate Recognition (ANPR) Using Google Cloud Vision API for OCR**

## **Overview**
Automatic Number Plate Recognition (ANPR) is a technology that uses **Optical Character Recognition (OCR)** to extract vehicle registration numbers from images or video streams. This project utilizes the **Google Cloud Vision API** to recognize and process license plate numbers efficiently.

## **Setup and Usage**
### **Clone the Repository**
```bash
git clone https://github.com/parul2903/anpr
cd anpr
```

### **Create a Virtual Environment**
Create a conda or virtual environment and activate it:
```bash
conda create -n anpr_env python=3.7 -y
conda activate anpr_env
```
**OR**
```bash
conda create -p ./env python=3.7 -y
conda activate ./env/
```

### **Install Requirements**
```bash
pip install -r requirements.txt
```

### **Run the Server**
```bash
python rest-server.py
```

### **Testing the API (Using Postman)**
- Open **Postman** and send a **POST** request to the API endpoint.
- Upload an image containing a vehicle number plate.
- The API will return the extracted number plate text.

---

## **Google Cloud Vision API Setup**

1. **Enable Cloud Vision API**: Go to the [Google Cloud Console](https://console.cloud.google.com/).
2. **Create a Service Account**: Generate a JSON key and store it securely.
3. **Set Environment Variable**: Point to the JSON key for authentication.
   ```bash
   export GOOGLE_APPLICATION_CREDENTIALS="/path/to/your/service-account-key.json"
   ```
4. **Install Google Cloud Client Library**:
   ```bash
   pip install google-cloud-vision
   ```

---

## **ANPR Implementation Code**

### **rest-server.py** (Main API Server)
```python
from flask import Flask, request, jsonify
from google.cloud import vision
import io
import os

# Initialize Flask app
app = Flask(__name__)

# Set Google Cloud credentials
os.environ['GOOGLE_APPLICATION_CREDENTIALS'] = 'path/to/your/service-account-key.json'

# Initialize Google Cloud Vision client
client = vision.ImageAnnotatorClient()

@app.route('/anpr', methods=['POST'])
def recognize_plate():
    if 'file' not in request.files:
        return jsonify({"error": "No file uploaded"}), 400
    
    file = request.files['file']
    image = file.read()
    image = vision.Image(content=image)
    
    # Perform OCR
    response = client.text_detection(image=image)
    texts = response.text_annotations
    
    if not texts:
        return jsonify({"error": "No text found in image"})
    
    plate_number = texts[0].description.strip()
    return jsonify({"plate_number": plate_number})

if __name__ == '__main__':
    app.run(debug=True)
```

---

## **Applications of This Project**

1. **Traffic Law Enforcement**
   - Detects vehicles violating speed limits and traffic rules.
   - Helps identify stolen or unregistered vehicles.

2. **Automated Parking Management**
   - Enables contactless parking systems.
   - Automatically logs vehicle entries and exits.

3. **Toll Collection Systems**
   - Enhances electronic toll collection (ETC) systems.
   - Reduces the need for manual intervention at toll plazas.

4. **Smart City Surveillance**
   - Monitors road congestion and optimizes traffic flow.
   - Supports city-wide security and surveillance initiatives.

5. **Fleet Management**
   - Helps logistics companies track their fleet.
   - Ensures proper monitoring of vehicle movement and efficiency.

---

## **Future Enhancements**
- Improve OCR accuracy using deep learning models like **YOLO + OCR**.
- Integrate real-time ANPR with CCTV feeds.
- Add a database to store number plate records for future reference.

---

## **License**
This project is licensed under the **MIT License**.

---

### **Contributors**
- **[Your Name]** – Developer
- **[Contributor Name]** – Maintainer

For any questions or contributions, feel free to open an issue or a pull request in the repository!

---

Happy Coding! 🚀
