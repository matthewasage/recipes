---
title: "Prevent PipeWire from suspending USB speakers"
date: 2025-04-27
---

Fedora kept suspending my Pebble v3 USB speakers. It turns out the speakers really did not like that, because they never came back from sleep and I had to continually turn then off and on for them to be usable again.

The solution is:

1. Create a new file named /etc/wireplumber/wireplumber.conf.d/disable-idle-timeout.conf
2. Add the following text:
    ```
    monitor.alsa.rules = [
      {
        matches = [
          {
            ## Matches all sources.
            node.name = "~alsa_input.*"
          }
          {
            ## Matches all sinks.
            node.name = "~alsa_output.*"
          }
        ]
        actions = {
          update-props = {
            session.suspend-timeout-seconds = 0
          }
        }
      }
    ]
    ```
3. Restart wireplumber: ```systemctl --user restart wireplumber.service```
