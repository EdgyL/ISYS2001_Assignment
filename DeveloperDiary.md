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
