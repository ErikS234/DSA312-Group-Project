# Decision Tree: notes for the group

## Part A. What I used

**Cleaning**
- Group cleaning from `Team Project.ipynb`, same steps and order.
- Two cells in `Team Project.ipynb` error when run. I fixed them in my copy:
  - `adr` 5400 cell: stray space at the start of a line (IndentationError), so the outlier was never replaced.
  - `country` fill with "Unknown": fails because `country` is already a category type (TypeError).
- Extra steps for the tree only: month, day of week and total nights taken from `arrival_date`; `arrival_date_week_number` dropped; `company` turned into a yes/no flag; top 15 countries and top 20 agents kept, rest "Other"; text columns one-hot encoded.

**Setup**
- Metrics: F1 and ROC-AUC
- Split: 80/20, stratified on `is_canceled`, `random_state=888`
- `assigned_room_type` and `room_changed` dropped by default (see leakage check)

**Results (validation set)**
- Tuned tree: F1 0.67, recall 0.83, precision 0.56, ROC-AUC 0.86.
- Unrestricted tree overfits (train F1 0.996 vs validation 0.61), so I tuned depth and leaf size with cross-validation.

**Leakage check**
- Without `assigned_room_type` / `room_changed`: F1 0.667. With them: F1 0.677.
- `room_changed` becomes the 4th most important feature. Bookings with a room change cancel only 4.5% vs 31% without.
- Room changes usually happen at check-in, so a cancelled booking rarely gets one. This looks like the outcome leaking in, so I suggest keeping both dropped.

**Odd things in the data**
- Every booking with a required parking space (5,873) did NOT cancel (0%). Might be recorded only for guests who showed up.
- "Non Refund" deposit bookings cancel 95% of the time (816 bookings), the opposite of what you would expect.
- Portugal is the strongest predictor. Worth confirming it is real.

## Part B. Findings for the report

Cancellation rates from the group-cleaned data (69,664 bookings).

- **Guest country matters most.** Portuguese guests cancel 35% of the time vs 24% for everyone else.
- **Longer lead time, more cancellations.** Booked within a week: 8%. Booked 180+ days ahead: 39%.
- **Booking channel.** Online travel agent bookings cancel 35%, vs about 14% for direct and offline agent bookings.
- **Engaged guests stay.** No special requests: 33% cancel. Five requests: 4%. Guests who required parking never cancelled in this data (see odd things above).

## Part C. Files

- `1_depth_chart.png`: train vs validation F1 by tree depth
- `2_confusion_matrix.png`: 7,621 correct "stay", 3,160 correct "cancel", 2,510 false alarms, 642 missed cancellations
- `3_feature_importance.png`: top 15 drivers
- `4_tree_top3_levels.png`: the first three levels of the tree
- `results_table.csv`: unrestricted vs tuned tree scores
- `leakage_check.csv`: with vs without `assigned_room_type` / `room_changed`
