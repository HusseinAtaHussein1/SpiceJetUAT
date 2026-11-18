# Automation Execution Report — Spicejet Ancillaries (Portal)

| Field | Value |
|---|---|
| Run | `ancillaries-run-020__2026-11-18_14-22-01__Targeted__Spicejet-FullSuite-Portal` |
| Feature / Supplier | Ancillaries / Spicejet (`spicejet`) |
| Credential | SpiceJetxxxx (staging — stated by Hussein 2026-11-18; the API_KEY pattern was not read by the assistant) |
| Environment | http://192.168.1.70:9500 (NDC1_Hussein (BRN1199) → AGN_HUS (AGN10546) → Hussein) |
| Execution | Headed, supplier-feature runner; Book / Hold pressed by the human in assisted groups |
| Executed | 2026-11-18 |
| Capabilities | Hold Supported · Pay after Hold Supported · Meal Supported · Baggage Supported · Seat Supported · Bundle Not Supported |
| Passengers | Generated test passengers (passengerEntry: manual), not saved profiles |
| Test data | DEL→DXB 19/12/2026; round trip 19/12 + 20/12; multi-city 19/12, 20/12, 21/12 |

## 1. Summary

| Status | Cases |
|---|---|
| Passed | 77 |
| Failed | 7 |
| Blocked | 8 |
| Inconclusive | 4 |
| Not Applicable | 5 |
| Not run | 6 |
| Excluded | 22 |
| **Total** | **129** |

Pass rate of cases that reached a verdict (Passed + Failed = 84): **92%**. Blocked (8), Inconclusive (4) and Not run (6) are **not** counted as passed. 25 group runs, retries included. Each case shows its latest execution; earlier attempts stay in the group logs.

## 2. Findings

- **Finding — needs a decision** — TC-SM-STA-004: the seat map shows the feature word "Overwing seat(s)" on seats 16A–F and 17A–F. It is not on the sheet's list of expected feature words (Leg space seat, Window seat, Aisle seat, Exit and emergency exit). Reproduced in runs 017, 019 and 020 (four attempts). A person must decide whether the application or the sheet is wrong. (TC-SM-STA-004 · Group 01)
- **Finding — needs a decision** — TC-PAH-004: after Pay And Book the booking and the passenger show "Booked", but the ticket number is NONE. (TC-PAH-004 · Group 17)
- **Observation** — TC-PAH-003 is Inconclusive: the price did not change between Hold and payment, so the price-change behaviour could not be tested. (Group 17)
- **Observation** — TC-SM-EXR-003 and TC-SM-EXR-004 are Inconclusive: no bookable "Exit and emergency exit" seat exists on the flight's map. (Group 05)
- **Finding — needs a decision** — TC-SM-LEG-003 and TC-SM-LEG-005: an adult holding an infant on leg space seat 2F, Book or Hold pressed by the human, ends with status "Failed" and the wallet unchanged. The case expects "In Progress" on Portal (supplier checklist §7c, 28 Oct 2026). Each case failed on both of its attempts on 192.168.1.70 (2 attempts each; the wallet was not charged). A person must decide whether the Portal changed or the recorded rule is out of date. (Groups 20, 22)
- **Finding — needs a decision** — TC-SM-CNF-001 (Group 14 rerun): the quote was 14,989.50 but the confirmation Total Amount is 14,989 — the 0.50 is dropped, beyond the 0.11 tolerance. The same pattern appears in TC-PAH-002 on run 020 (7,829.50 shown, 7,829 charged). A person must decide whether this rounding is a defect. (TC-SM-CNF-001 · Group 14)
- **Observation** — Group 14: every booking with the infant-holding adult on seat 2B or 2C ended Failed (runs 019 and 020); on 2A (PNR JBZ2HG) and 3A (PNR OGMT3R) it was Confirmed. Same pattern as the recorded seat 5F refusal. Inferred, not confirmed: the airline's refusal reason was not captured. The rerun used settings.local.json infantHolderAvoidSeats [5F, 2B, 2C]. (Group 14)
- **Observation** — TC-RPT-002 is Inconclusive after the Group 14 rerun: the Statement report updates hourly and had not refreshed since the booking — recheck it. (TC-RPT-002 · Group 14)
- **Finding — needs a decision** — Group 15 on Portal: the multi-city booking (DEL→BLR 17/12, BLR→DEL 18/12, DEL→DXB 19/12; 1 adult + 1 child + 1 infant) came back Failed at Book three times, with the infant-holding adult on different seats each time — 3B/6A/3A/2D, then 3B/6B/3B/3A, then aisle seats only 3D/6D/3D/3D. The same booking on Admin (run 019, seats 3A/5D/2A/2A) was Confirmed (PNR S8NTNP). No airline error was shown on the Portal confirmation, so the cause is not diagnosed; read the refusal reason for order AGN10546AGNNDCORD261118BEMJHC in Admin or PNR Query. This also weakens the 2B/2C seat inference recorded for Group 14. (TC-SM-MCT-004, TC-SM-MCT-005 · Group 15)
- **Finding — needs a decision** — TC-SM-MCT-007 (Group 10 rerun): seat maps follow flights correctly (4 maps, 8 meal boxes for 4 flights × 2 passengers), but 0 fare-package boxes are shown and the two flights of the connecting DEL→DXB trip show different seat prices. Spicejet has no Bundle (Not Supported), so the case's fare-package expectation does not apply; likely Not Applicable for this supplier rather than an application defect. (TC-SM-MCT-007 · Group 10)
- **Finding — needs a decision** — TC-SM-RND-007 (Group 11 rerun): SG614 (73HXCA) and SG141 (73HA01) are different aircraft with the same layout (189 seats, 32 rows, 3-3), so the runner reported the same map. The same trip's SG5060 (73HQSB) shows a different section layout; the runner compares only the first two flights. Likely a test-data / runner limitation, not an application defect. (TC-SM-RND-007 · Group 11)

## 3. What did not run, and why

- Group 14 (booked, with seat, meal and bag — 15 cases) was Blocked in the first pass (every booking Failed, infant-holder on 2C). Rerun 2026-11-18: attempt 1 stopped before Book (date picker timeout); attempt 2 Failed with the infant-holder on 2B; attempt 3 with the infant-holder on 3A: Confirmed, PNR OGMT3R — 13 Passed, 1 Failed (TC-SM-CNF-001), 1 Inconclusive (TC-RPT-002, report not refreshed yet).
- Multi-city cases (groups 10, 11) were rerun 2026-11-18 on DEL→BLR 17/12, BLR→DEL 18/12, DEL→DXB 19/12 and reached step 2: TC-SM-MCT-003 and TC-SM-MCT-006 Passed, TC-SM-MCT-007 and TC-SM-RND-007 Failed (see Findings). Group 15 on the same route reached Book four times on Portal and every booking came back Failed (latest order …PZD9QD, infant-holder on aisle seats only), so TC-SM-MCT-004/005 stay Blocked; the same booking on Admin (run 019) was Confirmed. Cause not diagnosed.
- TC-SM-CNF-016 and TC-SM-CNF-017 (group 16): no fare package offered that includes checked baggage.
- TC-SM-PRC-005 (group 12): the settings file has no non-EGP credential or ROE.
- TC-SM-EXR-005 (group 07): still Blocked after the 2026-11-18 rerun — timed out waiting for the "1- Adult" section.
- Groups 18 and 19 are not automated yet; groups 02 and 03 have no cases for this supplier.
- TC-SM-NEG-004, TC-SM-A11Y-001 and TC-SM-A11Y-002 have no result in any group log.
- TC-SM-PER-001 and TC-SM-CNF-018 (group 01): still Blocked after the 2026-11-18 rerun — after going back a step the runner times out waiting for the "1- Adult" section. Both passed on Admin (run 019); likely a Portal locator/timing issue in the runner, not confirmed.

## 4. Results

| Case | Title | Setup group | Status | Observed |
|---|---|---|---|---|
| TC-SM-NAV-001 | You can only pick seats on step 2 | 01 | Passed | Steps: '01 Passenger Info and Bundles', '02 Ancillaries', no third; Select Seat on step 1: no. |
| TC-SM-NAV-002 | You cannot reach the seats without choosing a fare package | 01 | Not Applicable | This supplier shows no fare package step. |
| TC-SM-NAV-002b | Missing passenger details and a missing fare package are both reported | 01 | Passed | First Next on an empty step 1: 25 "Required" messages, step 2 not reached. Second part not applicable: no fare package step. |
| TC-SM-NAV-003 | 'Select Seat' opens the seat map | 01 | Passed | Opened in 7.6 s, 100% of the window wide; title "Seat Map · DEL → BOM · Onward" — the first flight of the connecting trip DEL → DXB; left panel yes, chart 189 seats, colour key yes, bottom button yes. |
| TC-SM-NAV-005 | You can skip seats completely | 01 | Passed | With no seat chosen "Continue without seats" closed the map; nobody has a seat; amount 12,062 → 12,062. |
| TC-SM-RND-001 | The seat total matches the seats you can see | 01 | Passed | Left panel SEATS 189; 189 seats drawn. |
| TC-SM-RND-002 | Every seat has one clear state and one seat number | 01 | Passed | 171 available + 18 blocked + 0 occupied + 0 not offered + 0 held = 189 (panel 189); seat numbers unique; every row label matches. |
| TC-SM-RND-003 | Every seat has a full description | 01 | Passed | 189 of 189 descriptions complete (number, row, block, section, features, state). Observation: 57 featureless seats show an empty feature slot. |
| TC-SM-RND-005 | Cabin and section information agrees with the map | 01 | Passed | Colour key cabins: Economy; cabins on the map: Economy; sections Y-1, Y-2, Y-3 (seat labels: Y-1, Y-2, Y-3). |
| TC-SM-RND-006 | The flight shown on the map is the flight you chose | 01 | Passed | Map card SG475 · 73HXCA, departs Dec 19 17:20, arrives 19:40; booked flight departs Dec 19 17:20, arrives 19:40. Flight number not printed on the result card. |
| TC-SM-RND-007 | Two different flights give two different maps | 11 | Failed | SG614 DEL → BLR · Trip 1: 73HXCA, 189 seats, 32 rows, layout 3-3 / 3-3 / 3-3 \| SG141 BLR → DEL · Trip 2: 73HA01, 189 seats, 32 rows, layout 3-3 / 3-3 / 3-3 \| SG603 DEL → BOM · Trip 3 · Leg 1: 73HXCA, 189 seats, 32 rows, layout 3-3 / 3-3 / 3-3 \| SG5060 BOM → DXB · Trip 3 · Leg 2: 73HQSB, 189 seats, 32 rows, layout 3-3 / 3-3 / 3-3 / 3-3 / 3-3. Flights on different aircraft (73HXCA vs 73HA01) show the same map. Screenshot: `artifacts/failures/TC-SM-RND-007.png`. |
| TC-SM-RND-010 | Each flight in a trip gets its own map | 09 | Passed | 4 seat map(s): "Seat Map · DEL → BOM · Onward", "Seat Map · BOM → DXB · Onward", "Seat Map · DXB → BOM · Return", "Seat Map · BOM → DEL · Return"; aircraft SG475 · 73HXCA (189 seats) and SG5113 · 73HQSB (189 seats) and SG5023 · 73HQSB (189 seats) and SG670 · 73H189 (189 seats). 4 maps for 2 journeys: a connecting journey has one map per flight. Each titled with its own route and direction, each on its own flight. |
| TC-SM-STA-002 | A taken seat can be looked at but not booked | 01 | Passed | Clicked taken seat 1A: box says "Seat 1A is blocked."; button "Seat unavailable" greyed out; count 0/2 → 0/2. |
| TC-SM-STA-004 | Seat features use known wording | 01 | Failed | Feature words: Leg space seat 18, Window seat 63, Aisle seat 63, Exit and emergency exit 12, Overwing seat(s) 12 — NOT ON THE SHEET'S LIST: "Overwing seat(s)" on 16A, 16B, 16C, 16D, 16E, 16F, 17A, 17B, 17C, 17D, 17E, 17F. Screenshot: `artifacts/failures/TC-SM-STA-004__2026-11-18_13-35-01.png`. |
| TC-SM-STA-005 | Window and aisle labels match where the seat really is | 01 | Passed | Y-1 3-3: window AF, aisle CD; Y-2 3-3: window AF, aisle CD; Y-3 3-3: window AF, aisle CD — every label matches its position. |
| TC-SM-STA-006 | Emergency exit seats are marked | 01 | Passed | Exit seats: rows 16, 17 (12 seats, 12 also "Leg space seat"); offered to Rohan (holds the infant): none; offered to the child: none. |
| TC-SM-SEL-001 | One click only shows the seat - it does not book it | 01 | Passed | Clicking 2B only opened its box: count 0/2, Rohan's card still has no seat, bottom button "Continue without seats". |
| TC-SM-SEL-002 | A seat is only taken when you press the button in the box | 01 | Passed | Button "Book 2B for Rohan"; count 0/2 → 1/2; 2B now ", selected for Rohan Iyerrqnl"; Rohan's card "2B Economy EGP 50.5 ASSIGNED". |
| TC-SM-SEL-003 | The seat information box is correct | 01 | Passed | Box for 2B: "Row 2 2nd of 11 rows in Y-1", price EGP 50.5 shown, side "Left side", cabin shown — against "Seat 2B, row 2, Left side, Y-1, , available for EGP 50.5". |
| TC-SM-SEL-006 | The bottom button changes as seats are chosen | 01 | Passed | Bottom button: "Continue without seats" → "Confirm 1 of 2 seats" → "Confirm 2 seats"; "Clear all seats" beside it; counted out of 2 for 3 passengers (infant not counted). |
| TC-SM-SEL-007 | Moving a passenger to another seat frees the first seat | 01 | Passed | Rohan moved 2B → 3A ("Book 3A for Rohan"): 2B free again; Rohan holds 3A; count 2/2. |
| TC-SM-SEL-008 | 'Clear all seats' also removes the seat charge | 01 | Passed | Clear all: count 0/2; seats free again; bottom button "Continue without seats"; seat charge gone from the price. |
| TC-SM-SEL-009 | Confirming saves the seat and closes the map | 01 | Passed | Map closed; "Selected seats 1 traveller": Rohan Iyerrqnl / 2B / EGP 50.5; seat line in the price: yes; the opener now reads "Seat 1 more" (1 of 2 travellers seated). |
| TC-SM-SEL-010 | 'Change Seats' reopens the map with your choice still there | 01 | Passed | "Seat 1 more" reopened the map: 2B still held by Rohan Iyerrqnl; count 1/2; Rohan's card shows 2B. |
| TC-SM-SEL-012 | A seat held by another passenger in your booking is marked | 01 | Passed | Aanya clicked 2B: ", selected for Rohan Iyerrqnl"; book button not offered. |
| TC-SM-MPX-002 | The seat goes to the passenger who is selected | 01 | Passed | 2B → Rohan Iyerrqnl, 2C → Aanya Iyerrqnl; count 2/2. |
| TC-SM-MPX-004 | Two passengers cannot have the same seat | 01 | Passed | Aanya clicked 2B: ", selected for Rohan Iyerrqnl"; book button not offered. |
| TC-SM-MPX-006 | Seats on one flight do not affect the other flight | 09 | Passed | Journey 2 seat 2A chosen for Rohan; journey 1 still holds 2B → Rohan Reddykjuc, 2C → Aanya Reddykjuc. Step 2 shows 2 separate "Selected seats" summary block(s): Selected seats 2 traveller \\| Selected seats 1 traveller. |
| TC-SM-MPX-007 | Some passengers can have seats and others none | 01 | Passed | Confirmed with 1 of 2 chosen ("Confirm 1 of 2 seats"): allowed; summary lists Rohan Iyerrqnl 2B. |
| TC-SM-PRC-002 | A seat marked 'Chargeable seat' must always have a price | 01 | Passed | No seat carries the "Chargeable seat" tag on this supplier (accepted). Instead: 171 selectable seats, 22 free and 149 charged (25 to 512) under 'this fare'; every selectable seat shows a price. |
| TC-SM-PRC-003 | A paid seat is added as its own line in the price | 01 | Passed | Confirmed 3A at 154 under 'this fare': "Seat Selection +154" on its own line (Bundle Fare +0 separate); amount 13,523 → 13,677 (+154). |
| TC-SM-PRC-003b | One aircraft can have several seat prices at once | 01 | Passed | Prices for Rohan under 'this fare': EGP 512, EGP 154, EGP 50.5, Free, EGP 128, EGP 25, EGP 76.5; section headers Y-1 "Free – EGP 512", Y-2 "Free – EGP 154", Y-3 "Free – EGP 128" — each agrees with its seats. |
| TC-SM-PRC-004 | The price breakdown adds up | 01 | Passed | Base 8,006 + extras 1,615 (Seat Selection 154 + Meal Preference 148.5 + Extra Baggage 1,312.5 = 1,615) + tax 4,055.5 = 13,676.5; Total Amount 13,677; published 13,677. |
| TC-SM-GEN-003 | Gender comes from the title and cannot be edited | 01 | Passed | Gender box not editable; Mr → Male, Ms → Female, Mrs → Female; title back to Mr. |
| TC-SM-PER-001 | Seats survive going back a step | 01 | Blocked | Step "Back a step without editing" stopped: locator.waitFor: Timeout 15000ms exceeded. — waiting for getByRole('region', { name: ' 1- Adult' }).or(getByRole('tab', { name: /1- Adult/ })).first() to be visible Screenshot: `artifacts/failures/TC-SM-PER-001__2026-11-18_18-22-13.png`. |
| TC-SM-PER-002 | Seats, meals and bags all work together | 01 | Passed | Meal VGSW +148.5, bag Excess Baggage – 5 Kg +1,312.5, seat 3A +154: each on its own line. Changed the meal VGSW → VGML after the seat: seat kept at 154, total 13,677 → 13,528.5. |
| TC-SM-PER-004 | What happens when the time limit runs out | 13 | Not run | Skipped by human decision, 2026-10-19: needs the 60-minute time limit to run out (and a re-run at 15 minutes before go-live) — a separate scheduled check. |
| TC-SM-PER-007 | The seat appears on the payment summary | 01 | Passed | Payment summary: Net Payable 12,266.5 = published 12,266.5; seats listed: Rohan Iyerrqnl 2B EGP 50.5, Aanya Iyerrqnl 2C EGP 154; seat line +204.5 for 204.5 of seats. Book was not pressed. |
| TC-SM-NEG-003 | Clicking quickly does not book two seats | 01 | Passed | Pressed "Book 3A for Rohan" 5 times quickly: count 1/2; Rohan holds 3A. |
| TC-SM-NEG-004 | Two agents choosing the same seat at the same time | – | Not run | Not approved at Review — excluded by the Review gate; never executed. |
| TC-SM-NEG-006 | Opening the booking page with a broken or missing link | 13 | Passed | garbled token → "No Data Found · Go Back · Go Home"; no token → Dashboard; empty token → Dashboard. Access refused every time — no seat map, no prices, no technical error. The case expects Flight Search; Dashboard / "No Data Found" accepted by human ruling, 2026-10-19. An expired token was not tried (it needs a link over 60 minutes old). |
| TC-SM-A11Y-001 | The seat map works on a smaller screen | – | Not run | No result recorded in any group log: no executed group produced a result for this case. |
| TC-SM-A11Y-002 | Seat states can be read by screen readers | – | Not run | No result recorded in any group log: no executed group produced a result for this case. |
| TC-SM-PAX-001 | Infants get no seat and no extras; children get both | 01 | Passed | Step 2 extras for Expand All RI Rohan Iyerrqnl and AI Aanya Iyerrqnl only — infant Kabir Iyerrqnl not listed; seats counted out of 2 for 3 passengers; child given 2C; summary "2 travellers". |
| TC-SM-PAX-003 | An infant must be linked to an adult | 01 | Passed | "Assigned to" on adult / child / infant: 0 / 0 / 1; it lists Rohan Iyerrqnl. |
| TC-SM-PAX-004 | The map moves on to the next passenger automatically | 01 | Passed | After Rohan's seat the next click offered "Book 2C for Aanya"; cards: Rohan ASSIGNED, Aanya ACTIVE. |
| TC-SM-PAX-006 | Each passenger type is priced separately | 01 | Passed | Base fare Adult 3,378, Child 3,378, Infant 1,250; tax Adult 1,524, Child 1,524, Infant 1,007.5; infant baggage shows the search allowance (the package carries no baggage data): carry-on "1 additional piece up to 7 kg. Dimensions: 55 x 35 x 25 cm (total 115 cm).", check-in "0 KG Mumbai → Dubai". |
| TC-SM-PAX-007 | Closed passenger sections still report missing information | 01 | Passed | Collapse All → 0 open, Expand All → 3 open; opening Adult, Child, Infant one by one leaves 1, 2, 3 open — sections stay open once opened; the sheet says one at a time, accepted as the app's behaviour (decision 2026-10-26). |
| TC-SM-FRC-001 | A changed price is shown before the booking screen | 01 | Not Applicable | No Price Update pop-up appeared on this run: the airline did not change the fare for this search, so there is nothing to test. The pop-up does appear for this supplier when the fare changes (seen on the DEL-DXB round trip, 2026-10-27). |
| TC-SM-FRC-002 | Accepting the new price carries it forward | 01 | Not Applicable | No Price Update pop-up appeared on this run: the airline did not change the fare for this search, so there is nothing to test. The pop-up does appear for this supplier when the fare changes (seen on the DEL-DXB round trip, 2026-10-27). |
| TC-SM-FRC-003 | Cancelling leaves nothing behind | 01 | Not Applicable | No Price Update pop-up appeared on this run: the airline did not change the fare for this search, so there is nothing to test. The pop-up does appear for this supplier when the fare changes (seen on the DEL-DXB round trip, 2026-10-27). |
| TC-SM-FRC-005 | Every later total uses the accepted price | 01 | Not Applicable | No Price Update pop-up appeared on this run: the airline did not change the fare for this search, so there is nothing to test. The pop-up does appear for this supplier when the fare changes (seen on the DEL-DXB round trip, 2026-10-27). |
| TC-SM-ERR-001 | A server failure must not look like 'No Flights Found' | 01 | Passed | Search responses 200; 3 result cards shown; no server error this time. |
| TC-SM-ERR-002 | The session ending in the middle of a booking | 13 | Not run | Skipped by human decision, 2026-10-19: needs ~10–12 minutes idle until the session expires, which also ends the human's login. |
| TC-SM-ERR-004 | Typing in one dropdown must not affect another | 01 | Passed | First nationality list filtered to 0 option(s) ("ind"); the second opened with 3. |
| TC-SM-ERR-005 | Each offer says which airline connection it came from | 01 | Passed | 3 of 3 cards name their connection. |
| TC-SM-EXR-001 | An adult travelling alone CAN take an exit seat | 04 | Passed | Exit seat 16A: before "Overwing seat(s), available for EGP 512"; Book button worked, pressed; after "Overwing seat(s), selected for Rohan Reddyueor" — booked for Rohan, travelling alone. |
| TC-SM-EXR-002 | An adult with an infant CANNOT take an exit seat | 06 | Passed | Exit seat 16A with Rohan holding an infant: before "Overwing seat(s), Blocked"; Book not offered; after "Overwing seat(s), Blocked" — refused, correctly. Observation: the box reads a bare "Blocked" with no stated reason — worth reporting even though the refusal itself is correct. |
| TC-SM-EXR-003 | A child CANNOT take an exit seat | 05 | Inconclusive | No bookable "Exit and emergency exit" seat on this flight's map — needs a flight whose inventory actually offers one. Screenshot: `artifacts/failures/TC-SM-EXR-003.png`. |
| TC-SM-EXR-004 | The rule is rechecked when you switch passenger | 05 | Inconclusive | No bookable "Exit and emergency exit" seat on this flight's map — needs a flight whose inventory actually offers one. Screenshot: `artifacts/failures/TC-SM-EXR-004.png`. |
| TC-SM-EXR-005 | The rule follows the infant's linked adult | 07 | Blocked | Step "The exit-row refusal follows the infant's linked adult" stopped: locator.waitFor: Timeout 15000ms exceeded. — waiting for getByRole('region', { name: ' 1- Adult' }).or(getByRole('tab', { name: /1- Adult/ })).first() to be visible Screenshot: `artifacts/failures/TC-SM-EXR-005__2026-11-18_18-23-58.png`. |
| TC-SM-EXR-007 | An exit seat already taken is dealt with when an infant is added later | – | Not run | Not approved at Review — excluded by the Review gate; never executed. |
| TC-SM-RTP-001 | A return trip gives one seat map per journey | 09 | Passed | 4 seat map(s): "Seat Map · DEL → BOM · Onward", "Seat Map · BOM → DXB · Onward", "Seat Map · DXB → BOM · Return", "Seat Map · BOM → DEL · Return"; aircraft SG475 · 73HXCA (189 seats) and SG5113 · 73HQSB (189 seats) and SG5023 · 73HQSB (189 seats) and SG670 · 73H189 (189 seats). 4 maps for 2 journeys: a connecting journey has one map per flight. |
| TC-SM-RTP-004 | Seat maps are completely separate per journey | 09 | Passed | After confirming 2B, 2C on journey 1, its button reads "Change Seats" and journey 2's "Select Seat". Journey 2's map (SG5113 · 73HQSB) opened with 0/2 chosen; seat 2B there reads ", available for EGP 76.5". |
| TC-SM-MCT-003 | Three journeys give three seat maps and three sets of ancillaries | 10 | Passed | 4 seat map(s) over 3 journey(s): "Seat Map · DEL → BLR · Trip 1", "Seat Map · BLR → DEL · Trip 2", "Seat Map · DEL → BOM · Trip 3 · Leg 1", "Seat Map · BOM → DXB · Trip 3 · Leg 2". Each map has its own route title. |
| TC-SM-MCT-004 | A seat booked on one flight never appears against another | 15 | Blocked | Not reached: Step "Build the multi-city booking up to Book (seats on every flight, a meal, a bag)" stopped: the booking is not Confirmed (status "Failed") |
| TC-SM-MCT-007 | Seat maps follow FLIGHTS, fare packages follow JOURNEYS | 10 | Failed | 0 package box(es) vs 4 seat map(s); 8 meal box(es) (= 4 flights × 2 passengers). Connecting: Trip 3 (2 flights) DIFFERENT prices: EGP 128 \| EGP 154 \| EGP 25 \| EGP 50.5 \| EGP 512 \| EGP 76.5 \| Free. Trip 3: its legs show different seat prices (EGP 128 \| EGP 154 \| EGP 25 \| EGP 50.5 \| EGP 512 \| EGP 76.5 \| Free vs EGP 205 \| EGP 282 \| EGP 50.5 \| EGP 76.5 \| Free). Screenshot: `artifacts/failures/TC-SM-MCT-007.png`. |
| TC-SM-MCT-005 | Seats and ancillaries survive to the confirmation page per flight | 15 | Blocked | Not reached: Step "Build the multi-city booking up to Book (seats on every flight, a meal, a bag)" stopped: the booking is not Confirmed (status "Failed") |
| TC-SM-MCT-006 | Extra baggage should say which flight it belongs to | 10 | Passed | 6 baggage box(es) for 3 journey(s) × 2 passengers; labels: DEL→BLR Trip 1, BLR→DEL Trip 2, DEL→DXB Trip 3. One baggage panel per journey, each labelled with its journey. |
| TC-SM-RTP-005 | Seat and ancillary charges should be identifiable per journey | 09 | Passed | Accepted as a reporting gap, not a failure (human decision, 2026-10-27): the total is correct and the breakdown does not name each journey. Price breakdown lines: Seat Selection 486.5; base fare 3 line(s), tax 3 line(s), total 29750.5. No line names its journey — one merged package line, one base line and one tax line cover both journeys. The same applies to meals and baggage, per the human (none were chosen in this run). |
| TC-SM-CNF-001 | The confirmation total equals what was quoted | 14 | Failed | Quoted 14,989.50; confirmation Total Amount 14989. Screenshot: `artifacts/failures/TC-SM-CNF-001.png`. |
| TC-SM-CNF-002 | Every price line survives the booking unchanged | 14 | Passed | Booking screen: base Adult 3378, Child 3378, Infant 1250 = 8,006.00; taxes Adult 1524, Child 1524, Infant 1007.5 = 4,055.50. Confirmation: Base fare 8006, Taxes 4055.5. |
| TC-SM-CNF-003 | Every ancillary reaches the confirmation with its own price | 14 | Passed | Each chosen extra on its own line, quoted → confirmed: Meal 148.5 → 148.5, Seat 154 → 154, Baggage 2625 → 2625. |
| TC-SM-CNF-004 | Seats carry through to the right passenger with the right price | 14 | Passed | Passenger table: adult row shows seat 3A; child and infant rows show no seat. Itinerary (undefined-undefined): the adult has seat 3A; the child and infant show none. |
| TC-SM-CNF-007 | Infants appear as passengers but carry no seat or ancillaries | 14 | Passed | Infant row: "Kabir Iyeruuni - Infant SG 603 SG 5060 - - - Check In : 0 KG / Cabin :1 additional piece up to 7 kg. Dimensions: 55 x 35 x 25 cm (total 115 " — no seat and no meal, as an infant holds none. |
| TC-SM-CNF-008 | The e-ticket and the confirmation page agree exactly | 14 | Passed | E-ticket (/e-ticket/AGN10546AGNNDCORD261118DFTYZI) against the confirmation — fare lines identical (base 8006, taxes 4055.5, meal 148.5, seat 154, baggage 2625, bundle 0, total 14989); seat and meal on both. |
| TC-SM-CNF-010 | The wallet is reduced by exactly the booking amount | 14 | Passed | Wallet 4,948,824 EGP → 4,933,835 EGP: a drop of 14,989.00 against a total of 14989. |
| TC-SM-CNF-011 | The Costing tab adds up and is the figure to trust | 14 | Passed | Costing tab: Total Booking Amount 14989, Total Sales Amount 14989, Total Net Amount 14989, Total SSR Amount 2927.5 — against the confirmed 14989. |
| TC-SM-CNF-012 | The order page itemises every ancillary and reconciles | 14 | Passed | My Bookings list shows 12061.5 for this order — the fare only (base 8006 + taxes 4055.5 = 12,061.50), as the case says it should; the Costing tab shows the amount charged, 14989. |
| TC-SM-CNF-015 | Walking away before 'Book' creates nothing - THE SAFE TEST | 13 | Passed | Rohan Iyergjoo, seat 2B, meal VGSW, bag Excess Baggage – 10 Kg; quote 7,726 EGP; left without pressing Book. Wallet 5,000,000 EGP → 5,000,000 EGP; My Bookings 0 → 0 orders, newest null → null. Nothing was created and nothing was charged. |
| TC-SM-CNF-016 | Extra baggage must not clash with the baggage already in the fare package | 16 | Blocked | Not reached: Step "Build the clashing booking up to Book" stopped: no fare packages offered — this clash needs a package that includes checked baggage |
| TC-SM-CNF-017 | A failed booking must be reported as failed everywhere | 16 | Blocked | Not reached: Step "Build the clashing booking up to Book" stopped: no fare packages offered — this clash needs a package that includes checked baggage |
| TC-SM-CNF-018 | Going back a step must not silently throw away seats and ancillaries | 01 | Blocked | Step "Back a step and edit a passenger" stopped: locator.waitFor: Timeout 15000ms exceeded. — waiting for getByRole('region', { name: ' 1- Adult' }).or(getByRole('tab', { name: /1- Adult/ })).first() to be visible Screenshot: `artifacts/failures/TC-SM-CNF-018__2026-11-18_18-22-28.png`. |
| TC-SM-PAX-008 | Two infant-holding adults cannot sit in the same set of seats | 08 | Passed | Rohan (holds an infant) took 3A. For Priya (also holds an infant): set-mates 3B ", available for EGP 50.5", 3C "Aisle seat, available for EGP 154" — NOT blocked by the map; 3D in the other set "Aisle seat, available for EGP 154" — free. Passed by the recorded business ruling (27 Sep 2026: the airline enforces this at booking, not the seat map). Not verified with a completed booking. |
| TC-SM-PRC-005 | Seat map and ancillary prices use the right exchange rate | 12 | Blocked | Not reached: Step "Non-EGP credential: search, book, collect every price" stopped: settings.local.json needs "nonEgp": { "credentialName", "currency", "roe" } — the ROE is read from the Admin (Master → Miscellaneous) by the human. |
| TC-ML-001 | Meals are offered per passenger and per flight | 05 | Passed | 4 meal dropdown(s) for 2 non-infant passengers. Rohan: VGSW 148.5, VGML included, NCC1 154, NVSW 222.5, NVML 141, LCVS 272.5, JNSW 202.5, DBML included, CAKE 347.5; Aanya: VGSW 222.5, VGML included, NCC1 230.5, NVSW 247.5, NVML included, LCVS 297.5, JNSW 228, DBML included, CAKE 382. |
| TC-ML-002 | An infant is not offered a meal | 06 | Passed | 2 meal dropdown(s) for 1 non-infant passenger (Rohan; one per flight on a connecting trip); Kabir (infant) appears nowhere on step 2 — no meal choice for the infant. |
| TC-ML-003 | Choosing and removing a meal changes the price correctly | 01 | Passed | Meals offered: VGSW 148.5, VGML 0, NCC1 154, NVSW 222.5, NVML 141, LCVS 272.5, JNSW 202.5, DBML 0, CAKE 347.5. Chose VGSW: "Meal Preference +148.5", 12,266.5 → 12,415. Removed it: line gone, total 12,266.5. |
| TC-ML-005 | The chosen meal reaches the confirmation (SPENDS REAL MONEY) | 14 | Passed | Meal VGSW against the adult on the confirmation, the e-ticket and the itinerary. |
| TC-BG-001 | Extra baggage is offered per passenger and per journey | 05 | Passed | 2 baggage dropdown(s) for 2 non-infant passengers. Rohan: Excess Baggage – 10 Kg 2625, Excess Baggage – 5 Kg 1312.5; Aanya: Excess Baggage – 10 Kg 2625, Excess Baggage – 5 Kg 1312.5. |
| TC-BG-002 | An infant is not offered extra baggage | 06 | Passed | 1 baggage dropdown(s) for 1 non-infant passenger (Rohan); Kabir (infant) appears nowhere on step 2 — no extra baggage choice for the infant. |
| TC-BG-003 | Changing the bag weight replaces the bag, it does not add a second one | 01 | Passed | Chose Excess Baggage – 5 Kg (+1,312.5): 12,266.5 → 13,579. Changed to Excess Baggage – 10 Kg (+2,625): one "Extra Baggage +2,625" line, total 14,891.5 — the first bag is gone. |
| TC-BG-005 | The extra bag reaches the confirmation (SPENDS REAL MONEY) | 14 | Passed | Extra baggage Excess Baggage – 10 Kg: confirmation "Rohan Iyeruuni DEL-DXB Excess Baggage – 10 Kg EGP 2625.0", e-ticket "Rohan Iyeruuni DEL-DXB Excess Baggage – 10 Kg EGP 2625.0", itinerary under the adult. |
| TC-HLD-005 | Seats, meals and bags chosen before holding are kept on the held booking | 17 | Passed | Chosen before Hold: seat 2C, meal VGSW, bag 10 Kg (2,927.50 of extras). The held booking's itinerary keeps: seat 2C, meal VGSW, bag 10 Kg. Costing SSR amount 2,927.50. |
| TC-PAH-002 | Paying takes exactly the booking amount from the wallet | 17 | Passed | Wallet 4,981,958 EGP → 4,974,129 EGP: a drop of 7,829.00 against 7,829.50 shown when paying. Statement lists - (10 row(s)). |
| TC-PAH-003 | A price change between hold and payment is shown before paying | 17 | Inconclusive | The price did not change between hold and payment (7,829.50 at hold, 7829.5 charged) — the held price was charged, as the case says; the changed-price path was not exercised (a price change cannot be forced). Screenshot: `artifacts/failures/TC-PAH-003.png`. |
| TC-PAH-004 | After payment the e-ticket is issued and matches the booking | 17 | Failed | After payment: details status "Booked", passenger "BOOKED", ticket number NONE; flights SG 475 DEL-BOM + SG 5113 BOM-DXB (same as held); e-ticket total 7830 vs quote 7,829.50; e-ticket passenger row carries the name and ticket number. Dashboard not shown → not shown. Screenshot: `artifacts/failures/TC-PAH-004.png`. |
| TC-SM-BND-006 | A supplier with no fare packages shows no package step | 01 | Passed | "Select Bundle" not shown; Bundle Fare line absent. |
| TC-RPT-001 | A booking you just made appears in the reports | 14 | Passed | My Bookings lists AGN10546AGNNDCORD261118DFTYZI (ref NDCEG-BR-F261118Luk83g). Booking report, today, finds NDCEG-BR-F261118Luk83g (invoice INVNDC261118bbJuLS). |
| TC-RPT-002 | A payment appears in the account statement with the right amount | 14 | Inconclusive | Statement report, today (last updated 2026-11-18 18:05, booked 2026-11-18 18:39): PNR OGMT3R not there yet — the report was last updated before this booking (it updates hourly). Recheck after the next update. Screenshot: `artifacts/failures/TC-RPT-002.png`. |
| TC-RPT-003 | The reports agree with the booking and with each other | 14 | Passed | Sales report, today — invoice INVNDC261118bbJuLS: "INVNDC261118bbJuLS Spicejet Hussein 14,989.00 0.00 14,989.00 2 Rohan Kapooruiam 18-11-2026 NDCEG-TR261118Qtmz56 INVNDC261118r5c8vp Spicejet Hussein 14,989.00 0.". Shows 14,989.00, the amount charged — same as the confirmation, e-ticket and Costing tab. |
| TC-RPT-004 | Extras bought with a booking are visible in the reports | 14 | Passed | Sales row for invoice INVNDC261118bbJuLS: 14,989.00 includes the extras (meal 148.5, seat 154, bag 2625, bundle 0) — not fare-only. |
| TC-SM-LEG-001 | A child CAN select a leg space seat on the seat map | 05 | Passed | Leg space seat 2D — Aanya (child): "Leg space seat, available for EGP 512", selectable. Rohan (adult): "Leg space seat, available for EGP 512", selectable. A child may have a leg space seat; only an adult holding an infant is restricted. Book was not pressed. |
| TC-SM-LEG-002 | An adult holding an infant CAN select a leg space seat on the seat map | 06 | Passed | 18 leg space seat(s) checked with Rohan holding an infant: "Window seat, Blocked" on 1A \\| "Leg space seat, Blocked" on 1B, 1C \\| "Leg space seat, available for EGP 512" on 2D, 2E \\| "Window seat, available for EGP 512" on 2F \\| "Overwing seat(s), Blocked" on 16A, 16B, 16C, 16D, 16E, 16F, 17A, 17B, 17C, 17D, 17E, 17F — selectable on 2D, 2E, 2F, as the rule says (the refusal comes at Book/Hold, TC-SM-LEG-003/005). Book was not pressed. |
| TC-SM-LEG-003 | Booking an infant-holding adult on a leg space seat is refused | 20 | Failed | Rohan Reddycbld (Adult) on leg space seat 2F, the human pressed Book: status "Failed" (order AGN10546AGNNDCORD261118YFA3DG, ref NDCEG-BR-F261118eqpgrR); wallet 4,963,813 EGP → 4,963,813 EGP (not charged). Expected "In Progress" on Portal. Screenshot: `artifacts/failures/TC-SM-LEG-003__2026-11-18_13-48-28.png`. |
| TC-SM-LEG-004 | Booking a child on a leg space seat is confirmed | 21 | Passed | Aanya Reddyrdtc (Child) on leg space seat 2E, the human pressed Book: status "Confirmed" (order AGN10546AGNNDCORD261118CMTUE7, ref NDCEG-BR-F261118oYINn9); wallet 4,974,129 EGP → 4,963,813 EGP (CHARGED). Wallet charged 10,316.00 = the quote. |
| TC-SM-LEG-005 | Holding a booking with a leg space seat for an infant-holding adult is refused | 22 | Failed | Rohan Sharmammcj (Adult) on leg space seat 2F, the human pressed Hold: status "Failed" (order AGN10546AGNNDCORD261118PSP3BY, ref NDCEG-BR-F261118E4xVxv); wallet 4,963,813 EGP → 4,963,813 EGP (not charged). Expected "In Progress" on Portal. Screenshot: `artifacts/failures/TC-SM-LEG-005__2026-11-18_13-50-00.png`. |
| TC-SM-STA-007 | Women-only seats are marked clearly | – | Excluded | Supplier rule "ladies-only-seats" does not apply to this supplier |
| TC-SM-PRC-001 | A seat included in the fare package costs nothing | – | Excluded | Needs Bundle — Not Supported by this supplier |
| TC-SM-BND-002 | Changing the fare package recalculates the seats | – | Excluded | Needs Bundle — Not Supported by this supplier |
| TC-SM-BND-003 | Each flight uses its own fare package | – | Excluded | Needs Bundle — Not Supported by this supplier |
| TC-SM-BND-004 | Fare package names are never blank or repeated | – | Excluded | Needs Bundle — Not Supported by this supplier |
| TC-SM-BND-005 | What the package promises about seats is what you get | – | Excluded | Needs Bundle — Not Supported by this supplier |
| TC-SM-GEN-001 | A women-only seat is refused to a male passenger | – | Excluded | Supplier rule "ladies-only-seats" does not apply to this supplier |
| TC-SM-GEN-002 | A women-only seat is offered to a female passenger | – | Excluded | Supplier rule "ladies-only-seats" does not apply to this supplier |
| TC-SM-GEN-002b | The hidden seat data matches what is shown on screen | – | Excluded | Supplier rule "ladies-only-seats" does not apply to this supplier |
| TC-SM-GEN-004 | Each passenger sees their own seats in a mixed group | – | Excluded | Supplier rule "ladies-only-seats" does not apply to this supplier |
| TC-SM-GEN-005 | Female titles are 'Ms' and 'Mrs', for children as well as adults | – | Excluded | Supplier rule "ladies-only-seats" does not apply to this supplier |
| TC-SM-CAB-001 | The same flight gives a different map in each cabin | – | Excluded | Supplier rule "business-cabin" does not apply to this supplier |
| TC-SM-CAB-002 | The fare package decides WHICH seats are offered, not only their price | – | Excluded | Needs Bundle — Not Supported by this supplier; Supplier rule "business-cabin" does not apply to this supplier |
| TC-SM-CAB-003 | 'Not offered' sections can be told apart from single taken seats | – | Excluded | Supplier rule "business-cabin" does not apply to this supplier |
| TC-SM-CAB-004 | The Business row has its middle seats blocked | – | Excluded | Supplier rule "business-cabin" does not apply to this supplier |
| TC-SM-CAB-005 | Packages, meals and bags change with the cabin | – | Excluded | Needs Bundle — Not Supported by this supplier; Supplier rule "business-cabin" does not apply to this supplier |
| TC-SM-CAB-007 | A two-cabin aircraft shows both cabins properly | – | Excluded | Supplier rule "business-cabin" does not apply to this supplier |
| TC-SM-RTP-003 | Each journey's fare package sets that journey's seat prices | – | Excluded | Needs Bundle — Not Supported by this supplier |
| TC-SM-NAV-006 | A supplier with no seat selection shows no seat option | – | Excluded | Only for suppliers without Seat — this supplier supports it |
| TC-ML-004 | The meal list is separate from what the bundle includes | – | Excluded | Needs Bundle — Not Supported by this supplier |
| TC-ML-006 | A supplier with no meals shows no meal choice | – | Excluded | Only for suppliers without Meal — this supplier supports it |
| TC-BG-006 | A supplier with no extra baggage shows no baggage choice | – | Excluded | Only for suppliers without Baggage — this supplier supports it |

## 5. Staging orders recorded in the group logs (12 found; may be incomplete)

Assisted groups create real orders on staging with generated passengers. Only orders whose log line carries an order and ref are listed here, so bookings from groups 14 and 17 may be missing — read those group logs for the full set; check the wallet and clean up as needed.

| Group | Case | Order | Ref | Logged (UTC+local clock of the runner) |
|---|---|---|---|---|
| 20 | TC-SM-LEG-003 | AGN10546AGNNDCORD261118WQTJNZ | NDCEG-BR-F261118I9OZiW | 2026-11-18 13:12 |
| 21 | TC-SM-LEG-004 | AGN10546AGNNDCORD261118CMTUE7 | NDCEG-BR-F261118oYINn9 | 2026-11-18 13:14 |
| 22 | TC-SM-LEG-005 | AGN10546AGNNDCORD261118CQ0YQH | NDCEG-BR-F261118KwC1WS | 2026-11-18 13:15 |
| 20 | TC-SM-LEG-003 | AGN10546AGNNDCORD261118YFA3DG | NDCEG-BR-F261118eqpgrR | 2026-11-18 13:46 |
| 22 | TC-SM-LEG-005 | AGN10546AGNNDCORD261118PSP3BY | NDCEG-BR-F261118E4xVxv | 2026-11-18 13:48 |
| 14 | first pass — Failed (2C) | AGN10546AGNNDCORD261118XUATVB | NDCEG-BR-F261118oBgYLy | 2026-11-18 13:05 |
| 14 | rerun attempt 2 — Failed (2B), wallet unchanged | AGN10546AGNNDCORD261118VGQ3GE | NDCEG-BR-F261118G8OWDp | 2026-11-18 16:35 |
| 14 | rerun attempt 3 — Confirmed, PNR OGMT3R (CHARGED) | AGN10546AGNNDCORD261118DFTYZI | NDCEG-BR-F261118Luk83g | 2026-11-18 16:37 |
| 15 | rerun attempt 1 — Failed, wallet unchanged | AGN10546AGNNDCORD261118XJJHLH | NDCEG-BR-F261118Mx8KUl | 2026-11-18 16:50 |
| 15 | rerun attempt 2 — Failed, wallet unchanged | AGN10546AGNNDCORD261118L8PLW0 | NDCEG-BR-F2611181qCvTH | 2026-11-18 16:53 |
| 15 | rerun attempt 3 — Failed, wallet unchanged | AGN10546AGNNDCORD261118BEMJHC | NDCEG-BR-F261118xwCdWg | 2026-11-18 16:56 |
| 15 | rerun — Failed (aisle seats only), status Failed | AGN10546AGNNDCORD261118PZD9QD | NDCEG-BR-F2611189hon8r | 2026-11-18 18:28 |

## 6. Declared gaps and caveats

- Staging status comes from the human; the API_KEY pattern was not read by the assistant.
- Earlier runs 017 and 018 (same scope) were stopped partway by the human and are superseded by this run; their logs remain in their folders.
- No result is attributed to a root cause unless the log says so. Findings are not classified as application defects until a person reviews them.
- Updated 2026-11-18 (Group 14 rerun, log GroupResults_Group14_2026-11-18_16-37-53.md): the 15 Group 14 cases now carry the rerun's results instead of Blocked. Earlier rerun attempts: 16-33-47 stopped before Book (date picker timeout, nothing booked); 16-35-46 Book → Failed with the infant-holder on 2B. Settings file recreated; the infant-holding adult then kept off 5F, 2B, 2C.
- Updated 2026-11-18 (Group 15 rerun, latest log GroupResults_Group15_2026-11-18_16-56-17.md; earlier attempts 16-50-53 and 16-53-08): test data given by the human — DEL→BLR 17/12/2026, BLR→DEL 18/12/2026, DEL→DXB 19/12/2026. All three bookings Failed at Book, so TC-SM-MCT-004 and TC-SM-MCT-005 stay Blocked.
- Updated 2026-11-18 from the rerun of the 12 blocked Portal cases (headed, 18:20–18:28 UTC; groups 01, 07, 10, 11, 12, 16, 15, each with only its blocked cases). Groups 10 and 11 used the human's route DEL→BLR 17/12, BLR→DEL 18/12, DEL→DXB 19/12/2026 instead of DEL→DXB / DXB→DEL / DEL→DXB.
