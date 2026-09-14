# DSA312-Group-Project
| Column | Type | Notes |
|---|---|---|
| hotel | Category | |
| is_canceled | Boolean | target variable |
| lead_time | Integer | |
| arrival_date_year / month / day | → concatenate | combine into single `arrival_date` |
| arrival_date_week_number | Integer | drop after checking correlation with combined date |
| stays_in_weekend_nights | Integer | |
| stays_in_week_nights | Integer | |
| adults | Integer | |
| children | Integer | float until missing values filled |
| babies | Integer | |
| meal | Category | |
| country | Category | |
| market_segment | Category | |
| distribution_channel | Category | |
| is_repeated_guest | Boolean | |
| previous_cancellations | Integer | |
| previous_bookings_not_canceled | Integer | |
| reserved_room_type | Category | |
| assigned_room_type | Category | |
| booking_changes | Integer | |
| deposit_type | Category | |
| agent | Category | ID, keep for now |
| company | Category | check missingness (~94% missing) before deciding to drop |
| days_in_waiting_list | Integer | |
| customer_type | Category | |
| adr | Float | |
| required_car_parking_spaces | Integer | |
| total_of_special_requests | Integer | |
