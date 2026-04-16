This is an excellent and very detailed project presentation for an Enhanced University Database Management System. Based on the slides you provided and the specific instructions for the presentation format, I have structured a complete presentation script.

The script follows the 6-part sequence you outlined (PL, UR, DD, SP, QA, RW/PL), stays within the 25-minute timeframe, and incorporates the duplicated slides logically. I have also anticipated the types of questions a lecturer might ask and provided sample Q&A.

---

### Presentation Script: Enhanced University Database Management System

**Total Time:** ~22-23 minutes (plus Q&A)

---

#### Part 1: Project Leader (PL) - Introduction & Team Roles
**(Speaker: Xavier Wong | Time: ~3 minutes | Slides 1-4)**

**(Slide 1: Title Slide)**
"Good morning/afternoon, everyone. I am Xavier Wong, the Project Leader for the Enhanced University Database Management System for Elite University. On behalf of my team, I’m proud to present our complete academic management solution."

**(Slide 2: Group Members and Roles)**
"Let me first introduce the team. This project is the result of dedicated collaboration across six specialized roles:
- **Kwok Ho Yin (DD)** designed our robust database schema.
- **Young Ho Tim Adam (DP)** developed and optimized our core SQL triggers.
- **Zhang Yun Tung (RW)** managed all technical documentation and our future roadmap.
- **Chak Hoi Kin (UR)** gathered user requirements and ensured our system meets real business needs.
- **Li Pui Kwan (QA)** designed and executed our comprehensive test strategy.
- And myself, **Xavier Wong (PL)** , handled project coordination, system architecture, and trigger integration."

**(Slide 3: Leadership & User Focus)**
"Our leadership structure ensured we maintained a clear focus on both user needs and technical excellence. Chak Hoi Kin as UR worked directly with stakeholders, while Li Pui Kwan in QA ensured every feature was rigorously tested."

**(Slide 4: Technical & Documentation Roles)**
"On the technical side, our Database Designer and SQL Programmer built a high-performance, normalized database. The Report Writer ensured our progress and future plans are clearly documented. Together, we have delivered a production-ready system."

*[Transition to UR]* "Now, our User Representative, Chak Hoi Kin, will walk you through the key user requirements that drove our design."

---

#### Part 2: User Representative (UR) - Key User Requirements
**(Speaker: Chak Hoi Kin | Time: ~5 minutes | Slides 5, 11, 13, 15, 16)**

**(Slide 5: Business Background)**
"Thank you, Xavier. Elite University, with over 8,000 students and 600 staff, faced serious challenges: fragmented spreadsheets, manual prerequisite checks, and no automated GPA tracking. Our primary goal was to eliminate manual errors, enforce academic integrity, and provide real-time visibility."

**(Slide 11: User Requirement 1 - Single Current Term Management)**
"The Registrar’s Office had a critical requirement: only ONE academic term can be marked as 'current' at any time. Why? To ensure all enrollments, fee assessments, and scholarship applications reference the correct semester. If a new term is activated, the previous current term must be automatically demoted. This prevents students from accidentally enrolling in the wrong semester."

**(Slide 13: User Requirement 2 - Prerequisite Validation)**
"Next, the Academic Advising Team demanded automated prerequisite validation. The system must prevent enrollment in courses like CS301 if the student hasn't completed CS201 with a minimum grade. As you can see in the example on the next slide..."

**(Slide 14: Prerequisite Error Message - *refer to slide 14*)**
"...the system returns a runtime error blocking the enrollment. This prevents student failure and eliminates hours of manual checking by advisors."

**(Slide 15: User Requirement 3 - Outstanding Fees Blocking Enrollment)**
"Finally, the Finance Department required that students with outstanding fees be blocked from enrolling. The process flow is simple: Student attempts to enroll, a trigger checks the `HasOutstandingFees` flag. If TRUE, enrollment is rejected. Only after the student pays and the flag is updated to FALSE can they enroll. This ensures financial compliance and improves cash flow."

**(Slide 16: Additional User Requirements)**
"Beyond these, we addressed requirements from Teachers (attendance, grade locking), the Scholarship Committee (GPA-based adjustments), and IT (complete, tamper-proof audit trails)."

*[Transition to DD]* "All these requirements shaped our data model. Our Database Designer, Kwok Ho Yin, will explain how."

---

#### Part 3: Database Designer (DD) - Data Model & ERD
**(Speaker: Kwok Ho Yin | Time: ~4 minutes | Slides 19, 20, 21)**

**(Slide 19: Database Design - 20 Interconnected Tables)**
"Thank you, Chak. To support these requirements, I designed a schema of 20 interconnected tables. The core tables handle academics: `AcademicTerm`, `Student`, `Teacher`, `Course`, `Section`, and `Enrollment`. Supporting tables like `CoursePrerequisite`, `Fee`, `Scholarship`, `Attendance`, and `AuditLog` enforce business rules. A key design decision was removing a denormalized `CurrentTermID` and adding `TermID` to `Fee` and `Scholarship` for better term-bound tracking."

**(Slide 20: Entity-Relationship Diagram - ERD)**
"This is our Entity-Relationship Diagram. You can see the relationships: A `Student` has many `Enrollments`. A `Course` can have many `Sections`, and a `Section` has many `Enrollments`. The `CoursePrerequisite` table creates a self-referential relationship on `Course`, which is critical for validation. `Fee` and `Scholarship` are now properly linked to `AcademicTerm`, not just the student, allowing term-specific financial tracking."

**(Slide 21: Normalization Analysis)**
"We achieved full BCNF compliance for all major tables, including `Student`, `Enrollment`, and `Fee`. However, for performance, we introduced three strategic denormalized columns: `Student.CumulativeGPA` to avoid recalculating all enrollments for every query, `Student.AttendancePercentage` for real-time tracking, and `Section.CurrentEnrollment` for quick capacity checking. This balances integrity with speed."

*[Transition to SP]* "With a solid data model in place, our SQL Programmer, Young Ho Tim Adam, implemented the business logic. He will now demonstrate how one key requirement is fulfilled with SQL."

---

#### Part 4: SQL Programmer (SP) - SQL Implementation
**(Speaker: Young Ho Tim Adam | Time: ~4 minutes | Slides 29)**

**(Slide 29: SQL - Prerequisite Validation Trigger)**
"Thank you, Kwok. I will walk you through the `enrollment_prerequisite_check` trigger, which fulfills the prerequisite validation requirement our UR discussed.

This is a `BEFORE INSERT` trigger on the `Enrollment` table. It only runs when a student attempts to enroll with a status of 'Enrolled'.

**The logic works in three steps:**

1.  **Find Prerequisites:** The trigger first finds all prerequisite courses for the target course by joining `Section` to `CoursePrerequisite` using the `SectionID` from the new enrollment record.

2.  **Check Completion:** For each prerequisite, it checks if an `Enrollment` record exists for this student where:
    - The status is 'Completed'.
    - The `CourseID` matches the prerequisite.
    - The student's `NumericGrade` is greater than or equal to the `MinGradeRequired` defined in `CoursePrerequisite`.

3.  **Block or Allow:** If the `NOT EXISTS` condition is true for *any* prerequisite (meaning the student hasn't successfully completed it), the trigger uses `RAISE(ABORT)` to block the insert and returns a clear 'Prerequisites not met' error message.

This ensures that no business logic error—whether from a manual entry or an application bug—can bypass our academic rules, because the enforcement happens directly inside the database."

*[Transition to QA]* "Of course, triggers are only useful if they work correctly. Our Quality Assurance lead, Li Pui Kwan, will now show you how we tested this."

---

#### Part 5: Quality Assurance (QA) - Test Validation
**(Speaker: Li Pui Kwan | Time: ~3 minutes | Slides 34, 36, 37)**

**(Slide 34: Quality Assurance - Complete Test Strategy)**
"Thank you, Tim. I used a four-phase testing approach: Cleanup, Setup, Action, and Verification. Our goal was to prove the system behaves exactly as expected."

**(Slide 36: Outstanding Fees Blocking Enrollment - Test Case)**
"Let me show you a concrete test for the 'Outstanding Fees' requirement our UR covered. In Part A, we set up 'Student Emma' with `HasOutstandingFees = TRUE` and a $200 library fee. When we attempted to insert an enrollment record, the system correctly raised a runtime error: 'Cannot enroll student with outstanding fees'—a **[PASS]** .

In Part B, we updated the fee status to 'Paid'. This action triggered a separate process that updated the `HasOutstandingFees` flag to 0. Then, when we tried the same enrollment insert, it succeeded. Another **[PASS]** ."

**(Slide 37: Additional Test Cases - Dynamic Scholarship Adjustment)**
"We also tested the dynamic scholarship adjustment. We started with a student at GPA 3.9: Status 'Active', Amount $3,000. When GPA dropped to 3.4, status correctly changed to 'Under Review', amount unchanged. When GPA improved to 4.0, status returned to 'Active', and the amount increased by 10% to $3,300. The system responded perfectly to GPA changes—**[PASS]** for all steps.

Our final results: **7 out of 7 test cases passed, a 100% pass rate.** This validates the correctness of our data model and triggers."

*[Transition to RW/PL]* "With validation complete, our Report Writer, Zhang Yun Tung, will discuss our future roadmap, and I will conclude."

---

#### Part 6: Report Writer (RW) & Project Leader (PL) - Future & Conclusion
**(Speakers: Zhang Yun Tung & Xavier Wong | Time: ~3 minutes | Slides 39, 40, 42)**

**(Slide 39: Future Development - High Priority (0-6 months))**
"Thank you, Li. This is Zhang Yun Tung, the Report Writer. Our immediate focus (0-6 months) is on integration and user experience: a web-based UI with React/Node.js dashboards, RESTful APIs for third-party tools, and email notifications for waitlist offers."

**(Slide 40: Future Development - Medium Priority (6-12 months))**
"In the medium term (6-12 months), we plan to implement predictive analytics using machine learning for student retention, course demand forecasting to optimize scheduling, and an automated degree audit for graduation progress tracking."

**(Slide 42: Project Summary - Key Achievements)**
"Back to Xavier for the conclusion."

"Thank you, Zhang. In summary, our team has successfully delivered:
- A schema of **20 interconnected tables** in BCNF.
- **7 core database triggers** enforcing critical business rules.
- A **complete, immutable audit infrastructure**.
- **8+ reporting views** for management.
- And **100% test pass rate** for all requirements.

The 'Enhanced University Database Management System' is production-ready. It implements all user requirements, uses a trigger-based design for consistent enforcement, and provides comprehensive audit trails for compliance."

**(Slide 43: Conclusion and Questions)**
"On behalf of the entire team—Kwok Ho Yin, Young Ho Tim Adam, Zhang Yun Tung, Chak Hoi Kin, Li Pui Kwan, and myself—thank you for your attention. We are now ready for your questions."

---

### Anticipated Lecturer Q&A (for PL to assign)

**Question 1:** *Your design uses denormalized columns like `CumulativeGPA` for performance. How do you guarantee this denormalized data remains perfectly accurate given all the possible operations (grade changes, course drops, retroactive withdrawals)?*

**Answer (by DD or DP):** "Excellent question. We guarantee accuracy through a network of `AFTER UPDATE` triggers on any table that can affect GPA. For example:
1.  When a grade in `Enrollment` changes, a trigger recalculates that student's GPA and updates the `Student` table.
2.  When an `Enrollment` status changes from 'Enrolled' to 'Dropped' after the drop deadline, a trigger recalculates.
3.  Crucially, the `GradeHistory` table logs every change, and if an administrator performs a retroactive withdrawal, a specific stored procedure is *required* to handle the complex recalculation, which then fires the same GPA update triggers. The denormalized column is a *cache*, not a source of truth. The `Enrollment` table remains the source."

**Question 2:** *The `AuditLog` table is described as tamper-proof. What specific SQL mechanism prevents someone with database admin rights from simply deleting rows from the `AuditLog` table itself?*

**Answer (by PL or DP):** "We implemented two layers of protection. First, as shown on slide 24, we have `BEFORE DELETE` and `BEFORE UPDATE` triggers on the `AuditLog` table itself that unconditionally raise `RAISE(ABORT, 'Cannot modify or delete audit records')`. This stops accidental or malicious DML. Second, for a DBA with `DROP TABLE` privileges, we rely on standard database security: we enforce the principle of least privilege. The application user account used by the system has no DDL privileges. A separate, highly-controlled admin role, logged and monitored separately, would be required for any structural change. In a production environment, we would also configure database-level audit features (like `sqlite3_audit` hooks or server-level logging) that run outside the SQL layer entirely."
