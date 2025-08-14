# AI Backend System with LangChain and Gemini Integration

## Problem Statement

Build a comprehensive AI backend system using a flat file structure that integrates Flask, LangChain, and Google's Gemini models. The system should provide intelligent text processing, analysis, and evaluation capabilities with A/B testing functionality, all implemented across 7 core files.

## Project Overview

You are provided with a Flask-based AI project that demonstrates integration with multiple Gemini models through LangChain. Your task is to implement the missing functionality to make the system fully operational according to the specifications.

## File Structure

The project uses a flat file structure with these specific files:

```
project_root/
├── .env                           # Environment variables (Gemini API key)
├── README.md                      # Project documentation
├── app.py                         # Main Flask application with API endpoints
├── config.py                      # Configuration settings and model setup
├── evaluation_and_ab_testing.py   # Evaluation pipeline and A/B testing logic
├── requirements.txt               # Python dependencies
└── unit_test.py                   # Unit tests for all functionality
```

## Core Requirements

### 1. Environment Setup (`.env`)
Configure the environment file with:
```
GEMINI_API_KEY=your_actual_gemini_api_key
FLASK_ENV=development
FLASK_DEBUG=True
```

### 2. Dependencies (`requirements.txt`)
Must include all necessary packages:
- Flask and Flask extensions
- LangChain and LangChain-Google-GenAI
- Google GenerativeAI
- Additional utilities for evaluation and testing

### 3. Configuration (`config.py`)
Implement configuration management:
- Load environment variables
- Define model configurations for all three Gemini models:
  - `gemini-1.5-pro` (complex reasoning)
  - `gemini-1.5-flash` (fast responses)  
  - `gemini-pro-vision` (image analysis)
- Set up LangChain model instances
- Configure application settings

### 4. Flask Application (`app.py`)
Implement a complete Flask API with the following endpoints:

#### Core API Endpoints

**Text Summarization:**
```
POST /api/summarize
Content-Type: application/json
{
  "text": "Long text to summarize...",
  "model": "gemini-1.5-pro"  // optional, defaults to gemini-1.5-pro
}

Response:
{
  "summary": "Condensed summary text",
  "model_used": "gemini-1.5-pro",
  "token_count": 150,
  "processing_time": 2.3
}
```

**Question Answering:**
```
POST /api/qa
Content-Type: application/json
{
  "question": "What is artificial intelligence?",
  "context": "Optional context text",
  "model": "gemini-1.5-flash"  // optional
}

Response:
{
  "answer": "AI is a field of computer science...",
  "model_used": "gemini-1.5-flash",
  "confidence_score": 0.95,
  "processing_time": 1.8
}
```

**Image Analysis:**
```
POST /api/analyze-image
Content-Type: application/json
{
  "image_data": "base64_encoded_image_string",
  "prompt": "Describe what you see in this image"
}

Response:
{
  "analysis": "The image shows...",
  "model_used": "gemini-pro-vision",
  "processing_time": 3.1
}
```

**Model Information:**
```
GET /api/models
Response:
{
  "available_models": [
    {
      "name": "gemini-1.5-pro",
      "description": "Advanced reasoning and complex tasks",
      "status": "active"
    },
    {
      "name": "gemini-1.5-flash", 
      "description": "Fast responses for simple tasks",
      "status": "active"
    },
    {
      "name": "gemini-pro-vision",
      "description": "Image analysis and vision tasks", 
      "status": "active"
    }
  ]
}
```

**Health Check:**
```
GET /api/health
Response:
{
  "status": "healthy",
  "timestamp": "2025-08-14T10:30:00Z",
  "models_status": {
    "gemini-1.5-pro": "operational",
    "gemini-1.5-flash": "operational", 
    "gemini-pro-vision": "operational"
  }
}
```

#### Evaluation and A/B Testing Endpoints

**Model Comparison:**
```
POST /api/evaluate/compare
{
  "prompt": "Explain quantum computing",
  "models": ["gemini-1.5-pro", "gemini-1.5-flash"]
}

Response:
{
  "comparison_id": "comp_12345",
  "results": [
    {
      "model": "gemini-1.5-pro",
      "response": "Quantum computing is...",
      "processing_time": 2.5,
      "token_count": 200
    },
    {
      "model": "gemini-1.5-flash", 
      "response": "Quantum computing is...",
      "processing_time": 1.2,
      "token_count": 150
    }
  ]
}
```

**A/B Test Endpoint:**
```
POST /api/ab-test/participate
{
  "test_name": "summarization_quality",
  "user_id": "user_12345",
  "input_data": {
    "text": "Long article text...",
    "task": "summarize"
  }
}

Response:
{
  "test_group": "A",
  "model_used": "gemini-1.5-pro",
  "response": "Summary text...",
  "test_session_id": "session_67890"
}
```

**A/B Test Results:**
```
GET /api/ab-test/results/{test_name}
Response:
{
  "test_name": "summarization_quality",
  "total_participants": 100,
  "group_a": {
    "participants": 50,
    "model": "gemini-1.5-pro",
    "avg_processing_time": 2.3,
    "success_rate": 0.96
  },
  "group_b": {
    "participants": 50, 
    "model": "gemini-1.5-flash",
    "avg_processing_time": 1.8,
    "success_rate": 0.94
  }
}
```

### 5. Evaluation and A/B Testing (`evaluation_and_ab_testing.py`)

Implement the following classes and functions:

**ModelEvaluator Class:**
- `evaluate_single_model()` - Evaluate one model's performance
- `compare_models()` - Compare multiple models on same input  
- `generate_evaluation_report()` - Create performance metrics report
- `log_evaluation_results()` - Save results to file/memory

**ABTestManager Class:**
- `create_test()` - Set up new A/B test configuration
- `assign_user_to_group()` - Randomly assign users to test groups
- `record_test_result()` - Log individual test results
- `calculate_test_statistics()` - Analyze A/B test performance
- `get_test_results()` - Return aggregated test results

**Utility Functions:**
- `calculate_metrics()` - Compute performance metrics (latency, token usage, etc.)
- `export_results()` - Export evaluation data
- `load_test_config()` - Load A/B test configurations

### 6. Unit Tests (`unit_test.py`)

Implement comprehensive tests covering:

**Flask API Tests:**
- Test all endpoint responses and status codes
- Test input validation and error handling
- Test authentication and API key validation

**Model Integration Tests:**  
- Test LangChain model initialization
- Test each Gemini model functionality
- Test model switching and fallback mechanisms

**Evaluation System Tests:**
- Test model comparison functionality
- Test evaluation metrics calculations
- Test result logging and retrieval

**A/B Testing Tests:**
- Test user assignment to groups
- Test result recording and aggregation
- Test statistical calculations

**Error Handling Tests:**
- Test invalid inputs
- Test API failures and timeout handling
- Test missing configuration scenarios

## Technical Specifications

### Required Features

1. **LangChain Integration:**
   - Use LangChain chains for model interactions
   - Implement proper prompt templates
   - Handle streaming responses where applicable
   - Error handling for model API failures

2. **Multi-Model Support:**
   - Dynamic model selection based on task type
   - Fallback mechanisms when primary model fails
   - Model performance monitoring
   - Configuration-driven model parameters

3. **Evaluation Pipeline:**
   - Automated model comparison
   - Performance metrics tracking (latency, tokens, accuracy)
   - Result logging and analysis
   - Export functionality for results

4. **A/B Testing Framework:**
   - Random user assignment to test groups
   - Persistent session tracking
   - Statistical analysis of results
   - Configurable test parameters

5. **Error Handling:**
   - Comprehensive input validation
   - Graceful API failure handling
   - Proper HTTP status codes
   - Informative error messages

### Performance Requirements

- API response time < 5 seconds for text tasks
- API response time < 10 seconds for image analysis
- Support for concurrent requests (minimum 10 simultaneous users)
- Proper resource management and cleanup
- Memory-efficient processing

## Test Cases

Your implementation will be evaluated against these test scenarios:

### Basic Functionality (40 points)
1. All API endpoints return correct responses
2. Gemini models integrate successfully
3. LangChain chains work properly
4. Basic error handling functions correctly

### Advanced Features (30 points)  
5. Model comparison generates accurate metrics
6. A/B testing assigns users correctly
7. Evaluation pipeline produces meaningful results
8. Dynamic model selection works

### Code Quality (20 points)
9. Unit tests achieve >80% coverage
10. Code follows Python best practices
11. Proper documentation and comments
12. Configuration management works correctly

### Error Handling (10 points)
13. Invalid inputs handled gracefully
14. API failures don't crash the system
15. Proper logging and monitoring implemented

## Submission Requirements

### Testing
- All unit tests must pass
- Test coverage report showing >80% coverage
- Integration tests for API endpoints
- Example usage scripts or curl commands

## Time Limit

Complete this implementation within **4-5 hours**. Prioritize core functionality first, then add advanced features.

## Getting Started

1. Set up your environment and install dependencies
2. Configure your Gemini API key in `.env`
3. Implement the configuration in `config.py`
4. Build the Flask API in `app.py`
5. Create evaluation and A/B testing logic
6. Write comprehensive unit tests
7. Test the complete system

## Success Criteria

Your solution is successful when:
- All API endpoints respond correctly
- All three Gemini models are integrated and functional
- Evaluation pipeline compares models accurately
- A/B testing assigns users and tracks results
- Unit tests pass with high coverage
- System handles errors gracefully

Good luck with your implementation!
