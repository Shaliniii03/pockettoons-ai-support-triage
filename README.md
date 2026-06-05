# PocketToons AI Support Triage Agent

## Live Application

https://support-triage-agenti.lovable.app

---

## Overview

This project is an AI-powered support ticket triage system built for a subscription-based consumer app like PocketToons.

The system reads incoming support tickets, classifies them into categories, generates suggested replies, and identifies tickets that require human review. It also includes a Human Review Queue and a Slack Notification Preview for escalated tickets.

The goal is to reduce manual effort for support agents while ensuring that high-risk tickets are reviewed by humans.

---

## Category Taxonomy

I classified tickets into the following five categories:

* Billing & Payments
* Account Issues
* Content Access Problems
* Technical Bugs
* General Feedback

### Why These Categories?

These categories cover the most common support requests for a subscription-based content platform and make it easy to route tickets to the correct team.

Examples:

* Billing issues → Payments team
* Account issues → Customer Support or Security team
* Technical bugs → Engineering team
* Content access issues → Content Operations team
* Feedback → Product team

The categories are mutually exclusive, simple to understand, and suitable for an initial support triage workflow.

---

## Approach

For every incoming ticket, the system performs four steps:

1. Classify the ticket into a category
2. Generate a confidence score
3. Generate a suggested support reply
4. Determine whether the ticket should be escalated for human review

The workflow is designed to automate routine tickets while ensuring that potentially risky situations receive human attention.

---

## Escalation Logic

A ticket is escalated if it contains:

* Account hacking or security concerns
* Fraud complaints
* Payment disputes or chargebacks
* Legal threats
* Abusive or threatening language
* Low-confidence predictions

One design choice I made was to treat escalation as a business-risk problem rather than only a confidence problem.

For example, a hacked account should always be reviewed by a human even if the model is highly confident about the category.

Similarly, legal threats and payment disputes are escalated because the potential business impact is high.

---

## Suggested Replies

Suggested replies are generated for all tickets.

Reusable response templates are available for the two highest-volume categories:

* Billing & Payments
* Account Issues

These templates can be edited by support agents before being sent to customers.

---

## Human Review Queue

Escalated tickets are displayed in a dedicated Human Review Queue.

For each escalated ticket, the system shows:

* Ticket ID
* Original ticket text
* Escalation reason
* Confidence score

This helps support agents quickly identify tickets requiring manual review.

---

## Slack Integration

The current version includes a Slack Notification Preview.

The preview shows the message that would be sent when a ticket is escalated.

I did not implement a real Slack integration for this assignment.

In a production system, I would use Slack Incoming Webhooks to automatically send escalated tickets to a dedicated support channel such as:

#support-escalations

The notification would include:

* Ticket ID
* Category
* Escalation reason
* Action required
* Link back to the ticket

---

## Evaluation

To evaluate the system, I manually reviewed all 20 synthetic tickets and compared the predicted categories and escalation outcomes against the expected results.

### Results

* Total evaluation tickets: 20
* Correct classifications: 20
* Classification accuracy: 100%

### Escalation Results

* Tickets that should be escalated: 4
* Tickets correctly escalated: 4
* Escalation recall: 100%

For this project, escalation recall was considered more important than overall accuracy because missing a hacked account, payment dispute, or legal threat would create a higher business risk than over-escalating a ticket.

### Note on Confidence Scores

The confidence score shown by the model should not be interpreted as actual accuracy.

Confidence only reflects how certain the model is about its prediction.

Actual performance must be measured against manually reviewed examples such as the evaluation set above.

---

## Production Considerations (10,000 Tickets per Month)

If this system were deployed at production scale, I would make the following improvements:

* Queue-based processing to handle traffic spikes reliably
* Batch processing for non-urgent tickets to reduce costs
* Monitoring and logging for classification quality and failures
* Human review workflows with SLA tracking
* Real Slack notifications for escalated tickets
* Feedback loops using agent corrections to improve performance over time
* Model abstraction layer to allow switching between providers if needed
* Caching common ticket patterns to reduce repeated LLM calls

I would also explore multi-label classification because some customer tickets may contain more than one issue.

For example, a user could report both a billing problem and a login issue in the same message.

---

## Cost Estimate

### Assumptions

* 10,000 tickets per month
* Approximately 800 tokens processed per ticket
* Lightweight LLM such as GPT-4o-mini or similar

### Estimated Cost

The estimated monthly LLM cost would likely be in the range of:

$5–25 per month

depending on ticket length, prompt design, and model selection.

Additional costs would include:

* Hosting
* Monitoring
* Logging
* Storage

Even after these costs, the system would be significantly cheaper than manually reviewing every incoming ticket.

---

## Limitations

The evaluation dataset used in this project is synthetic and cleaner than real customer support data.

Real tickets often contain:

* Spelling mistakes
* Incomplete information
* Mixed intents
* Multiple languages

The current version assigns only one category per ticket, which may not fully capture tickets containing multiple issues.

The system currently relies on a single model provider.

In production, I would introduce a model abstraction layer to make switching providers easier and reduce vendor dependency.

---

## Tools Used

* Lovable
* LLM-based classification workflow
* CSV / JSON export
* Human Review Queue
* Slack Notification Preview

I used Lovable to rapidly build and test the workflow.

The category taxonomy, escalation logic, evaluation methodology, production considerations, and cost estimates were designed manually.

---

## Deliverables

Included in this repository:

* Live application
* Project README
* CSV / JSON output examples
* Evaluation dataset (20 manually reviewed tickets)
* Human Review Queue
* Slack notification preview
