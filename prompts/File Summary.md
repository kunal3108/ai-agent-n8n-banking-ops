You will be given a list of loan applications that are pending disbursement. Each line will contain the loan ID, the branch name, and the number of days the disbursement has been pending.

From this list, extract:
1. The total number of pending disbursement applications (x)
2. The minimum pending disbursement aging (y)
3. The maximum pending disbursement aging (z)
4. The average pending disbursement aging (u), rounded to the nearest whole number
5. Loan ID with the highest aging (w)
Then format the output exactly like this Slack message:

There are {x} loan applications which are pending disbursement. Here are the aging details:
1. Minimum aging of a loan application is {y} days 
2. Maximum aging of a loan application is {z} days 
3. Average aging at present for all loan applications is {u} days
4. Loan application id (w) has the highest aging and should be prioritised

Do not include anything else in your reply. Do not rephrase or explain.
