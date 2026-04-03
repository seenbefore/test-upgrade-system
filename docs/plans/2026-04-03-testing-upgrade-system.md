# Testing Upgrade System Implementation Plan

> **For Claude:** REQUIRED SUB-SKILL: Use superpowers:executing-plans to implement this plan task-by-task.

**Goal:** Build a lightweight starter kit that helps a Vue2 + Java8 full-stack engineer go from "only manual clicking" to "can write and maintain minimal high-value tests with Cursor support."

**Architecture:** This repository stays docs-first and example-driven. It contains three layers: operational templates for daily use, Cursor prompt assets for assisted maintenance, and one runnable Java8 example that demonstrates the smallest practical testing workflow before any real project integration.

**Tech Stack:** Markdown, PowerShell-friendly commands, Maven, Java 8, JUnit 5

---

## Model Portability Note

This plan was originally written in a stronger model environment, but it must also remain usable on a company computer with `Kimi K2.5`.

When executing this plan in that environment:

1. prefer fixed prompt templates over open-ended requests
2. give only the smallest necessary code context
3. ask for one output artifact at a time
4. use the companion assets in `prompts/kimi/`
5. follow `docs/guides/kimi-k2.5-company-computer-guide.md`

### Task 1: Create the repo entrypoint and map the system

**Files:**

- Create: `README.md`
- Modify: `docs/superpowers/specs/2026-04-03-testing-upgrade-design.md`
- Test: `README.md`

**Step 1: Write the README skeleton**

Add a top-level README with these sections:

```md
# Testing Upgrade System

## Who this is for
- Full-stack engineers who mainly rely on manual testing today

## What this repo contains
- Design spec
- Implementation plan
- Operational templates
- Cursor prompts
- Java8 runnable example

## Suggested usage order
1. Read the design spec
2. Use the learning guide
3. Start with the Java example
4. Reuse the templates in a real project
```

**Step 2: Verify the README has the expected sections**

Run: `rg "^## " "README.md"`

Expected: output includes `Who this is for`, `What this repo contains`, and `Suggested usage order`

**Step 3: Add a cross-link from the spec to the plan**

Append a short section near the end of `docs/superpowers/specs/2026-04-03-testing-upgrade-design.md`:

```md
## Related Implementation Plan

See `docs/plans/2026-04-03-testing-upgrade-system.md` for the execution plan that turns this design into concrete assets and examples.
```

**Step 4: Verify the cross-link exists**

Run: `rg "Related Implementation Plan|testing-upgrade-system.md" "docs/superpowers/specs/2026-04-03-testing-upgrade-design.md"`

Expected: output shows the new section and plan path

**Step 5: Commit**

```powershell
git add "README.md" "docs/superpowers/specs/2026-04-03-testing-upgrade-design.md" "docs/plans/2026-04-03-testing-upgrade-system.md"
git commit -m "docs: scaffold testing upgrade starter kit"
```

### Task 2: Add the operating templates for daily use

**Files:**

- Create: `docs/operations/risk-register.md`
- Create: `docs/operations/test-debt.md`
- Create: `docs/operations/release-checklist.md`
- Test: `docs/operations/risk-register.md`

**Step 1: Create the risk register template**

Add a markdown table with these exact columns:

```md
# Risk Register

| Area | Why risky | Last incident | What to verify next time | Suggested smallest test |
| --- | --- | --- | --- | --- |
| Example: order status transition | Frequent edits | Duplicate submit bug | Illegal status jump | Service-level unit test |
```

**Step 2: Create the test debt template**

Add a markdown table with these exact columns:

```md
# Test Debt

| Area | Missing test | Why it matters | Why not now | Next trigger to add it |
| --- | --- | --- | --- | --- |
| Example: payment callback | Retry idempotency case | Repeated incidents | Hotfix deadline | Next payment module change |
```

**Step 3: Create the release checklist template**

Add these sections:

```md
# Minimal Release Checklist

## Changed areas
## Highest-risk paths
## Must-run automated tests
## Must-run manual checks
## Rollback notes
```

**Step 4: Verify all three templates contain the required headings**

Run: `rg "^# |^## " "docs/operations"`

Expected: output shows all top-level and section headings for the three templates

**Step 5: Commit**

```powershell
git add "docs/operations/risk-register.md" "docs/operations/test-debt.md" "docs/operations/release-checklist.md"
git commit -m "docs: add daily testing operations templates"
```

### Task 3: Add Cursor prompt assets for the recurring workflows

**Files:**

- Create: `prompts/cursor/high-risk-change.md`
- Create: `prompts/cursor/bug-to-test.md`
- Create: `prompts/cursor/weekly-review.md`
- Test: `prompts/cursor/high-risk-change.md`

**Step 1: Create the high-risk change prompt**

Add a reusable prompt with these sections:

```md
# High-Risk Change Prompt

## Use when
- I am changing a backend area that often breaks

## Prompt
[Ask Cursor to identify the risk points, recommend 1-3 minimum tests, and list the smallest manual checks.]
```

The prompt body must explicitly ask Cursor to return:

1. changed risk points
2. one minimum automated test per risk point
3. a minimal pre-release check list

**Step 2: Create the bug-to-test prompt**

Add a reusable prompt with these sections:

```md
# Bug To Test Prompt

## Use when
- I fixed a production bug and need a regression test

## Prompt
[Ask Cursor to derive the smallest reproducible test from the bug description, existing code, and fix.]
```

The prompt body must explicitly ask Cursor to return:

1. the bug hypothesis
2. the minimum reproducible test
3. the smallest implementation-safe assertion

**Step 3: Create the weekly review prompt**

Add a reusable prompt with these sections:

```md
# Weekly Review Prompt

## Use when
- I want Cursor to summarize risk areas, untested changes, and next high-value tests
```

The prompt body must explicitly ask Cursor to update:

1. `docs/operations/risk-register.md`
2. `docs/operations/test-debt.md`
3. `docs/operations/release-checklist.md` only when a fresh release is being prepared

**Step 4: Verify the prompt files contain the expected trigger headings**

Run: `rg "^## Use when|^## Prompt" "prompts/cursor"`

Expected: output shows `Use when` and `Prompt` headings for the prompt assets

**Step 5: Commit**

```powershell
git add "prompts/cursor/high-risk-change.md" "prompts/cursor/bug-to-test.md" "prompts/cursor/weekly-review.md"
git commit -m "docs: add cursor prompt pack for test maintenance"
```

### Task 4: Build the first runnable Java8 example with TDD

**Files:**

- Create: `examples/java8-minimal-tests/pom.xml`
- Create: `examples/java8-minimal-tests/src/test/java/com/example/testing/OrderStatusServiceTest.java`
- Create: `examples/java8-minimal-tests/src/main/java/com/example/testing/OrderStatusService.java`
- Test: `examples/java8-minimal-tests/src/test/java/com/example/testing/OrderStatusServiceTest.java`

**Step 1: Write the failing test**

Create `OrderStatusServiceTest.java` with one focused test like this:

```java
package com.example.testing;

import org.junit.jupiter.api.Test;

import static org.junit.jupiter.api.Assertions.assertFalse;
import static org.junit.jupiter.api.Assertions.assertTrue;

class OrderStatusServiceTest {

    @Test
    void shouldAllowPendingOrderToBecomePaid() {
        OrderStatusService service = new OrderStatusService();
        assertTrue(service.canTransition("PENDING", "PAID"));
    }

    @Test
    void shouldRejectPaidOrderReturningToPending() {
        OrderStatusService service = new OrderStatusService();
        assertFalse(service.canTransition("PAID", "PENDING"));
    }
}
```

**Step 2: Add the smallest Maven setup and run the test to verify it fails**

Create `pom.xml` with Java 8 and JUnit 5 support, then run:

Run: `mvn -f "examples/java8-minimal-tests/pom.xml" -q -Dtest=OrderStatusServiceTest test`

Expected: FAIL with compilation error or symbol-not-found error because `OrderStatusService` does not exist yet

**Step 3: Write the minimal implementation**

Create `OrderStatusService.java` with only the smallest rules needed for the tests:

```java
package com.example.testing;

public class OrderStatusService {

    public boolean canTransition(String from, String to) {
        if ("PENDING".equals(from) && "PAID".equals(to)) {
            return true;
        }
        return !("PAID".equals(from) && "PENDING".equals(to));
    }
}
```

**Step 4: Run the test to verify it passes**

Run: `mvn -f "examples/java8-minimal-tests/pom.xml" -q -Dtest=OrderStatusServiceTest test`

Expected: PASS with 2 tests run and 0 failures

**Step 5: Commit**

```powershell
git add "examples/java8-minimal-tests/pom.xml" "examples/java8-minimal-tests/src/main/java/com/example/testing/OrderStatusService.java" "examples/java8-minimal-tests/src/test/java/com/example/testing/OrderStatusServiceTest.java"
git commit -m "feat: add first java8 minimal testing example"
```

### Task 5: Add the "bug fix to regression test" example

**Files:**

- Create: `examples/java8-minimal-tests/src/test/java/com/example/testing/OrderStatusServiceRegressionTest.java`
- Modify: `examples/java8-minimal-tests/src/main/java/com/example/testing/OrderStatusService.java`
- Create: `examples/java8-minimal-tests/README.md`
- Test: `examples/java8-minimal-tests/src/test/java/com/example/testing/OrderStatusServiceRegressionTest.java`

**Step 1: Write the failing regression test**

Create a regression-style test that models a realistic boundary case:

```java
package com.example.testing;

import org.junit.jupiter.api.Test;

import static org.junit.jupiter.api.Assertions.assertFalse;

class OrderStatusServiceRegressionTest {

    @Test
    void shouldRejectNullTargetStatus() {
        OrderStatusService service = new OrderStatusService();
        assertFalse(service.canTransition("PENDING", null));
    }
}
```

**Step 2: Run the regression test to verify it fails**

Run: `mvn -f "examples/java8-minimal-tests/pom.xml" -q -Dtest=OrderStatusServiceRegressionTest test`

Expected: FAIL because the current implementation does not explicitly guard the null target status

**Step 3: Implement the minimal fix and document the learning**

Update `OrderStatusService.java` to reject null or blank inputs before the existing transition logic.

Create `examples/java8-minimal-tests/README.md` with these sections:

```md
# Java8 Minimal Tests Example

## What this example teaches
## How to run the tests
## How to extend the example with a real bug
```

Also explain in the README that this second test demonstrates the rule:

`Fix a bug, then keep the bug from coming back with the smallest regression test.`

**Step 4: Run the focused regression test and then the full example test suite**

Run: `mvn -f "examples/java8-minimal-tests/pom.xml" -q -Dtest=OrderStatusServiceRegressionTest test`

Expected: PASS

Run: `mvn -f "examples/java8-minimal-tests/pom.xml" -q test`

Expected: PASS for the full example suite

**Step 5: Commit**

```powershell
git add "examples/java8-minimal-tests/src/main/java/com/example/testing/OrderStatusService.java" "examples/java8-minimal-tests/src/test/java/com/example/testing/OrderStatusServiceRegressionTest.java" "examples/java8-minimal-tests/README.md"
git commit -m "feat: add regression test example for bug fixes"
```

### Task 6: Write the guided learning path that connects docs, prompts, and code

**Files:**

- Create: `docs/guides/java8-testing-learning-path.md`
- Modify: `README.md`
- Test: `docs/guides/java8-testing-learning-path.md`

**Step 1: Write the learning guide**

Create a guide with these sections:

```md
# Java8 Testing Learning Path

## Week 1: Learn one passing unit test
## Week 2: Turn one bug into one regression test
## Week 3: Start using the risk register
## Week 4: Run a minimal pre-release checklist
## How to use Cursor in each step
```

Each week must contain:

1. one learning goal
2. one repository file to use
3. one smallest success signal
4. one Cursor prompt to run

**Step 2: Add a README link to the guide**

In `README.md`, add a short "Start here" section that links to:

1. the design spec
2. the implementation plan
3. `docs/guides/java8-testing-learning-path.md`
4. `examples/java8-minimal-tests/README.md`

**Step 3: Verify the guide contains all four week headings**

Run: `rg "^## Week " "docs/guides/java8-testing-learning-path.md"`

Expected: output includes `Week 1`, `Week 2`, `Week 3`, and `Week 4`

**Step 4: Verify the README start links exist**

Run: `rg "Start here|java8-testing-learning-path|java8-minimal-tests/README.md" "README.md"`

Expected: output shows the new onboarding links

**Step 5: Commit**

```powershell
git add "README.md" "docs/guides/java8-testing-learning-path.md" "examples/java8-minimal-tests/README.md"
git commit -m "docs: add guided learning path for java8 testing"
```

### Task 7: Add a repeatable weekly operating rhythm

**Files:**

- Create: `docs/guides/weekly-testing-rhythm.md`
- Modify: `README.md`
- Test: `docs/guides/weekly-testing-rhythm.md`

**Step 1: Write the weekly rhythm guide**

Create a guide with these sections:

```md
# Weekly Testing Rhythm

## Monday: identify this week's high-risk changes
## During development: add the smallest useful test
## After each bug fix: add the regression case
## Before release: run the minimum checklist
## Friday: update risk and debt lists with Cursor
```

For each section, include:

1. a 5-15 minute target duration
2. one expected artifact update
3. one prompt or command to use

**Step 2: Add the weekly rhythm link to the README**

Add one bullet under the onboarding area that links to `docs/guides/weekly-testing-rhythm.md`

**Step 3: Verify the guide contains all five operating checkpoints**

Run: `rg "^## " "docs/guides/weekly-testing-rhythm.md"`

Expected: output shows all five checkpoint headings

**Step 4: Verify the README now links to the weekly rhythm**

Run: `rg "weekly-testing-rhythm.md" "README.md"`

Expected: output shows the weekly rhythm link

**Step 5: Commit**

```powershell
git add "README.md" "docs/guides/weekly-testing-rhythm.md"
git commit -m "docs: add weekly operating rhythm for testing practice"
```

## Verification checkpoint after all tasks

Run these commands in order:

1. `mvn -f "examples/java8-minimal-tests/pom.xml" -q test`
2. `rg "^# |^## " "README.md" "docs" "prompts" "examples/java8-minimal-tests/README.md"`
3. `git status --short`

Expected results:

1. Maven tests pass
2. Markdown files expose the required headings and structure
3. Git status is clean after the final commit

## Execution handoff

Plan complete and saved to `docs/plans/2026-04-03-testing-upgrade-system.md`. Two execution options:

**1. Subagent-Driven (this session)** - I dispatch fresh subagent per task, review between tasks, fast iteration

**2. Parallel Session (separate)** - Open new session with executing-plans, batch execution with checkpoints

Which approach?