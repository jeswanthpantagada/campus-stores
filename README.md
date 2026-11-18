# Campus Stores

Campus Stores is a Product Engineer work sample set at a fictional campus media equipment desk. Work through a club’s screening, from equipment pickup to a partial return and the next morning’s handover. Open **Candidate brief** in the app for the assignment.

## Run the app

Open `index.html` in a modern browser. The app includes its scripts, styles, and icons and can run offline.

To serve it over local HTTP, run this from the repository folder and visit `http://localhost:8000`.

```sh
python3 -m http.server 8000 --bind 127.0.0.1
```

Each browser stores its own sample shift locally when browser storage is available. **Restart sample shift** restores the fictional starting records.

## Source

- `index.html` contains the desk layout, candidate brief, and bundled icons.
- `app.js` contains the sample records and interactions.
- `styles.css` defines the desktop and mobile layouts.

Icon license notices are in [third-party/lucide-LICENSE.txt](third-party/lucide-LICENSE.txt).


## Submit your work

1. [Fork Campus Stores](https://github.com/virinchi-healthcare/campus-stores/fork) into your GitHub account. Complete the candidate brief and push your changes to your public fork. In your README, point reviewers to your work and explain the decision you made.
2. In your fork, open **Commits**, select the final commit you want reviewed, and copy the page URL. It should look like `https://github.com/YOUR-USERNAME/campus-stores/commit/COMMIT-SHA`.
3. [Submit your work through the Google Form](https://docs.google.com/forms/d/e/1FAIpQLScvzoh1R4WS-SkiwAVBQbMvniieJYSoOEfLMyVo5CyjOvYUgQ/viewform). Your **roll number**, **email ID**, and **final commit link from your public fork** are required. Enter your roll number and email ID in the form only.
---

## Candidate Evaluation & Prototype

### 1. One moment the product handles well
**Enforcing inventory and approval guardrails during pickup (Friday · 3:45 pm).**
When issuing the Film Society kit (`RQ-214`) to Kabir Sen under time pressure, the **New issue slip** form automatically hides units that are in repair (`PR-06`) or already on loan (`MI-09`). If a desk worker accidentally selects an unapproved equipment type or tries to issue a second unit of an already-issued type, the app blocks the slip and explains why in the status bar. This prevents double-booking and over-issuing without slowing down the counter.

### 2. One moment I would change
**Partial returns and cross-shift handovers (Friday · 7:30 pm → Saturday · 9:00 am).**
When Kabir returns the projector (`PR-04`) and speaker (`SP-02`) but leaves the adapter (`AD-12`) with Leela Das for Saturday at 9 am, the **Loan register** only displays raw IDs (`L-404 · AD-12` and `RQ-214 · Kabir Sen · Due Fri 8 pm`). There is no way to attach Leela’s name or the updated return time directly to slip `L-404`. On Saturday morning, the next worker has to cross-reference four tabs (`Desk book`, `Requests`, `Inventory`, and `Loan register`) just to confirm that `AD-12` is the Film Society’s adapter and close the right slip.

### 3. Prototype & Walkthrough
**Changes made (`app.js`, `styles.css`):**
- **Contextual loan slips:** Each row in `Loan register` now displays the equipment type (e.g., `USB-C → HDMI adapter`) and club name (`Film Society`) alongside the asset and request IDs.
- **Inline handover notes & due-time updates:** Open slips now include a **Handover note** button. Saving a handover note attaches a highlighted callout directly to that loan slip, updates its due time, and automatically logs a matching entry in the `Desk book`.
- **Custody transfer ("Now with"):** Staff can update who currently holds an item (e.g., from `Kabir Sen` to `Leela Das`) right inside the handover form.
- **Undo return safeguard:** Returned slips include an **Undo** button so accidental clicks during a busy shift can be reversed safely.

**Walkthrough of the Film Society shift with the prototype:**
1. **Friday · 3:45 pm:** Issue `PR-04`, `SP-02`, and `AD-12` to Kabir Sen under `RQ-214`.
2. **Friday · 7:30 pm:** Click **Record return** on `PR-04` and `SP-02`. On the remaining `AD-12` slip, click **Handover note**, enter *"Returning adapter tomorrow at 9 am"*, change **Now with** to `Leela Das`, update the due time to `Sat 9 am`, and save.
3. **Saturday · 9:00 am:** When Leela walks in, the morning worker opens **Loan register**, immediately sees `AD-12 · USB-C → HDMI adapter · Film Society · Leela Das · Due Sat 9 am` with the highlighted handover note, and clicks **Record return** in one step.

<img width="799" height="439" alt="Screenshot 2026-11-19 at 12 38 58 AM" src="https://github.com/user-attachments/assets/00c357dd-17d4-40f8-92e2-f44641820e83" />



### 4. What I would keep
* **One slip per physical unit:** Tracking individual units (rather than a single bulk loan) is essential when kits come back in pieces.
* **Approval and availability validation:** Filtering out unavailable/repair units and blocking duplicate or unapproved equipment types.
* **Clean, offline-first simplicity:** Fast, zero-dependency interface with local persistence and a lightweight `Desk book` audit trail.

### 5. Real-counter observation to measure success
**Time and tab-switches required to close a partial return at shift change.**
At a real counter, observe how often morning-shift staff can close a handed-over return directly from the **Loan register** without flipping back to the `Desk book`, `Inventory`, or calling the previous shift worker to clarify who has which item.
