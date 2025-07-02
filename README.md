# FaceNet Proof-of-Concept: Facial Recognition Authentication

This project is a proof-of-concept demonstrating a facial recognition authentication system using `facenet-pytorch`. The entire workflow, from registering a user's face to verifying their identity, is contained within a single Jupyter Notebook (`main.ipynb`).

---

## How It Works

The system authenticates a user by converting their facial features into a unique numerical "fingerprint" (an embedding) and comparing it against a database of known users.

The process is divided into four key stages within the notebook:

### 1. Face Detection and Alignment
* **Action**: The notebook first reads images from a local folder designated for registration.
* **Technology**: A **Multi-task Cascaded Convolutional Network (MTCNN)** is used to detect and crop the face from each image, ensuring consistency by aligning the facial features. These aligned faces are saved to an `aligned_faces/` directory.

### 2. Generating Facial Embeddings
* **Action**: Each aligned face is then passed through a deep learning model to be converted into a numerical vector.
* **Technology**: A pre-trained **InceptionResnetV1** model generates a 512-dimension "facial embedding" for each image. This vector is a unique mathematical representation of that face.

### 3. Storing the Embeddings
* **Action**: The generated embeddings for the user are collected and saved.
* **Technology**: The embeddings are stored in a simple `embeddings.json` file, which acts as the "database" of known facial fingerprints.

### 4. The Sign-In Verification
* **Action**: A new image is provided for sign-in. The notebook generates an embedding for this new image and compares it to the stored embeddings.
* **Technology**: The **Euclidean distance** is calculated between the new embedding and the stored ones. If this distance is below a set threshold (`0.5`), the identity is verified.

---

## Setup and Usage

Follow these steps to run this proof-of-concept on your own machine.

### 1. Prerequisites
Make sure you have Python installed. Then, install the required libraries:
```bash
pip install torch torchvision facenet-pytorch python-dotenv Pillow
```

### 2. Folder and File Setup
1.  **Create Folders**: In the same directory as your `main.ipynb` notebook, create two folders:
    * `registration_photos/`
    * `signin_photos/`
2.  **Add Photos**:
    * Place several clear, well-lit photos of the person you want to register into the `registration_photos/` folder.
    * Place a single photo you want to use for sign-in into the `signin_photos/` folder.
3.  **Create `.env` File**: Create a file named `.env` in the same directory and add the following lines, replacing the example image name with your actual sign-in image name:
    ```
    register_path=./registration_photos
    save_path=./aligned_faces
    signin_image=./signin_photos/your_signin_image.jpg
    ```

### 3. Running the Notebook
Open `main.ipynb` and run the cells in order:

1.  **Registration**: Run all the cells under the "Face Detection and Alignment" and "Generating Facial Embeddings" sections. This will process your registration photos and create the `embeddings.json` file.
2.  **Sign-in**: Run the cells in the "Sign-In Verification" section. The final cell will print a success or failure message based on the comparison.

---

## Proof of Concept & Limitations

This project is a **proof-of-concept** and is not a production-ready application. Key limitations include:

* **Hardcoded User**: The system is designed for a single, hardcoded user (`"andrew"`).
* **Manual Workflow**: The registration and sign-in processes require manually running separate cells in a notebook.
* **Simple Storage**: Using a single JSON file as a database is not scalable for multiple users.
* **No User Interface**: The entire process is managed within a code environment with no front-end for user interaction.

---

## Future Potential

This proof-of-concept can be expanded into a full-fledged application. Future improvements could include:

* **Full Web Application**: Building a web interface with Flask or Django to allow users to register and sign in dynamically.
* **Database Integration**: Replacing `embeddings.json` with a scalable database like PostgreSQL or MongoDB to manage multiple users.
* **Live Webcam Support**: Integrating `cv2` to perform real-time facial recognition using a live webcam feed.
* **Dynamic User Management**: Creating a complete system where users can register, update their photos, and delete their accounts.

---

## Technologies Used

* **PyTorch**
* **facenet-pytorch**
* **Pillow (PIL)**
* **NumPy**
