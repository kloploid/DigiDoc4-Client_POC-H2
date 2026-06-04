# DigiDoc4 — AppleScript injection PoC

Local PoC accompanying ADVISORY.txt in this directory.

## The file

```
rickroll" & (do shell script ("open https:" & (ASCII character 47)
& (ASCII character 47) & "youtu.be" & (ASCII character 47)
& "dQw4w9WgXcQ")) & ".cdoc
```

A byte-identical copy of a valid signed container with a
payload-bearing filename. Only the filename is the exploit.

## Reproduction

1. The system `mailto:` handler must be Mail.app or Outlook
   (otherwise the AppleScript branch at
   `Application_mac.mm:107`/`:119` is not taken and execution
   falls through to `QDesktopServices::openUrl` at line 145).
2. Launch DigiDoc4.
3. Double-click the file.
4. On the container view, click the **"Send by email"** button
   (envelope icon).
5. The default browser opens `https://youtu.be/dQw4w9WgXcQ`.

Afterwards Mail.app opens its normal new-message window — the
attack masquerades as ordinary application behaviour.

## This is not a prank

Opening a YouTube URL is the most benign payload available.
The underlying primitive — **arbitrary AppleScript execution
with `do shell script` reach outside the application sandbox** —
is independent of what the payload does.

The outcome is determined by the shell command inside the
filename. The PoC demonstrates the primitive is open.

## Cleanup

```bash
rm -rf ~/Desktop/digidoc_applescript_poc
```

## Source references

- Injection point (AppleScript concatenation):
  `client/Application_mac.mm:110-111, :122-123`
- In-process `mailto:` handler registration:
  `client/Application.cpp:417`
- Source of `attachment`/`subject` from the filename:
  `client/widgets/ContainerPage.cpp:84-85`
- Entitlements that broaden impact inside the sandbox:
  `qdigidoc4.entitlements:17-33`

Full analysis is in `ADVISORY.txt` in this directory.

## Disclosure status & license

See [`SECURITY.md`](SECURITY.md) for disclosure status and contact, and
[`LICENSE`](LICENSE) (MIT) for licensing. This is a private, coordinated
disclosure — please do not republish before the fix is released.
