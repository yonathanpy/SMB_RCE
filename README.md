# SMB_RCE SMB Remote Code Execution Implementation

SMB_RCE is a proof-of-concept implementation designed to demonstrate remote code execution primitives targeting the Microsoft Server Message Block (SMB) protocol stack. The project focuses on low-level protocol interaction, malformed packet construction, and memory manipulation techniques that can be triggered through specially crafted network requests.

This repository provides a research-oriented environment for analyzing how SMB communication flows can be manipulated to interact with vulnerable components of the Windows networking subsystem. By controlling packet structures, compression behavior, and memory layout interactions, the PoC demonstrates the mechanics behind network-triggered kernel execution.

---

## Technical Overview

The implementation interacts directly with the SMB protocol layer, constructing raw SMB packets and manipulating protocol negotiation sequences in order to trigger abnormal memory behavior inside the SMB service.

The research focuses on several key technical areas:

* SMB protocol internals and packet structure
* Low-level socket communication and network packet crafting
* Compression transformation manipulation within SMBv3
* Kernel memory structure discovery
* Page table interaction and memory primitives
* Execution flow redirection through kernel-level structures

The exploit workflow demonstrates how network-controlled input can pivot from memory corruption into controlled execution by leveraging internal Windows kernel mechanisms.

---

## Repository Structure

```
SMB_RCE/
│
├── exploit.py              # Main exploit implementation
├── smb_win.py              # SMB packet construction and protocol logic
├── lznt1.py                # Compression routines used in packet crafting
├── kernel_shellcode.asm    # Kernel payload used for execution
├── README.md               # Project documentation
```

Each component plays a role in building the exploitation chain, from packet generation to kernel-level payload execution.

---

## Exploit Workflow

The PoC performs the following high-level operations:

1. Establish connection to the target SMB service.
2. Negotiate SMB protocol capabilities.
3. Deliver specially crafted SMB packets containing malformed compression structures.
4. Trigger memory corruption inside the SMB processing routine.
5. Locate critical kernel structures and memory regions.
6. Deploy kernel shellcode and redirect execution flow.

Once successful, the payload executes in kernel context, demonstrating the impact of the vulnerability.

---

## Usage

Run the PoC against a target system:

```
python3 exploit.py -ip <TARGET_IP>
```

Example:

```
python3 exploit.py -ip 192.168.1.25
```

Example output:

```
[+] Connected to target SMB service
[+] SMB negotiation successful
[+] Compression structure crafted
[+] Kernel memory primitives established
[+] Shellcode deployed
[+] Execution flow redirected
```

---

## Payload

Custom payloads can be inserted directly in the exploit implementation.

Modify the payload inside the exploit source:

```
USER_PAYLOAD = b"\x90\x90\x90..."
```

Payload size limitations depend on the memory region used by the kernel shellcode.

---

## Notes

The PoC focuses on demonstrating the technical mechanics behind SMB-based remote execution. It exposes how complex protocol interactions combined with memory manipulation can create powerful exploitation primitives inside kernel-level services.

This project is intended for vulnerability research, reverse engineering, and deep analysis of SMB attack surfaces within controlled environments.

---

## Author

Yonathanpy aka witwizard/sergie/orlserg
