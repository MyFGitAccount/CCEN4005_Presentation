Here is a **tight 4-minute script** for the SQL Programmer (Young Ho Tim Adam), focusing on the **prerequisite validation trigger** as the key example. The script is concise, technical, and designed to be delivered clearly within the time limit.

---

## SQL Programmer Script (4 Minutes)

**[Slide: Title - SQL Implementation: Prerequisite Validation Trigger]**

"Thank you, Kwok. I'm Young Ho Tim Adam, the SQL Programmer for this project.

In the next 4 minutes, I will demonstrate how we implemented one of our most critical business rules: **automated prerequisite validation** – directly inside the database using a trigger.

---

**[Slide: Business Rule - What We Need to Enforce]**

First, let me restate the rule our User Representative shared:

> A student CANNOT enroll in a course unless they have completed all prerequisites with the required minimum grade.

Examples:
- CS201 requires CS101 with ≥60%
- CS301 requires CS201 with ≥65%

If a student tries to enroll in CS301 without CS201, the system must **block the enrollment immediately** – no exceptions, no application-level workarounds.

---

**[Slide: The Solution - BEFORE INSERT Trigger]**

We solved this using a **`BEFORE INSERT` trigger** on the `Enrollment` table.

Why `BEFORE INSERT`? Because we want to validate the enrollment *before* any data is written to the database. If validation fails, the insert is aborted – leaving the database in a clean, consistent state.

Here is the actual SQL code.

---

**[Slide: Trigger Code - Line by Line]**

```sql
CREATE TRIGGER enrollment_prerequisite_check
BEFORE INSERT ON Enrollment
WHEN NEW.Status = 'Enrolled'
BEGIN
    SELECT CASE
        WHEN EXISTS (
            -- Find all prerequisites for the target course
            SELECT 1 FROM CoursePrerequisite cp
            WHERE cp.CourseID = (
                SELECT s.CourseID FROM Section s
                WHERE s.SectionID = NEW.SectionID
            )
            AND NOT EXISTS (
                -- Check if student completed this prerequisite
                SELECT 1 FROM Enrollment e
                JOIN Section s ON e.SectionID = s.SectionID
                WHERE e.StudentID = NEW.StudentID
                AND e.Status = 'Completed'
                AND s.CourseID = cp.PrerequisiteCourseID
                AND e.NumericGrade >= cp.MinGradeRequired
            )
        ) THEN RAISE(ABORT, 'Prerequisites not met')
    END;
END;
```

Let me explain the three logical steps.

---

**[Step 1 - Find Prerequisites]**

**Step 1:** The inner subquery finds the `CourseID` from the `Section` table using the `SectionID` from the new enrollment. Then it looks up all prerequisite courses for that `CourseID` in the `CoursePrerequisite` table.

In plain English: *"What courses does the student need to have completed before taking this class?"*

---

**[Step 2 - Verify Completion]**

**Step 2:** For each prerequisite found, the `NOT EXISTS` clause checks:
- Does the student have an `Enrollment` record?
- Is that enrollment status `'Completed'`?
- Does the `CourseID` match the prerequisite?
- Is the `NumericGrade` at least the required minimum?

If **all** prerequisites pass this check, `NOT EXISTS` returns false for each one, and the outer `EXISTS` becomes false – meaning validation passes.

If **any** prerequisite fails – meaning the student never completed it, or completed with a grade too low – then `NOT EXISTS` returns true for that prerequisite, the outer `EXISTS` becomes true, and we move to Step 3.

---

**[Step 3 - Block or Allow]**

**Step 3:** If the `EXISTS` condition is true, the trigger executes `RAISE(ABORT, 'Prerequisites not met')`.

This does two things:
1. It **cancels the INSERT operation entirely**.
2. It **returns a clear error message** to the application or user.

The enrollment never touches the database. No partial data. No inconsistency.

---

**[Slide: Why This Approach is Superior]**

Why did we choose a database trigger instead of handling this in application code?

**Three reasons:**

1. **Enforcement at the data layer** – No matter what application accesses the database – whether it's our official web portal, a mobile app, or a direct admin query – the rule is enforced. There is no way to bypass it.

2. **Atomic and consistent** – The check happens in the same transaction as the insert. There is no race condition where another process changes the student's grades between the check and the insert.

3. **Performance** – The entire validation runs inside the database engine. No round trips to an application server. No extra network latency.

---

**[Slide: Test Result - Proof of Correctness]**

How do we know it works? Our QA team tested this trigger extensively.

As you saw in the QA section, the test case was simple:
- **Part A:** Student without CS101 tries to enroll → **BLOCKED** with error message. ✅ PASS
- **Part B:** Same student completes CS101 with 70% → tries to enroll again → **ALLOWED**. ✅ PASS

This trigger passed all tests on the first attempt.

---

**[Slide: Summary]**

To summarize:

- We implemented a **`BEFORE INSERT` trigger** on the `Enrollment` table.
- It validates **all prerequisites** against the student's completed enrollments.
- If validation fails, the insert is **aborted with a clear error**.
- This ensures **100% enforcement** at the database level – no bypass possible.

This same pattern – using triggers to enforce business rules at the data layer – was applied to our other six triggers: fee blocking, teacher workload, GPA calculation, late fees, scholarship adjustment, and attendance tracking.

---

**[Slide: Transition]**

That completes my technical walkthrough. Now, our Quality Assurance lead, Li Pui Kwan, will show you how we validated this and all other triggers with a comprehensive test suite.

Thank you."

---

### Timing Breakdown (Total: 4 minutes)

| Section | Duration |
|---------|----------|
| Introduction | 15 seconds |
| Business rule reminder | 20 seconds |
| BEFORE INSERT concept | 20 seconds |
| Code walkthrough (line by line) | 60 seconds |
| Three steps explanation | 60 seconds |
| Why trigger > application code | 30 seconds |
| Test result reference | 15 seconds |
| Summary | 15 seconds |
| Transition to QA | 5 seconds |
| **Total** | **~4 minutes** |

---

### Delivery Tips for the SQL Programmer

1. **Speak clearly but not slowly** – You have dense technical content. Pause briefly after each logical step.

2. **Point to the code** – If presenting live, use a laser pointer or cursor to highlight the three main sections of the trigger: the `EXISTS`, the `NOT EXISTS`, and the `RAISE`.

3. **Emphasize key phrases** – "BEFORE INSERT", "RAISE ABORT", "no bypass possible" – these should be spoken with slightly more force.

4. **Don't read the code verbatim** – Explain what it *does*, don't just recite it. Your audience can read. Your job is to interpret.

5. **Practice the transition** – The last line should flow naturally into the QA presenter's introduction.
