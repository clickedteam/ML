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





import numpy as np

# Function to apply softmax
def softmax(x):
    e_x = np.exp(x - np.max(x))
    return e_x / e_x.sum(axis=-1, keepdims=True)

# Input embeddings: 5-dimensional vectors for each token
embeddings = np.array([
    [0.2, 0.1, 0.9, 0.3, 0.5],  # Data
    [0.5, 0.6, 0.1, 0.8, 0.2],  # is
    [0.7, 0.2, 0.4, 0.1, 0.9]   # power
])

# Weight matrices for transforming to Query (Q), Key (K), Value (V)
W_Q = np.array([
    [0.1, 0.2, 0.1, 0.3, 0.1],
    [0.2, 0.1, 0.3, 0.1, 0.2],
    [0.3, 0.2, 0.1, 0.1, 0.3],
    [0.1, 0.3, 0.2, 0.2, 0.1],
    [0.2, 0.1, 0.2, 0.3, 0.2]
])
W_K = W_Q  # For simplicity, using the same matrix for K
W_V = W_Q  # For simplicity, using the same matrix for V

# Transform embeddings to Query (Q), Key (K), Value (V) vectors
Q = embeddings.dot(W_Q.T)
K = embeddings.dot(W_K.T)
V = embeddings.dot(W_V.T)

# Calculate attention scores (Q dot K transpose)
attention_scores = Q.dot(K.T)

# Apply softmax to attention scores to get attention probabilities
attention_probs = softmax(attention_scores)

# Calculate output vectors as weighted sum of V vectors
output_vectors = attention_probs.dot(V)

print("Query Vectors (Q):\n", Q)
print("Key Vectors (K):\n", K)
print("Value Vectors (V):\n", V)
print("Attention Scores:\n", attention_scores)
print("Softmax Normalized Attention Probabilities:\n", attention_probs)
print("Output Vectors (Contextual Embeddings):\n", output_vectors)
