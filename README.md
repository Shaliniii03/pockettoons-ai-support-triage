# PocketToons AI Support Triage Agent

### Live Application
https://support-triage-agenti.lovable.app

### Overview
This project is an AI-powered support ticket triage system built for a subscription-based consumer app like PocketToons.
The system reads incoming support tickets, classifies them into categories, suggests a reply for the support team, and identifies tickets that should be reviewed by a human.
It also includes a Human Review Queue and a Slack Notification Preview for escalated tickets.

### Category Taxonomy
I classified tickets into the following 5 categories:
Billing & Payments
Account Issues
Content Access Problems
Technical Bugs
General Feedback

### Why these categories?

I chose these categories because they cover most common support requests in a subscription-based app and are easy to route to the right team.
For example:
Billing issues can go to the payments team
Account issues can go to customer support/security
Technical bugs can go to engineering
Feedback can be shared with the product team

### Approach

For every ticket, the system performs four steps:
Classify the ticket into a category
Generate a confidence score
Generate a suggested support reply
Check whether the ticket should be escalated

### Escalation Rules

A ticket is escalated if it contains:
Account hacking or security concerns
Fraud complaints
Payment disputes
Legal threats
Abusive language
Low confidence predictions
One design choice I made was to treat escalation as a business-risk problem rather than only a confidence problem.
For example, a hacked account should always be reviewed by a human, even if the model is very confident about the classification.

### Evaluation

To validate the system, I manually reviewed all 20 synthetic tickets and compared the predicted categories against the expected categories.

### Results
Total evaluation tickets: 20
Correct classifications: 20
Classification accuracy: 100%

Escalation results:

Tickets that should be escalated: 4
Tickets correctly escalated: 4
Escalation recall: 100%

### Note on Confidence Scores
The confidence score shown by the model should not be treated as actual accuracy.
Confidence represents how certain the model is about a prediction, while accuracy must be measured against manually reviewed examples.

### Slack Integration

The current version includes a Slack Notification Preview.
The preview shows the message that would be sent when a ticket is escalated.
I did not implement a real Slack integration for this assignment. In a production system, I would use Slack Incoming Webhooks to automatically send escalated tickets to a dedicated support channel such as #support-escalations.

### Production Considerations

If this system needed to handle around 10,000 tickets per month, I would add:
Batch processing
Queue-based processing
Monitoring and logging
Human review workflows
Slack notifications
Caching common ticket patterns
Feedback loops to improve performance over time

I would also explore multi-label classification because some real customer tickets may contain more than one issue.

### Cost Estimate
Assumptions:
10,000 tickets per month
Around 800 tokens processed per ticket
The estimated monthly LLM cost would likely be in the range of $5–25 depending on the model used and average ticket length.
Additional costs would include hosting, monitoring, and storage.
Even with these costs, the system would be significantly cheaper than manually reviewing every ticket.

### Limitations
The evaluation dataset is synthetic and cleaner than real customer support data.
Real tickets may contain spelling mistakes, multiple issues, incomplete information, and multiple languages.
The current version assigns only one category to each ticket.
The system currently relies on a single model provider.
These limitations would need to be addressed before production deployment.

### Tools Used

Lovable
LLM-based classification workflow
CSV / JSON export
Human Review Queue
Slack Notification Preview
I used Lovable to quickly build and test the workflow. The category design, escalation logic, evaluation approach, and production recommendations were designed manually.

