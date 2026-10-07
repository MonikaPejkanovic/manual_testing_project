# Bug Reports

| ID | Summary | Priority | Severity | Status |
|----|---------|----------|----------|--------|
| [BR01](#br01) | Non-numeric value can be entered in Result fields | Low | Medium | New |
| [BR02](#br02) | Barcode in patient header mismatches barcode on sample label | High | High | New |
| [BR03](#br03) | Field edits (Result fields, Blood type, Comment) are not isolated per patient, edits leak into other open samples | High | High | New |
| [BR04](#br04) | FLAG value incorrectly reflects Result vs Ref. Range | High | High | New |
| [BR05](#br05) | Collected date/time is later than current system date | Low | Medium | New |
| [BR06](#br06) | Age of patients are incorrectly displayed | Medium | High | New |
| [BR07](#br07) | Same analysis appears twice per panel | Medium | Medium | New |
| [BR08](#br08) | Ref. Range mismatches patient's Sex | High | High | New |
| [BR09](#br09) | Save and Approve buttons remain fixed in viewport and appear in the middle of the page when scrolling | Low | Low | New |
| [BR10](#br10) | Sample can be approved with empty field | High | High | New |
| [BR11](#br11) | Sample status doesn't change after approval | Low | Medium | New |
| [BR12](#br12) | Sample with non-numeric value in Result field can be approved | Low | Medium | New |
| [BR13](#br13) | Print button displays unchanged (invalid) results instead of the saved and approved edits | High | High | New |
| [BR14](#br14) | Field edit(s) are always displayed on the first sample loaded after Refresh, regardless of which sample was actually edited | High | High | New |
| [BR15](#br15) | Only the last edit is retained when navigating away and back without refresh (single or multiple fields edited) | High | High | New |
| [BR16](#br16) | RBC and Hemoglobin field values swap in Sample 2608-0142 after refresh when a field is edited and saved  | High | High | New |
| [BR17](#br17) | Tab 2 does not pull the latest saved value after refresh when the same sample is edited and saved in Tab 1 | High | High | New |
| [BR18](#br18) | Browser 2 does not pull the latest saved value after refresh when the same sample is edited and saved in Browser 1 | High | High | New |
| [BR19](#br19) | Sample can be approved when a field contains negative numeric value | High | High | New |

---

<a id="br01"></a>

## BR01: Non-numeric value can be entered in Result fields

| Status | Priority | Severity | Date | Environment |
|--------|----------|----------|------|-------------|
| New | Low | Medium | 26.08.2026 | Browser: Brave, Google Chrome · OS: Windows 11 |

**Connected test cases:** [TC08](test-cases.md#tc08)

**Steps to reproduce:**

1. Open sample record 2608-0140 (John Smith)
2. Click on a Sodium Result field
3. Enter "te12#$%"

**Expected result:**

Field rejects the non-numeric entry and shows an error message stating a numeric value is required.

**Actual result:**

"te12#$%" is entered in the field

**Screenshot:**

![BR01](evidence/photos/BR01.png)

---

<a id="br02"></a>

## BR02: Barcode in patient header mismatches barcode on sample label

| Status | Priority | Severity | Date | Environment |
|--------|----------|----------|------|-------------|
| New | High | High | 21.08.2026 | Browser: Brave, Google Chrome · OS: Windows 11 |

**Connected test cases:** [TC11](test-cases.md#tc11)

**Steps to reproduce:**

1. Open sample record 2608-0142 (Ana Marić)
2. Compare barcode value in worklist (2608-0142) with the number printed under the barcode image (top-right panel), and barcode in patient header and barcode on sample label

**Expected result:**

All 4 values are identical

**Actual result:**

Barcodes from worklist, sample label and patient header are identical but barcode number below barcode image are different

**Screenshot:**

![BR02](evidence/photos/BR02.png)

---

<a id="br03"></a>

## BR03: Field edits (Result fields, Blood type, Comment) are not isolated per patient, edits leak into other open samples

| Status | Priority | Severity | Date | Environment |
|--------|----------|----------|------|-------------|
| New | High | High | 22.08.2026 | Browser: Brave, Google Chrome · OS: Windows 11 |

**Connected test cases:** [TC12](test-cases.md#tc12), [TC13](test-cases.md#tc13), [TC14](test-cases.md#tc14), [TC33](test-cases.md#tc33), [TC35](test-cases.md#tc35), [TC37](test-cases.md#tc37), [TC44](test-cases.md#tc44), [TC48](test-cases.md#tc48)

**Related bugs:** [BR14](#br14), [BR15](#br15)

**Steps to reproduce:**

1. Open sample 2608-0140 (Sample B), check WBC and Comment fields
2. Open sample 2608-0142 (Sample A), edit WBC value, click Save
3. Open Sample B, check WBC value
4. Go back to Sample A, enter "Test" in Comment field, click Save, check Blood type field
5. Open Sample B, check WBC and Comment fields, select "B+" Blood type from drop down, click Save
6. Open Sample A, check Blood type field

**Expected result:**

WBC, Comment, and Blood type values remain fully isolated per sample, no edit made on one sample should ever appear on the other, regardless of field type or order of edits.

**Actual result:**

WBC edited on Sample A leaks into Sample B. Once Comment is edited on Sample A the leaked WBC value on Sample B reverts to its original value, but Comment on Sample B now shows Sample A's value ("Test") instead of its own. When "B+" is then selected and saved on Sample B, this value leaks into Sample A's Blood type field overwriting Sample A's own selection.

**Additional notes:**

- This behavior is confirmed in both direction (Sample B -&gt; Sample A).
- Leaked value only resets once a different field is edited and that is direct symptom of the Save issue, which only keeps the last field you edited, as covered in BR15
- This leak also affects Approved samples. Sample B does not need to be open in the worklist or modified directly. Editing a field on Sample A changes the value on Sample B, even if Sample B has status "Approved".
- The outcome is the same regardless of whether Save is clicked after the edit or not, the leak occurs even without clicking Save, indicating the values are shared in real time as they're entered, not only upon saving.

**Video:** [BR03.mp4](https://github.com/MonikaPejkanovic/manual_testing_project/raw/main/evidence/videos/BR03.mp4)

---

<a id="br04"></a>

## BR04: FLAG value incorrectly reflects Result vs Ref. Range

| Status | Priority | Severity | Date | Environment |
|--------|----------|----------|------|-------------|
| New | High | High | 21.08.2026 | Browser: Brave, Google Chrome · OS: Windows 11 |

**Connected test cases:** [TC15](test-cases.md#tc15), [TC16](test-cases.md#tc16)

**Steps to reproduce:**

1. Open sample record 2608-0142 (Ana Marić).
2. For each row, edit the Result value to test the following scenarios:
Enter a value below the Ref. Range.
Enter a value above the Ref. Range.
Enter a value within the Ref. Range.
3. Check the Flag column after each change and verify that the correct flag is displayed:
L = below the Ref. Range
H = above the Ref. Range
N = within the Ref. Range
4. Do the same for sample record 2608-0140 (John Smith)

**Expected result:**

Flag should be "L" when the Result is below the lower Ref. Range limit, "N" when the Result is within the Ref. Range, and "H" when the Result exceeds the upper Ref. Range limit.

**Actual result:**

Tested at all defined boundary points (1.0 and 0.1 below/above the minimum and maximum, and at the limits) for all analyses. Flags change correctly at the boundaries: "L" below the lower limit, "N" at/within the range, and "H" above the upper limit, except for the upper duplicated Glucose field in sample 2608-0142 (TC21/BR07) and the Glucose field in sample 2608-0140. For the Glucose field in both samples, "H" changes between 30.0 and 30.01 instead of at the defined upper limit of 6.1 (BR04).

**Screenshot:**

![BR04](evidence/photos/BR04.png)

---

<a id="br05"></a>

## BR05: Collected date/time is later than current system date

| Status | Priority | Severity | Date | Environment |
|--------|----------|----------|------|-------------|
| New | Low | Medium | 21.08.2026 | Browser: Brave, Google Chrome · OS: Windows 11 |

**Connected test cases:** [TC17](test-cases.md#tc17)

**Steps to reproduce:**

1. Open sample record 2608-0142 (Ana Marić)
2. Compare "Collected" value (21 Aug 2026 08:14) with the system date shown top-right (Thu 20 Aug 2026)

**Expected result:**

Collected date should not be later than the current system date

**Actual result:**

Collected date (21 Aug 2026) is one day after the current system date (20 Aug 2026)

**Screenshot:**

![BR05](evidence/photos/BR05.png)

---

<a id="br06"></a>

## BR06: Age of patients are incorrectly displayed

| Status | Priority | Severity | Date | Environment |
|--------|----------|----------|------|-------------|
| New | Medium | High | 21.08.2026 | Browser: Brave, Google Chrome · OS: Windows 11 |

**Connected test cases:** [TC18](test-cases.md#tc18)

**Steps to reproduce:**

1. Open sample record 2608-0142
2. Check Age
3. Check DOB
4. Check Date of collection

**Expected result:**

Age is displayed correctly

**Actual result:**

The patient’s age for Sample 2608-0142 is displayed incorrectly. Ana is 35 years old, but the system displays her age as 34.

**Screenshot:**

![BR06](evidence/photos/BR06.png)

---

<a id="br07"></a>

## BR07: Same analysis appears twice per panel

| Status | Priority | Severity | Date | Environment |
|--------|----------|----------|------|-------------|
| New | Medium | Medium | 21.08.2026 | Browser: Brave, Google Chrome · OS: Windows 11 |

**Connected test cases:** [TC21](test-cases.md#tc21)

**Steps to reproduce:**

1. Open sample record 2608-0142 (Ana Marić)
2. Review the ANALYSIS column for duplicate entries

**Expected result:**

Each analysis type should appear exactly once in the CBC panel

**Actual result:**

"Glucose" appears twice with different values (18.2 mmol/L and 5.1 mmol/l)

**Screenshot:**

![BR07](evidence/photos/BR07.png)

---

<a id="br08"></a>

## BR08: Ref. Range mismatches patient's Sex

| Status | Priority | Severity | Date | Environment |
|--------|----------|----------|------|-------------|
| New | High | High | 21.08.2026 | Browser: Brave, Google Chrome · OS: Windows 11 |

**Connected test cases:** [TC22](test-cases.md#tc22)

**Steps to reproduce:**

1. Open sample record 2608-0142 (Ana Marić), Sex: F
2. Check the Ref. Range sub-label for RBC and Hemoglobin rows

**Expected result:**

Sub-label should read "Adult female" with corresponding female reference values

**Actual result:**

Sub-label reads "Adult male" despite patient being female

**Screenshot:**

![BR08](evidence/photos/BR08.png)

---

<a id="br09"></a>

## BR09: Save and Approve buttons remain fixed in viewport and appear in the middle of the page when scrolling

| Status | Priority | Severity | Date | Environment |
|--------|----------|----------|------|-------------|
| New | Low | Low | 21.08.2026 | Browser: Brave, Google Chrome · OS: Windows 11 |

**Connected test cases:** [TC24](test-cases.md#tc24)

**Steps to reproduce:**

1. Open a record
2. Scroll down through the page.
3. Observe the position of the Save and Approve buttons.
4. Scroll back to the top of the page.
5. Observe the position of the buttons.

**Expected result:**

Save and Approve buttons should remain positioned according to the intended page layout and should not appear in the middle of the page content when scrolling.

**Actual result:**

Save and Approve buttons stay in the same position while scrolling, so they can appear in the middle of the page content. When scrolling back to the top, the buttons are still visible instead of being at the bottom of the page.

**Screenshot:**

![BR09](evidence/photos/BR09.png)

---

<a id="br10"></a>

## BR10: Sample can be approved with empty field

| Status | Priority | Severity | Date | Environment |
|--------|----------|----------|------|-------------|
| New | High | High | 21.08.2026 | Browser: Brave, Google Chrome · OS: Windows 11 |

**Connected test cases:** [TC26](test-cases.md#tc26)

**Steps to reproduce:**

1. Leave RBC field empty for sample 2608-0142
2. Click on Approve button

**Expected result:**

Sample isn't approved, and an error pop-up message is displayed.

**Actual result:**

Sample is approved, and a confirmation pop-up message is displayed.

**Screenshot:**

![BR10](evidence/photos/BR10.png)

---

<a id="br11"></a>

## BR11: Sample status doesn't change after approval

| Status | Priority | Severity | Date | Environment |
|--------|----------|----------|------|-------------|
| New | Low | Medium | 22.08.2026 | Browser: Brave, Google Chrome · OS: Windows 11 |

**Connected test cases:** [TC28](test-cases.md#tc28)

**Steps to reproduce:**

1. Click on the sample 2608-0140
2. Click the Approve button
3. Check the sample status in the upper-right corner of the patient information, verify the color of the dot next to the sample number in the worklist and color of the dot next to the name in Patient information header.

**Expected result:**

The status in the upper-right corner of the patient information is changed from New to Approved. The dot next to the sample number in the worklist changes from orange to green, and the dot next to the patient’s name also changes accordingly.

**Actual result:**

The status in the upper-right corner of the patient information is changed from New to Approved. However, the dot next to the sample number in the worklist does not change from orange to green, and the dot next to the patient’s name also remains unchanged.

**Screenshot:**

![BR11](evidence/photos/BR11.png)

---

<a id="br12"></a>

## BR12: Sample with non-numeric value in Result field can be approved

| Status | Priority | Severity | Date | Environment |
|--------|----------|----------|------|-------------|
| New | Low | Medium | 26.08.2026 | Browser: Brave, Google Chrome · OS: Windows 11 |

**Connected test cases:** [TC29](test-cases.md#tc29)

**Steps to reproduce:**

1. Click on the sample 2608-0142
2. Enter "te12#$%" in RBC field
3. Click "Approve" button

**Expected result:**

The sample isn't approved because field contains a non-numeric value. An appropriate error message is displayed indicating that the field accepts numeric values only.

**Actual result:**

The sample is approved. Confirmation message is displayed.

**Screenshot:**

![BR12](evidence/photos/BR12.png)

---

<a id="br13"></a>

## BR13: Print button displays unchanged (invalid) results instead of the saved and approved edits

| Status | Priority | Severity | Date | Environment |
|--------|----------|----------|------|-------------|
| New | High | High | 23.08.2026 | Browser: Brave, Google Chrome · OS: Windows 11 |

**Connected test cases:** [TC30](test-cases.md#tc30)

**Steps to reproduce:**

1. Click on the sample 2608-0142
2. Enter "1" in RBC field
3. Enter "22" in WBC field
4. Click on "Save" button
5. Click on "Approve" button
6. Click on "Print" button
7. Check information on pop up window

**Expected result:**

Pop up window is displayed with edited sample 2608-0142

**Actual result:**

Pop up window is displayed with sample 2608-0142 invalid results (shown results were valid before last edit, save and approve)

**Additional notes:**

- The same bug occurs even when the Save button is not clicked before clicking the Approve button.

**Video:** [BR13.mp4](https://github.com/MonikaPejkanovic/manual_testing_project/raw/main/evidence/videos/BR13.mp4)

---

<a id="br14"></a>

## BR14: Field edit(s) are always displayed on the first sample loaded after Refresh, regardless of which sample was actually edited

| Status | Priority | Severity | Date | Environment |
|--------|----------|----------|------|-------------|
| New | High | High | 25.08.2026 | Browser: Brave, Google Chrome · OS: Windows 11 |

**Connected test cases:** [TC32](test-cases.md#tc32), [TC34](test-cases.md#tc34), [TC36](test-cases.md#tc36), [TC45](test-cases.md#tc45), [TC46](test-cases.md#tc46), [TC47](test-cases.md#tc47), [TC52](test-cases.md#tc52)

**Related bugs:** [BR15](#br15), [BR03](#br03)

**Steps to reproduce:**

1. Open Sample 2608-0140
2. Enter "9" in the Potassium field
3. Click "Save"
4. Refresh the page

**Expected result:**

"9" is saved and displayed in the Potassium field for Sample 2608-0140

**Actual result:**

The edit is not tied to the sample it was made on. After refreshing the page, the edit (whether a single field was edited or multiple fields) is displayed on whichever sample loads first by default. In this case Sample 2608-0142 loads first after refresh, so "9" appears there instead of on Sample 2608-0140, where the edit was actually made.

**Additional notes:**

- This bug occurs whether a single field or multiple fields are edited, and also across sequential single-field edit/save cycles whether performed on one sample or across multiple samples.Bug occurs only if Save button was clicked before Refresh. After refresh, all edited fields appear together on the default/first-loaded sample (currently Sample 2608-0142), regardless of which sample they were actually entered and saved on.
- If Sample 2608-0142 (the sample that loads first after refresh) were the one actually edited, the edit would appear there correctly after refresh but only coincidentally, because it happens to be the default/first-loaded sample.
- Once a different sample is opened or another field is edited, none of the previously displayed fields are retained, not even the most recently edited one. This differs from the no-refresh scenario (see bug report ID:BR15), where the last edited field remains visible after navigating away and back.

**Video:** [BR14.mp4](https://github.com/MonikaPejkanovic/manual_testing_project/raw/main/evidence/videos/BR14.mp4)

---

<a id="br15"></a>

## BR15: Only the last edit is retained when navigating away and back without refresh (single or multiple fields edited)

| Status | Priority | Severity | Date | Environment |
|--------|----------|----------|------|-------------|
| New | High | High | 26.08.2026 | Browser: Brave, Google Chrome · OS: Windows 11 |

**Connected test cases:** [TC12](test-cases.md#tc12), [TC33](test-cases.md#tc33), [TC35](test-cases.md#tc35), [TC37](test-cases.md#tc37), [TC44](test-cases.md#tc44)

**Related bugs:** [BR14](#br14), [BR03](#br03)

**Steps to reproduce:**

1. Open Sample 2608-0140
2. Enter "1" in Glucose field
3. Enter "2" in Potassium field
4. Enter "3" in Sodium field
5. Click on "Save" button
6. Click on Sample 2608-0142
7. Go back to Sample 2608-0140

**Expected result:**

All three edits are successfully saved and displayed in the correct sample and field.

**Actual result:**

Only the last field edited (Sodium) is saved and displayed in Sample 2608-0140. The earlier edits made to Glucose and Potassium are lost, even though "Save" was clicked once after all three edits were entered.  
Additionally, the retained value (Sodium) is not isolated to Sample 2608-0140, it also appears in Sample 2608-0142

**Additional notes:**

- This bug describes behavior specific to the no-refresh scenario (only the last edited field persists). This leaked value remains present on both samples until another field is edited (whether on the same or a different field). If a different field is edited, the original field reverts to its previous value. This bug occurs even is Save button isn't clicked.
- This bug occurs whether a single field or multiple fields are edited, and also across sequential single-field edit/save cycles whether performed on one sample or across multiple samples, not only when multiple fields are edited before a single Save.
- A related bug occurs after refresh (see bug report ID:BR14) where all edited fields initially appear together on the default-loaded sample, then all disappear on further navigation.

**Video:** [BR15.mp4](https://github.com/MonikaPejkanovic/manual_testing_project/raw/main/evidence/videos/BR15.mp4)

---

<a id="br16"></a>

## BR16: RBC and Hemoglobin field values swap in Sample 2608-0142 after refresh when a field is edited and saved 

| Status | Priority | Severity | Date | Environment |
|--------|----------|----------|------|-------------|
| New | High | High | 26.08.2026 | Browser: Brave, Google Chrome · OS: Windows 11 |

**Connected test cases:** [TC39](test-cases.md#tc39)

**Steps to reproduce:**

1. Open Sample 2608-0142
2. Edit Sodium field, enter "130"
3. Click Save
4. Record results
5. Perform a hard refresh of the page (Ctrl+F5)
6. Wait for the page to fully load
7. Compare the result with the one recorded in step 4

**Expected result:**

Sodium value is successfully saved, and all other results remain unchanged.

**Actual result:**

Sodium value is saved but RBC field had a value of 5.61 and the Hemoglobin field had 16.8 before the refresh. After the refresh, the values swapped fields, RBC now shows 16.8, and Hemoglobin shows 5.61.

**Additional notes:**

- This bug occurs with both a hard refresh (Ctrl+F5) and a regular refresh (F5), RBC and Hemoglobin field values swap in both cases, as long as any field was edited and saved beforehand.
- The bug consistently occurs on Sample 2608-0142 even when the edit is made and saved on a different sample (2608-0140).

**Video:** [BR16.mp4](https://github.com/MonikaPejkanovic/manual_testing_project/raw/main/evidence/videos/BR16.mp4)

---

<a id="br17"></a>

## BR17: Tab 2 does not pull the latest saved value after refresh when the same sample is edited and saved in Tab 1

| Status | Priority | Severity | Date | Environment |
|--------|----------|----------|------|-------------|
| New | High | High | 25.08.2026 | Browser: Brave, Google Chrome · OS: Windows 11 |

**Connected test cases:** [TC42](test-cases.md#tc42), [TC46](test-cases.md#tc46)

**Steps to reproduce:**

1. Open the app in tab 1
2. Open a new tab (tab 2) and open the app in it
3. In both tab 1 and tab 2, open the same Sample 2608-0140
4. In Tab 1, edit a Result field (e.g., RBC), do not save yet
5. Switch to Tab 2 (without refreshing) and check if the field shows Tab 1's unsaved edit. record result
6. In Tab 1, save the edit
7. Switch to Tab 2 (without refreshing) and check if the field shows the saved value, record result
8. In tab 2, perform a hard refresh (Ctrl+F5)
9. Check if tab 2 now shows the updated value

**Expected result:**

After refresh, tab 2 shows the value saved in tab 1

**Actual result:**

After refresh, Tab 2 does not show the updated value saved in Tab 1, the field still displays the old value.

**Video:** [BR17.mp4](https://github.com/MonikaPejkanovic/manual_testing_project/raw/main/evidence/videos/BR17.mp4)

---

<a id="br18"></a>

## BR18: Browser 2 does not pull the latest saved value after refresh when the same sample is edited and saved in Browser 1

| Status | Priority | Severity | Date | Environment |
|--------|----------|----------|------|-------------|
| New | High | High | 25.08.2026 | Browser: Brave, Google Chrome · OS: Windows 11 |

**Connected test cases:** [TC43](test-cases.md#tc43), [TC47](test-cases.md#tc47)

**Steps to reproduce:**

1. Open the app in browser 1
2. Open the app in browser 2
3. In both browsers, open the same Sample 2608-0140
4. In Browser 1, edit a Result field (e.g., RBC), do not save
5. Switch to Browser 2 (without refreshing) and check if the field shows Browser 1's unsaved edit, record result
6. In Browser 1, save the edit
7. Switch to Browser 2 (without refreshing) and check if the field shows the saved value, record result
8. In browser 2, perform a hard refresh (Ctrl+F5)
9. Check if browser 2 now shows the updated value

**Expected result:**

After refresh, browser 2 shows the value saved in Tab 1

**Actual result:**

After refresh, Browser 2 does not show the updated value saved in Browser 1, the field still displays the old value.

**Video:** [BR18.mp4](https://github.com/MonikaPejkanovic/manual_testing_project/raw/main/evidence/videos/BR18.mp4)

---

<a id="br19"></a>

## BR19: Sample can be approved when a field contains negative numeric value

| Status | Priority | Severity | Date | Environment |
|--------|----------|----------|------|-------------|
| New | High | High | 26.08.2026 | Browser: Brave, Google Chrome · OS: Windows 11 |

**Connected test cases:** [TC53](test-cases.md#tc53)

**Steps to reproduce:**

1. Open sample record 2608-0140 (John Smith)
2. Click on Sodium Result field
3. Enter "-5"
4. Observe field value and Flag column
5. Click "Save" button
6. Refresh the page and verify persisted value
7. Click "Approve" button

**Expected result:**

Field should reject the negative value immediately with a validation error (numeric values must be ≥ 0), or if entered, Save/Approve should be blocked with an appropriate error message

**Actual result:**

"-5" is accepted in the field, sample is approved. Confirmation message is displayed.

**Screenshot:**

![BR19](evidence/photos/BR19.png)

---
