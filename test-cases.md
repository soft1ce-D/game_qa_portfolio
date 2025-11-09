#  Test Cases of game 1 based on bugs from bug_reports.md
*Designed to prevent recurrence of verified defects (BUG-1 to BUG-8). Derived directly from root causes and repro steps in test build.*  
🔗 Evidence: [Google Drive](https://drive.google.com/drive/folders/1XOdYZ8Ud7ecojAKNLQFxVDB6H2femekn)

>  Focus: reproducible, high-impact issues only. Animation/UI polish bugs excluded per tester decision.

---

##  System: Quest Triggering Logic  
*Prevents BUG-1: unintended quest grant on `J` key press*

| ID | Test Case | Preconditions | Steps | Expected Result |
|----|-----------|---------------|-------|-----------------|
| TC-QUEST-001 | `J` opens journal — **no quest granted** | Any location, any time | 1. Press `J` 5× rapidly | Journal UI toggles; **no quest notifications**, no quest counter change |
| TC-QUEST-002 | Quest *"Silent Observer"* only via intended path | Day 1+, near Alice | 1. Talk to Alice → select *"Tell me about the observer"* | Quest granted **once**, with notification |
| TC-QUEST-003 | Duplicate quest prevention | Quest *"Silent Observer"* already active | 1. Press `J` repeatedly | No additional grants; log warning if attempted |

---

##  System: Multi-Cell Inventory Placement & State Integrity  
*Prevents BUG-4, BUG-5, BUG-6: cell overlap, ghost items, visual corruption, infinite duplication*

| ID | Test Case | Preconditions | Steps | Expected Result |
|----|-----------|---------------|-------|-----------------|
| TC-INV-001 | Horizontal placement of vertical-pref item in **bottom row** | Full inventory, 2×1 item (e.g. toolbox) | 1. Place item horizontally in bottom row<br>2. Drag & drop | Item remains horizontal; toolbar position unchanged; no empty “ghost” cells |
| TC-INV-002 | Overlap: item below **left anchor** | Full inventory, two 2×1 items | 1. Place Item A horizontally (row 2)<br>2. Place Item B so its **top-left cell** is directly under Item A’s left cell<br>3. Drag Item A | Placement blocked *or* Item B auto-shifts; **no overlap**, no item loss |
| TC-INV-003 | Overlap: item below **right anchor** | Same as TC-INV-002 | 1. Place Item B so its **top-right cell** is under Item A’s left cell<br>2. Drag Item A | Same as TC-INV-002 — no visual glitch, no cell corruption |
| TC-INV-004 | Post-bug recovery: visual integrity | After triggering BUG-5/6 state | 1. Close inventory<br>2. Reopen | All items visible; **no gray loading rectangles**, no texture corruption |
| TC-INV-005 | “Ghost cell” safety | After BUG-5 visual glitch | 1. Hover over gray/empty cell with tooltip<br>2. Click & drag | No item spawn; cell treated as *empty and inert* — **no infinite duplication** |
| TC-INV-006 | Storage round-trip resilience | After BUG-5 item corruption | 1. Store bugged item in chest<br>2. Retrieve it | Item restored cleanly; no visual artifacts, no state carryover |

>  Insight: Bugs 4–6 suggest **race condition** between visual snap and cell occupancy map update. Recommend code review of `InventoryGrid.PlaceItem()`.

---

## ⚙️ System: Item Durability & Buff Lifecycle  
*Prevents BUG-3: expired items remain functional*

| ID | Test Case | Preconditions | Steps | Expected Result |
|----|-----------|---------------|-------|-----------------|
| TC-DUR-001 | Buff removed at 0 durability | *Egg Vibrator* equipped, durability = 1/100 | 1. Wait until 0/100 | Vibration stops; `+0.5 Arousal/Horny` buff **removed** |
| TC-DUR-002 | Re-equip at 0 durability → no effect | *Egg Vibrator* in inventory, durability = 0/100 | 1. Equip item | No sound, no buff, no durability drain |
| TC-DUR-003 | Repair restores functionality | *Egg Vibrator*, 0/100 | 1. Repair to ≥1/100<br>2. Equip | Vibration + buff active; drains normally |

---

## 🗨️ System: Interaction Flow (UI & State)  
*Prevents BUG-2: *"Leave"* opens inventory instead of exiting*

| ID | Test Case | Preconditions | Steps | Expected Result |
|----|-----------|---------------|-------|-----------------|
| TC-INT-001 | *"Leave"* exits interaction (Alice’s boxes) | Day 1+, in Alice’s room | 1. Interact with boxes<br>2. Select *"Leave"* | UI closes; player resumes movement; **inventory does NOT open** |
| TC-INT-002 | Post-*Leave* state persistence | After TC-INT-001 | 1. Re-interact with same boxes | Boxes marked as *searched*; no duplicate dialogue; timer unchanged |
| TC-INT-003 | *"Leave"* safety under input spam | While *"Leave"* UI is open | 1. Press `Esc`, `J`, movement keys | UI stable; no crash / inventory pop-up / quest trigger |

>  Root cause hypothesis: `"Leave"` button bound to `OpenStorage()` instead of `CloseInteraction()`.

---

##  System: Collision Mesh Integrity  
*Prevents BUG-7 (door clipping) & BUG-8 (missing fence/chair collision)*

| ID | Test Case | Preconditions | Steps | Expected Result |
|----|-----------|---------------|-------|-----------------|
| TC-COLL-001 | Door–player collision during animation | Near any hinged door | 1. Stand in door’s swing arc<br>2. Open door | Door stops / pushes player — **no clipping through player model** |
| TC-COLL-002 | Fence collision after Day 2 spawn | After Day 2 chair dialogue (fence appears in basement) | 1. Walk into fence from any angle | Player **blocked**; collision mesh active |
| TC-COLL-003 | Chair collision persistence | Same as TC-COLL-002 | 1. Attempt to walk through chair | Player **blocked**; chair non-traversable |
| TC-COLL-004 | Collision persistence across save/load | After saving post-dialogue | 1. Reload game<br>2. Re-enter basement | Fence & chair collision **still active** |

>  Root cause hypothesis (BUG-8): collision meshes not enabled on spawned objects after dialogue event.

---

##  Regression Priority Matrix

| Priority | Test Cases | Rationale |
|----------|------------|-----------|
| **Critical** | TC-QUEST-001, TC-INV-002, TC-INV-003, TC-INV-005, TC-COLL-002, TC-COLL-004 | Prevent save corruption, progression blocks, exploits, infinite duplication |
| **High** | TC-INV-001, TC-INV-004, TC-INV-006, TC-DUR-001 | Prevent UI/data desync, balance breaks, visual corruption |
| **Medium** | TC-QUEST-002, TC-DUR-002, TC-INT-001, TC-COLL-001 | UX/immersion fixes |

---

>  QA Recommendation:  
> Add **“Stress Inventory Placement”** and **“Edge-Case Durability”** suites to **smoke test** for every pre-release build.
