# Jane Street ASIC Reverse-Engineering Challenge

My workspace for replaying Jane Street's 2026 ASIC reverse-engineering puzzle
(the challenge is closed — this is for the fun and the learning).

## The challenge

Given a **GDS layout file** of a real chip (sky130 standard cells, all
cell/net names stripped) plus a sample VCD waveform, work out what the chip
does and find the input sequence that drives the `success` flag high.

What the chip turned out to be: an **11×11 Star Battle validator**
("Two Not Touch"). Shift a 121-bit grid in serially (row-major, one cell per
clock while `enable` is high); if the grid has exactly two stars per row,
column, and each of eleven irregular regions — 22 stars total, no two touching
even diagonally — `success` goes high and the chip streams out
`(* TWO STARS *)` in ASCII on `O[7:0]`. Bad inputs get `TRY AGAIN`.

## Layout

| Path | Contents |
|---|---|
| `puzzle/` | Original main-puzzle files: `puzzle.gds` (1.4 MB), `example_inputs.vcd`, `layout.png` (I/O labels) |
| `warmup/` | Warmup set (full RTL→GDS flow): `00_source.v`, `01_netlist.v`, `02_netlist_with_power_rails.v`, `03_post_place_and_route.def`, `04_final.gds` |
| `docs/` | Challenge background and references |
| `tools/` | GDS→netlist extraction and solving scripts |
| `notes/` | Working notes and findings |

## Roadmap

1. **Parse the GDS** — `gdstk` or KLayout; inventory cells against the sky130 PDK.
2. **Extract the netlist** — geometry → gates (overlap-based connectivity, validated against the warmup's golden netlist).
3. **Recover structure** — flip-flop graph, find the 121-cycle pattern feeding `success`.
4. **Identify the function** — probe regions, map the Star Battle grid checker.
5. **Solve it** — SAT/BMC unrolling to find the 121-bit input that asserts `success`; verify in simulation.
6. **Write the RTL** — behavioral model, prove cycle-equivalence vs. extracted gates.

## References

- Jane Street blog: "Can you reverse engineer an ASIC?" — the original puzzle post
- https://jestoph.com/2026/09/04/jane-street-challenge.html — solver writeup (custom simulator rabbit hole, plus a real bug he reported to Jane Street and got confirmed)
- https://github.com/notcleo/gds-to-rtl — rigorous 4-week solution (extraction proven exact, SAT-solved twice, recovered RTL cycle-equivalent to gates)
- GDS viewer: https://gds-viewer.tinytapeout.com
- sky130 PDK docs: https://skywater-pdk.readthedocs.io
