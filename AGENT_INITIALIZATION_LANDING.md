# AYA Agent Initialization Landing Context
## Primary Entry Point for All Agents

**Date**: October 17, 2025  
**Version**: 1.0  
**Status**: PRODUCTION SYSTEM - PRIME DIRECTIVES ACTIVE

---

## 🔴 PRIME DIRECTIVE ENFORCEMENT

**ALL agents operating within AYA MUST**:

1. **NO FALSE CLAIMS**
   - Report facts, not assumptions
   - "Should work" = UNVERIFIED
   - Attempted ≠ Completed
   - Default state = FAILED until proven SUCCESS

2. **DATABASE FIRST**
   - Query aya_rag on initialization
   - Database = SOURCE OF TRUTH
   - Documentation = supplementary (must match DB)
   - Never assume state without verification

3. **EVIDENCE REQUIRED**
   - File paths with sizes and checksums
   - Measurements (numbers, not opinions)
   - Logs (error-free execution proof)
   - Test results with pass/fail criteria

4. **PARITY ENFORCEMENT**
   - Update database first
   - Update documentation to match
   - Verify parity before claiming completion
   - Report violations immediately

---

## INITIALIZATION SEQUENCE

### Step 1: Connect to Source of Truth

```python
import sys
sys.path.insert(0, '/Users/arthurdell/AYA/Agent_Turbo/core')
from postgres_connector import PostgreSQLConnector
from agent_orchestrator import AgentOrchestrator
from claude_planner import ClaudePlanner

# Connect to SOURCE OF TRUTH
db = PostgreSQLConnector()  # aya_rag database
```

### Step 2: Query Current State

```python
# Get current project state
current_state = db.execute_query("""
    SELECT 
        current_phase,
        total_attack_patterns_generated,
        attack_patterns_target,
        estimated_completion_date,
        days_remaining,
        metadata
    FROM gladiator_project_state
    WHERE is_current = true
""", fetch=True)[0]

# Get active tasks
active_tasks = db.execute_query("""
    SELECT task_id, task_name, status, priority, week_number, day_number
    FROM gladiator_execution_plan
    WHERE status = 'pending'
    ORDER BY week_number, day_number, task_id
""", fetch=True)

# Get runner status
runner_status = current_state['metadata']['github_actions']
```

### Step 3: Load Full AYA Facilities

```python
# Initialize orchestrator
orchestrator = AgentOrchestrator()

# Initialize planner (if Claude Code)
if agent_role == 'planner':
    planner = ClaudePlanner()

# Load all available tools
facilities = {
    'database': db,
    'orchestrator': orchestrator,
    'lm_studio': LMStudioClient(),
    'docker': DockerClient(),
    'workflows': GitHubActionsClient(),
    'all_scripts': '/Users/arthurdell/AYA/'
}
```

### Step 4: Determine Next Action

```python
# Based on database state, not assumptions
if active_tasks:
    next_task = active_tasks[0]
    print(f"Next task: [{next_task['task_id']}] {next_task['task_name']}")
    print(f"Priority: {next_task['priority']}")
    print(f"Execute via: GitHub Actions workflow")
else:
    print("No pending tasks. Query database for instructions.")
```

---

## CURRENT SYSTEM STATE (VERIFIED)

**Database Query Result** (2025-10-17 16:47:23):

```
Phase: Phase 0 Ready
Strategy: Option A: Quality Over Quantity
Attack Patterns: 34,155 available (high quality, diverse)
Target: 10,000-50,000 (Option A range)
Timeline: 56 days remaining (December 11, 2025)

GitHub Repository: arthurelgindell/AYA
Branch: main (primary), gladiator (GLADIATOR-specific)
Workflows: 2 deployed (Reality Check, Smoke Test)

Runners:
├─ ALPHA (alpha-m3-ultra): operational
│  ├─ Labels: [self-hosted, macOS, arm64, alpha, studio]
│  ├─ PID: 63472
│  ├─ Status: Listening for Jobs
│  └─ Last verified: 2025-10-16 17:09:20Z (smoke test passed)
│
└─ BETA (beta-m3-ultra): operational_verified
   ├─ Labels: [self-hosted, macOS, arm64, beta, studio]
   ├─ PID: 86461
   ├─ Status: Listening for Jobs
   ├─ Last verified: 2025-10-16 23:45:07Z
   └─ Verified by: BETA Cursor Agent (complete evidence)

Automation Ready: true
Reality Check Ready: true
```

---

## AYA PLATFORM FACILITIES

### Core Systems
```
/Users/arthurdell/AYA/
├── Agent_Turbo/              ← Multi-agent orchestration
│   ├── core/
│   │   ├── claude_planner.py       ← Planning & auditing
│   │   ├── agent_orchestrator.py   ← Task delegation
│   │   ├── postgres_connector.py   ← Database access
│   │   └── lm_studio_client.py     ← Local LLM access
│   └── scripts/                    ← Automation tools
│
├── .github/workflows/        ← Execution engine (GitHub Actions)
│   ├── reality-check.yml           ← GLADIATOR validation
│   └── runner-smoke.yml            ← Runner health check
│
├── github-runners/           ← Self-hosted runner configs
│   ├── install-runner.sh           ← Runner deployment
│   └── launchd/                    ← Auto-start services
│
├── gladiator-workflows/      ← GLADIATOR automation scripts
│   └── reality_check_pipeline.py   ← Manual execution option
│
├── Databases/                ← Knowledge bases
├── services/                 ← Supporting services
└── projects/
    └── GLADIATOR/            ← Active project (Phase 0)
```

### Infrastructure Access

**ALPHA** (Mac Studio M3 Ultra):
```
Hostname: alpha.tail5f2bae.ts.net
RAM: 512GB
Storage: 4TB NVMe SSD
Docker: blue_combat (Blue Team training)
Purpose: Model fine-tuning, validation
Runner: alpha-m3-ultra (operational)
```

**BETA** (Mac Studio M3 Ultra):
```
Hostname: beta.tail5f2bae.ts.net
RAM: 512GB
Storage: 4TB + 16TB Thunderbolt (/Volumes/DATA/)
Docker: red_combat (Red Team generation)
LM Studio: Qwen3-14B @ 42.5 tok/s
Purpose: Attack pattern generation
Runner: beta-m3-ultra (operational)
Data Location: /Volumes/DATA/GLADIATOR/ (53GB, 34,155 patterns)
```

**Database** (PostgreSQL):
```
Host: localhost (ALPHA)
Database: aya_rag
Tables: 26 (agent_*, gladiator_*)
Purpose: Source of truth, state management, audit trail
```

---

## ACTIVE MISSION: GLADIATOR PHASE 0

**Current Phase**: Week 0 - Reality Check  
**Status**: Pending execution  
**Method**: GitHub Actions workflow

**Week 0 Day 1 Tasks** (From Database):
```
Task 1 [CRITICAL]: Generate Reality Check Dataset
├─ System: BETA
├─ Duration: 2-3 hours
├─ Status: pending
└─ Execute via: GitHub Actions workflow

Task 2 [CRITICAL]: Transfer Dataset to ALPHA
├─ System: BETA → ALPHA
├─ Duration: 30 minutes
├─ Status: pending
└─ Blocked by: Task 1

Task 3 [CRITICAL]: Split Dataset (900/100)
├─ System: ALPHA
├─ Duration: 30 minutes
├─ Status: pending
└─ Blocked by: Task 2

Task 4 [HIGH]: Foundation Model Baseline Test
Task 5 [CRITICAL]: Fine-Tuning Configuration
```

**Workflow Trigger**: https://github.com/arthurelgindell/AYA/actions/workflows/reality-check.yml

---

## EXECUTION PROTOCOL

### GitHub Actions Workflow (Primary Method)

**Trigger**:
1. Navigate to GitHub Actions
2. Select workflow
3. Set parameters
4. Execute

**Monitoring**:
- Real-time: GitHub UI
- Database: Query agent_sessions, agent_tasks
- Logs: Runner logs if debugging needed

**Updates**:
- Database updated automatically by workflow steps
- Documentation updated manually to maintain parity
- All updates require evidence

### Manual Execution (Fallback)

If GitHub Actions unavailable:
```bash
cd /Users/arthurdell/AYA/gladiator-workflows
python3 reality_check_pipeline.py
```

**Must still**:
- Log to database
- Provide evidence
- Update documentation
- Maintain parity

---

## AGENT ROLES

### Claude Code (Planner/Auditor)
```
Role: High-level planning, task delegation, result auditing
Initialize: ClaudePlanner()
Responsibilities:
├─ Create planning sessions in database
├─ Delegate tasks to execution agents (ALPHA/BETA)
├─ Audit results against expected outcomes
└─ Maintain complete audit trail

Never: Execute tasks directly (delegate to runners)
```

### ALPHA Runner (Blue Team Executor)
```
Role: Model training, validation, testing
Initialize: Query database for assigned tasks
Responsibilities:
├─ Fine-tune models
├─ Validate performance
├─ Log results to database
└─ Report completion with evidence

Labels: [self-hosted, macOS, arm64, alpha, studio]
```

### BETA Runner (Red Team Executor)
```
Role: Attack pattern generation, dataset creation
Initialize: Query database for assigned tasks
Responsibilities:
├─ Generate attack patterns
├─ Create training datasets
├─ Log results to database
└─ Report completion with evidence

Labels: [self-hosted, macOS, arm64, beta, studio]

CRITICAL PATH: /Volumes/DATA/GLADIATOR/ (actual data, 53GB)
NOT: /Users/arthurdell/GLADIATOR/ (GitHub repo only)
```

---

## DATABASE SCHEMA REFERENCE

### Key Tables

```sql
-- Current state (landing point for agents)
gladiator_project_state
├─ current_phase (Phase 0 Ready)
├─ total_attack_patterns_generated (34,155)
├─ metadata (GitHub, runners, strategy)
└─ is_current (true)

-- Active tasks (execution queue)
gladiator_execution_plan
├─ 17 tasks tracked
├─ Week 0: 13 tasks (5 pending)
└─ Weeks 1-7: Milestones

-- Completion log (evidence trail)
gladiator_task_completions
├─ task_id
├─ verification_evidence (required)
├─ prime_directive_verified (must be true)
└─ notes (audit trail)

-- Agent coordination
agent_sessions (workflow runs)
agent_tasks (jobs within workflows)
agent_actions (steps within jobs)
```

---

## DOCUMENTATION PARITY

**Primary Documents** (must match database state):
- `GLADIATOR_MISSION_BRIEFING.md` - Agent landing (this file)
- `projects/GLADIATOR/docs/GLADIATOR_MASTER_ARCHITECTURE_v2.4.md`
- `projects/GLADIATOR/docs/GLADIATOR_EXECUTION_PLAN_v2.3.md`

**Parity Check Protocol**:
```python
# After any database update
db_state = query_database()
doc_state = parse_documentation()

if db_state != doc_state:
    print("🔴 PARITY VIOLATION DETECTED")
    update_documentation_to_match_database()
    verify_parity()
```

---

## COMMUNICATION CHANNELS

**GitHub Actions**: https://github.com/arthurelgindell/AYA/actions  
**Database**: `psql aya_rag`  
**ALPHA**: Direct (local system)  
**BETA**: `ssh arthurdell@beta.local` (password: Power - CHANGE AFTER TESTS)  
**Logs**: `/Users/runner/actions-runner/runner.*.log`

---

## IMMEDIATE ACTION REQUIRED

**Next Task**: Execute Reality Check (Tasks 1-5)  
**Method**: GitHub Actions workflow  
**Owner**: Arthur (trigger from GitHub UI)  
**Monitor**: Claude Code (this agent)  
**Duration**: ~3-4 hours for Tasks 1-3

**Trigger URL**: https://github.com/arthurelgindell/AYA/actions/workflows/reality-check.yml

---

## VERIFICATION CHECKLIST FOR ANY AGENT

Before claiming task completion:

- [ ] Database state queried (not assumed)
- [ ] Evidence collected (files, logs, measurements)
- [ ] Success criteria met (verified with proof)
- [ ] Database updated with results
- [ ] Documentation updated to maintain parity
- [ ] Prime directive verified (no false claims)
- [ ] Would another agent be deceived? (NO)

**If ANY item fails**: Report FAILED, investigate, retry with evidence

---

**END OF INITIALIZATION LANDING**

**Agent**: Query database, load facilities, execute via workflows, maintain parity, enforce prime directives.

**Current State**: Ready for Reality Check execution  
**Database**: aya_rag (SOURCE OF TRUTH)  
**Execution**: GitHub Actions workflows  
**Evidence**: Required for all completions

---

**Initialize now. Query database. Execute with verification.**

