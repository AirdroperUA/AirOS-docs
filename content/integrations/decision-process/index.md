+++
title = "Decision Process"
description = "How to decide what kind of integration is most appropriate."
date = 2024-05-09T17:00:00+11:00
sort_by = "weight"
weight = 0
template = "docs/page.html"
draft = false
aliases = ['/airos/latest/integrations/decision-process']

[extra]
toc = true
top = false
+++
## Options

- AirOS core
- AirOS Extension
   - Independent
   - Internal (Jupyter, Node-RED, etc)
   - Add-on (Cockpit widget, etc)
- Autopilot
   - Firmware driver
   - Lua script

## Diagram

{{ mermaid(path="overview.mermaid", width="220%", curve="monotoneY") }}
