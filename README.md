# Sturdy-Octo-Disco-Adding-Sunglasses-for-a-Cool-New-Look

Sturdy Octo Disco is a fun project that adds sunglasses to photos using image processing.

Welcome to Sturdy Octo Disco, a fun and creative project designed to overlay sunglasses on individual passport photos! This repository demonstrates how to use image processing techniques to create a playful transformation, making ordinary photos look extraordinary. Whether you're a beginner exploring computer vision or just looking for a quirky project to try, this is for you!

## Features:
- Detects the face in an image.
- Places a stylish sunglass overlay perfectly on the face.
- Works seamlessly with individual passport-size photos.
- Customizable for different sunglasses styles or photo types.

## Technologies Used:
- Python
- OpenCV for image processing
- Numpy for array manipulations

## How to Use:
1. Clone this repository.
2. Add your passport-sized photo to the `images` folder.
3. Run the script to see your "cool" transformation!

## Applications:
- Learning basic image processing techniques.
- Adding flair to your photos for fun.
- Practicing computer vision workflows.

## Program and Output:
```
import cv2
import numpy as np
import matplotlib.pyplot as plt

faceImage = cv2.imread('M.5906 shriyha.JPG')
plt.imshow(faceImage[:,:,::-1]); plt.title("Face")
print("Face shape:", faceImage.shape)
```
<img width="344" height="435" alt="image" src="https://github.com/user-attachments/assets/94bc7a18-2ad6-43ec-9db4-8ccabe256d7f" />

```
glassJPG = cv2.imread('istockphoto-1081398784-612x612.jpg')
plt.imshow(glassJPG[:,:,::-1]); plt.title("glassJPG")
print("Glass shape:", glassJPG.shape)
```

<img width="555" height="396" alt="image" src="https://github.com/user-attachments/assets/baac045b-c473-4003-b708-787261d755d3" />



```
glassBGR = glassJPG[:,:,0:3]
glassGray = cv2.cvtColor(glassBGR, cv2.COLOR_BGR2GRAY)
_, glassMask1 = cv2.threshold(glassGray, 240, 255, cv2.THRESH_BINARY_INV)  # detect non-white

plt.figure(figsize=[15,15])
#Show sunglasses color channels
plt.subplot(121)
plt.imshow(glassBGR[:,:,::-1])  # BGR → RGB
plt.title('Sunglass Color channels')
```


<img width="587" height="418" alt="image" src="https://github.com/user-attachments/assets/d246cfcf-1fd7-4285-9438-97d7e3feeb6b" />

```
#Show generated mask
plt.subplot(122)
plt.imshow(glassMask1, cmap='gray')
plt.title('Sunglass Mask (generated)')
```


<img width="290" height="216" alt="image" src="https://github.com/user-attachments/assets/bbc2d1b5-764a-4fd2-bf66-e158b1708efc" />



```
import cv2
import numpy as np
import matplotlib.pyplot as plt

#Load images
faceImage = cv2.imread('M.5906 shriyha.JPG')
glassJPG = cv2.imread('istockphoto-1081398784-612x612.jpg')

#Check if images loaded correctly
if faceImage is None or glassJPG is None:
    print("Error: Check your file paths!")
else:
    face_h, face_w, _ = faceImage.shape

    #Resize glasses to ~50% of face width
    new_w = int(face_w * 0.5)
    new_h = int(new_w * glassJPG.shape[0] / glassJPG.shape[1])
    glass_resized = cv2.resize(glassJPG, (new_w, new_h))

    #Create mask
    glass_gray = cv2.cvtColor(glass_resized, cv2.COLOR_BGR2GRAY)
    _, mask = cv2.threshold(glass_gray, 240, 255, cv2.THRESH_BINARY_INV)
    mask_inv = cv2.bitwise_not(mask)

    # Adjusted position to place glasses on eyes
    x = int(face_w * 0.25)   # x offset (centered)
    y = int(face_h * 0.25)   # y offset (move up from nose to eyes)

    #ROI on face
    roi = faceImage[y:y+new_h, x:x+new_w]

    if roi.shape[0] > 0 and roi.shape[1] > 0:
        bg = cv2.bitwise_and(roi, roi, mask=mask_inv)
        fg = cv2.bitwise_and(glass_resized, glass_resized, mask=mask)
        combined = cv2.add(bg, fg)
        faceImage[y:y+new_h, x:x+new_w] = combined

    #Show result
    plt.figure(figsize=[10,10])
    plt.imshow(cv2.cvtColor(faceImage, cv2.COLOR_BGR2RGB))
    plt.title("Face with Sunglasses")
    plt.axis("off")
    plt.show()
```

<img width="620" height="812" alt="image" src="https://github.com/user-attachments/assets/809fa340-282f-4d89-a38f-0fcb7f42e05d" />

