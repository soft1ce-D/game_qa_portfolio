#  Verified Bug Reports  
## game 1 - DELUDED (adult 18+ rpg-survival)
**Tested on internal build (pre-release), Windows 10/11**  
 All videos & screenshots: [Google Drive Folder](https://drive.google.com/drive/folders/1XOdYZ8Ud7ecojAKNLQFxVDB6H2femekn?usp=sharing)  
*(Filenames match bug IDs: `BUG-5 video.mp4`, `BUG-5 screenshot.png`, etc.)*

>  Focus: Only **reproducible, gameplay-affecting bugs** are included. Animation/UI polish issues (e.g., clothing glitches, NPC idle poses) were intentionally excluded.

---

### [BUG-1] Quest *"Silent Observer"* granted on every `J` key press  
- **Priority**: Critical  
- **Repro Rate**: 10/10  
- **Steps to Reproduce**:  
  1. Press `J` anywhere in the game world  
- **Expected Result**:  
  Journal UI opens (no quest should be granted).  
- **Actual Result**:  
  Quest *"Silent Observer"* is granted on **every** `J` press — notification appears repeatedly, quest counter increments.  
- **Impact**:  
  Quest progression broken; potential save corruption due to duplicate quest instances.  )

---

### [BUG-2] *"Leave"* option in Alice’s boxes opens inventory instead of exiting interaction  
- **Priority**: Standard  
- **Preconditions**: Day 1+, free movement in house  
- **Steps to Reproduce**:  
  1. Go to Alice’s room (2nd floor)  
  2. Interact with cardboard boxes  
  3. Select *"Leave"* in dialogue  
- **Expected Result**:  
  Interaction ends; player resumes free movement.  
- **Actual Result**:  
  Box inventory opens — player can take items, modify storage. Timer does **not** advance.  
  On re-interaction, boxes are treated as *unsearched*.  
- **Impact**:  
  Progression logic broken; time-gating bypass possible.  

---

### [BUG-3] *Egg Vibrator* works with 0/100 durability  
- **Priority**: Standard  
- **Preconditions**: Day 1+, *Egg Vibrator* in inventory  
- **Steps to Reproduce**:  
  1. Equip item  
  2. Wait until durability = `0/100`  
- **Expected Result**:  
  Vibration sound stops; `+0.5 Arousal/Horny` buff is removed.  
- **Actual Result**:  
  Vibration continues; buff persists. Item also works *when re-equipped at 0 durability*.  
- **Impact**:  
  Balance broken — infinite arousal gain without resource cost.  


---

### [BUG-4] Inventory UI breaks when placing vertically-preferred item horizontally (bottom row)  
- **Priority**: Standard  
- **Preconditions**: Full inventory; multi-cell items present  
- **Steps to Reproduce**:  
  1. Place vertically-preferring item *horizontally* in bottom row  
  2. Drag & drop the item  
- **Expected Result**:  
  Item returns to horizontal position.  
- **Actual Result**:  
  - Item snaps to vertical, occupying extra row  
  - Toolbar shifts down; empty row appears  
  - Adjacent cell (should be occupied) is treated as *empty* — other items can be placed there  
- **Impact**:  
  UI inconsistency; potential for item overlap/corruption.  
- **Evidence**:  
   [BUG-4 video](https://drive.google.com/file/d/131YdyiIp4nF7AXov3hZLNquE_HLFZ_Yf/view?usp=sharing) |  [BUG-4 screenshot](https://drive.google.com/file/d/1pBgaf8pamJgQdxQsCYsdHKuVfFff3nwB/view?usp=drive_link)

---

### [BUG-5] Inventory corruption: item duplication, ghost cells, visual glitches  
- **Priority**: Critical  
- **Preconditions**: Full inventory; multi-cell items present  
- **Steps to Reproduce**:  
  1. Place vertically-preferring item *horizontally* (not in bottom row)  
  2. Place another item directly below its leftmost cell  
  3. Drag & drop the first item  
- **Actual Result**:  
  - Item snaps vertical → lower item **vanishes**  
  - After storing:  
    • Gray loading rectangles appear  
    • Random items change texture to gray  
    • "Empty" cells retain hidden item data → can be **duplicated infinitely**  
- **Impact**:  
  **Save corruption**, economy break, potential crash on load.  
- **Evidence**:  
   [BUG-5.1 video](https://drive.google.com/file/d/1HE03crRdf0YycAkd9i7gWPWS9XU-Hk0v/view?usp=sharing) | [BUG-5.2 video](https://drive.google.com/file/d/1-tLLZs55Xa707eMRRC5iafDKp_9iYiQt/view?usp=drive_link) |
---

### [BUG-6] Inventory cell collision failure when items overlap vertically  
- **Priority**: Critical  
- **Preconditions**: Full inventory; multi-cell items present  
- **Steps to Reproduce**:  
  1. Place vertically-preferring item *horizontally*  
  2. Place another item so its **top-right cell** aligns under the first item’s **top-left**  
  3. Drag & drop the first item  
- **Actual Result**:  
  - Item snaps vertical → overlaps lower item  
  - Overlapping cell becomes **non-interactive/ghost**  
  - Adjacent cell (should be occupied) is treated as *empty*  
- **Impact**:  
  Item loss, UI state desync, potential for exploit.  
- **Evidence**:  
   [BUG-6 video](https://drive.google.com/file/d/1zw-S-VicaN520oWMbRj55s3XjvU6hORe/view?usp=sharing) | [BUG-6 screenshot](https://drive.google.com/file/d/1R1HUr6rLfDgDNUE9PVvTWrnP9O9ObIEk/view?usp=drive_link)

---

### [BUG-7] Door model clips through player during animation  
- **Priority**: Standard  
- **Preconditions**: Day 1+, free movement in house  
- **Steps to Reproduce**:  
  1. Stand directly in front of a door  
  2. Press interact  
- **Expected Result**:  
  Door has collision with player during opening/closing animation.  
- **Actual Result**:  
  Door model passes **through** player model.  
- **Impact**:  
  Minor immersion break; no functional impact.  
- **Evidence**:  
   [BUG-7 video](https://drive.google.com/file/d/1R1HUr6rLfDgDNUE9PVvTWrnP9O9ObIEk/view?usp=drive_link)

---

### [BUG-8] Fence & chair in basement lack collision after Day 2 dialogue  
- **Priority**: Critical  
- **Preconditions**: Day 2, after first chair dialogue (fence spawned)  
- **Steps to Reproduce**:  
  1. Attempt to walk through fence/chair  
- **Expected Result**:  
  Fence has solid collision.  
- **Actual Result**:  
  Player walks through fence and chair freely.  
- **Impact**:  
  **Progression break** — player can bypass intended interaction/scene trigger.  


---

##  Summary  
| Priority    | Count | Bugs |
|-------------|-------|------|
| **Critical** | 4 | BUG-1, BUG-5, BUG-6, BUG-8 |
| **Standard** | 4 | BUG-2, BUG-3, BUG-4, BUG-7 |



---

##  QA Insight  
Inventory bugs (BUG-4/5/6) suggest a deeper issue in **multi-cell item placement logic** — likely a race condition between visual snap and cell occupancy update. A single fix here may resolve 3+ critical bugs.
