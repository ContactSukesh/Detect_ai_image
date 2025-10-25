# AI Image Detective 🔍

A powerful web application built with PyTorch and Streamlit that detects whether images are AI-generated or real using advanced deep learning models.

## Features

### 🎯 Core Capabilities
- **Single Image Analysis**: Detailed detection with confidence scores and heatmap visualization
- **Batch Processing**: Analyze multiple images simultaneously
- **Ensemble Detection**: Combine 3 different AI models (ResNet-50, ResNet-34, MobileNetV2) for improved accuracy
- **Detection History**: Track all your detections with database persistence
- **Export Functionality**: Download detection reports in CSV or JSON format

### 🔬 Advanced Features
- **GradCAM Heatmaps**: Visual representation showing which image regions influenced the AI detection
- **Confidence Scoring**: Detailed probability metrics for predictions
- **Model Comparison**: See individual results from each model in ensemble mode
- **Historical Analytics**: View statistics and trends across all detections

## Installation

### Prerequisites
- Python 3.11 or higher
- PostgreSQL database (automatically configured in Replit)

### Dependencies
The following packages are automatically installed:
```
streamlit
torch
torchvision
numpy
opencv-python
psycopg2-binary
sqlalchemy
pillow
```

### Environment Variables
Required environment variables (automatically set in Replit):
- `DATABASE_URL`: PostgreSQL connection string
- `PGHOST`, `PGPORT`, `PGUSER`, `PGPASSWORD`, `PGDATABASE`: Database credentials

## Usage

### Starting the Application
```bash
streamlit run app.py --server.port 5000
```

The application will be available at `http://0.0.0.0:5000`

### Using the Application

#### 1. Single Image Mode
1. Select "Single Image" mode
2. Optionally enable "Use Ensemble Detection" for multi-model analysis
3. Upload an image (PNG, JPEG, or WebP)
4. Click "Analyze Image"
5. View results including:
   - Prediction (AI-Generated or Real)
   - Confidence score
   - Detection heatmap (showing influential regions)
   - Detailed analysis metrics

#### 2. Batch Processing Mode
1. Select "Batch Processing" mode
2. Upload multiple images at once
3. Click "Analyze All Images"
4. View summary statistics and individual results for each image

#### 3. History Mode
1. Select "History" mode
2. View all past detections with statistics
3. Export history to CSV or JSON format
4. Clear history if needed

## Technical Details

### Model Architecture

#### Single Model Detection
- **Base Architecture**: ResNet-50 with custom classification head
- **Input Resolution**: 224×224 pixels
- **Preprocessing**: ImageNet normalization
- **Output**: Binary classification (Real vs AI-Generated)

#### Ensemble Detection
- **Models Used**:
  - ResNet-50: Deep residual network with 50 layers
  - ResNet-34: Lighter residual network with 34 layers
  - MobileNetV2: Efficient mobile architecture
- **Ensemble Method**: Majority voting with confidence averaging
- **Improved Accuracy**: Multiple perspectives reduce false positives

### Heatmap Visualization
- **Technique**: GradCAM (Gradient-weighted Class Activation Mapping)
- **Purpose**: Shows which image regions contributed most to the detection
- **Visualization**: Red/yellow areas indicate high influence on AI detection

### Database Schema

```sql
CREATE TABLE detection_history (
    id SERIAL PRIMARY KEY,
    filename VARCHAR(255) NOT NULL,
    prediction INTEGER NOT NULL,
    confidence FLOAT NOT NULL,
    is_ensemble BOOLEAN DEFAULT FALSE,
    ensemble_details TEXT,
    timestamp TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    image_size VARCHAR(50),
    file_format VARCHAR(20)
);
```

## File Structure

```
.
├── app.py                  # Main Streamlit application
├── model.py               # AI detection models (Single & Ensemble)
├── database.py            # Database management and history tracking
├── utils.py               # Image preprocessing utilities
├── .streamlit/
│   └── config.toml        # Streamlit configuration
└── README.md             # This file
```

## Supported Image Formats

- JPEG (.jpg, .jpeg)
- PNG (.png)
- WebP (.webp)

## Configuration

### Streamlit Configuration (`.streamlit/config.toml`)
```toml
[server]
headless = true
address = "0.0.0.0"
port = 5000
```

## Export Formats

### CSV Export
Includes columns:
- ID, Filename, Prediction, Confidence (%), Method, Image Size, Format, Timestamp

### JSON Export
Structured JSON with:
- All detection metadata
- Ensemble model details (if applicable)
- ISO formatted timestamps

## Performance

- **Single Model Processing**: ~2.5 seconds per image
- **Ensemble Processing**: ~3.5 seconds per image
- **Batch Processing**: Processes images sequentially with progress tracking
- **GPU Support**: Automatically uses CUDA if available, falls back to CPU

## How It Works

1. **Image Upload**: User uploads image through web interface
2. **Preprocessing**: Image is resized to 224×224 and normalized
3. **Model Inference**: 
   - Single model: ResNet-50 analyzes the image
   - Ensemble: 3 models vote on the prediction
4. **Heatmap Generation**: GradCAM creates visual attention map (single model only)
5. **Result Display**: Prediction, confidence, and visualization shown to user
6. **Database Storage**: Detection saved to history for future reference

## Detection Accuracy

The model looks for common AI generation artifacts:
- Over-smoothness in textures
- Unusual color distributions
- Perfect symmetries
- Other learned patterns specific to AI-generated content

**Note**: This tool provides probabilistic predictions and should be used as a reference rather than definitive proof.

## Database Management

### View History
Access through "History" mode to see:
- Total detections
- AI-generated vs Real statistics
- Ensemble usage count
- Individual detection details

### Export Data
- Download CSV for spreadsheet analysis
- Download JSON for programmatic processing
- Includes up to 10,000 most recent detections

### Clear History
- One-click deletion of all detection records
- Irreversible action - use with caution

## Troubleshooting

### Model Loading Issues
- Ensure sufficient memory (ResNet-50 requires ~100MB)
- Check internet connection for downloading pretrained weights

### Database Connection Errors
- Verify DATABASE_URL environment variable is set
- Check PostgreSQL service is running

### Image Upload Failures
- Ensure image is in supported format (PNG, JPEG, WebP)
- Check file size is reasonable (<10MB recommended)
- Verify image dimensions are between 32×32 and 4000×4000 pixels

## API Reference

### Key Functions

#### `AIImageDetector`
- `predict(image_tensor)`: Returns prediction and confidence
- `generate_heatmap(image_tensor, prediction)`: Creates GradCAM visualization
- `overlay_heatmap(original_image, heatmap)`: Overlays heatmap on image

#### `EnsembleAIDetector`
- `predict(image_tensor)`: Returns ensemble prediction with individual results
- `load_ensemble()`: Initializes all three models

#### `DatabaseManager`
- `add_detection()`: Save detection to history
- `get_all_detections()`: Retrieve detection records
- `export_to_csv()`: Export history as CSV
- `export_to_json()`: Export history as JSON

## Contributing

This is a demonstration project showcasing AI image detection capabilities using PyTorch and Streamlit.

## License

This project uses pretrained models from torchvision which are subject to their respective licenses.

## Acknowledgments

- PyTorch for deep learning framework
- Streamlit for web interface
- torchvision for pretrained models
- OpenCV for image processing
- SQLAlchemy for database management

---

**Built with PyTorch & Streamlit** 🔬
