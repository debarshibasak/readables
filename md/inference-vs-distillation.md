# Inference to Distillation

Let me set a baseline before I make my argument.

Inference is simple. You have a model, you give it an input, you get an output. Nothing about the model changes. It does not learn anything from the exchange.

Distillation is different. You ask a big model to solve 100,000 programming problems. You collect the code it wrote, its explanations, its corrections. Then you train a smaller model on all of that. The smaller model picks up some of what the big one knew.

My argument is that coding agents need to move from inference alone to inference plus distillation.

Right now an agent reasons through the same things over and over. Understand the system. Pick an API. Handle the errors. Check the result. Next task, same system, it does all of it again from scratch. Nothing is kept.

What I want is for a successful run to leave something behind. Code, a tool, a workflow, a skill. Something the next agent can call instead of re-deriving.

Take a Kubernetes failure. Today the agent works out how to diagnose it every single time. What I want is for that process to become a tested diagnostic tool, and for the agent to only read its output and deal with whatever the tool did not expect.

To be precise, this is not model distillation. No weights get trained. It is reasoning being distilled into software. I have been calling it capability distillation, and I think the term holds up as long as you are clear about that difference.

The thesis in one line: every task an agent solves should leave behind tested software that makes the next run cheaper, faster, and more reliable.

Some places where this applies:

| Use case | What the agent reasons through today | What gets distilled into software |
|---|---|---|
| **Kubernetes incident diagnosis** | Pod events, logs, resource limits, recent changes, to find the OOM or crash loop. | A diagnostic tool that gathers the evidence, checks the known failure patterns, and returns findings. The model handles the unfamiliar cases. |
| **Database performance investigation** | Queries for locks, blocking sessions, slow queries, missing indexes. | A database-specific tool that runs approved queries and produces the same report every time. |
| **Employee offboarding** | Which accounts, sessions, credentials, and permissions need revoking, across every system. | A workflow that discovers access, gets the approvals, revokes it, and verifies it is gone. |
| **API integration** | Read the docs, figure out auth, pagination, rate limits, error handling. | A connector with typed operations, retries, and tests. The next agent just calls it. |
| **Security remediation** | Read a finding and work out how to fix a public bucket or an overly permissive Kubernetes role. | A remediation tool with precondition checks, a proposed change, an approval gate, and verification. |
| **Data reconciliation** | How customer or invoice records map between the CRM, billing, and the database. | A reconciliation job with explicit mappings, discrepancy detection, and an exception queue. |
| **Legacy application automation** | Which screens and steps complete a business transaction. | A validated automation with checkpoints. The model steps in when the UI changes or something odd happens. |
| **Repository maintenance** | The project's conventions for adding an endpoint, migrating a schema, upgrading a dependency. | Repo-specific generators and checks that encode those conventions. |

A concrete one from our own platform. Someone asks the agent to investigate why production is slow.

The first time, the agent finds the database, writes diagnostic queries, checks Kubernetes health, correlates the evidence, and writes a report. Once that run is validated, it saves the whole thing as a parameterized investigation tool.

The next time, the agent calls something like this:

```
investigate_performance(environment="production", window="30m")
```

The tool collects and analyzes the predictable part. The model reasons about what is left over.

And it compounds. Every successful investigation can add a check or widen the coverage of the tool. You still need inference for new situations. But familiar work becomes software, which is what it should have been all along.
