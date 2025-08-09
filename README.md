# anomaly-detection
### Log Anomaly Detection System

[![Python Version](https://img.shields.io/badge/python-3.8%2B-blue)](https://www.python.org/)
[![License](https://img.shields.io/badge/license-MIT-green)](LICENSE)

This system detects anomalous patterns in log files using clustering, dimensionality reduction, and statistical analysis. It processes raw logs, identifies key event templates, and flags suspicious time windows based on reconstruction error metrics.

## Features
- 🕒 Time-based windowing of log events
- ✨ Automatic template generation via K-Means clustering
- 🔍 PCA-based anomaly detection
- 📊 Performance evaluation with precision/recall metrics
- 🧹 Advanced log scrubbing and preprocessing
- 📈 Adaptive threshold calculation

## Installation
1. Clone repository:
```bash
git clone //https://github.com/Omega-Makena/anomaly-detection.git
cd log-anomaly-detection
```

2. Install dependencies:
```bash
pip install -r requirements.txt
```

3. Download NLTK resources:
```python
python -c "import nltk; nltk.download(['wordnet', 'punkt', 'averaged_perceptron_tagger'])"
```

## Usage
```python
from log_processor import LogProcessor
from log_scrubber import LogScrubber
from adaptive_parser import AdaptiveLogParser
from anomaly_detector import AdaptiveAnomalyDetector

# Process log file
processor = LogProcessor()
log_data = processor.processLogFile("path/to/logfile.log")

# Clean and preprocess
scrubber = LogScrubber()
log_df = pd.DataFrame(log_data)
log_df['clean_message'] = log_df['Data'].apply(scrubber.extract_key_phrases)

# Generate event templates
parser = AdaptiveLogParser()
templates = parser.generate_event_templates(log_df['clean_message'].dropna().tolist())

# Detect anomalies
detector = AdaptiveAnomalyDetector()
detector.fit(normal_windows_matrix)
spe_scores, anomalies = detector.detect_anomalies(full_event_matrix)
```

## Configuration
Modify these parameters for custom behavior:

| Component          | Parameter               | Default       | Description                          |
|--------------------|-------------------------|---------------|--------------------------------------|
| `LogProcessor`     | `extensions`            | `['.log','.txt']` | File types to process          |
|                    | `logPattern`            | Custom regex   | Log parsing pattern             |
| `LogScrubber`      | `windowSizeMinutes`     | 5             | Time window duration           |
|                    | `stopwords`             | Custom set    | Words to ignore during cleaning |
| `AdaptiveLogParser`| `min_df`                | 0.01          | Minimum document frequency      |

## Class Structure
### LogProcessor
- `getLogFiles()`: Discovers log files in directory
- `extractLog()`: Parses raw log lines
- `processLogFile()`: Converts logs to structured format

### LogScrubber
- `createTimeStamp()`: Creates datetime objects
- `scrubWords()`: Cleans special characters/hex values
- `extract_key_phrases()`: Extracts meaningful log phrases
- `createTimeWindows()`: Groups logs into time windows

### AdaptiveLogParser
- `generate_event_templates()`: Creates log templates via clustering

### AdaptiveAnomalyDetector
- `fit()`: Trains PCA model on normal data
- `detect_anomalies()`: Flags anomalous windows

## Workflow
1. **Log Parsing** → 2. **Cleaning** → 3. **Time Windowing** →  
4. **Template Generation** → 5. **Event Matrix Creation** →  
6. **Anomaly Detection** → 7. **Results Evaluation**

## Sample Output
```
=== Results ===
True Positives: 12
False Positives: 3
False Negatives: 1
Precision: 0.80
Recall: 0.92
F1 Score: 0.86

Detected 15 anomalous windows:
Window 5 (2023-01-01 12:05:00 to 12:10:00):
  SPE score: 8.42 (threshold: 5.00)
  Log count: 120
  Errors: 3
  Key messages:
    - connection timeout
    - socket exception
```

## Dependencies
- Python 3.8+
- pandas
- scikit-learn
- rapidfuzz
- nltk
- numpy

## Contributing
Contributions are welcome! Please open an issue or submit a PR for:
- Bug fixes
- Performance improvements
- Additional log formats
- Enhanced documentation

## License
This project is licensed under the MIT License - see [LICENSE](LICENSE) for details.
