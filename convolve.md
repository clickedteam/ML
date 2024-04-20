import numpy as np
from scipy.signal import convolve2d
from PIL import Image

# Load an image
img = Image.open('path_to_your_image.jpg').convert('L')  # Convert to grayscale for simplicity
img_array = np.array(img)

# Define the sharpening filter
sharpening_filter = np.array([[0, -1, 0],
                              [-1, 5, -1],
                              [0, -1, 0]])

# Apply the filter using convolution
sharpened_image = convolve2d(img_array, sharpening_filter, mode='same', boundary='fill', fillvalue=0)

# Convert the result to an image and save or display it
sharpened_image = Image.fromarray(np.clip(sharpened_image, 0, 255).astype('uint8'))  # Ensure pixel values are valid
sharpened_image.show()
