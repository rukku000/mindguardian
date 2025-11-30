# MindGuardian: Adaptive Emotional Digital Twin for Burnout Prevention

**Track: Agents for Good**  

## Problem Statement

University students and self-learners face a vicious cycle of overwork followed by procrastination, leading to chronic burnout and inconsistent academic performance. Traditional tools like timers, to-do apps, and generic study planners treat all users identically—they ignore individual emotional states, fatigue patterns, and cognitive capacity, often exacerbating the problem by pushing users past healthy limits. No existing AI system continuously learns a student's unique focus windows, distraction triggers, and overload signals to proactively protect mental health while sustaining productivity. This gap is critical: burnout affects 60-70% of college students annually, correlating with lower GPAs and higher dropout rates.

MindGuardian solves this by creating an "emotional digital twin"—a multi-agent system that becomes your personal cognitive guardian, detecting burnout before it spirals and dynamically rebalancing your study flow for sustainable performance.

## Why Agents?

Burnout prevention requires continuous monitoring, contextual memory, real-time adaptation, and coordinated intervention—capabilities single LLMs or rule-based apps cannot achieve. Agents excel here because:

- **Specialization**: Different agents handle distinct tasks (profiling, planning, monitoring, coaching, evaluation) with domain expertise
- **Orchestration**: Sequential startup (build profile → plan schedule) + loop monitoring (Sentinel continuously checks state) creates an "always-on" guardian
- **Memory across time**: Long-term Memory Bank tracks patterns over weeks ("You crash every Thursday after 7pm"), enabling truly personalized adaptation
- **Negotiation**: Agents communicate via structured A2A messages ("BurnoutAlert" → "PlanRevision"), resolving conflicts intelligently
- **Evaluation**: Meta-agent assesses intervention effectiveness, closing the feedback loop

A single prompt-based system would forget context mid-session or fail to monitor continuously. Agents create a living ecosystem that evolves with you.

## What You Created

MindGuardian orchestrates 5 specialized LLM-powered agents using sequential + loop patterns, long-term memory, observability, and evaluation—all built with Google's Agent Development Kit (ADK).

### Architecture Overview

```
Twin Builder → Planner → [Sentinel Loop: Monitor → Coach/Planner if Alert] → Evaluator
```

**Core Agents**:

1. **Twin Builder (Memory)**: Loads/creates user profile from Memory Bank (JSON/SQLite). Tracks goals, mood history (1-5 scale), task completion patterns, peak focus hours
2. **Planner (Gemini)**: Breaks courses into atomic tasks, tags load levels (high/medium/low), schedules around profile data using calendar tools. Sequential: reads Twin first
3. **Sentinel (Loop Agent)**: Monitors signals every 3 interactions/minutes—chat sentiment ("I'm tired"), skip rates, continuous work time. Triggers "BurnoutAlert" via A2A if thresholds crossed
4. **Coach (Gemini)**: Delivers empathetic interventions ("45min high-load detected—switch to review?"), micro-routines (breathing prompts), negotiates changes. Memory-aware ("Last week this helped")
5. **Evaluator**: Scores interventions (completion rate pre/post, mood delta), suggests system tweaks. Demonstrates agent evaluation requirement

**Key Features Implemented** (meets 5+ rubric items):
- Multi-agent: Sequential startup + loop monitoring + A2A protocol
- Tools: Calendar tool, Memory Bank read/write, Google Search for break science
- Sessions & Memory: InMemorySessionService + long-term Memory Bank + context compaction
- Observability: Structured JSON logs ("Sentinel: high stress detected, mood=2/5"), live metrics dashboard
- Gemini: Powers Planner/Coach natural language + reasoning
- Deployment: Vertex AI Agent Engine config ready (bonus)

## Demo

**Live Kaggle Notebook**: https://www.kaggle.com/code/rukminibheemavarapu/mindguardian-demo

**Scenario**: Exam week, user reports "feeling drained after math, exam tomorrow."

```
User: "Plan my day: 3hrs math review, 2hrs notes, exam tomorrow but I'm drained"

[Twin Builder loads profile: "User crashes after 90min math, needs 15min breaks"]

[Planner outputs initial schedule with high-load math blocks]

[Sentinel after 45min]: "Alert: 2 skips + 'drained' detected. Burnout risk HIGH."

[Coach intervenes]: "I see you've done 45min high-load math. Your profile shows you need a 10min reset now. Switch to notes or breathing routine?"

[User: "Ok, breathing first"] → [Planner rebalances: lighter tasks next]

[End of day Evaluator]: "Interventions successful: 85% completion vs historical 60%, mood improved +1.2pts"
```

**Metrics Dashboard** (live in notebook):

| Metric          | Before | After | Improvement |
|-----------------|--------|-------|-------------|
| Burnout Alerts  | 3      | 1     | -67%        |
| Task Completion | 62%    | 89%   | +44%        |
| Mood Score      | 2.3    | 3.8   | +65%        |

**YouTube Demo** (6:40): https://youtube.com/watch?v=https://youtu.be/ZyJaZ_7_Q7Y

## The Build

Built in 7 milestones over 5 days using Python + Google ADK:

```
Milestone 1 (20%): CLI skeleton + Planner/Coach prototypes
Milestone 2 (40%): Twin Builder + JSON Memory Bank
Milestone 3 (60%): Sentinel loop + A2A messaging
Milestone 4 (80%): Evaluator + observability logging
Milestone 5 (100%): Notebook UI + deployment docs
```

**Tech Stack**:
```
Core: Google ADK-Python, Gemini 1.5 Flash (core LLM)
Memory: SQLite Memory Bank + InMemorySessionService
Tools: Custom calendar tool, Google Search MCP
Observability: Structured JSON logs → Pandas metrics
Orchestration: Sequential/loop patterns + A2A protocol
Deployment: Vertex AI Agent Engine config (dockerized)
Frontend: Streamlit notebook interface
```

**Key Code Patterns**:
```
# A2A BurnoutAlert example
class SentinelAgent:
    def check_signals(self, session):
        if mood < 3 and skips > 2:
            return {"type": "BurnoutAlert", "severity": "HIGH", "suggested_action": "replan"}
```

**GitHub Repo**: https://github.com/rukku000/mindguardian.git [Public, README w/ setup + diagrams]

**Deployment**: Dockerized for Vertex AI Agent Engine—`gcloud run deploy mindguardian --image gcr.io/project/mindguardian`. Bonus points documentation included.

## If I Had More Time

1. **Wearables Integration**: Heart rate variability + eye-tracking via Fitbit/Apple Watch APIs for true biometric burnout detection
2. **Multi-User Mode**: Anonymized insights for teachers ("Class avg burnout rising Wed 3pm")
3. **Advanced Evaluation**: A/B testing interventions across user cohorts
4. **Mobile App**: Push notifications for break reminders, voice interface
5. **Research Validation**: Longitudinal study partnering with universities to measure GPA/mood impact

MindGuardian demonstrates agents' power to solve real human problems through coordinated intelligence, memory, and adaptation. Students deserve AI companions that protect their most valuable resource: mental energy.

```
