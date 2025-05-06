# Customer Churn Prediction API

## Overview
This project provides a robust REST API for predicting customer churn using multiple machine learning models. Built with FastAPI, it offers high-performance predictions using both Random Forest and XGBoost models, with secure API key authentication and comprehensive error handling.

## Features
- **Multiple ML Models**: Supports both Random Forest and XGBoost predictions
- **Secure Authentication**: API key-based access control
- **Fast Processing**: Asynchronous request handling with FastAPI
- **Cross-Origin Support**: Built-in CORS middleware for frontend integration
- **Error Handling**: Comprehensive error management and validation
- **Docker Support**: Containerized deployment ready

## Project Structure
``` bash
project/
│   .env.example           # Example environment variables template
│   .gitignore             # Git ignore rules
│   Dockerfile             # Docker configuration
│   main.py               # FastAPI application entry point
│   README.md             # Project documentation
│   requirements.txt      # Python dependencies
├───models/               # Trained model files
│       forest_tuned.pkl  # Random Forest model
│       preprocessor.pkl  # Data preprocessor
│       xgb-tuned.pkl    # XGBoost model
├───notebooks/            # Jupyter notebooks
│       notebook.ipynb    # Model development notebook
└───utils/               # Utility modules
        config.py        # Configuration settings
        CustomerData.py  # Data models
        inference.py     # Prediction logic
        __init__.py     # Package initialization
```

## Prerequisites
- Python 3.9+
- Docker (for containerized deployment)
- Required Python packages (see `requirements.txt`)

## Installation

### Local Development
1. Clone the repository:
```bash
git clone https://github.com/yourusername/Churn-Modelling.git
cd Churn-Modelling
```

2. Create and activate a virtual environment:
```bash
python -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate
```

3. Install dependencies:
```bash
pip install -r requirements.txt
```

4. Set up environment variables:
```bash
cp .env.example .env
# Edit .env file with your configuration
```

### Docker Deployment
1. Build the Docker image:
```bash
docker build -t churn-prediction-api .
```

2. Run the container:
```bash
docker run -d -p 8080:8080 churn-prediction-api
```

## API Documentation

### Authentication
All endpoints require API key authentication via the `X-API-Key` header:
```bash
X-API-Key: your-api-key-here
```

### Endpoints

#### 1. Health Check
```
GET /
```
Returns API status and version information.

#### 2. Random Forest Prediction
```
POST /predict/forest
```
Predicts customer churn using Random Forest model.

#### 3. XGBoost Prediction
```
POST /predict/xgboost
```
Predicts customer churn using XGBoost model.

### Request Format
```json
{
    "CreditScore": 0,
    "Geography": "France",
    "Gender": "Male",
    "Age": 18,
    "Tenure": 10,
    "Balance": 0,
    "NumOfProducts": 1,
    "HasCrCard": 0,
    "IsActiveMember": 0,
    "EstimatedSalary": 0
}
```

### Example Usage

Using Python requests:
```python
import requests

url = "http://localhost:8080/predict/forest"
headers = {
    "X-API-Key": "your-api-key",
    "Content-Type": "application/json"
}
data = {
    "CreditScore": 650,
    "Geography": "France",
    "Gender": "Male",
    "Age": 35,
    "Tenure": 5,
    "Balance": 75000,
    "NumOfProducts": 2,
    "HasCrCard": 1,
    "IsActiveMember": 1,
    "EstimatedSalary": 50000
}

response = requests.post(url, json=data, headers=headers)
print(response.json())
```

Using curl:
```bash
curl -X 'POST' \
  'http://localhost:8080/predict/forest' \
  -H 'accept: application/json' \
  -H 'X-API-Key: your-api-key' \
  -H 'Content-Type: application/json' \
  -d '{
    "CreditScore": 650,
    "Geography": "France",
    "Gender": "Male",
    "Age": 35,
    "Tenure": 5,
    "Balance": 75000,
    "NumOfProducts": 2,
    "HasCrCard": 1,
    "IsActiveMember": 1,
    "EstimatedSalary": 50000
  }'
```

## Environment Variables
Create a `.env` file based on `.env.example` with the following variables:
- `APP_NAME`: Application name
- `VERSION`: API version
- `SECRET_KEY_TOKEN`: API authentication key

## Model Information
The project includes pre-trained models:
- `forest_tuned.pkl`: Optimized Random Forest model
- `xgb-tuned.pkl`: Tuned XGBoost model
- `preprocessor.pkl`: Data preprocessing pipeline

The models were trained on the `churn-data.csv` dataset, and the training process is documented in `notebooks/notebook.ipynb`.

## Error Handling
The API implements comprehensive error handling:
- 400: Bad Request (Invalid input data)
- 403: Forbidden (Invalid API key)
- 500: Internal Server Error (Prediction errors)

## Contributing
1. Fork the repository
2. Create a feature branch: `git checkout -b feature-name`
3. Commit changes: `git commit -am 'Add feature'`
4. Push to the branch: `git push origin feature-name`
5. Submit a Pull Request

## License
This project is licensed under the MIT License - see the LICENSE file for details.

## Support
For support, please open an issue in the GitHub repository or contact the maintainers.
