# ISYS2001_Assignment
Assessment 2: Programming Project

This repo contains an interest over time calculator.

## Savings Dashboard
It allows you to upload a CSV of their savings account, view your account history on a graph, and see how much you have contributed to it each week since the account was opened. Additionally you can enter your weekly take-home income to see the percentage of your income you've put aside each week since the account opened.

### Example
<img width="1195" height="562" alt="image" src="https://github.com/user-attachments/assets/6017dcc6-e3f4-447f-8bcf-2646bec0263c" />

## Future Savings Projection
Then in the 'Savings Projection' section you can find a series of settings, some taken from your previously entered data, that will allow you to see how your account will grow over [x] years. 

You can fully customise this projection using the following variables:
- Annual interest rate (%)
- Starting Balance
- Weekly contributions
- Projection Length (years)

Additionally you can see the final figures for your chosen time just above the graph: 
- Projected balance
- Total future contributions
- Future interest earned

### Example
<img width="1191" height="603" alt="image" src="https://github.com/user-attachments/assets/8dc75266-8657-4be5-9bae-44912ba1b6a3" />

## Savings Assistant
An AI chatbot called "Savings Assistant" will be able to read the values you enter into the program. Using this it can help explain and conceptualise what the numbers mean.

### Example
<img width="1203" height="456" alt="image" src="https://github.com/user-attachments/assets/2ad91d38-a962-4581-aa3a-d31144f4b60e" />

## How to use the calculator

To use this program you must enter a CSV with the following format: `date,transaction_type,amount,description`
- Date must be in `yyyy-mm-dd` format
- *Description is optional*

Here is an example of a valid transaction history:
```csv
date,transaction_type,amount,description
2025-01-01,opening_balance,1000.00,Initial savings balance
2025-01-03,deposit,200.00,Weekly savings
2025-01-10,deposit,200.00,Weekly savings
2025-01-17,deposit,150.00,Weekly savings
2025-01-24,deposit,250.00,Weekly savings
2025-01-31,deposit,175.00,Weekly savings
2025-01-31,interest,6.58,Sample monthly interest
```

You can upload your `.csv` to program at the very top of the dashboard. Any errors with the file will be displayed.


## AI Chatbot API Key
If you are intending to run this using the AI chatbot feature please follow the steps below.

To set this up (Google Colab only):
1. Get your API key from google studio
2. Open the Secrets section in Colab using the key icon
3. Add a secret called `GEMINI_API_KEY` and paste your API key 
4. Turn on notebook access for this secret
