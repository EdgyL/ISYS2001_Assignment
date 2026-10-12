# Developer Diary

## Entry 1
After reading the assignment specifications, I have decided to make an application that takes a percentage of the user's weekly income and adds it to a savings account. The program will then run a series of calculations that will calculate the compound interest for the user over a set period of time. A user can then view the projected balance of their savings account after a specific amount of time. The graph will display the savings total ($) on the Y-axis and months/years on the X-axis. 

A user will define the percentage of their income they want to allocate to their savings; e.g. a user can choose 30% to go into their savings. Instead, a user can choose to contribute a flat amount every week, such as $200/wk.

A user can then choose different finance plans/projections for the growth of the account, for example:
- Based on average savings contribution (calculates the total_savings_added / num_weeks)
- Based on a user-specified weekly amount (user can choose how much they want to add on a weekly basis)

### The user flow will work as follows:
1. User chooses either:
- Load a historical account savings data CSV file.
- Enter current savings balance.

2. Choose future contributions:
- Average weekly contribution from the CSV (option only allowed if the user loaded a CSV).
- A fixed weekly amount.
- A percentage of weekly income.

3. Enter projection settings:
- Annual interest rate of the account.
- Compounding frequency.
- Projection length.

4. View the savings dashboard:
- Current balance.
- Projected balance.
- Total future contributions.
- Interest earned.
- Projected savings graph.

## Entry 2: Logic Planning
After creating my initial plan, I need to plan how to implement it.

### AI help with CSV formatting
The first step is deciding the required CSV format. For this, I used the help of ChatGPT to suggest the best fields for my application's needs.

I entered my plan from "Entry 1" and asked: “Based on my application plan, can you suggest a CSV format for historical savings account data? Please include the required columns and example rows, and explain how the data could be used to calculate the current balance and average weekly savings contribution.”

ChatGPT suggested using one row per transaction, with columns for date, `transaction_type`, `amount` and an `optional description`. Transaction types would include an `opening_balance`, `deposit`, `withdrawal` and `interest`.

For example: 
```
date,transaction_type,amount,description
2026-01-01,opening_balance,1000.00,Initial savings balance
2026-01-02,deposit,200.00,Weekly savings
2026-01-09,deposit,200.00,Weekly savings
...
2026-01-31,interest,5.20,Interest earned
```

I kept this as it aligned with what I needed.

### Loading The Data
The next step would be loading the data into a pandas Dataframe to be used inside the Gradio UI. I researched the Gradio Docs website here and landed on a block to use: https://gradio.app/docs/gradio/blocks

The following is the pseudocode logic for displaying the data (testing):
```
load_csv(file_path)
    READ the CSV at file_path into a DataFrame
    RETURN the DataFrame

CREATE block
    ADD csv_upload = CSV upload endpoint

    DISPLAY preview = displays a dataframe

    WHEN a CSV is uploaded
        CALL load_csv with the file path
        CALL preview to display data

RUN app
```

## Entry 3: Calculate current savings balance
The next step is to calculate the historical data uploaded by the user, for this we can use the simple logic shown below:
- `current_balance = opening_balance + deposits + interest − withdrawals`

The following is the pseudocode logic for the `current_balance()` function:
```
FUNCTION calculate_balance(data)
    SET balance to 0

    FOR each transaction in data
        IF transaction type is withdrawal
            SUBTRACT amount from balance
        ELSE IF transaction type is opening_balance, deposit or interest
            ADD amount to balance

    RETURN balance
```

After implementing this we now need to display the calculated balance to the user.

To implement this a helper method could be used to process the data for us by running `load_csv()` and `calculate_balance()`. 

```
FUNCTION process_csv(file_path)
    CALL df = load_csv(file_path)
    CALL balance = calculate_balance(df)

    RETURN balance
```

Now we need to display this inside the Gradio app, this would require updating the Block to instead display "Current Balance ($)" instead of our preview for the loaded Dataframe we added for testing. 

The updated pseudocode now looks like this:
```
CREATE Gradio app
    ADD CSV upload
    ADD current balance display

    WHEN a CSV is uploaded
        LOAD CSV into a DataFrame
        CALCULATE balance using the DataFrame
        DISPLAY current balance

RUN app
```

## Entry 4: Calculate average weekly contributions
Following our user flow, the next thing the user needs to be able to see is their average weekly contributions to their account. This too is for users who choose to load their historical data. 

The logic for this would be as shown below:
- `average_weekly_contribution = total_deposits / num_weeks`

The pseudocode for implementing this would look like:
```
FUNCTION calculate_average_weekly(df)
    GET dates from date column

    SET earliest_date to the first date
    SET latest_date to the first date

    FOR each date
        IF date is before earliest_date
            SET earliest_date to date

        IF date is after latest_date
            SET latest_date to date


    CALCULATE number_of_days as latest_date minus earliest_date
    DIVIDE number_of_days by 7 to get number_of_weeks

    IF number of weeks is 0
        RETURN 0

    SET total_deposits to 0

    FOR each transaction
        IF transaction type is deposit
            ADD amount to total_deposits

    DIVIDE total_deposits by number_of_weeks

    RETURN average weekly contribution
```

Implementing this was fairly simple, however I did look up how to get dates from a Dataframe and used the method from the google AI overview for this.
This led to me finding the source of this info at this Pandas documentation: 
- https://pandas.pydata.org/docs/getting_started/intro_tutorials/03_subset_data.html
- https://pandas.pydata.org/docs/reference/api/pandas.to_datetime.html

Now to display this we add `calculate_average_weekly()` into our `process_csv()` function and return the output along with the `current_balance` so our Gradio interface can use the variable.

This essentially works by creating a copy of the `current_balance` variable and just changing around the definitions as shown below:

```
    weekly_avg = gr.Number(
        label="Average Weekly Contributions ($)",
        precision=2,
        interactive=False,
    )
```

After that we then add the new variable into our `csv_upload.upload()` function inside the Gradio Block as shown below:

```
    csv_upload.upload(
        fn=process_csv,
        inputs=csv_upload,
        outputs=[current_balance, weekly_avg],
    )
```

We can essentially repeat this workflow of adding a new function that calculates some number based on the Dataframe, we add that function to our `process_csv()` function and then reference it in the Gradio block to see in the UI.



