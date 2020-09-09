# CAPTCHA-prediction

# Summary
This project consists of 2 tasks: 
  1. Create a convolutional neural network (CNN) to predict the letters in normal (without noise) 2D images of shape (30,28) (dataset 1)
  2. Use the CNN created above to predict CAPTCHAs (dataset 2)
 
# Approach
 1. An 8-layer CNN is created with the Keras library; on the given data, it obtains ~ 92% accuracy.
 2. The main challenges stem from the images of the CAPTCHA dataset. As opposed to the dataset of task 1, here we have:
    - images of 5 consecutive, often overlapping letters
    - a lot of noise present in the images
    
    Firstly, to help the CNN deal with the noisiness of the task 2 images, the task 1 images were added random noise (sklearn) and fed to the CNN for retraining.
    
    Secondly, although the CNN was trained with noisy images, its performance is better with denoised images. Hence, the images of task 2 were denoised with the median filter,     which is the most appropriate for 'salt and pepper' noise.
 
    Since the CAPTCHA images contain 5 letters, the denoising step was followed by the splitting of each image into 5 smaller images representing the letters. This was done by     creating bounding boxes around the letters, using the CV2 library. 
    There were a few challenges:
      - The denoising process did not completely remove all noise, which is why bounding boxes would sometimes be wrongly formed around noisy image sections which did not               represent any letter. Such new images were removed.
      - Since some letters overlapped in the original image, bounding boxes would occasionally contain 2 letters. In such cases, the image would be split at the letter                 overlap point, creating two separate images.
    
    Note that ~ 86% of task 2 images were split into 5 different letter images. The remaining images were wrongly split into a different number of letters.
    
    Once the new dataset was formed, it was fed to the CNN for prediction. Each series of 5 letters was inputted to the CNN 5 times, to allow for a best-of-5 results               evaluation. Finally, the predictions are saved to a .csv file for comparison with the true labels.
