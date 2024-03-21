The project addresses the challenge of enabling an mobile robot to perceive and understand its environment to autonomously navigate through the shortest path in the maze.

![Screenshot 2024-03-21 173052](https://github.com/AabidPatel/monocular_perception/assets/73630123/d76ddb78-e35c-40ee-8d68-68859f9a6f47)

# Methodolgy

## Exploration

### Creating a Database
The robot first creates a database storing each frame taken by the camera while the robot explores the environment manually. With these saved frames, each frames associated with the odometry values are stored and appended these to a list.

### VLAD implementation

Using the ORB feature detection we create an ORB object and using the  detectAndcompute function we extract the descriptors and keypoints of every image in the dataset as part of the first step of the procedure for implementing VLAD. We implement VLAD in another file and import its function in the player.py file. Using these descriptors , we perform K-means where we build 8 clusters based on all the dataset image descriptors. The algorithm tries to partition the dataset into k clusters based on similarity. We use a seed value of 42 (chosen arbitrarily) for reproducing the same results. Using the K-means estimator from the previous step , we compute the visual vocabulary for each image and assign it to one of the clusters based on the similarity between the respective image and the clusters.  This is done by finding the Euclidean distance between each descriptor of the cluster centers and then assigns each descriptor to the nearest center using argmin and stores it as a label. For every image, we compute a vlad vector which is the aggregated difference between their descriptors and their cluster centers and append it to a list for each of the images in the dataset. This how we compute VLAD for the dataset images.


## Navigation

### Extracting Target Images

After entering the navigation phase, the robot first extracts the target images and then performs the VLAD technique similar to the procedure it followed for the dataset images.

### VLAD implementation:

We extract the image descriptors using ORB and compute the visual vocabulary, vlad vector for each of the target images with the same cluster center found for the dataset images. This is done so that we understand which cluster does each of the target image belong to based on the nearest distance. We then find the dot product of the entire database vlad vector with each target vlad vector and return the argmax from the matrix which is considered as the similarity matrix. We store the 4 most  similar images from the database and return the image with maximum similarity. Once we extract the most similar image to the target location from the database images, we navigate the robot iterating through the saved keystrokes associated with each of the frames until it reaches the indexed database image which matches with the target location. This is how we implement the actuation of the robot from the starting position to the exact goal position.



