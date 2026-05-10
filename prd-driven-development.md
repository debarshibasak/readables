# PRD-driven development

In the past few weeks, we have been able to churn out features and production-grade code using Claude. One of the ideas that helped us ship with confidence is the process called PRD-driven development.

The issue with vibecoding without a plan is that the layers of changes made to the project can lead to exponential tech debt. This is a real problem with AI-driven development. A junior engineer can implement a very complex TCP proxying project and will be happy with the outcome if it works, but would not understand what is happening behind the scenes or how they arrived at this outcome.

To tackle that, we built a process where we generate a PRD for every task, and every time the agent updates the code, it also updates the PRD to indicate how it arrived at the solution. Now, you have some documentation of how it was achieved, which helps when tackling tech debt. In principle, it improves project or product understanding, as there is a lot of documentation and context to assist developers and agents.

