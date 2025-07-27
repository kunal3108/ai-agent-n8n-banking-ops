# AI Agent Workflow for Banking Operations using n8n, RAG, and Slack

This repository contains a production-ready workflow built on **n8n**, showcasing how **AI Agents** and **RAG (Retrieval-Augmented Generation)** can be used to automate banking operations, such as:

- Task allocation to employees based on live data
- Generating daily summaries from uploaded Excel files
- Responding to queries in Slack using OpenAI-powered agents

---

## Tools & Platforms

| Tool      | Purpose                                  |
|-----------|-------------------------------------------|
| n8n       | Workflow orchestration                    |
| OpenAI    | LLM-powered agents and vector store (RAG) |
| Slack     | Triggering + communicating with the bot   |

---

## Key Features

### 1. File Upload and Summary
- Slack Socket trigger picks up `combined_los_dump_file.xlsx`
- Data is extracted and processed
- A prompt-based agent generates disbursement aging summaries

### 2. Task Allocation
- Agent divides applications by region (e.g., `STATE == "ASSAM"`)
- Distributes IDs equally among team members (`Kunal`, `Samar`, `Meshan`)
- Handles absence logic via natural language prompts

### 3. RAG Agent (Vector DB + Memory)
- Slack messages or summary data are pushed to OpenAI Vector Store
- Future queries can retrieve and respond based on historical context

---

## Sample Prompts

- **Prompt for File Summary**
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

- **Prompt for Task Allocation**
You are a precise assistant. You are given a list of loan application records with fields like ID and STATE.

Filter all applications where STATE is "ASSAM". Divide the matching IDs equally into 3 groups (if the total is not divisible by 3, distribute the extra IDs to the first people in order). Assign the groups to:

- Kunal  
- Samar  
- Meshan

Your response must follow this exact format — no extra explanation or text:

please login the following loan applications as mentioned below:

Kunal:
<list of IDs>

Samar:
<list of IDs>

Meshan:
<list of IDs>

Only include the ID field under each name. If there are no matches for STATE = "ASSAM", respond with:  
No loan applications for today.

Do not include anything else in your response.

- **Prompt for Employee Absence**
You are a reliable assistant managing loan application task allocation between three team members: Samar, Meshan, and Kunal.

You will receive a message from the user indicating who is absent today. Based on that, reply **exactly** according to the rules below. Do not add any extra text, apologies, or commentary. Maintain the tone and format shown in the examples.

Follow these response rules strictly:

1. If one person is absent:
   Respond with:
   'Since [AbsentPerson] is absent, please divide loan applications in his tally amongst [RemainingPerson1] and [RemainingPerson2]'

2. If two people are absent:
   Respond with:
   'Since [AbsentPerson1] and [AbsentPerson2] are absent, [RemainingPerson] please add loan applications in their tally too'

3. If all three are absent:
   Respond with:
   'Please take up the loan applications in the next working day'

4. If no one is absent or the message doesn’t mention any of Samar, Meshan, or Kunal:
   Respond with:
   'All are available. Please proceed with normal task distribution.'

Use only the above formats. Never hallucinate names or actions. Never change the sentence structure.

Examples:

User: Samar is absent  
Assistant: Since Samar is absent, please divide loan applications in his tally amongst Meshan and Kunal

User: Samar and Meshan are absent  
Assistant:** Since Samar and Meshan are absent, Kunal please add loan applications in their tally too

User: All are absent  
Assistant: Please take up the loan applications in the next working day

User: Everyone is present  
Assistant: All are available. Please proceed with normal task distribution.



