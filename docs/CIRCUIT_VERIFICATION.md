[CIRCUIT_VERIFICATION.md](https://github.com/user-attachments/files/32818468/CIRCUIT_VERIFICATION.md)
# CIRCUIT VERIFICATION

Binding for every circuit and for every technique example, problem, or question that uses one. A wrong polarity, direction or reference silently teaches wrong signs, so it is top-severity.

## Source of truth
The **structured circuit specification** is the source of truth. Its final file representation/path is provisional until D-02.
- Where technically possible, the rendered figure is **generated from the spec**.
- If a figure is hand-authored or otherwise not generated directly from the spec, it must be explicitly checked against the spec and, when the circuit comes from the PPT, against the original slide figure.
- Final rendering technology is open (D-03).

## Terminology (used consistently in specs, figures, and audits)
- **Source polarity**: the + / − terminals of a voltage source (independent or dependent).
- **Source direction**: the arrow of a current source (independent or dependent).
- **Voltage reference polarity**: the + / − assigned to a *labelled voltage*. It is a reference choice, not a property of the component.
- **Current reference direction**: the arrow assigned to a *labelled current*. Also a reference choice.

Not every component has an inherent polarity. A resistor, for example, has none. Polarity marks appear only where the element type has one or where a voltage or current is being labelled.

## Circuit spec contents (provisional until D-02/D-03)
Nodes and labels; components with type, value, unit and terminal connections; source polarity/direction; voltage reference polarity and current reference direction for every labelled quantity; dependent sources with type, gain, and controlling variable *with its reference*; ground/reference node; `origin` and slide ref; `variants[]`; `author_check`.

## Variants
A circuit may carry `variants[]`. A visual change is not automatically an electrical change. Each variant declares its `kind`, its `relation` to the base circuit, and the expected effect. Verification then checks that claim.

| Kind | Electrical circuit | What QA verifies |
|---|---|---|
| `layout_rotation` | Unchanged | Connectivity, source polarity/direction, and references are the same relative to the terminals, not the page. |
| `mirror` | Unchanged | Same as above. A mirrored drawing can make arrows look reversed while staying identical relative to the terminals. Confirm marks were preserved relative to terminals. |
| `reference_direction` | Same circuit; labelled quantity relabelled | Only the labelled quantity's sign flips; other quantities are unchanged. |
| `source_polarity` | Electrically different (a source is reversed) | The stated expected effect, by computation. Do not assume the response simply negates; that holds only for the effect of that source alone in a linear circuit. |
| `equivalent_representation` | Same *behavior* for a declared port or quantity | Equivalence is checked for the claimed terminals/quantity only (below). |
| `electrically_different` | Different circuit | Treated as an independent circuit with its own audit; a separate circuit item is preferred unless the contrast is the point. |

The representation of variants may change once the PPT figures are analyzed (D-03).

## Verification checks (each recorded pass / fail / n.a. with evidence)
1. **Connections**: every terminal goes to the intended node; nothing extra or missing.
2. **Polarity**: source polarity of every voltage source, including dependent voltage sources.
3. **Current direction**: current reference directions of labelled currents.
4. **Voltage reference direction**: voltage reference polarity of every labelled voltage.
5. **Figure/source representation**: confirm the visual placement/orientation of each source matches the spec and its defined source polarity/direction.
6. **Node labels**: names and the reference node.
7. **Component values and units**: no transcription errors against the slide or statement.
8. **Dependent sources**: type, gain, controlling variable and its reference, and the dependent source's own polarity/direction.
9. **Sign conventions**: consistent with `content/notation.md` (the course convention). If the PPT is ambiguous, flag; do not assume.
10. **Equivalent-circuit transformations**: equivalence is verified *for the terminal behavior or quantity for which equivalence is claimed*. Internal voltages and currents need not match. QA computes that behavior in both circuits; inspection alone does not count.
11. **Alternate orientations and variants**: each variant is checked against the kind and relation it declares, per the table above.
Also: **figure fidelity**: generated figures match the spec by construction; any other figure is compared against the spec and, when applicable, the slide.

## Who does what
- **Circuit Specialist**: authors spec, figure and variants; completes `author_check` (self-reading of checks 1-9). This is not verification.
- **QA / Auditor**: rebuilds from the source (slide or statement), compares with spec and figure, independently computes key quantities (for example, solves the network from the spec), and compares with any solution that depends on it.
- **Content Analyst and Problem/Solution Specialist**: reference circuits by ID only. They may submit work for REVIEW only when the circuit is VERIFIED.

## Change and dependents
- A change to the circuit's electrical/semantic meaning (connections, values, component type, source polarity/direction, voltage/current reference directions, or dependent-source definition) **invalidates its circuit audit**: status resets and the circuit is re-verified.
- A pure presentation change (spacing, positioning, typography, or other non-semantic layout) requires a figure/spec consistency check, but does not automatically invalidate the electrical audit when the electrical meaning is unchanged.
- The Circuit Specialist lists dependents (techniques, examples, problems, questions found by searching the circuit ID) in the handoff. QA decides which need re-review; a value, polarity, or reference change normally requires it, a verified layout-only change may not. Rechecks are tracked in `docs/CONTENT_STATUS.md`.

## Audit record (`audits/<item-id>.md`)
```
item: <id>   audited_by: <account/role>   date: <>   base: <SHA/date>
source_compared: <slide ref | statement>
checks: {connections: pass, polarity: pass, ..., variants: n.a.}
independent_computation: <method, quantity, result, matches: yes/no>
defects: <list with location>
verdict: verified | returned
```
Any defect returns the item to its owner; QA does not fix it.
