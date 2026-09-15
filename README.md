# SleepTracker — Sleep Health Predictor

A machine learning system that predicts the likelihood of a sleep disorder from everyday health and lifestyle metrics, paired with a desktop GUI for real-time predictions. The best-performing model (Random Forest) achieved **96% accuracy** on the validation set.

## Overview

This project takes raw sleep and health survey data, cleans and engineers it into model-ready features, trains and compares five different machine learning classifiers, and deploys the best-performing model behind a custom-built Tkinter desktop application. A user can enter their own stress level, sleep quality, sleep duration, BMI category, heart rate and blood pressure, and get an instant prediction of whether a sleep disorder is likely, along with the model's confidence.

## Features

- End-to-end pipeline: data cleaning → feature engineering → model training → evaluation → deployment
- Comparison of 5 classification algorithms, evaluated on the same train/validation/test split
- Automatic selection and export of the best-performing model
- A modern, custom-designed desktop GUI for live predictions
- Input validation with clear, human-readable error messages
- Confidence score displayed alongside every prediction

## Dataset & Preprocessing

The source data (`Sleep_Data_Sampled.csv`) includes lifestyle and health indicators alongside a labelled sleep disorder status. Preprocessing steps included:

- Filling missing `Sleep Disorder` values with `"Healthy"`, since a missing label represents the absence of a diagnosed disorder
- Standardising `BMI Category` values (e.g. `"Normal Weight"` → `"Normal"`) before encoding
- Binary-encoding `BMI Category` and `Sleep Disorder` into model-ready numeric features
- Splitting the combined `Blood Pressure` field (e.g. `"120/80"`) into separate `Systolic BP` and `Diastolic BP` features
- Exporting the cleaned dataset to `Sleep_Health_Cleaned.csv`

**Final feature set:** Stress Level, Quality of Sleep, Sleep Duration, BMI, Heart Rate, Systolic BP, Diastolic BP
**Target:** Presence of a sleep disorder (binary)

Data was split 60/20/20 into training, validation and test sets using stratified sampling, to preserve the class balance of the target variable across all three sets.

## Model Training & Evaluation

Five classification models were trained and evaluated on identical data splits:

- Logistic Regression
- K-Nearest Neighbors (KNN)
- Naive Bayes
- Random Forest
- Gradient Boosting

Each model was assessed using:

- Validation and test accuracy
- Confusion matrices
- ROC curves and AUC scores

The model with the highest validation accuracy was selected as the final model and exported using `joblib` for use in the GUI application. **Random Forest was the top performer, reaching 96% accuracy.**

## The GUI Application

The trained model is deployed behind a custom Tkinter desktop application, `sleep_health_gui.py`, allowing anyone to get a prediction without touching a line of code.

**The interface includes:**
- A clean, modern two-panel layout with an informational sidebar and an input form
- Live input validation (e.g. rejecting out-of-range heart rates or non-numeric entries) with clear feedback
- A results panel showing the prediction and the model's confidence score
- A clear disclaimer that predictions are educational only, not a medical diagnosis

## Technologies Used

- **Python**
- **Pandas** & **NumPy** — data cleaning and manipulation
- **Scikit-learn** — model training, evaluation and metrics
- **Matplotlib** — confusion matrices, accuracy comparisons and ROC curves
- **Joblib** — model serialisation
- **Tkinter** — desktop GUI application

## How to Run

1. Clone the repository and ensure `Sleep_Data_Sampled.csv` is in the project directory.
2. Run the training script to clean the data, train all five models and export the best one:
   ```
   python train_model.py
   ```
   This produces `best_health_model.joblib`.
3. Launch the GUI (make sure it's in the same folder as the saved model):
   ```
   python sleep_health_gui.py
   ```
4. Enter your sleep and health metrics and select **Check My Sleep Health** to get a prediction.

## What I Learned

Through this project I improved my understanding of:
- End-to-end ML pipeline design, from raw, messy survey data through to a deployed application
- Practical feature engineering, including splitting composite fields and encoding categorical variables
- Comparative model evaluation using confusion matrices, ROC curves and AUC scores rather than accuracy alone
- The importance of preserving 1D target shapes (`Series`, not single-column `DataFrame`) when training scikit-learn classifiers, to avoid unexpected multi-output behaviour
- Building and styling a desktop GUI in Tkinter, including live input validation and dynamic UI feedback
- Serialising and loading trained models with `joblib` for use outside the training environment

## Future Improvements

- Add cross-validation rather than a single train/validation/test split, for a more robust accuracy estimate
- Explore additional features (e.g. age, occupation, physical activity level) if available in extended data
- Add SHAP or feature-importance visualisations so users can see which inputs most influenced their prediction
- Package the app as a standalone executable (e.g. with PyInstaller) so it can run without a Python environment
- Deploy as a web application for broader accessibility

## Disclaimer

This project was built for educational purposes to practise applied machine learning and software development. Predictions are based on a limited feature set and a sampled dataset, and should not be used as a substitute for professional medical advice or diagnosis.

## Author

Vanessa Aigbekaen
[GitHub Profile](https://github.com/vanessaaigbekaen)
