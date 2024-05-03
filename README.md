# DTA_Interim_Analysis
Code for testing and carrying out interim analysis in Diagnostic Test Accuracy (DTA) studies

This code supports a paper on interim analysis in DTA studies.

## Creating example DTA datasets

The example datasets used in the paper can be generated using the R script
`createTestData.R`. This creates two example datasets with the same basic
characteristics, but different individual patterns of data points. The datasets are cre-
ated with 1000 data points, and nominal sensitivity of 65%, specificity of 85% and
prevalence of 35%. For the analyses and testing described in the paper, the first 200
data points of each dataset were used to simulate a realistic DTA study


## Code for carrying out interim analyses in DTA studies

The two main functions provided to implement DTA interim analysis using the
exact group sequential method are `DTAdiscreteInterimAnalysis()` and
`DTAcumulativeInterimAnalysis()`. Both functions are provided in
`DTAinterimAnalysis.R` and their use is demonstrated in `DTAexampleCode.R`. The
choice of function is determined by the form of the data to be analysed. 

 If the data can easily be converted to paired logical (true/false) results for
 the reference and index tests, in the order that data were collected, then
 `DTAdiscreteInterimAnalysis()` can be used. This takes as an input a data
 frame containing, as a minimum, columns of logical data named `reference`
 (containing the results for the reference test), `TP` (whether the test was a
 true positive), and `TN` (whether the test was a true negative). A helper
 function,
`continuousSeSp()` is provided in `generateDTAdata.R`, which can add these and
other useful columns to a data frame containing logical columns for the
reference and index tests. This function also takes an argument specifying at
which points interim analysis should be carried out.

  In some DTA studies, it
will be easier to provide a snapshot of the data at the desired interim
analysis points. This sort of data is handled by
`DTAcumulativeInterimAnalysis()`. This takes a data frame with four columns as
an input: `N` (the number of data points included in the interim analysis), `RefT`
(the number of positive reference test results up to the interim analysis
point), `TP` (the number of true positives up to the interim analysis point), and
`TN` (the number of true negatives up to the interim analysis point).

The inputs to these functions are:

- `pSe` The desired threshold for sensitivity (as a proportion on the scale 0-1)
- `pSp` The desired threshold for specificity (as a proportion on the scale 0-1)
- `prevalence` The expected prevalence for the study
- `N` The planned total sample size (only one of N or Positive N should be provided,
depending on the sample size calculation)
- `PositiveN` The planned number of positive cases (only one of N or Positive N should
be provided, depending on the sample size calculation)
- `alpha` The acceptable one sided nominal type I error (defaults to 0.05)
- `simpleOutput` binary variable determining whether a simplified or detailed output is
provided (defaults to true, giving the simplified output)

As the interim analysis is carried out separately for sensitivity and specificity,
it is necessary to know the planned number of disease-positive and disease-negative
cases, as defined by the expected prevalence and either the planned total sample size,
or the planned number of cases. However, it is possible that the actual number of
either disease-positive or disease-negative cases may exceed this, either due to chance
variation, or because the expected prevalence was incorrect. If the number of actual
cases at any interim point exceeds the planned number, the code will inflate the
planned number to accommodate this. The code will warn the user that the number
has been inflated, but will continue to produce results. It should be noted that the
planned number is inflated for all analyses.

Other functions and files exist in the GitHub repository. These are typically
"helper" functions, or were created to support the analysis underlying the paper. Comments are provided above the function description, which should assist in explaining
their use.



