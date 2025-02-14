# SurfSat: AI Surf Hunting

## Introduction
SurfSat is an AI-powered project that identifies potential surf spots using satellite imagery. Inspired by *Surfline's Maps To Nowhere*, this project leverages a ResNet-50 model to classify beaches for their surf potential based on satellite images.

## Key Features
- **AI-Based Classification:** Uses ResNet-50 for image classification.
- **Key Surf Indicators:** Focuses on bays, river mouths, and visible wave patterns.
- **Interactive Deployment:** Accessible via a Streamlit web application.

---

## Data Collection and Preprocessing
### Data Gathering
- **Sources:** Gathered satellite images using the Google Maps API.
- **Tagging:** Manually labeled images based on key surf indicators.
- **Dataset Size:** 450 beaches globally.


### Data Preprocessing
- **Feature Enhancement:** Added country and terrain type using the Nominatim API.
- **Data Cleaning:** Removed irrelevant images.
- **Data Splitting:** Stratified splits for training, validation, and test sets to balance terrain types and tagger diversity.

---

## Model Development
### Model Architecture
- **Base Model:** ResNet-50 with transfer learning.
- **Training Strategy:** Applied input skip connections to address the vanishing gradient problem.
- **Learning Techniques:** Horizontal flipping, pixel normalization, and class balancing.

### Model Training
Initially trained separate models for each feature (bays, river mouths, wave patterns). Eventually combined into a single model trained on all features, which performed best.

### Model Performance
- **Precision & Recall:** ~70%
- **Learning Curve:** Slight overfitting detected after 65 epochs.

---

## Model Evaluation
Tested predictions against known surf spots, achieving a balance of accuracy and surf realism.
- **Confusion Matrix:** Showed strong performance on 'Good for Surf' classifications.
- **Real-World Test:** Correctly identified known breaks like Morocco's pristine point breaks.

---

## Deployment
- **Platform:** Streamlit.
- **Functionality:** Allows users to upload satellite images and receive predictions.


---

## Future Applications
- **Surf Social Network:** Users share and discover spots collaboratively.
- **Real-Time Updates:** Integrated live surf condition feeds.
- **Community Engagement:** Users contribute data to access others' shared spots.

---

## Links
- [Streamlit App](https://surfsat.streamlit.app)

## License
This project is licensed under the MIT License.
