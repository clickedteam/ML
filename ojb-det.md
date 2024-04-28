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



#vizualize

import torchvision.transforms as T
from PIL import Image, ImageDraw, ImageFont
import torch
from torchvision.models.detection import fasterrcnn_resnet50_fpn

# Load a pre-trained Faster R-CNN model
model = fasterrcnn_resnet50_fpn(pretrained=True)
model.eval()

# Function to perform inference
def detect_objects(image, model):
    transform = T.Compose([T.ToTensor()])
    image = transform(image)
    with torch.no_grad():
        predictions = model([image])  # Returns predictions
    return predictions[0]

# Load and prepare the image
image_path = 'path_to_your_image.jpg'
image = Image.open(image_path)

# Detect objects
predictions = detect_objects(image, model)

# Draw bounding boxes and labels on the image
draw = ImageDraw.Draw(image)
for element in range(len(predictions['boxes'])):
    if predictions['scores'][element] > 0.8:  # threshold to filter weak detections
        box = predictions['boxes'][element].tolist()
        label = predictions['labels'][element].item()
        score = predictions['scores'][element].item()
        
        draw.rectangle(box, outline='red', width=3)
        draw.text((box[0], box[1]), f'{label} {score:.2f}', fill='red')

# Save or display the image
image.show()


# Perform object detection
predictions = detect_objects(image_path, model)

# Print results
print(predictions)
