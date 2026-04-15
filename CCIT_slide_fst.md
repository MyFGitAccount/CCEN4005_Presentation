# Presentation Speech for Enhanced University Database Management System

---

## SLIDE 1: Title Slide

"Good morning/afternoon everyone. Today, our team is proud to present the Enhanced University Database Management System for Elite University.

My name is Xavier Wong, and I'm the Project Leader. Let me introduce my teammates:

- Kwok Ho Yin is our Database Designer
- Young Ho Tim is our SQL Programmer
- Zhang Yun Tung is our Report Writer
- Chak Hoi Kin is our User Representative
- Li Pui Kwan is our Quality Assurance specialist

Together, we've built a complete database system to help Elite University manage their 8,000+ students, 600 staff members, and all the daily operations like enrollment, grades, fees, and scholarships.

Let me hand over to Chak Hoi Kin, our User Representative, to explain what the university needed from this system."

---

## SLIDE 2: Team Introduction - Leadership & User Focus

"Thank you. I'm Chak Hoi Kin, the User Representative.

My job was to talk to the actual people who would use this system every day - the registrars, teachers, finance staff, and students. I listened to their problems and figured out what the system absolutely must do.

Let me quickly introduce our team structure:

Xavier Wong, our Project Leader, kept everyone on track and made sure we delivered on time. He also built the core triggers that make the system work automatically.

I, as User Representative, gathered requirements and made sure what we built actually solved real problems.

Li Pui Kwan, our Quality Assurance person, designed all the tests to make sure everything works correctly.

Now let me pass to Zhang Yun Tung to introduce the rest of the team."

---

## SLIDE 3: Team Introduction - Technical & Documentation Roles

"Hi, I'm Zhang Yun Tung, the Report Writer.

I handled all the technical documentation and will talk about our future plans later.

Our Database Designer, Kwok Ho Yin, designed the entire database structure - all 20 tables and how they connect to each other. He made sure there's no duplicate or messy data.

Young Ho Tim, our SQL Programmer, wrote the triggers - those are automatic rules that check things like 'did this student pass the prerequisite?' before allowing enrollment.

And Li Pui Kwan made sure everything passed our tests.

Now back to Xavier to explain the business background."

---

## SLIDE 4: Business Background - Elite University

"Thanks. So what's the problem we're solving?

Elite University has over 8,000 students and runs three semesters every year - Fall, Spring, and Summer.

But before our system, everything was a mess. They used separate spreadsheets and old software that didn't talk to each other.

Here are the three biggest problems:

First, registrars had to manually check prerequisites. A student could sign up for an advanced course without ever taking the beginner class.

Second, there was no automatic GPA tracking. Someone had to calculate grades by hand.

Third, students with unpaid fees could still enroll in new courses. The finance department and registrar's office weren't connected.

We built one integrated system to fix all of this."

---

## SLIDE 5: Project Objectives and Success Criteria

"So what were our goals?

Number one: Stop manual data entry mistakes. Let the computer handle the checks automatically.

Number two: Enforce academic rules at the database level. That means the system literally won't let you break the rules - you can't enroll without prerequisites, you can't take too many classes, you can't register if you owe money.

Number three: Show real-time academic standing. Students and advisors can see GPA and attendance instantly.

Number four: Track everything with audit logs. Every change is recorded forever.

And number five: Make sure the system can grow as the university grows.

We measured success by: automated prerequisite checking, zero manual GPA calculation, real-time fee status, and a 100% test pass rate."

---

## SLIDE 6: Project Scope & Deliverables

"What's inside our project?

We built a complete database with 20 tables, 7 core business rule triggers, 8 reporting views, and full audit tracking.

We also created 7 comprehensive test cases and a configuration system where administrators can change settings like 'maximum credits per semester' without touching the code.

What's NOT included? That's for the future - a web interface, a mobile app, and payment gateway integration. Those are phase 2.

Our system is production-ready right now for the database backend."

---

## SLIDE 7: Technical Architecture Overview

"We built this using SQLite database with something called WAL mode - that just means it handles multiple users well.

The architecture has four layers:

Layer 1 - Storage: 20 organized tables
Layer 2 - Business Logic: Triggers that automatically enforce rules
Layer 3 - Reporting: Views that make data easy to read
Layer 4 - Audit: Immutable logs that cannot be changed or deleted

Now I'll pass back to Chak Hoi Kin to explain the user requirements in detail."

---

## SLIDE 8: Configurable System Settings

"Thanks Xavier. I'm back.

One thing the university loved is that they can change rules without hiring programmers.

For academics: They can set max sections per teacher (4), max credits per semester (18), minimum attendance needed (75%), and add/drop days (14).

For grades: They set probation threshold at 2.0 GPA, good standing at 2.5, and honors at 3.5.

For money: Late fee is 5%, scholarship requires 3.5 GPA, and grades lock after 30 days.

All of these can be changed anytime by administrators."

---

## SLIDE 9: SystemSettings Table - Complete Configuration

[Continue from previous - this slide shows the full table]

"As you can see, there are over 30 settings covering academics, finance, security, and system behavior. This makes the system flexible for years to come."

---

## SLIDE 10: User Requirement 1 - Single Current Term Management

"Now let me explain three key requirements starting with the registrar's biggest need.

The registrar said: 'We can only have ONE current term at any time. When Spring semester becomes current, Fall must automatically stop being current.'

Why does this matter? Because if two terms are both marked 'current', enrollment records get mixed up. Students might register for Fall classes when they meant Spring.

Our solution: A trigger that automatically demotes the old term when a new one is promoted. The registrar doesn't have to remember to do it - the system handles it."

---

## SLIDE 11: AcademicTerm Table - Term Management

[Show table]

"This is the AcademicTerm table. You can see each term has a start date, end date, add/drop deadline, and that important IsCurrent flag - which only ONE term can have set to 1."

---

## SLIDE 12: User Requirement 2 - Prerequisite Validation

"Second key requirement from the academic advisors:

They said: 'Stop students from signing up for advanced courses without passing the beginner courses first.'

For example: To take CS201, you need CS101 with at least 60%. To take CS301, you need CS201 with 65%. To take CS401, you need CS301 with 70%.

If a student tries to skip ahead, the system blocks them with a clear error message."

---

## SLIDE 13: Prerequisite Error Message - Example

[Show error message]

"This is what the student sees when they try to enroll without prerequisites. Notice the error says exactly what's wrong: 'Student has not met the prerequisites for this course.'

This saves advisors hours of manual checking every semester."

---

## SLIDE 14: User Requirement 3 - Outstanding Fees Blocking Enrollment

"Third key requirement - this one came from the finance department.

They said: 'Students who owe money should NOT be allowed to enroll in new classes until they pay.'

Here's how it works:

Step 1: Student tries to enroll
Step 2: System checks the HasOutstandingFees flag
Step 3: If flag is TRUE, enrollment is rejected
Step 4: Student pays fees
Step 5: System automatically updates flag to FALSE
Step 6: Student can now enroll

This is completely automatic. No one has to manually check payment status."

---

## SLIDE 15: Additional User Requirements by Stakeholder

"Other stakeholders had requirements too:

Teachers wanted: attendance tracking, assessment management with weights that add to 100%, automatic letter grade conversion, and grade locking after a set period.

The scholarship committee wanted: term-based scholarship tracking, GPA-based eligibility monitoring, and automatic amount increases for excellent students (like 10% for a 4.0 GPA).

IT and compliance wanted: complete audit logs that cannot be changed or deleted, soft-delete so history isn't lost, and configurable settings."

---

## SLIDE 16: Requirements Validation Matrix

"Here's our validation matrix showing every requirement and whether we met it:

Single Current Term - PASS
Prerequisite Validation - PASS
Outstanding Fees Blocking - PASS
GPA Calculation - PASS
Teacher Workload - PASS
Late Fee - PASS
Scholarship - PASS
Attendance - PASS
Audit Trail - PASS

Every single requirement passed our tests."

---

## SLIDE 17: Stakeholder Benefits Summary

"Let me summarize what each stakeholder gets:

Registrar: No more term conflicts, automated workflows
Academic Advising: Less manual checking, fewer student withdrawals
Finance: Better cash flow, automated late fees
Teachers: Saved time, automatic grade calculations
Students: Real-time transparency, fair enrollment access
IT: Complete audit trails for compliance

Now let me pass to Kwok Ho Yin, our Database Designer, to explain how we structured the data."

---

## SLIDE 18: Database Design - 20 Interconnected Tables

"Thank you. I'm Kwok Ho Yin, the Database Designer.

We built 20 tables that all work together. Let me group them for you:

Core academic tables: AcademicTerm (semesters), Department (organizational units), Student, Teacher, Course, Section (specific class offerings), and Enrollment (which student is in which section).

Supporting tables: CoursePrerequisite (rules like 'CS201 needs CS101'), Fee, Scholarship, Attendance, and AuditLog.

Key improvements in our Enhanced version: We removed a redundant 'CurrentTermID' from the Student table and added TermID to Fee and Scholarship tables so everything is properly linked to the right semester."

---

## SLIDE 19: Entity-Relationship Diagram (ERD)

[Show ERD diagram]

"This is our Entity-Relationship Diagram. It shows all 20 tables and how they connect.

You can see the Student table connects to Enrollment, which connects to Section, which connects to Course and Teacher and AcademicTerm.

Fee and Scholarship both connect to Student AND AcademicTerm - meaning every fee and scholarship is tied to a specific semester.

The diagram uses Crow's Foot notation - those little 'crow feet' symbols show relationships like 'one student can have many enrollments'."

---

## SLIDE 20: Normalization Analysis

"Let me explain normalization - that's just a fancy word for 'organizing data to avoid repetition and errors.'

All our main tables pass the highest standards - 1NF, 2NF, 3NF, and BCNF.

But we did add some 'denormalized' columns for performance - meaning we store some calculated values to make the system faster.

For example: Student.CumulativeGPA is stored even though we could calculate it from enrollments. Why? Because calculating it from scratch for 8,000 students every time would be too slow.

Similarly, Section.CurrentEnrollment is stored so we can quickly check if a class is full without counting enrollments every time.

This is a smart trade-off between perfect organization and actual speed."

---

## SLIDE 21: 7 Core Database Triggers

"Now let me introduce our 7 core triggers - these are the automatic rules that make the system smart.

1. academic_term_before_update - Makes sure only one term is current
2. enrollment_prerequisite_check - Blocks enrollment without prerequisites
3. teacher_after_section_insert - Limits teachers to 4 sections per semester
4. enrollment_fees_check - Blocks enrollment for students with unpaid fees
5. fee_auto_late_fee - Adds 5% late fee automatically
6. scholarship_dynamic_adjustment - Updates scholarships based on GPA
7. student_attendance_update - Recalculates attendance percentage

Each trigger fires automatically when someone tries to insert or update data."

---

## SLIDE 22: Sample Trigger - academic_term_before_update

[Show trigger code]

"Here's the actual code for the academic term trigger.

It's surprisingly simple. When someone tries to set a term as current, this trigger runs BEFORE the update happens.

It says: 'Before you make this term current, go find any other term that is current and set it to NOT current.'

That's it. One simple rule that prevents a huge problem.

Now let me pass to Young Ho Tim to show you more trigger code in action."

---

## SLIDE 23: Audit and Compliance Infrastructure

"Thanks Kwok. I'm Young Ho Tim, the SQL Programmer.

Let me explain our audit system. Every single change to important data is recorded in the AuditLog table.

The log captures: which table was changed, which record, what action (insert, update, or delete), the old value and new value, who made the change, and when.

And here's the key part: We added triggers that PREVENT anyone from modifying or deleting audit logs. Even administrators cannot erase the audit trail. This is what we call 'tamper-proof' - once a change is logged, it's logged forever."

---

## SLIDE 24: AuditLog Table - Complete Structure

[Show AuditLog table]

"This is what the AuditLog table looks like. You can see it records everything - TableName, RecordID, Action, OldValue, NewValue, TriggerName, ChangedBy, and ChangedAt.

This is crucial for compliance. If there's ever a dispute about who changed a grade or when a fee was paid, we have an unchangeable record."

---

## SLIDE 25: Reporting Views for Management

"We also built reporting views - these are like pre-made questions you can ask the database.

Operational views:
- UnifiedStudentProfile: See everything about a student in one place
- TeacherWorkloadReport: See which teachers are overloaded
- GraduationAudit: Track degree progress

Analytical views:
- StudentRiskAssessment: Identify at-risk students automatically
- WaitlistStatus: Monitor enrollment queues
- TopStudentsByGPA: Generate honor roll

Let me show you the StudentRiskAssessment view in action."

---

## SLIDE 26: StudentRiskAssessment View - Output Example

[Show output]

"This view automatically classifies students into risk levels.

Look at this example: A student with 1.9 GPA and 65% attendance is marked 'Critical Risk' with a recommendation for 'Immediate academic intervention.'

Another student with 2.3 GPA and 80% attendance is 'Moderate Risk' with 'Academic probation - mandatory advisor meeting.'

Advisors can run this report every week and reach out to struggling students before they fail."

---

## SLIDE 27: View Logic Example - UnifiedStudentProfile

[Show code]

"Here's the code for the UnifiedStudentProfile view. Even if you don't know SQL, you can see it's pulling information from multiple tables - Student, Department, and Enrollment - and combining them into one useful report.

The CASE statement creates a 'RegistrationStatus' column that says 'Hold' if the student owes money, or 'Clear' if they're good to register.

These views make it easy for non-technical staff to get answers without writing complicated queries."

---

## SLIDE 28: SQL - Prerequisite Validation Trigger

[Show trigger code]

"Now let me show you the actual prerequisite validation trigger.

It runs BEFORE a student enrolls. It finds all prerequisites for the course the student wants. Then it checks: 'Has this student completed each prerequisite with at least the minimum grade?'

If any prerequisite is missing, the trigger raises an error and stops the enrollment. The student never gets added to the class.

This is all automatic. No registrar has to manually check anything."

---

## SLIDE 29: SQL - GPA Calculation Trigger

[Show GPA trigger code]

"This is the GPA calculation trigger. It runs when a student completes a course.

The formula looks complicated but it's simple: New GPA equals (Old GPA times Old Credits) plus (New Grade Points times New Credits), all divided by Total Credits.

For example: If you had a 3.2 GPA over 30 credits, and you get an A- (3.7 grade points) in a 3-credit course, your new GPA is (3.2×30 + 3.7×3) ÷ 33 = 3.25.

The trigger calculates this automatically. No one does math by hand."

---

## SLIDE 30: SQL - Teacher Workload Management

[Show teacher workload trigger]

"This trigger enforces that no teacher gets overloaded.

When someone tries to assign a teacher to a new section, the trigger counts how many active sections that teacher already has in this semester.

If adding this section would exceed the limit (which is 4 by default), the trigger blocks it with an error: 'Teacher cannot teach more than 4 active sections.'

This protects both teachers and students. Teachers don't get burned out, and students get quality instruction."

---

## SLIDE 31: SQL - Auto-Late Fee Calculation

[Show late fee trigger]

"When a fee becomes overdue, this trigger automatically adds a late fee.

It reads the LATE_FEE_PERCENTAGE from SystemSettings - usually 5% - and calculates: Amount times 5 divided by 100.

For a $5,000 tuition fee, that's $250 automatically added.

No finance staff has to manually calculate late fees or chase students. The system does it instantly."

---

## SLIDE 32: SQL - Dynamic Scholarship Adjustment

[Show scholarship trigger]

"This trigger automatically adjusts scholarships based on student performance.

When a student's GPA changes, the trigger checks: If GPA drops below 3.5, status becomes 'Under Review.' If GPA is 3.9 or above, the scholarship amount increases by 5% or 10%.

For example, a $3,000 scholarship becomes $3,300 when a student achieves a 4.0 GPA.

This rewards excellence automatically and ensures scholarships only go to qualifying students."

---

## SLIDE 33: Quality Assurance - Complete Test Strategy

"Now let me pass to Li Pui Kwan, our Quality Assurance specialist, to explain our testing."

"Thank you Young. I'm Li Pui Kwan.

We used a four-phase testing approach:

Phase 1 - Cleanup: Delete any old test data
Phase 2 - Setup: Insert fresh controlled test data
Phase 3 - Action: Execute the operation we're testing
Phase 4 - Verification: Compare actual results to expected results

We wrote 7 complete test cases covering all the core triggers. Each test is a complete script that anyone can run to verify the system works."

---

## SLIDE 34: Test Coverage Summary

"Here's our test coverage:

- academic_term_before_update - PASS
- enrollment_prerequisite_check - PASS
- teacher_after_section_insert - PASS
- enrollment_fees_check - PASS
- fee_auto_late_fee - PASS
- scholarship_dynamic_adjustment - PASS
- student_attendance_update - PASS

Total: 7 tests, 7 passed, 0 failed. 100% pass rate.

Let me walk you through one test in detail."

---

## SLIDE 35: Outstanding Fees Blocking Enrollment - Test Results

"This is our test for outstanding fees blocking enrollment.

Part A: We set up a student with HasOutstandingFees = TRUE and an unpaid $200 library fee. When we tried to enroll them, the system correctly returned the error: 'Cannot enroll student with outstanding fees.' PASS.

Part B: We updated the fee to 'Paid.' The system automatically updated HasOutstandingFees to FALSE. Then we tried enrollment again - this time it succeeded. PASS.

This test proves the financial hold works exactly as designed."

---

## SLIDE 36: Additional Test Cases

"Let me summarize our other tests:

Prerequisite Validation: First attempt without CS101 was BLOCKED. After completing CS101, enrollment was ALLOWED. PASS.

Dynamic Scholarship Adjustment: Started with GPA 3.9 and $3,000 scholarship. GPA dropped to 3.4 - status became 'Under Review.' GPA improved to 4.0 - status returned to 'Active' and amount increased to $3,300. PASS."

---

## SLIDE 37: Attendance Percentage Update Test

"Our attendance test: Student started with 100% attendance. We recorded Present, Present, Absent. System calculated 2 out of 3 = 66.67%. PASS.

We also tested trying to record attendance for a future date. The system blocked it with error: 'Cannot insert attendance record with future date.' PASS.

This prevents teachers from adding fake attendance records."

---

## SLIDE 38: Future Development - High Priority (0-6 months)

"Now let me pass to Zhang Yun Tung to discuss future plans."

"Thank you Li. I'm Zhang Yun Tung.

In the next 0-6 months, we recommend three high-priority enhancements:

First: Payment gateway integration with Stripe or PayPal so students can pay fees online in real-time.

Second: A web-based user interface with dashboards for students, teachers, and administrators. Right now, users need to know SQL to use the system.

Third: Email notifications for waitlist offers so students know immediately when a spot opens.

These improvements would reduce manual data entry and enable self-service."

---

## SLIDE 39: Future Development - Medium Priority (6-12 months)

"In months 6-12, we suggest:

Predictive analytics using machine learning to identify students at risk of dropping out before they fail.

Course demand forecasting to help the registrar schedule the right number of sections.

Teacher evaluation trends to support faculty development.

Automated degree audit so students can see exactly what they need to graduate.

These would increase retention, reduce audit time, and enable data-driven decisions."

---

## SLIDE 40: Implementation Roadmap

"Here's our roadmap:

Phase 1 (0-6 months): Payment gateway, RESTful API, web UI

Phase 2 (6-12 months): Predictive analytics, automated degree audit

Phase 3 (12+ months): Mobile app, LMS integration for automatic grade transfer

This phased approach lets the university get value quickly while building toward a complete solution."

---

## SLIDE 41: Project Summary - Key Achievements

"Now let me pass back to Xavier Wong to conclude."

"Thank you Zhang. I'm back to wrap up.

Let me summarize what we achieved:

✅ 20 interconnected tables - a complete data model
✅ 7 core database triggers - automatic rule enforcement
✅ Complete audit infrastructure - tamper-proof logging
✅ 8+ reporting views - easy access to information
✅ 30+ configurable parameters - no coding required for changes
✅ Automated GPA calculation - no manual math
✅ Dynamic scholarship adjustment - rewards excellence automatically
✅ Automated late fees - 5% applied instantly
✅ Teacher workload management - prevents overload
✅ Real-time attendance tracking - instant visibility

Everything is tested and working."

---

## SLIDE 42: Conclusion and Questions

"In conclusion:

The Enhanced University Database Management System is production-ready right now.

We successfully implemented every user requirement. The trigger-based design ensures business rules are always enforced - the system literally will not let you break the rules.

Our audit infrastructure supports full compliance. And our 100% test pass rate validates correctness.

The system can evolve to support the university's growth for years to come.

Thank you for your attention. We're now ready for your questions."

---

## SLIDE 43: Thank You

[Final slide - Q&A]

"Any questions? Our team is happy to answer anything about the design, implementation, testing, or future plans."

---

# Quick Reference: Who Speaks for Which Slides

| Slide Range | Speaker |
|-------------|---------|
| 1 | Xavier Wong (PL) |
| 2 | Chak Hoi Kin (UR) |
| 3 | Zhang Yun Tung (RW) |
| 4-9 | Xavier Wong (PL) |
| 10-17 | Chak Hoi Kin (UR) |
| 18-22 | Kwok Ho Yin (DD) |
| 23-32 | Young Ho Tim (DP) |
| 33-37 | Li Pui Kwan (QA) |
| 38-40 | Zhang Yun Tung (RW) |
| 41-43 | Xavier Wong (PL) |
