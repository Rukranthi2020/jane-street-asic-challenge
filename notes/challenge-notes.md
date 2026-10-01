# Challenge notes (fill in as we go)

## Known facts (from published solutions)

- Chip: 11×11 Star Battle ("Two Not Touch") validator, ~728 cells / 738 nets.
- Interface: serial input `I`, clock `clk`, `enable`; 8-bit output `O[7:0]`; `success` flag.
- Protocol: 121 bits row-major, one per rising clock while `enable` high; next edge raises `success` on a valid grid; then ASCII verdict streams out one char/clock.
- Verdicts: `(* TWO STARS *)` on success, `TRY AGAIN` otherwise.
- Warmup is the calibration target: full RTL→GDS flow, golden netlist available.

## Open questions / experiments

- [ ] Inventory sky130 cell types present in puzzle.gds
- [ ] Identify power/ground/clock/reset nets
- [ ] Build overlap-based netlist extractor, validate on warmup first
- [ ] Flip-flop graph: find the 121-cycle structure
- [ ] Decode region map via single-cell probes
- [ ] SAT-solve for the 121-bit success input
- [ ] Recover behavioral RTL and prove equivalence
