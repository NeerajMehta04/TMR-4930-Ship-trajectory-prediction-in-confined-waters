# TMR-4930-Ship-trajectory-prediction-in-confined-waters
TMR 4930 Ship trajectory prediction in confined waters
## AIS dataset process files

### 1) 0_ais_dataset_process_hs- 
- Dataset split by months
- Visualization:
  - By MMSI population
  - By message time interval
  - Map plots

### 2) 1_ais_dataset_process_hs-
- Dataset processing:
  - Handling column names
  - Voyage identification

### 3) 2_ais_dataset_process_hs_uniform_ts
- Dataset processing:
  - By navigation status
  - Uniform timestamp generation
  - Handling features:
    - Distance interpolation
    - Speed over ground (SOG) calculation
    - Course over ground (COG) interpolation
    - Handling voyage halts/zero SOG scenarios (sub voyage identification)
- Visualizations:
  - By navigation status
  - By lower SOG values (slow vessel dynamics)
 
### 4) 3_ais_dataset_process_hs_news_uniform_ts
#### (Note: NEWS has not been tried as a feature for model training as a part of this work.)
- Dataset processing/feature engineering:
  - NEWS generation (distance to coast towards North, East, West, and South direction) - geolocation generalized feature
  - Other feature engineering (* have been used as a part of training data):
    - The Vessel is in TSS (In_TSS)
    - The vessel belongs to TSS (Vessel_TSS)
    - Nearest distance to coast (NDC)
   
### 5) 4_ais_dataset_process_hs_ttv_rnn
- Dataset processing:
  - Standardization (Min-Max)
- Dataset creation:
  - Create sequences of data with a length of 20 timestamps.
  - For the nth timestamp in a voyage, the sequence [n:n+20) is created, where [n:n+10) represents the observed trajectory (X) and [n+10:n+20) represents the predicted trajectory (Y).

### 6) 5_ais_dataset_process_hs_other_encounter
- Dataset processing:
  - Used to consider all vessels towards encounter vessels, irrespective of zero SOG/stationary status.

### 7) 6_ais_dataset_process_hs
- Training dataset creation:
  - Concatenate data from months January to October to create the training dataset.
  - Separate data into:
    - X_train: Input features
    - Y_lat_train: Ground truth latitude for training
    - Y_lon_train: Ground truth longitude for training
    - Y: Combined ground truth latitude and longitude for training

### 8) 7_ais_dataset_process_encounter_uniform_ts
#### (Note: ENCOUNTERS have not been used as a part of the training/this work)
- Dataset generator:
  - Identification of encounter vessels for each dynamic vessel (Encounter vessel data generator).
  - Accommodating encounters could improve predictions and provide real-world scenario representation.

### 9) 8_ais_dataset_process_dist_angle
- Dataset generator:
  - Implementation of the proposed RDA (Relative Displacement Angle) approach.
  - Map the relative displacement of prediction timestamps to the last observed state of the 10-minute voyage set.
  - Map the relative change in the bearing angle to the last observed state of the 10-minute voyage set.


## ml models 

### - 3 models developed towards prediction and comparison:
#### 1) CNN LSTM
#### 2) LSTM
#### 3) GRU

### - Training/predictions made using two approaches:
#### 1) Normal - latitude, longitude predictions (file tags: _normal)
#### 2) RDA - displacement/distance, bearing/angle (file tags: _with_distance, _with_angle, _rda_model)
