# NVIDIA PeopleNet Guide 🧑‍🤝‍🧑

## What is PeopleNet?

**PeopleNet** is a computer vision AI model developed by NVIDIA that detects persons, bags, and faces in images. It's a commercial-ready, pre-trained model specifically designed for **real-time object detection**.

## Detection Capabilities

PeopleNet detects one or more physical objects from three categories within an image and returns a bounding box around each object, as well as a category label for each object:

1. **👤 People/Persons** - Primary focus
2. **🎒 Bags** - Secondary capability  
3. **😊 Faces** - Secondary capability

## Technical Architecture

### Core Technology
- **Base Architecture**: NVIDIA DetectNet_v2 detector with ResNet34 as feature extractor
- **Detection Method**: GridBox object detection using bounding-box regression on a uniform grid
- **Grid System**: Divides input image into a grid which predicts four normalized bounding-box parameters (xc, yc, w, h) and confidence value per output class

### Input Requirements
- **Image Format**: RGB color images
- **Resolution**: 960 x 544 pixels (standard)
- **Channel Ordering**: NCHW format (Batch, Channels, Height, Width)

## Performance Metrics

### Speed & Efficiency
- **Processing Speed**: 1000 requests per second
- **Efficiency**: 50% more efficient than comparable models
- **Precision Rate**: 91.82%
- **Recall Rate**: 88.60%
- **Overall Accuracy**: 82.17% (FP16) and 80.21% (INT8)

### Training Data
- **Dataset Size**: Over 7.6 million images
- **Object Count**: 71 million annotated objects
- **Test Dataset**: 90,000+ proprietary images across various environments

## Model Variants

### Standard PeopleNet
- Basic DetectNet_v2 + ResNet34 architecture
- Optimized for general people detection

### PeopleNet Transformer
- Based on Deformable DETR with attention modules
- Uses ResNet50 as feature extractor
- Optimized training and inference speed

### PeopleNet AMR (Autonomous Mobile Robot)
- Specialized for robotic applications
- Trained with data from robot perspective (2-5 feet height)
- Includes low-density scene detection

### PeopleNet Transformer v2.0
- Latest version using DINO object detector
- FAN-Small feature extractor
- Pre-trained on OpenImages dataset

## Limitations & Considerations

### Detection Limitations
- **Minimum Object Size**: Cannot detect objects smaller than 10x10 pixels
- **Occlusion**: May not detect objects with less than 20% visibility
- **Person Detection**: For people, head and shoulders must be visible OR >60% of person visible
- **Lighting**: Trained on RGB images in good lighting - poor performance in dark conditions
- **Camera Types**: Not optimized for fish-eye lenses or moving cameras

### Class-Specific Performance
- **Person Detection**: Highest accuracy (primary use case)
- **Bag & Face Detection**: Much lower accuracy than person detection
- **Geographic Bias**: Training data mostly consists of North American content

## Common Use Cases

### Smart Cities & Urban Planning
- Pedestrian monitoring and counting
- Traffic flow analysis
- Public space utilization

### Retail & Commercial
- Customer counting and analytics
- Store occupancy monitoring
- Queue management

### Security & Surveillance
- People detection in security cameras
- Perimeter monitoring
- Access control systems

### Autonomous Systems
- Pedestrian detection for self-driving cars
- Robot navigation and collision avoidance
- Drone surveillance applications

### Video Analytics
- Crowd analysis and density estimation
- Event monitoring
- Content moderation

## How to Get Started

### Requirements
1. **NGC Account**: Sign up at https://ngc.nvidia.com
2. **API Key**: Generate from NGC console
3. **NGC CLI**: For model download
4. **Hardware**: NVIDIA GPU recommended (but CPU inference supported)

### Download Command
```bash
ngc registry model download-version nvidia/tao/peoplenet:pruned_quantized_v2.3.4
```

### Supported Frameworks
- **TensorRT**: Optimized inference
- **ONNX Runtime**: Cross-platform deployment
- **DeepStream**: Video analytics pipeline
- **TAO Toolkit**: Custom training and fine-tuning

## PeopleNet vs Other Models

### vs YOLO
- **PeopleNet**: Specialized for people detection, higher accuracy for humans
- **YOLO**: General-purpose object detection, broader class support

### vs OpenCV HOG
- **PeopleNet**: Deep learning-based, much higher accuracy
- **HOG**: Traditional computer vision, faster but less accurate

### vs Custom Models
- **PeopleNet**: Pre-trained, commercial-ready, extensive testing
- **Custom**: Requires training data, time, and expertise

## Best Practices

### Image Quality
- Use good lighting conditions
- Ensure minimum 10x10 pixel object size
- Avoid heavily occluded scenes

### Performance Optimization
- Use TensorRT for inference acceleration
- Consider INT8 quantization for edge deployment
- Batch processing for multiple images

### Integration Tips
- Implement NMS (Non-Maximum Suppression) for post-processing
- Set appropriate confidence thresholds
- Consider tracking algorithms for video streams

## Ethical Considerations

### Privacy & Bias
- Model detects faces but doesn't infer demographic information
- Training data has geographic bias toward North American content
- Consider privacy implications in deployment

### Responsible Use
- Ensure compliance with local privacy laws
- Implement appropriate data governance
- Consider algorithmic bias in different environments

## Resources & Documentation

- **Official Documentation**: [NVIDIA NGC PeopleNet](https://catalog.ngc.nvidia.com/orgs/nvidia/teams/tao/models/peoplenet)
- **Developer Guide**: [NVIDIA TAO Toolkit](https://developer.nvidia.com/tao-toolkit)
- **DeepStream Integration**: [DeepStream SDK](https://developer.nvidia.com/deepstream-sdk)
- **Community Forum**: [NVIDIA Developer Forums](https://forums.developer.nvidia.com/)

---

*Last Updated: July 2025*  
*For technical support and questions, visit the NVIDIA Developer Forums*
