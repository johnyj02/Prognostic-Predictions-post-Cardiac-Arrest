# Prognostic-Predictions-post-Cardiac-Arrest
## 1. Introduction
Cardiac arrest is the most common cause of death in the United States and a major public health problem. Of more than 125,000 comatose patients hospitalized after cardiac arrest each year, nearly half die after withdrawal of life-sustaining therapy for perceived poor neurological prognosis (WLST-N). At the same time, most survivors enjoy favorable neurological recovery. When choosing WLST-N, clinicians demand high levels of certainty. Unfortunately, current tools to predict these patients’ outcomes are inadequate: no clinical findings or tests offer adequate specificity predicting poor outcome for at least 3 days post-arrest, and prognosis often remains uncertain far longer. Each year tens of thousands of patients with unrecognized irrecoverable brain injury receive prolonged care at staggering cost. Thousands more have unrecognized recovery potential yet die after misguided WLST-N. Improving the speed and accuracy of post-arrest prognostication will save lives, allow appropriate resources to be directed to patients who are likely to benefit, avoid long and difficult care for patients who cannot recover, and spare families the agony of uncertainty.

## 2. Data sources| Imputations |Feature generation:
  * <b>Patient Time invariant data</b> - This contains the time invariant attributes for about 3000 patients. The are about 255 attributes that are recored at a patient level. 
      * Since some of the variable are a consequence of death, Expert opinion was used to filter such variables and others that were obviously not related to the problem at hand
      * The outcome variable was a combination of two features <I>survive</I> and <I>follow_commands</I>. The combination of these two features was used to indicate poor neuralogical outcome which is the positive class used in our analysis.
      * Variable imputation were done using rule based imputation based on certain business rules and a combination of KNN and Random forest imputer. Details of all the above data preperation for this data source can be found here
      
      
   * <b>EEG data summarized by hour</b>: Electroencephalography (EEG) is a rich source of data that offers insight into brain structure and function.EEG can be described by expert interpretation or quantitative (qEEG) methods. Both approaches can predict post-arrest outcomes.Unlike expert interpretation, qEEG is objective and can be automated, making it broadly accessible. This data Contains EEG brain wave signals for about 2500 patients for a period of 48 hours post resuscitation. The model only uses 10 hours of data which proved to be sufficient to obtain the most out of the qEEG signals.
      * Three methods were used to impute the time series EEG data. One hot deck, Random one hot deck (radomized to choose data 3 hours around the point of interest), a quadratic spline
   * <b> EEG second by second data </b>: The data contain the above mentioned EEG data at the higher resolution (granularity= every seconds). Since the data is extremely huge we extracted only the first 6 hours. The aim of using this dataset is the fact that we confirmed an anomalous pattern in the second by second which appear in a sub category of patients. This anomalous behaviour is not captured in the existing summarized data and hence had to be extracted during feature generation (refer to code).
   * <b> Medications data</b>: This data set captures all medications that is admisitered to the patients post resuscitation. The data is recorded at periodic intervals i.e. a record is generated with a start and stop time for every medication administered.
   * <b> Temperature and blood pressure data</b>: This data continously captures the temperature and blood pressure of the patients post resuscitation. THe data is then aggregated at hourly intervals. We use the first 10 hours of the data in our analysis 
   * <b> Expert interpretaion data</b>: The dataset comprises of EEG interpretations (Background and superimposed) from experts categorized into 3 -Flat activity, continous background activity and burst-suppression (alternating periods of flat with periods of activity). This data set was built in an effort to capture the anomalous patterns in the EEG second by second data. The data contains interpretation from multiple experts at various time periods, hence we can see that a single patient can have more than one EEG interpretation.
 
## 3. Model Building:
 There were many challenges that needed to be addressed during the model building stage. In particular the subset of patients across random states in the test set was not in many cases a representative of the universe of patients and the absolute necessity of maintaining a low false positive rate => 100% specificity. Maintaining 100% specificity ensured that  patients classified as irrecoverable had little to no chance of recovery thus making WLST-N acceptable.

  Of the many models that were tried Xgboost proved to be the most promissing of them all with a <b>73% sensitivity</b> while still maintaining <b>~100% specificity (99.7%)</b>. We also employed Neural Networks - a CNN-LSTM architecture (EEGnet) was used to boost sensitivity but it proved extremely difficult to tune it to maintain 100% specificity within the period of my internship. In order to maintain 100% specificity, a custom loss function was used that penalized false positives and rewarded for each true positive, this ensured the boost in sensitivity maintaining close to 100% specificity. In order to overcome issues where the random test train splits having varying distribution of data I used the following methods each of which gave similar results thus validating the process.
  * quadriple fold stratification while creating train test splits and cross validation splits. The data was stratified on age buckets, cardiac arrest type, rhythm,suppression ratio buckets. This ensured that the distribution of the the universe of patients were maintained both test and cv splits.
  * Another method used to avoid the above mentioned issue was to create a randomized mode selected model. In this we ran the entire hyper parameter tuning through 100 different random state splits of train and test and choose the model that was selected the maximum number of times. This gave some statistical significance that the selected hyper parameters were the closest to explaining the underlying distribution of data.
 
  <c>The results of this excercise are currently under review by the doctors at UPMC and written manuscripts are underway for publication purposes.</c>

