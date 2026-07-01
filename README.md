# marketing_ai_agent_flow.md
flowchart TD
    Start(["Campaign Request Input"]) --> Orch["Orchestrator Agent<br/>Parses goals, budget, audience, deadline"]

    Orch --> Strat["Strategy Agent<br/>Builds positioning, channel mix, KPIs"]
    Strat --> StratCheck{"Strategy Validator<br/>Feasible vs budget/time?"}
    StratCheck -- "No" --> StratRefine["Refine constraints"] --> Strat
    StratCheck -- "Yes" --> Research["Research Agent<br/>Audience, competitor, trend data"]

    Research --> ResCheck{"Data Sufficient<br/>& Confidence High?"}
    ResCheck -- "No" --> ResRetry["Expand sources / re-query"] --> Research
    ResCheck -- "Yes" --> Content["Content Agent<br/>Copy, creative briefs, assets"]
    Research --> SEO["SEO/Ads Agent<br/>Keywords, bidding, targeting rules"]

    Content --> QA["QA & Brand Compliance Agent<br/>Tone, legal, brand guidelines"]
    SEO --> QA

    QA --> QAGate{"Pass Compliance<br/>& Quality Score?"}
    QAGate -- "No (score < threshold)" --> ContentRevise["Send back with edit notes"] --> Content
    QAGate -- "Yes" --> Scheduler["Scheduling/Distribution Agent<br/>Channel timing, sequencing"]

    Scheduler --> Deploy["Deployment<br/>(Publish to channels)"]

    Deploy --> Monitor["Analytics/Monitoring Agent<br/>CTR, conversions, spend, engagement"]

    Monitor --> PerfCheck{"Performance vs KPI<br/>Threshold?"}
    PerfCheck -- "Underperforming" --> Optim["Optimization Agent<br/>A/B test, reallocate budget, tweak targeting"]
    Optim --> Deploy
    PerfCheck -- "On/Above Target" --> Report["Reporting Agent<br/>Insights, ROI summary"]

    Report --> HumanReview{"Human-in-the-loop<br/>Approval / Escalation?"}
    HumanReview -- "Escalate" --> Orch
    HumanReview -- "Approved" --> End(["Campaign Cycle Complete"])

    %% Failure handling
    QA -. "Agent failure/timeout" .-> Fallback["Failure Handler<br/>Retry (3x) → fallback model → alert human"]
    Research -. "Agent failure/timeout" .-> Fallback
    Content -. "Agent failure/timeout" .-> Fallback
    Monitor -. "Agent failure/timeout" .-> Fallback
    Fallback --> Orch
