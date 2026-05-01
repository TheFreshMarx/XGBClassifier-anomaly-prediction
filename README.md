# XGBClassifier-anomaly-prediction
A machine learning model that it's trained on a specific topic to create a signal that predicts if in 2 days in the future an anomaly on the time series will happen.

## HOW TO USE
In the first part of the script 'ai prediction xgboost.ipynb', you will find the part where the dataset AI_prediction.csv is uploaded. You should change the code to upload your own dataset cleaned using 'Data cleaning.ipynb'. If you want to change the name of the variables keep an eye on changing all the name in the file!.

1. Take the dataset from GDELT (each keyword will have its own csv file)
2. Take the dataset from wikiviews
3. Use the 'Data cleaning.ipynb'
4. Use then the 'ai prediction xgboost.ipynb'
5. Fine tune the model
6. See the result

After uploading the dataset and training the model, you will have to fine tune the model to your specific case, meaning that you will have to try different numbers on the parameters of the model until it reaches the accuracy that you need. 

I will also upload the original dataset to try and test it for yourself. I will upload the ai dataset already processed and the GDELT and wikiviews dataset before processed. 

## Information Anomaly Detection Pipeline

This repository contains the implementation of an anomaly detection pipeline designed to analyze the relationship between information supply and information demand over time.

The pipeline processes time-series data to identify periods of significant mismatch—such as "Void" (extreme lack of supply relative to demand) or "Overabundance" (extreme excess of supply relative to demand).
### Pipeline Overview

The core architecture follows a straightforward Input -> Process -> Output flow:
1. INPUT

The pipeline takes in two distinct time series:

- Supply (st​): Data sourced from GDELT (Global Database of Events, Language, and Tone), representing the volume of news/information generated.
- Demand (dt​): Data sourced from Wikiviews (Wikipedia Pageviews), representing the public's search and reading interest.

Initially, the time series are rescaled using their expected values:

$x̂_t = \frac{x_t} {E(x)} $

2. PROCESS (Computing Delta Information)

The core of the analysis relies on computing the Information Delta ($δ_t$​), which measures the divergence between supply and demand.

This project utilizes a robust logarithmic formula to handle the scaling of the delta values:

$δ_t​=\log{(\frac{d_t​+1}{s_t​+1​})}$

(Note: The +1 smoothing factor is added to prevent division by zero or taking the log of zero).

Once the delta time series is calculated, we apply two different methods for anomaly detection:

Time series decomposition + IQR (Interquartile Range) method

3. OUTPUT

The final output consists of the exact points in the time series flagged as anomalies.

Based on the δt​ value and the ratio of supply to demand (st​/dt​), the pipeline classifies the information environment into five distinct states. These states are grouped into "Regular" fluctuations and severe "Anomaly" events:
Anomalies

    🔵 Void (δt​≪0): An extreme anomaly where demand heavily outweighs supply.

    🔴 Overabundance (δt​≫0): An extreme anomaly where supply vastly exceeds demand (st​/dt​≫1).

Regular States

    Lack (δt​<0): A moderate state where demand is slightly higher than supply.

    Balance (δt​=0): Perfect equilibrium where supply equals demand (st​/dt​=1).

    Abundance (δt​>0): A moderate state where supply is slightly higher than demand (st​/dt​>1).





ALl of the method above is taken from the current paper: https://doi.org/10.48550/arXiv.2602.15476.
