# LabDesk – Manual Testing Project
 
Manual QA project created as an assignment for the QA hiring process at **Wizard Health**. It contains test cases, bug reports, and supporting evidence (screenshots and videos) for **LabDesk**, a web-based laboratory worklist and results application.
 
## Application Under Test
 
LabDesk is a lab information app where a lab technician can:
 
- browse a worklist of samples and search by patient name or barcode
- open a sample record (patient header, barcode panel, CBC panel)
- enter and edit results, blood type and comments
- save, approve, print, and return an approved sample to work
**Test environment:** Brave and Google Chrome on Windows 11  
**Testing period:** 21–26 August 2026
 
## Repository Structure
 
| Path | Description |
|------|-------------|
| [`test-cases.md`](test-cases.md) | 53 test cases with steps, expected/actual results, status, and links to related bugs |
| [`bug-reports.md`](bug-reports.md) | 19 bug reports with reproduction steps, severity/priority, and links to the test cases that found them |
| [`evidence/photos/`](evidence/photos) | Screenshots attached to test cases and bug reports |
| [`evidence/videos/`](evidence/videos) | Screen recordings of bugs that are hard to describe in text (state leaks, save/refresh issues) |
 
Test cases and bug reports are cross-referenced in both directions (e.g. `TC11` ↔ `BR02`), so every failed test case links to the bug it uncovered.
 
## Results at a Glance
 
**Test cases: 53**
 
| Status | Count |
|--------|-------|
| ✅ Passed | 21 |
| ❌ Failed | 31 |
| ⚠️ Passed with exception | 1 |
 
**Bug reports: 19**
 
| Priority | Count |
|----------|-------|
| High | 12 |
| Medium | 2 |
| Low | 5 |
 
## Test Coverage
 
- **UI and display:** worklist, patient header, barcode panel, CBC table structure
- **Input handling:** editable fields, non-numeric, negative and empty values
- **Business logic:** flag calculation (L/N/H) and boundary values, sex-specific reference ranges, age and date checks
- **Workflow:** save, approve, print, return to work, post-approval read-only behavior
- **Data integrity:** edit isolation between samples, persistence after refresh and navigation, unsaved edits
- **Concurrency:** multiple tabs and multiple browsers editing the same or different samples
- **Search:** by patient name and barcode number
## Key Findings
 
### Data integrity and state management (most critical)
 
- Edits made on one sample leak into another, including already approved samples (BR03)
- After refresh, saved edits appear on the first loaded sample instead of the one that was edited (BR14)
- Only the last edited field is retained when navigating between samples (BR15)
- RBC and Hemoglobin values swap after refresh (BR16)
- A second tab or browser does not pick up values saved elsewhere (BR17, BR18)
### Validation
 
- Non-numeric, negative, and empty values are accepted in Result fields, and such samples can be approved (BR01, BR10, BR12, BR19)
### Clinical data accuracy
 
- Flag is wrong for Glucose (switches to "H" at 30.0 instead of 6.1) (BR04)
- Female patient shows the "Adult male" reference range (BR08)
- Patient age is calculated incorrectly (BR06)
- Collected date is later than the current system date (BR05)
- Glucose appears twice in one panel (BR07)
- Barcode under the barcode image differs from the other barcode values (BR02)
### Workflow and UI
 
- Print shows outdated results instead of the approved ones (BR13)
- Status indicator dots do not update after approval (BR11)
- Save/Approve buttons stay fixed mid-page while scrolling (BR09)
## Test Case Format
 
Each test case includes: ID and summary, status, priority, severity, date, environment, connected bugs, related test cases, steps to reproduce, expected result, actual result, and evidence (screenshot or video where relevant).
 
## Bug Report Format
 
Each bug report includes: ID and summary, status, priority, severity, date, environment, connected test cases, related bugs, steps to reproduce, expected result, actual result, additional notes, and evidence.
 
## Tools
 
Markdown, GitHub, screen recording and screenshot tools, browser DevTools.
 
## Author
 
**Monika Pejkanović**  
📧 [pejkanovic9@gmail.com](mailto:pejkanovic9@gmail.com)
 