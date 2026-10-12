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


