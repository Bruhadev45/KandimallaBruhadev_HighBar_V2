# Kasparro Agentic System - Visual Architecture

## System Flow Diagram

```
┌─────────────────────────────────────────────────────────────────────────┐
│                          USER QUERY INPUT                                │
│                   "Analyze ROAS drop in last 7 days"                    │
└────────────────────────────────┬────────────────────────────────────────┘
                                 │
                                 ▼
┌─────────────────────────────────────────────────────────────────────────┐
│                          ORCHESTRATOR                                    │
│                    (Coordinates Agent Flow)                             │
└─────┬──────────────────────┬──────────────────┬──────────────────┬──────┘
      │                      │                  │                  │
      ▼                      ▼                  ▼                  ▼
┌──────────┐          ┌──────────┐       ┌──────────┐      ┌──────────┐
│ PLANNER  │          │   DATA   │       │ INSIGHT  │      │EVALUATOR │
│  AGENT   │──────────│  AGENT   │───────│  AGENT   │──────│  AGENT   │
└──────────┘          └──────────┘       └──────────┘      └──────────┘
     │                      │                   │                 │
     │ Subtasks            │ Analysis          │ Hypotheses      │ Validated
     │                      │                   │                 │ Insights
     └──────────────────────┴───────────────────┴─────────────────┘
                                 │
                                 ▼
                          ┌──────────────┐
                          │  CREATIVE    │
                          │  GENERATOR   │
                          │    AGENT     │
                          └──────────────┘
                                 │
                                 │ Recommendations
                                 ▼
┌─────────────────────────────────────────────────────────────────────────┐
│                        REPORT GENERATOR                                  │
│            (Synthesizes all outputs into final report)                  │
└────────────────────────────────┬────────────────────────────────────────┘
                                 │
                ┌────────────────┼────────────────┐
                │                │                │
                ▼                ▼                ▼
        ┌──────────────┐  ┌──────────────┐  ┌──────────────┐
        │  report.md   │  │insights.json │  │creatives.json│
        └──────────────┘  └──────────────┘  └──────────────┘
```

## Agent Details

```
┌─────────────────────────────────────────────────────────────────┐
│                      1. PLANNER AGENT                            │
├─────────────────────────────────────────────────────────────────┤
│ Input:   User query + Data summary                              │
│ Process: Understand → Decompose → Prioritize → Route            │
│ Output:  Task plan with subtasks and dependencies               │
│ Prompt:  prompts/planner_prompt.md                             │
└─────────────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────────────┐
│                      2. DATA AGENT                               │
├─────────────────────────────────────────────────────────────────┤
│ Input:   Task description from Planner                          │
│ Process: Load → Filter → Aggregate → Analyze → Summarize        │
│ Output:  Quantitative analysis with segments and trends         │
│ Prompt:  prompts/data_agent_prompt.md                          │
│ Methods: load_data(), get_data_summary(), execute()            │
└─────────────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────────────┐
│                     3. INSIGHT AGENT                             │
├─────────────────────────────────────────────────────────────────┤
│ Input:   Data analysis results                                  │
│ Process: Observe → Hypothesize → Reason → Assess                │
│ Output:  Hypotheses with evidence and confidence                │
│ Prompt:  prompts/insight_agent_prompt.md                       │
│ Focus:   Creative fatigue, audience saturation, competition     │
└─────────────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────────────┐
│                     4. EVALUATOR AGENT                           │
├─────────────────────────────────────────────────────────────────┤
│ Input:   Hypotheses + Evidence + Data                           │
│ Process: Parse → Test → Quantify → Judge → Filter               │
│ Output:  Validated insights (confidence >= threshold)           │
│ Prompt:  prompts/evaluator_prompt.md                           │
│ Critical: Acts as quality gate - prevents hallucinations        │
└─────────────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────────────┐
│                  5. CREATIVE GENERATOR AGENT                     │
├─────────────────────────────────────────────────────────────────┤
│ Input:   Low performers + Top performers + Insights             │
│ Process: Diagnose → Learn → Innovate → Diversify                │
│ Output:  Creative variations with rationale                     │
│ Prompt:  prompts/creative_generator_prompt.md                  │
│ Focus:   Data-driven, diverse, testable recommendations         │
└─────────────────────────────────────────────────────────────────┘
```

## Data Flow Detail

```
┌─────────────┐
│   CSV Data  │ (4,500 rows, 15 columns)
└──────┬──────┘
       │ load_data()
       ▼
┌─────────────────┐
│ pandas DataFrame│
└──────┬──────────┘
       │ get_data_summary()
       ▼
┌──────────────────────┐
│  High-Level Summary  │ ──────────────┐
│  (Date range, totals,│               │
│   averages, etc.)    │               │
└──────┬───────────────┘               │
       │                               │
       │ execute(task)                 │ To Planner
       ▼                               │
┌──────────────────────┐               │
│ Detailed Analysis    │ ──────────────┘
│ (Segments, trends,   │
│  top/bottom, etc.)   │
└──────┬───────────────┘
       │
       │ To LLM (summarized)
       ▼
┌──────────────────────┐
│  Insight Generation  │
│  (Hypotheses)        │
└──────┬───────────────┘
       │
       │ + Evidence
       ▼
┌──────────────────────┐
│    Validation        │
│  (Confidence scores) │
└──────┬───────────────┘
       │
       │ High confidence only
       ▼
┌──────────────────────┐
│  Final Insights      │
│  (Actionable)        │
└──────────────────────┘
```

## Validation Flow

```
Hypothesis (Insight Agent)
        │
        │ confidence = 0.75 (preliminary)
        ▼
┌─────────────────────────┐
│   EVALUATOR AGENT       │
│                         │
│ 1. Parse claim          │
│ 2. Find evidence        │
│ 3. Calculate metrics    │
│ 4. Assess significance  │
│ 5. Assign confidence    │
└────────┬────────────────┘
         │
    ┌────┴────┐
    │         │
    ▼         ▼
Score >= 0.6  Score < 0.6
    │         │
    │         └──────► REJECTED
    │
    └──────► VALIDATED ──► To Report
```

## Prompt Engineering Pattern

```
All Agents Follow:

┌─────────────────────────────────────┐
│        STRUCTURED PROMPT            │
├─────────────────────────────────────┤
│ 1. Role Definition                  │
│    "You are a [specialist]..."      │
│                                     │
│ 2. Context                          │
│    User query, data, etc.           │
│                                     │
│ 3. Task Instructions                │
│    Clear, specific, actionable      │
│                                     │
│ 4. Reasoning Framework              │
│    Think → Analyze → Conclude       │
│                                     │
│ 5. Output Format                    │
│    JSON schema with examples        │
│                                     │
│ 6. Guidelines                       │
│    Best practices, constraints      │
└─────────────────────────────────────┘
```

## Technology Stack

```
┌──────────────────────────────────────────────┐
│              APPLICATION LAYER                │
├──────────────────────────────────────────────┤
│  Python 3.10+  │  CLI Interface (run.py)     │
└────────┬───────────────────────────┬─────────┘
         │                           │
         ▼                           ▼
┌──────────────────┐        ┌──────────────────┐
│   AGENT LAYER    │        │  ORCHESTRATION   │
├──────────────────┤        ├──────────────────┤
│ • Planner        │        │ • Workflow mgmt  │
│ • Data           │◄──────►│ • State tracking │
│ • Insight        │        │ • Error handling │
│ • Evaluator      │        └──────────────────┘
│ • Creative Gen   │
└────────┬─────────┘
         │
         ▼
┌──────────────────────────────────────────────┐
│              LLM LAYER                        │
├──────────────────────────────────────────────┤
│  Anthropic API  │  Claude Sonnet 4           │
└────────┬─────────────────────────────────────┘
         │
         ▼
┌──────────────────────────────────────────────┐
│              DATA LAYER                       │
├──────────────────────────────────────────────┤
│  pandas  │  CSV Processing  │  Analytics     │
└──────────────────────────────────────────────┘
```

## Output Generation

```
Agent Outputs
     │
     ├──► insights.json
     │    ├── hypotheses[]
     │    ├── evaluations[]
     │    └── validated_insights[]
     │
     ├──► creatives.json
     │    ├── recommendations[]
     │    ├── creative_patterns{}
     │    └── testing_strategy
     │
     ├──► report.md
     │    ├── Executive Summary
     │    ├── Key Findings
     │    ├── Detailed Hypotheses
     │    └── Creative Recommendations
     │
     └──► execution_*.json (logs)
          ├── timestamp
          ├── agent
          ├── event
          └── data
```

## Error Handling Flow

```
┌─────────────┐
│ Agent Call  │
└──────┬──────┘
       │
  ┌────▼────┐
  │ Success?│
  └────┬────┘
       │
  ┌────┴─────┐
  │          │
  ▼          ▼
 YES        NO
  │          │
  │          ├──► Log Error
  │          │
  │          ├──► Return Safe Fallback
  │          │
  │          └──► Continue/Retry
  │
  └──► Process Result
       │
       └──► Continue Flow
```

## Configuration Hierarchy

```
config/config.yaml
        │
        ├──► python: "3.10"
        │
        ├──► data:
        │    ├── full_csv
        │    ├── sample_csv
        │    └── use_sample_data
        │
        ├──► thresholds:
        │    ├── confidence_min: 0.6
        │    ├── low_ctr_threshold: 0.015
        │    └── low_roas_threshold: 2.0
        │
        ├──► llm:
        │    ├── model
        │    ├── temperature: 0.7
        │    └── max_tokens: 4000
        │
        └──► outputs:
             ├── reports_dir
             ├── logs_dir
             └── file paths
```

## Legend

```
┌─────┐
│ Box │  = Component/Agent
└─────┘

  ────►  = Data flow

  ═══►   = Control flow

  ┌───┐
  │ ? │  = Decision point
  └───┘

  [###]  = Process/Action
```

---

**Visual Guide**: This diagram provides a high-level overview of the system architecture, data flow, and component interactions.
