# Scene Recognition by Combining Local and Global Image Descriptors


**Approach** : We make use of global feature descriptors as well as local feature descriptors 
simultaneously to represent each image, to improve recognition accuracy. Our basic steps are as follows

**1. Feature Extraction** : From each image, we first extract DAISY features [1] corresponding to the keypoints detected from the image. We make use of the skimage library (scikit-image) [2] for feature extraction. Corresponding to each keypoint in the image, there would be a DAISY descriptor with dimensions controlled by the parameters step size (distance between descriptor sampling points) and radius (describing size of the scanning area). We also extract a standard HOG descriptor corresponding to the entire image at a different granularity (by making use of the parameters pixels_per_cell, cells_per_block and orientations), effectively allowing us to choose features at different scales.

**2. Encoding** : We make use of the local DAISY features corresponding to each keypoints, to perform the encoding. We use the standard "bag-of-visual-words" concept here. Concretely, we apply K-means algorithm to quantize DAISY features into 'K' clusters to form "visual words". Thus, K becomes the size of our vocabulary. Corresponding to each image, we construct a histogram with 'K' as the dimensionality, based on this vocabulary.


**3. Pooling** : We make use of a 2-level pooling scheme. From the DAISY features, we construct a histogram by representing frequency of each visual word in each image. We do this by taking each key point in the image and looking up the cluster_id corresponding to that DAISY descriptor and incrementing the count corresponding to that bin in our histogram. This procedure is effectively a "sum pooling". We do an L2 normalization of the resulting histogram to form a "DAISY histogram feature". For the 2nd level pooling, we take the HOG global descriptor corresponding to each image and do an L2 normalization followed by a concatenation with the corresponding DAISY histogram feature.


**4. Classification** : We make use of the standard SVM classifier with various kernels. We utilize the sckit-learn (sklearn) library [3] for the SVM implementations. We do cross-validation by randomly splitting the dataset into a training and testing set. We construct the "visual vocabulary" as well as training feature vectors from the training split. We report the overall accuracy, confusion-matrix as well as standard information-retrieval statistics such as Precision, Recall and F-measure.




#### REFERENCES ########

[1] Tola, Engin, Vincent Lepetit, and Pascal Fua. "A fast local 
descriptor for dense matching." Computer Vision and Pattern Recognition, 
2008. CVPR 2008. IEEE Conference on. IEEE, 2008.

[2] Van der Walt, Stefan, et al. "scikit-image: image processing in 
Python." PeerJ 2 (2014): e453.

[3] Pedregosa, Fabian, et al. "Scikit-learn: Machine learning in 
Python." Journal of Machine Learning Research 12.Oct (2011): 2825-2830.

## How to Run 
The Jupyter notebook containing a full implementation of our scene-recognition model is available in our repository.
Simply download and execute it from your Jupyter installation. Please make sure that scikit-learn and scikit-image libraries are installed. The fifteen scene categories dataset used by the model would be automatically downloaded when the code is executed for the first time. Our model is described in detail in the accompanying paper. 

In case if you find our code to be helpful, please consider citing our paper http://arxiv.org/abs/1702.06850.
Wilson, J., Arif, M. (2017). Scene Recognition by Combining Local and Global Image Descriptors. arXiv preprint arXiv:1702.06850.



