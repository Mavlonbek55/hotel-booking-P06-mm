# P06 · Hotel booking as a modular monolith (Python)

Extract the ZIP and open a terminal in the extracted folder. The project is the finished P06
app: the P04 code sorted into four modules (rooms, reservations, payments, notifications),
each with a public contract and internal code, and three architecture checks that keep the
borders between them.

Install once (internet needed the first time):

```text
python -m venv .venv
.venv\Scripts\activate          # macOS/Linux: source .venv/bin/activate
python -m pip install import-linter
```

Run the checks, the demo and the race:

```text
python -m unittest -v
lint-imports
python main.py
python main.py --race
```

The checks print:

```text
python -m unittest
Ran 8 tests
OK

lint-imports
Reservations uses only the public contracts of other modules KEPT
Rooms does not depend on any other module KEPT
Payments and Notifications do not depend on Reservations KEPT

Contracts: 3 kept, 0 broken.
```

The demo prints exactly the same lines as P04:

```text
Free rooms 12-14 Nov: R7, R8
Maya books R7 12-14 Nov: Accepted R101
Omar books R7 13-15 Nov: Unavailable
Nora books R7 14-16 Nov: Accepted R102
Omar books R8 15-15 Nov: InvalidDates
Free rooms 12-14 Nov: R8
Maya cancels R101: True
Free rooms 12-14 Nov: R7, R8
Confirmation emails sent: 2
```

The race demo still double-books R7: moving code into modules does not change what it does (P09 fixes it):

```text
Maya and Omar both book R7 for 12-14 Nov at the same moment...
Maya: Accepted R101
Omar: Accepted R102
Confirmed reservations for R7 on 12-14 Nov: 2
Double booking! Both requests checked before either one saved.
```

docs/adr-001-modular-monolith.md records why the app is one program with four modules.
