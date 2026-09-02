# SANTool

Browser-only Brocade SAN zoning planner.

## What It Does

- Imports Brocade `switchshow` and `alishow` text captures for Fabric A and Fabric B.
- Works with Fabric A only when Fabric B is not supplied.
- Parses aliases, zones, active configs, switch ports, WWPNs, and NPIV summary warnings.
- Adds extra `switchshow` captures to an already-loaded fabric so WWPNs on other switches get port mappings.
- Imports supplemental `nsshow` or `portshow xx` evidence to map NPIV ports.
- Lets you select aliases/devices and generate Brocade zoning commands without connecting to a switch.
- Generates `zonecreate`, `cfgadd` or `cfgcreate`, `cfgenable`, and optional `cfgsave` command text.
- Produces a plain-English summary of what the generated commands will do.
- Exports parsed data as an XLSX workbook.
- Saves and restores complete project state as JSON, with browser memory of the last project.
- Bulk-selects aliases from pasted Windows `Get-InitiatorPort` WWPN output.
- Builds an OS disk to Storwize/FlashSystem volume mapping list by matching Windows disk UniqueId values to `lshostvdiskmap` `vdisk_UID` values.

## Using It

Open `index.html` directly in a browser, or serve it as a static web page.

The OpenClaw deployment currently exposes it here:

```text
https://openclaw.blackcarburning.com/brocade-zoning-planner/
```

## Capture Preparation

Use one main plain text capture file per fabric. Before collecting with PuTTY, increase the scrollback buffer, then capture:

```text
switchshow
alishow
```

For best port mapping, capture `switchshow` from every switch in the fabric. Those outputs can be in the main fabric file before `alishow`, or imported later through the **Additional switchshow port maps** panel. Later switchshow-only imports update port mappings only; they do not replace the aliases, zones, or active config from the main fabric capture.

For switch ports that show only NPIV summary text such as `1 N Port + 4 NPIV public`, collect extra evidence from:

```text
nsshow
portshow <port>
```

The tool does not run commands on switches. It only parses captures and prepares commands for review.

## OS Disk To Storwize Volume Mapping

On the Windows host, run the **Windows UID script** from the tool and paste the output into the OS UID box.

On IBM Storage Virtualize / Storwize / FlashSystem, run the **Storwize UID commands** from the tool. The key command is:

```text
lshostvdiskmap -delim :
```

You can run it for all mappings or for one host:

```text
lshostvdiskmap -delim : <host_name_or_id>
```

Paste the storage output into the Storwize box and build the map. The output list is keyed by UID and includes OS disk number, OS disk name, OS UID, Storwize volume name, Storwize volume ID, host, and SCSI/LUN ID.
