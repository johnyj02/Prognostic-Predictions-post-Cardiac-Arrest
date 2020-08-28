# Prognostic-Predictions-post-Cardiac-Arrest
## 1. Introduction
Cardiac arrest is the most common cause of death in the United States and a major public health problem. Of more than 125,000 comatose patients hospitalized after cardiac arrest each year, nearly half die after withdrawal of life-sustaining therapy for perceived poor neurological prognosis (WLST-N). At the same time, most survivors enjoy favorable neurological recovery. When choosing WLST-N, clinicians demand high levels of certainty. Unfortunately, current tools to predict these patients’ outcomes are inadequate: no clinical findings or tests offer adequate specificity predicting poor outcome for at least 3 days post-arrest, and prognosis often remains uncertain far longer. Each year tens of thousands of patients with unrecognized irrecoverable brain injury receive prolonged care at staggering cost. Thousands more have unrecognized recovery potential yet die after misguided WLST-N. Improving the speed and accuracy of post-arrest prognostication will save lives, allow appropriate resources to be directed to patients who are likely to benefit, avoid long and difficult care for patients who cannot recover, and spare families the agony of uncertainty.

## 2. Data sources used:
  * <b>Patient Time invariant data</b> - This contains the time invariant attributes for about 3000 patients. The are about 255 attributes that are recored at a patient level. 
      * Since some of the variable are a consequence of death, Expert opinion was used to filter such variables and others that were obviously not related to the problem at hand
      * Variable imputation were done using rule based imputation based on certain business rules and a combination of KNN and Random forest imputer. Details of all the above data preperation for this data source can be found here
      
   * <b>EEG data summarized by hour</b>: Electroencephalography (EEG) is a rich source of data that offers insight into brain structure and function.EEG can be described by expert interpretation or quantitative (qEEG) methods. Both approaches can predict post-arrest outcomes.Unlike expert interpretation, qEEG is objective and can be automated, making it broadly accessible. This data Contains EEG brain wave signals for about 2500 patients for a period of 48 hours post resuscitation. The model only uses 10 hours of data which proved to be sufficient to obtain the most out of the qEEG signals.
      * Three methods were used to impute the time series EEG data. One hot deck, Random one hot deck (radomized to choose data 3 hours around the point of interest), a quadratic spline
   * <b> EEG second by second data </b>: 
