# Depth-Measurement-using-Stereo-Camera

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
</head>
<body>
    <h1>Depth Camera Project</h1>
    <p>This project is focused on developing a portable and cost-effective depth measurement system using a USB webcam, applying computer vision techniques, object detection, and feature matching. The design utilizes the Cambridge EDC Inclusive Design Methodology, with the primary goal of achieving accurate depth measurements by leveraging a single camera setup.</p>

  <h2>Key Features</h2>
    <ul>
        <li><strong>Camera Calibration:</strong> Custom functions were used with chessboard images to calibrate the camera and obtain essential parameters for depth calculations. This involves calculating intrinsic parameters (camera matrix and distortion coefficients) to ensure accurate depth estimation.</li>
        <li><strong>Object Detection:</strong> The YOLOv8 model, custom-trained with a dataset from Roboflow, was used to detect objects and output bounding boxes. This step is critical for identifying the regions of interest within the captured images where depth will be measured.</li>
        <li><strong>Feature Matching & Depth Calculation:</strong> Depth was calculated using triangulation based on the matrices obtained from the camera calibration. The process involves:
            <ul>
                <li><strong>Image Capture:</strong> The camera captures images from two distinct positions, achieved by rotating the camera using a stepper motor.</li>
                <li><strong>Feature Matching:</strong> Key features within the bounding boxes are matched between the two images. This is done using algorithms like SIFT or ORB.</li>
                <li><strong>Triangulation:</strong> Using the matched features and the known rotation angle between the two camera positions, the depth of the objects is computed through triangulation. The rotational matrices and translation vectors are applied to compute the 3D coordinates of the features.</li>
            </ul>
        </li>
        <li><strong>Rotational Mechanism:</strong> A stepper motor mechanism, controlled by an ATmega microcontroller, rotates the camera to capture images from different angles. This controlled movement is crucial for obtaining the positional data needed for accurate depth calculation.</li>
    </ul>

  <h2>Code Structure</h2>
    <h3>Calibration Module (<code>calibrate_camera.py</code>)</h3>
    <ul>
        <li><strong>Input:</strong> Chessboard images for calibration.</li>
        <li><strong>Output:</strong> Camera matrix and distortion coefficients.</li>
        <li><strong>Functions:</strong>
            <ul>
                <li><code>calibrate_camera(images):</code> Performs intrinsic camera calibration.</li>
                <li><code>stereo_calibrate(mtx, dist, c1_images, c2_images):</code> Computes extrinsic parameters (rotation and translation matrices).</li>
            </ul>
        </li>
    </ul>

  <h3>Object Detection & Feature Matching Module (<code>object_detection.py</code>)</h3>
    <ul>
        <li><strong>Input:</strong> Images captured from the camera.</li>
        <li><strong>Output:</strong> Detected objects and matched features.</li>
        <li><strong>Functions:</strong>
            <ul>
                <li><code>detect_objects(image):</code> Uses YOLOv8 to detect objects in the image.</li>
                <li><code>match_features(image1, image2):</code> Matches features between two images using feature matching algorithms.</li>
            </ul>
        </li>
    </ul>

  <h3>Depth Calculation Module (<code>depth_calculation.py</code>)</h3>
    <ul>
        <li><strong>Input:</strong> Matched features and calibration data.</li>
        <li><strong>Output:</strong> Depth values for detected objects.</li>
        <li><strong>Functions:</strong>
            <ul>
                <li><code>calculate_depth(matched_features, rotation_matrix, translation_vector):</code> Uses triangulation to calculate the depth of objects.</li>
            </ul>
        </li>
    </ul>

  <h3>Motor Control Module (<code>motor_control.py</code>)</h3>
    <ul>
        <li><strong>Input:</strong> Commands for rotating the camera.</li>
        <li><strong>Output:</strong> Controlled camera movement.</li>
        <li><strong>Functions:</strong>
            <ul>
                <li><code>rotate_camera(angle):</code> Rotates the camera to the specified angle using the stepper motor.</li>
            </ul>
        </li>
    </ul>

  <h2>Depth Calculation Process</h2>
    <ol>
        <li><strong>Capture Images:</strong> The camera is rotated to two different positions using the motor control module, and images are captured at each position.</li>
        <li><strong>Detect Objects:</strong> The YOLOv8 model identifies objects in the images, and bounding boxes are generated around them.</li>
        <li><strong>Match Features:</strong> Features within these bounding boxes are matched between the two images using the feature matching module.</li>
        <li><strong>Calculate Depth:</strong> Using the matched features, the rotation matrix, and the translation vector, the depth of each object is calculated through triangulation. The triangulation formula typically involves solving the equation:
            <br><code>Z = (B * f) / d</code>
            <ul>
                <li><strong>Z:</strong> Depth.</li>
                <li><strong>B:</strong> Baseline (distance between the two camera positions).</li>
                <li><strong>f:</strong> Focal length of the camera.</li>
                <li><strong>d:</strong> Disparity (difference in the x-coordinates of the matched features).</li>
            </ul>
        </li>
        <li><strong>Output Depth Values:</strong> The calculated depth values are then outputted and can be used for further analysis or applications.</li>
    </ol>
</body>
</html>
