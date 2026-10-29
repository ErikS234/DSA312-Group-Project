# Decision Tree: notes for the group

## Part A. What I used

**Data**
- Clea's cleaned data (`df1` from the `clea` branch, same as Inez's logistic regression). Checked: identical rows and columns, 69,646 bookings.
- Parking (`has_parking`) and `room_changed` dropped as leakage, as in Clea's cleaning.
- Extra steps for the tree only: `log_lead_time` dropped (`lead_time` kept, a tree treats them the same), `arrival_date_month` turned into a month number, text columns one hot encoded.

**Setup**
- Metrics: F1 and ROC AUC
- Split: 80/20, stratified on `is_canceled`, `random_state=888`
- Tuned depth, leaf size, split criterion and class weights with 5 fold cross validation on the training set only

**Results (validation set)**
- Tuned tree: F1 0.67, recall 0.81, precision 0.57, ROC AUC 0.87. Best settings: depth 15, at least 100 bookings per leaf, balanced class weights. 310 leaves.
- Unrestricted tree overfits (train F1 0.996 vs validation 0.60), so tuning was needed.

**Leakage check (what putting each dropped variable back would do)**
- Baseline: F1 0.670, ROC AUC 0.872
- With `room_changed`: F1 0.678, ROC AUC 0.880. Bookings with a room change cancel 4.5% vs 31% without.
- With `has_parking`: F1 0.691, ROC AUC 0.888. Bookings with parking cancel 0% vs 30% without.
- Both are only known once the guest arrives, so the gains are not real. Supports Clea's decision to drop them.

**For the group to check**
- `arrival_date_year` is the 5th most important feature. If the Part 3 test file covers a different year, the model has never seen that value. Worth deciding whether to keep it.

## Part B. Findings for the report

- **Guest country matters most.** Portugal is the top feature. Portuguese guests cancel 35% of the time vs 24% for everyone else.
- **Longer lead time, more cancellations.** The tree's first split is lead time of 8 days or less.
- **Booking channel.** Online travel agent bookings are the riskiest segment.
- **Engaged guests stay.** More special requests means fewer cancellations.
- **Non Refund deposits cancel 95% of the time** (816 bookings), the opposite of what you would expect.

## Part C. Files

- `Decision_Tree.ipynb` (repo root): the code, runs top to bottom
- `1_depth_chart.png`: train vs validation F1 by tree depth
- `2_confusion_matrix.png`: 7,819 correct "stay", 3,078 correct "cancel", 2,311 false alarms, 722 missed cancellations
- `3_feature_importance.png`: top 15 drivers
- `4_tree_top3_levels.png`: the first three levels of the tree
- `results_table.csv`: unrestricted vs tuned tree scores
- `leakage_check.csv`: baseline vs putting `room_changed` or `has_parking` back
