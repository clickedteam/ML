import torchvision.transforms as T
from PIL import Image
import torch
from torchvision.models.detection import fasterrcnn_resnet50_fpn

# Load a pre-trained Faster R-CNN model
model = fasterrcnn_resnet50_fpn(pretrained=True)
model.eval()

# Function to perform inference
def detect_objects(image_path, model):
    image = Image.open(image_path)
    transform = T.Compose([T.ToTensor()])  # Define the transformation
    image = transform(image)
    with torch.no_grad():
        predictions = model([image])  # Returns predictions

    return predictions

# Path to your image
image_path = 'path_to_your_image.jpg'

# Perform object detection
predictions = detect_objects(image_path, model)

# Print results
print(predictions)
