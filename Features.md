Creating the target variable Y:
    Y = 1 if Date Closed - Date Created < 24 hrs
    Y = 0 if Date Closed - Date Created > 24 hrs

There are some rows where Date Closed is NaN (blank)
I assume the date in which the dataset is "published" is the latest among the dates --> 2026-04-19 01:35:00 = max_date
Then for each row with Date Closed == NaN we check:
    max_date - Date Created < 24 hrs --> this means we can't properly evaluate the ticket because it did not have the chance yet to reach the 24 hrs cutoff --> WE DROP THE ROW
    max_date - Date Created > 24 hrs --> this means the ticket has been open for more than 24 hrs for sure and still was not closed --> Gets Y = 0

We observe no dropped rows by this logic, meaning the dataset actually includes only rows which are evaluated at least 24 hours before the last date included


The dataset has a Y proportion of 61% (1) vs 39% (0)
This must be taken into account when we create a validation set on the train set (ideally 80-20 split) 


REMEMBER WE CAN ONLY INPUT IN THE MODEL AS A FEATURE THE DATE CREATED
Why? --> Because if we also input the date closed, we would basically be incurring into data leakage, the model could see exactly when the difference is less than 24 hours!

So, to input information about the date created, we convert the date format into multiple columns:
Created_Hour --> hour of creation of the ticket (0 - 23)
Created_DayOfWeek --> day of the week (monday = 0 - 6 = sunday)
Created_Month --> month (1 - 12)
Is_weekend --> 1 if sunday or saturday, 0 else