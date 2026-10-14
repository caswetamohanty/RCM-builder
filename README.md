# RCM-builder: Process Narrative to Risk & Control Matrix
Offline browser tool that turns a process narrative into a draft Risk and Control Matrix (RCM) with validation and Excel/Word/CSV export


**[Open the live tool](https://caswetamohanty.github.io/RCM-builder/RCM_builder.html)**. No install, no server, no sign-up. Everything runs in the browser and nothing is uploaded anywhere.

All files in Demo_data are **synthetic sample data**.

---

## 1. The problem

In audit and compliance engagements, building an RCM from a process narrative is slow, repetitive and inconsistent between reviewers. Two problems come up repeatedly:

- Generic AI tools **invent** risks, owners and frequencies that the narrative never stated.
- Gaps (no owner, no frequency, missing expected controls) get lost inside long tables.

## 2. The approach

Three steps, with a human reviewer in control throughout:

| Step | What happens |
|---|---|
| **1. Narrative in** | Paste text or upload `.docx`, `.xlsx`, `.txt`, `.csv`. A rule-based baseline drafts one row per control sentence and leaves anything the narrative does not state **blank or flagged**. |
| **2. Optional AI assist** | The tool builds a structured prompt (role, output structure, 7 rules such as *use only the narrative*, *flag missing owners*, *do not merge or split controls*). You run it in ChatGPT, Claude or Gemini and paste the reply back. The tool imports the table, skips look-alike duplicates, fills blank cells and keeps the AI's `FLAGS` line. |
| **3. Review and export** | Click-to-edit table. Red cells fail a format check, orange cells are gaps the narrative did not state. Export to `.xlsx` (with a review-notes sheet), `.docx` or `.csv`. |

### Design choices worth noting

- **Narrative-only rule.** A missing owner reads *"Owner not specified in narrative"* and a missing frequency reads *"Not specified"*. The tool never guesses.
- **Gaps are reported, not fabricated.** Expected-but-absent controls (for example segregation of duties) go into the `FLAGS` line instead of being added as controls.
- **Validation checks form, and says so.** It checks that Key/Non-Key is exactly one of the two values, that each row has exactly one of the eight approved assertions (Occurrence, Completeness, Accuracy, Cutoff, Existence, Valuation, Rights and Obligations, Presentation and Disclosure), that test procedures are not generic, and that there are no duplicate controls. Judgement on risk wording and Key status stays with the reviewer.
- **Privacy by design.** No network calls, nothing stored in the browser, a clear-everything button, and CSV/Excel exports prefix cells starting with `= + - @` so they cannot run as formulas.
- **No dependencies.** Plain HTML, CSS and JavaScript. The `.xlsx` and `.docx` files are written and read directly as zipped XML, with no libraries.

## 3. Demo: Fixed Assets process

The demo runs the tool on an 11-sentence Fixed Assets narrative (capex approval, vendor quotations, goods receipt, three-way match, capitalisation, depreciation, physical verification, disposals, sub-ledger to GL reconciliation).

**Result:** 9 controls (7 Key, 2 Non-Key), validation **9 of 9 rows pass**, with a `FLAGS` summary of owner gaps, frequency gaps and expected controls the narrative does not describe (for example no review of the capital vs revenue decision, no control over asset master changes).

### Try it in two minutes

1. Open the tool at https://caswetamohanty.github.io/RCM-builder/RCM_builder.html (or download RCM_builder.html and open it in any browser).
2. Upload `Demo_data/sample-narratives/Fixed_Assets_Narrative.docx` (or paste the `.txt`).
3. Click **Generate baseline RCM**. You get 8 controls, with owner and frequency gaps highlighted orange.
4. Click **Build AI prompt** and compare it with `Demo_data/sample-outputs/Fixed_Assets_AI_Prompt.txt`.
5. Paste `Demo_data/sample-outputs/Fixed_Assets_Sample_AI_Reply.md` into the reply box and click **Import AI rows**. Expected result: 1 new row, 8 recognised as existing rows and filled, validation **9 of 9 rows pass**.
6. Export Excel, Word or CSV and compare with `Demo_data/sample-outputs/`.

### Demo files

```
Demo_data/
├── sample-narratives/
│   ├── Fixed_Assets_Narrative.txt        # input (11 sentences, synthetic)
│   └── Fixed_Assets_Narrative.docx       # same text, to test the Word upload
├── sample-outputs/
│   ├── Fixed_Assets_RCM_baseline.csv     # step 1 output, before any AI
│   ├── Fixed_Assets_AI_Prompt.txt        # exact prompt the tool generates
│   ├── Fixed_Assets_Sample_AI_Reply.md   # illustrative AI reply (see note)
│   ├── Fixed_Assets_RCM.xlsx             # final export, with Review notes sheet
│   ├── Fixed_Assets_RCM.docx             # final export, landscape table
│   └── Fixed_Assets_RCM.csv              # final export
└── screenshots/                          # the four workflow screens
```

**Note on the sample AI reply.** It is an illustrative reply written by following the generated prompt's rules, not a recording of a specific chatbot session. Real AI output varies, which is why the review step and validation exist. After import, one reviewer edit was made in the table (owner "Head" corrected to "Plant Head"), as a reviewer would do.

## 4. Skills this project demonstrates

- Translating audit methodology (RCM structure, Key vs Non-Key, financial statement assertions, test procedure design) into a working tool
- Prompt design with explicit guardrails against hallucination and with gap flagging
- Control-minded software design: input validation, duplicate detection, formula-injection protection, data minimisation
- Delivering to non-technical users: one file, zero setup, Word and Excel outputs they already use

## 5. Known limitations

Listed here because they are real, and they are the roadmap:

- The step 1 baseline is keyword-based (approves, reviews, reconciles and similar). It can miss controls phrased differently, such as the system-calculated depreciation control in the demo, which only the AI step picked up.
- The owner detector uses a fixed list of job titles. It can mark an owner as unspecified when a title is not on the list (for example "Stores team"), or capture only part of a title ("Head" from "Plant Head"). The demo shows both being resolved in the AI import and review steps.
- Rows imported from an AI reply are appended at the end, so row order can differ from narrative order. The `Process Step Ref.` column keeps the link to the narrative.
- Validation checks form only. It cannot judge whether a risk is well worded or a control is truly Key.
- Generated output is a **draft for professional review**, not an audit conclusion.

## 6. Run or host it

- **Locally:** download `RCM_builder.html` and double-click it.
- **GitHub Pages:** https://caswetamohanty.github.io/RCM-builder/RCM_builder.html

## 7. About

Built by CA Sweta Mohanty | Risk, audit and compliance professional| www.linkedin.com/in/caswetamohanty | caswetamohanty@gmail.com
