# 🤝 Contributing to the GCP ACE Master Playbook

Thank you for your interest in making this playbook the best possible resource for the Google Cloud Associate Cloud Engineer (ACE) community!

---

## 📌 Contribution Standards

Whether you are contributing notes for an upcoming phase, adding new questions, or fixing typos, please follow these guidelines.

### 1. Chapter Structure Standard (The 21-Section Rule)
All phase chapters in `phases/` must strictly adhere to the 21-section blueprint:
1. Definition (Clear, Plain English Definition)
2. Why does this service exist?
3. Architecture / Working (Internals, Lifecycle, ASCII diagrams)
4. Core Components (Table: Component, What it is, Why it exists, Analogy)
5. Key Features
6. Types / Variants
7. Configuration & Important Settings
8. Pricing & Cost Considerations
9. Security Considerations
10. Advantages
11. Limitations (When NOT to use)
12. Real-World Use Cases (10+ real-world examples)
13. Hands-on (Console + gcloud CLI with explanations of every flag)
14. ACE Exam Focus ⭐⭐⭐⭐⭐ (Traps, keywords, recurring patterns)
15. Service Comparisons (Direct markdown tables)
16. Best Practices
17. Common Mistakes
18. Quick Revision Sheet (Mnemonics, bulleted cheatsheet)
19. Interview Questions (Top 10 with clear answers)
20. ACE Practice Questions (10 MCQs + 5 MSQs with deep option breakdowns)
21. Related Topics (Prerequisites, next steps, exam domain mappings)

### 2. Practice Question Format
When submitting questions to `practice-questions/` or individual chapter sets, format them using this markdown template:

```markdown
### Question X [Topic Tag]
**Scenario / Question text**

- [ ] A. Option text
- [ ] B. Option text
- [ ] C. Option text
- [ ] D. Option text

<details>
<summary><b>View Answer & Detailed Explanation</b></summary>

**Correct Answer:** `[Option Letter]`

**Why it's correct:**
Detailed breakdown of why this option satisfies the requirements (cost-effective, minimal operational overhead, Google best practice).

**Why other options are incorrect:**
- **Option A:** Specific reason why this is sub-optimal or invalid.
- **Option B:** Specific reason why this is sub-optimal or invalid.
- **Option C:** Specific reason why this is sub-optimal or invalid.

**Key Exam Takeaway / Keyword:**
> `[One-line rule to remember for the exam]`
</details>
```

### 3. Question Categories
To keep the question bank organized:
- **Phase-specific drills:** Place in `practice-questions/by-phase/phase-XX.md`.
- **Full mocks (50 questions):** Chunk into `practice-questions/mock-exams/mock-exam-XX.md`.
- **Repeated exam questions / Top traps:** Add to `practice-questions/high-frequency-questions.md`.

---

## 🚀 Submitting Pull Requests

1. Fork the repo and create a feature branch (`git checkout -b feature/phase-02-iam` or `fix/typo-phase-00`).
2. Verify all markdown formatting and links render cleanly.
3. Submit a pull request with a descriptive title and summary of changes.
