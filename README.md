# Object detection-and-Depth-Estimation

1 Introduction 

The goal of this project is to use 2D object detections from images to estimate the 3D depth of objects, specifically cars, using a subset of the KITTI dataset. The object detection is performed using YOLOv8x, and the depth estimation leverages the intrinsic matrix of the camera setup provided by the KITTI dataset. This report outlines the steps taken, the mathematical approach for depth estimation, the results obtained, and the analysis of discrepancies between estimated distances and ground truth values.

2 Methodology

The algorithm begins by importing necessary libraries such as os for file system interactions, numpy for numerical operations, cv2 for computer vision tasks, and matplotlib.pyplot for image visualization. It then initializes a pre-trained YOLOv8 model using the Ultralytics library for object detection. Directories are set up to store input images, calibration matrices, ground truth labels, and annotated output images. Key constants are defined, including the class ID for ”car” in the COCO dataset. Functions are created to load intrinsic camera matrices, ground truth labels, and calculate Intersection over Union (IoU) for evaluation. The algorithm iterates over image files, loading each image, its corresponding intrinsic matrix, and ground truth labels. Object detection is performed on the images, with detected cars analyzed to estimate distances from the camera and compare with ground truth. Annotated images are then generated, displaying bounding boxes, midpoints, annotations, and distance estimates, which are saved to the output directory.

2.1 Geometrical/mathematical reasoning of distance calculation

To calculate the distance to a detected car, we employ the intrinsic camera matrix and geometric principles. Initially, the coordinates of the detected bounding box representing the car are extracted from the image. Subsequently, the midpoint of the lower bound of this bounding box is computed, serving as the focal point for distance estimation. Utilizing the intrinsic matrix, a 2D homogeneous coordinate vector is constructed to represent this midpoint. By applying the inverse of the intrinsic matrix to this vector, a direction vector from the camera to the midpoint in camera coordinates is derived. This directional vector is then intersected with the ground plane, typically defined at a known height above the ground level (e.g., -1.65 meters), producing a 3D point in space. Finally, the Euclidean distance from the camera to this intersection point on the ground plane is calculated, representing the estimated distance to the detected car. This comprehensive process effectively utilizes the intrinsic camera matrix and geometric reasoning to accurately estimate distances to detected objects in the image.

2.2 Matching Detections with Ground Truth: To match detections with ground truth objects, we calculate the Intersection over Union (IoU) between each detected bounding box and the ground truth bounding boxes. The IoU measures the overlap between two bounding boxes, providing a metric for their similarity. We select the ground truth box with the highest IoU for each detected car, considering it as the matched ground truth object. If the IoU is below a certain threshold, or if no ground truth box is available, we consider the detection as a false positive.



![Screenshot 2024-07-16 233617](https://github.com/user-attachments/assets/257f0a1b-bb3f-4943-a952-a3560abd628c)


![Screenshot 2024-07-16 233629](https://github.com/user-attachments/assets/e3113d29-f242-4c26-95d2-3c829f0e1fe4)


![Screenshot 2024-07-16 233646](https://github.com/user-attachments/assets/886b1ef1-d108-4816-b353-858f0c6ae6b9)


![Screenshot 2024-07-16 233709](https://github.com/user-attachments/assets/ee9cc3fa-d227-43be-9329-b621f9027324)

