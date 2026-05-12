# activity-satisfaction

Predicting how enjoyable, fulfilling, or satisfying an activity will be,
using data I collected myself via a Google Form I designed and filled in
over several weeks.

*In progress — the preprocessing pipeline and exploratory analysis are
done; modelling is the next focus.*

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
- **Hash-based train/test split** using timestamps, so the same example
  always lands in the same set when more data is added.
- Correlation analysis and scatter matrix for feature exploration.

### What I'm working on next

- Collecting more training data: 50 examples is far too few given the
  dimensionality after one-hot encoding, and the initial linear regression
  overfits badly.
- Trying regularised and tree-based models.
- Feature engineering ideas I want to try: RBF features for multimodal
  distributions, vector embeddings for activity names via a small neural
  network.

### What I've learned

I learned a lot about sklearn pipelines from this project: how to use
ColumnTransformer to handle different feature types separately, how to
write my own `FunctionTransformer`s for things like parsing duration strings
or computing the cost-per-minute ratio, and how to chain pipelines together
for steps like parsing and then scaling. I also realised that good
preprocessing can't make up for not having enough data - 50 examples is
just too few for this many features.
