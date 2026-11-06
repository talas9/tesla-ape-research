# FSD region gates and CAN request research

Review draft · milestones shown in relative order

I started with two messages on the screen: FSD asked for updated maps, while TLSSC said the region was unsupported. I wanted to understand what actually decided those messages, then check whether changing the outgoing requests produced an independent response from the vehicle. I used a custom C6 board with a CAN transceiver.

First, I followed the GUI text into the functions behind the switches. I found that the two warnings use different checks, and that gateway configuration, managed feature values and localized country are separate inputs. Then I followed the switch actions forward to the request encoder. [Read that path](docs/trace-details.md).

Next, I used disassembly and REA-assisted analysis to follow an older HW2.5 APE request into shared state and state-machine checks. That gave us a concrete older receiver path, but it did not establish the same behavior on HW4. I also corrected several assumptions about ForceFSD, message IDs and what a successful transmission proves. [See the timeline](docs/research-paper.md) and [corrections](docs/corrections.md).

Finally, I ran stationary comparisons and built better diagnostic tools. The measured feedback candidates did not show a reversible change that proves request acceptance. The tools are built and reviewed, but not deployed. **I still need a matching HW4 receiver trace and an independent acceptance signal.** [Experiments and remaining work](docs/evidence-and-open-work.md).

## Start here

- [Research paper: timeline and diagrams](docs/research-paper.md)
- [Recovered GUI/request paths](docs/trace-details.md)
- [Experiments and recovery](docs/experiments.md)
- [Signal candidates and profile boundaries](docs/signal-profiles.md)
- [Inspected upstream findings and attribution](docs/sources.md)
- [Evidence scope and open tasks](docs/evidence-and-open-work.md)
- [Complete route index](docs/route-index.md)
- [Methodology and limits](docs/methodology.md)
- [Correction log](docs/corrections.md)
- [Structured sanitized findings](findings.json)

The issue comment is a separate private review draft and is excluded from this export.

This export contains independent notes and minimal numeric findings only. It excludes firmware binaries, extracted source/decompilation dumps, bulk DBC/database copies, raw captures, vehicle identifiers, credentials and device connection details. Source copyrights/licenses remain with their respective holders; publication licensing for these notes has not yet been selected.
