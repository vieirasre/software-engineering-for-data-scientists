## Simplicity

Good code should avoid repetition, unnecessary complexity, and unneeded lines of code.

The goal of simplicity is to make code easier to understand, change, and maintain while reducing opportunities for bugs.

### Don't Repeat Yourself (DRY)

Information should have a single representation in the codebase.

Duplicating the same knowledge or logic in multiple places creates additional work when requirements change, because the same change may need to be made in several locations.

Duplicated code also increases cognitive load. When two pieces of code look very similar but are not exactly the same, it can be difficult to understand whether they are intentionally different or whether they are doing the same thing.

#### Check

- [ ] Is the same logic repeated in multiple places?
- [ ] Is the same information represented more than once?
- [ ] Could repeated logic be extracted into a reusable function or component?
- [ ] Would changing one business rule require updating several parts of the code?

---

### Avoid Unnecessary Complexity

Prefer the simplest solution that clearly solves the problem.

Code should not introduce abstractions, structures, or logic that are not needed for the current problem.

Complexity increases the amount of information a developer needs to keep in mind while reading and modifying the code.

#### Check

- [ ] Is there a simpler way to express the same logic?
- [ ] Are there abstractions that are not providing real value?
- [ ] Is the code harder to understand than the problem itself?
- [ ] Am I solving requirements that do not exist yet?

---

### Avoid Verbose Code

Sometimes code can be simplified by reducing unnecessary lines.

Less code can mean:

- fewer opportunities for bugs;
- less code to read;
- less code to maintain;
- lower cognitive load.

The objective is not to write the fewest possible lines, but to avoid code that adds no useful information.

#### Check

- [ ] Are there unnecessary intermediate steps?
- [ ] Are there redundant conditions or operations?
- [ ] Can the same idea be expressed more clearly with less code?
- [ ] Would shortening this code improve clarity rather than make it cryptic?

---

### Revisit Code After the Urgency Has Passed

Code written under time pressure does not always need to remain in its original form.

After the immediate task is complete, revisit the code and clean it up for future use.

Writing good software is an iterative process. Once you know what to look for, you can deliberately improve the quality of your code over time.

#### Check

- [ ] Was this code written quickly to solve an urgent problem?
- [ ] Are there temporary shortcuts that should now be cleaned up?
- [ ] Is there duplicated or unnecessarily complex logic left behind?
- [ ] Would another developer understand this code months from now?
