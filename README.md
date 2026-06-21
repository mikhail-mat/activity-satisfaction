# activity-satisfaction

Predicting how enjoyable, fulfilling, or satisfying an activity will be,
using data I collected myself via a Google Form I designed and filled in
over several weeks.

*In progress — the preprocessing pipeline and exploratory analysis are
done; choosing an appropriate model and fine-tuning is the next focus.*

### What I've built so far

- **Self-collected dataset.** Designed a survey covering activity features
  (type, social context, location, duration, cost, time of day) and three
  target variables (Enjoyment, Fulfillment, and Satisfaction) on a 1–10
  scale. Currently ~50 responses, mostly my own activities.
- **Full sklearn preprocessing pipeline** using `ColumnTransformer` with
  separate per-column pipelines:
  - One-hot encoding for categorical features (social context, location,
    autonomy)
  - Ordinal encoding for weekday
  - Log transforms for skewed numerical features
  - Cyclical (sin/cos) encoding for time of day
  - Custom parsers for duration (handles both minutes and hours) and cost
    (handles both £ and EUR)
  - An engineered cost-per-minute ratio feature
- **Comparing various models** using cross-validation and seeing if they
  underfit or overfit the data
  - Simple linear regression
  - Linear regression with L1 and L2 regularisation, using cross-validation
    for hyperparameter tuning (alpha)
  - K-Nearest Neighbours regression
  - Decision tree regression, using GridSearchCV to find the best
    hyperparameter values like max tree depth and min samples of leaves
- **Hash-based train/test split** using timestamps, so the same example
  always lands in the same set when more data is added.
- Correlation analysis and scatter matrix for feature exploration.

### What I'm working on next

- Trying out other models like Random Forest and Gradient Boosting
- Potentially collecting more training data: 71 examples is too few given the
  dimensionality after one-hot encoding
- Feature engineering ideas I want to try: vector embeddings for activity
  descriptions, reducing dimensionality with PCA or k-means clustering.

### What I've learned

I learned a lot about sklearn pipelines from this project: how to use
ColumnTransformer to handle different feature types separately, how to
write my own FunctionTransformers for things like parsing duration strings
or computing the cost-per-minute ratio, and how to chain pipelines together
for steps like parsing and then scaling. I learned how to compare models using 
cross-validation scores and tune hyperparameters
with GridSearchCV. 

I also realised that starting with 50 examples was too few for this many features, which was amplified by the presence of collinear features like cost, duration, and the derived cost-per-minute ratio, plus the one-hot encoded columns. This led to unstable training in some cross-validation folds, producing extremely high errors. Collecting more observations to reach 71 made training much more stable, and using regularisation (Ridge) helped the model generalise even further.
