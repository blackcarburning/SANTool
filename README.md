# SANTool

Browser-only Brocade SAN zoning planner.

## What It Does

- Imports Brocade `switchshow` and `alishow` text captures for Fabric A and Fabric B.
- Works with Fabric A only when Fabric B is not supplied.
- Parses aliases, zones, active configs, switch ports, WWPNs, and NPIV summary warnings.
- Imports supplemental `nsshow` or `portshow xx` evidence to map NPIV ports.
- Lets you select aliases/devices and generate Brocade zoning commands without connecting to a switch.
- Generates `zonecreate`, `cfgadd` or `cfgcreate`, `cfgenable`, and optional `cfgsave` command text.
- Produces a plain-English summary of what the generated commands will do.
- Exports parsed data as an XLSX workbook.
- Saves and restores complete project state as JSON, with browser memory of the last project.
- Bulk-selects aliases from pasted Windows `Get-InitiatorPort` WWPN output.

## Using It

Open `index.html` directly in a browser, or serve it as a static web page.

The OpenClaw deployment currently exposes it here:

```text
https://openclaw.blackcarburning.com/brocade-zoning-planner/
```

## Capture Preparation

Use one plain text capture file per fabric. Before collecting with PuTTY, increase the scrollback buffer, then capture:

```text
switchshow
alishow
```

For switch ports that show only NPIV summary text such as `1 N Port + 4 NPIV public`, collect extra evidence from:

```text
nsshow
portshow <port>
```

The tool does not run commands on switches. It only parses captures and prepares commands for review.
