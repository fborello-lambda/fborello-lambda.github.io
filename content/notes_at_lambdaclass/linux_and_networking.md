+++
title = "Linux and Networking"
date = 2024-05-01

[taxonomies]
tags=["cs", "linux"]
+++

## [Unix Philosophy](https://github.com/lambdaclass/lambdaclass_hacking_learning_path?tab=readme-ov-file#unix-philosophy)

- [Unix Timeline](https://upload.wikimedia.org/wikipedia/commons/c/cd/Unix_timeline.en.svg)
- [Basics of the Unix Philosophy](http://www.catb.org/~esr/writings/taoup/html/ch01s06.html)
  - “This is the Unix philosophy: Write programs that do one thing and do it well. Write programs to work together. Write programs to handle text streams, because that is a universal interface.”
- [Modularity](http://www.catb.org/~esr/writings/taoup/html/modularitychapter.html)
- [Transparency](http://www.catb.org/~esr/writings/taoup/html/ch06s02.html)

Questions:

- How does complexity relate to modularity?

  More complexity results in larger programs, contrary to Unix’s philosophy. Modular and small programs that are not complex to use—although their internal parts may be complex—are preferred because they perform specific tasks effectively and facilitate proper communication between them.

- Why is the text-stream interface important in the Unix Philosophy?

  The text-stream interface serves as the communication interface between programs (via piping like “prog1 | prog2”).

- Why should design for transparency encourage simple interfaces?

  Transparency aims to have a clean communication interface, debug programs/watchers, and should work on any program.

- How does robustness relate to transparency and simplicity?

  Robustness means that a program is reliable. For example, if it depends on user input, the program should account for any possible—even impossible—situations. If it’s simple, the result should be more predictable, making it easier to monitor if the program follows transparency rules.

- Even with video processing, why should the output of programs be terse?

  Because old programs that already work wouldn’t have a way to interface with newer ones. Following a convention and avoiding unexpected results help create new programs that can operate with both new and existing ones.

- According to the Unix Philosophy, how noisy should errors be?

  Errors should be as loud as possible, meaning the user should be notified. If a program B expects the output of program A and it fails, program B should receive an error. The way to handle the error depends on program B.

- How does the economy of programmer time relate to robustness?

  If software can write software, outcomes are faster, cheaper, and more reliable (robust).

- Why does premature local optimization reduce overall performance?

  Premature optimization can overcomplicate the code, reducing performance. It’s recommended to create a working prototype first and then optimize.

- If there is only "one true way" of doing things, how does it affect extensibility?

  If there were only one way to do things, it would create a closed system where communications are restricted to a specific interface.

## [Linux](https://github.com/lambdaclass/lambdaclass_hacking_learning_path?tab=readme-ov-file#linux)

- /**root** → first directory, where all directories are mounted to. (Linux Filesystem Hierarchy Standard).
- The contents and/or purpose of a file is determined by other means. Although Unix-like operating systems don’t use file extensions to determine the contents/purpose of files, some application programs do. The `file` command gives a brief description of the file's contents.
- /usr/bin contains the executable programs installed
  by your Linux distribution. It is not uncommon for this
  directory to hold thousands of programs.
- Use of symbolic links or soft links and hard links. `ln` command to create soft/hard links.
- `type`
  - Indicates how a command name is interpreted.
- `which`
  - Displays which executable program will be executed.
- `man`
  - Displays a command’s manual page.
- `apropos`
  - Displays a list of appropriate commands.
- `info`
  - Displays a command’s info entry.
- `whatis`
  - Displays a very brief description of a command.
- `alias`
  - Creates an alias for a command.
- `cat`
  - Concatenates files.
- `sort`
  - Sorts lines of text.
- `uniq`
  - Reports or omits repeated lines.
- `wc`
  - Prints newline, word, and byte counts for each file.
- `grep`
  - Prints lines matching a pattern.
- `head`
  - Outputs the first part of a file.
- `tail`
  - Output the last part of a file.
  - `tail -f` prints and "watches" the file until ctrl-c. a file and can be used as input for the following program.
- `tee`
  - Read from standard input and write to standard output and files.
  - `tee` creates a "T" fitting on the pipe. The output of a program can be redirected to
- Redirecting stdout with `>`(creates new file) `>>`(appends). FileDescriptors(1: stdout, 2:stderr). Redirecting stderr and stdout: `{command} > test.txt 2>&1` == `{command} &> test.txt` . `/dev/null` to suppress errors. Redirecting stdin with `<`.
- Piping `programA | programB` the output of programA is used as programB's input
- Bracket expansion: `mkdir {2009..2011}-0{1..9} {2009..2011}-{10..12}` it creates multiple directories divided by month and for 2009 up to 2011.
- Command substitution: `$(command)` or backquotes can be used too.
- Double quotes allow spacing, single quotes suppress all expansions, backslash escapes a single character.
- Terminal
  - CTRL-K Kill text from the cursor location to the end of line.
  - CTRL-U Kill text from the cursor location to the beginning of the line.
  - CTRL-A Move cursor to the beginning of the line.
  - CTRL-E Move cursor to the end of the line.
  - CTRL-R Reverse incremental search. Searches incrementally from the current command line up the history list.
  - ALT-P Reverse search, non-incremental. With this key, type the search string and press ENTER before the search is performed.
  - ALT-N Forward search, non-incremental.
- `history | tail` -> outputs a numbered list with all the last commands that then can be called used in the following way `!number`
- `id`
  - Displays user identity.
- `chmod`
  - Changes a file’s mode.
- `umask`
  - Sets the default file permissions.
- `su`
  - Runs a shell as another user.
- `sudo`
  - Executes a command as another user.
- `chown`
  - Changes a file’s owner.
- `chgrp`
  - Changes a file’s group ownership.
- `passwd`
  - Changes a user’s password.
- `ps`
  - Reports a snapshot of current processes.
- `top`
  - Displays tasks. (htop) . It can send signals too.
- `jobs`
  - Lists active jobs.
- `bg`
  - Places a job in the background.
- `fg`
  - Places a job in the foreground. (ctrl-z and then fg can be used to run that process. the jobs command displays the number of every job, and fg can be used as follows: `fs %(job's number)`)
- `kill`
  - Send a signal to a process.
- `killall`
  - Kill processes by name.
- `shutdown`
  - Shut down or reboot the system.
- “When a system starts up, the kernel initiates a few of its own activities as processes and launches a program called init. init, in turn, runs a series of shell scripts (located in /etc) called init scripts, which start all the system services. Many of these services are implemented as daemon programs” (systemctl to manage daemon programs in linux)
- Package Management: difference between `rpm` and `yum` // `dpkg` and `apt`/`aptitude`.

- `ping`
  - Sends an ICMP ECHO_REQUEST to network hosts.
- `traceroute`
  - Prints the route packets trace to a network host.
- `netstat`
  - Prints network connections, routing tables, interface statistics, masquerade connections, and multicast memberships. (netstat -t displays the kernel’s network routing table)
- `ftp`
  - Internet file transfer program.
- `lftp`
  - An improved Internet file transfer program.
- `wget`
  - Non-interactive network downloader.
- `ssh`
  - OpenSSH SSH client (remote login program). (port 22)
- `scp`
  - Secure copy (remote file copy program).
- `sftp`
  - Secure file transfer program. (`rsync` is also used)
- `locate`
  - Finds files by name.
- `find`
  - Searches for files in a directory hierarchy.
- `xargs`
  - Builds and executes command lines from standard input.
- `touch`
  - Creates or modifies "last time opened".
- `stat`
  - Displays file or filesystem status.
- Archiving → tar // gzip-gunzip.
- Bit-by-Bit copy `dd`
- sda2 is partition 2 of drive sda.
- `fsck` -> checks for errors.
- [Is /dev/sda a file or a directory?](https://superuser.com/questions/1496858/is-dev-sda-a-file-or-a-directory)

## Networking

- ARP stands for **Address Resolution Protocol**, which is a process used to connect a dynamic IP address to a physical machine's MAC address. ARP is used to resolve IP addresses into MAC addresses, which means that the IP address is already known, but the MAC address is not. ARP is necessary because it helps in resolving dynamic IP addresses into MAC addresses.
- hardware → protocols → apps (OSI model).

{% mermaid() %}
flowchart TD

subgraph Z[" "]
direction LR
Packets -- Have --> Headers -- Help to --> Redirect
end

subgraph ZA[" "]
direction LR
HTTP --> TCP --> IP
HTTPS -- HTTP but encrypted --> TCP
end
{% end %}

- static and dynamic ip (internet protocol). Internet Protocol Security (IPSec), a protocol that comes with a diversity of mechanisms to ensure the integrity, authenticity, and confidentiality of data packets.
- transport protocols: UDP(fast, but no reliable), TCP(reliable, uses a handshake mechanism), QUIC(uses a faster handshake mechanism)
- Domain Name System (DNS), a public, decentralized database that links a unique name to an IP address. DNS Security Extensions (DNSSEC). DNS over HTTPS (DOH)
- HTTPS is purely defined as securing HTTP connections by means of the cryptographic protocol Transport Layer Security (TLS).
- In order to set up a TLS connection between server and client, their communicating nodes establish a shared secret key through a handshake. Server Name Indication (SNI), certificate authorities (CA).

{% mermaid() %}
graph TD
CryptographicTechniques --> SigningData -- Used As--> AuthenticationMethod
CryptographicTechniques --> Encryption
Encryption --> SymmetricCryptography
Encryption --> AsymmetricCryptography
AsymmetricCryptography --> Private&Public_Keys
SymmetricCryptography --> SingleShared_Key
EndToEndEncryption --> DoubleRatchetAlgo
EndToEndEncryption --> OpenPGP_GPG
{% end %}

Many modern encryption algorithms feature **Forward Secrecy**, which ensures that intercepted and stored encrypted packets can’t be decrypted in the future, even if the long-term encryption keys of the sender and receiver are compromised.

- Packet Analysis
- Burpsuite
- How packet sniffers work?
  - Collection of the packet → Promiscuous mode
  - Conversion
  - Analysis
- Issues that need to be solved by protocols:
  - Connection Initiation
  - Negotiation of connection characteristics (handshaking)
  - Data formatting
  - Error Detection and Correction
  - Connection Termination

The OSI model uses specific terms to describe packaged data at each layer. The physical layer contains bits, the data link layer contains frames, the network layer contains packets, and the transport layer contains segments. The top three layers simply use the term data. This nomenclature isn’t used much in practice, so we’ll generally just use the term packet to refer to a complete or partial PDU that includes header and footer information from a few or many layers of the OSI model. (encapsulation of data)

The extent to which broadcast packets can travel is called the **broadcast
domain**, which is the network segment where any computer can directly transmit to another computer without going through a router.

### Wireshark

- From ChatGPT:
  Wireshark, the digital detective for networks, captures and unveils the content of data "packets" traveling through your network. An Ethernet frame acts like an envelope for this data, containing layers like IP (with addresses) and TCP (ensuring reliable data transfer). Importantly, each layer carries information from the layer above, and Wireshark helps you dissect these layers, revealing the intricate details of your network's communication. Understanding the Ethernet frame is key, as it serves as the outer layer transporting information across the network.
- [Youtube Tutorial](https://www.youtube.com/watch?v=OU-A2EmVrKQ&list=PLW8bTPfXNGdC5Co0VnBK1yVzAwSSphzpJ)
- ctrl-m → mark packet
- shift-ctrl-n → next marked packet // shift-ctrl-b → previous(back) marked packet.
- time shift and time ref is useful (time ref → ctrl-t)
- Output Tab → for logging data.
- The `ping` command generates ICMP traffic.
- Capture and Display filters. Berkeley Packet Filter (BPF) → expressions.
  - Capture Filters are applied before a “Packet Capturing Session”
    - A common scenario is to capture only TCP packets with the RST flag
      set. Offset 13 has the TCP’s packet flags. The Capture Filter to get the a flag is: `tcp[13] & 4 == 4` this filter gets the RST flag.
    - Commonly Used Capture Filters:
      - tcp[13] & 32 == 32 TCP packets with the URG flag set
      - tcp[13] & 16 == 16 TCP packets with the ACK flag set
      - tcp[13] & 8 == 8 TCP packets with the PSH flag set
      - tcp[13] & 4 == 4 TCP packets with the RST flag set
      - tcp[13] & 2 == 2 TCP packets with the SYN flag set
      - tcp[13] & 1 == 1 TCP packets with the FIN flag set
      - tcp[13] == 18 TCP SYN-ACK packets
      - ether host 00:00:00:00:00:00 Traffic to or from your MAC address
      - !ether host 00:00:00:00:00:00 Traffic not to or from your MAC address
      - broadcast Broadcast traffic only
      - icmp ICMP traffic
      - icmp[0:2] == 0x0301 ICMP destination unreachable, host unreachable
  - Display filters are applied and can be reverted. (A common case is to “hide” arp packets)
    - Analyze → Display Filters. GUI for creating filters.
    - format: protocol.feature.subfeature
    - Commonly Used Display Filters:
      - ip.addr==192.168.0.33
      - !tcp.port==3389 Filter out RDP traffic
      - tcp.flags.syn==1 TCP packets with the SYN flag set
      - tcp.flags.reset==1 TCP packets with the RST flag set
      - !arp Clear ARP traffic
      - http All HTTP traffic
      - `tcp.port==23 || tcp.port==21` Telnet or FTP traffic
      - smtp || pop || imap Email traffic (SMTP, POP, or IMAP)
- Layer3, Network Layer Protocols: ARP, IPv4, IPv6, ICMP, and ICMPv6.
  - Ethernet is Layer2 → Data Link
  - ARP:
    ”MAC addresses are needed because a switch that interconnects devices
    on a network uses a Content Addressable Memory (CAM) table, which lists the MAC addresses of all devices plugged into each of its ports. When the switch receives traffic destined for a particular MAC address, it uses this table to know which port to send the traffic through. If the destination MAC address is unknown, the transmitting device will first check for the address in its cache; if the address isn’t there, then it must be resolved through additional communication on the network. The resolution process that TCP/IP networking (with IPv4) uses to resolve an IP address to a MAC address is called the Address Resolution Protocol (ARP), which is defined in RFC 826. The ARP resolution process uses only two packets: an ARP request and an ARP response.” - ARP request (The packet destination address is the broadcast address ff:ff:ff:ff:ff:ff)→ Asks for the MAC address given an IP.
    ARP response → responds to the request with the MAC address of the device whose IP address matches the one that was sent in the request. - The gratuitous ARP packet → when ip changes and for load balancing. - [ARP is Layer2/3](https://learningnetwork.cisco.com/s/question/0D53i00000Ksz5aCAB/is-arp-a-layer-2-or-3-protocol)
  - IP:
    - IPv4 & IPv6 // Netmask // CIDR notation. The Netmask determines which part corresponds to the network and which part corresponds to the host.
    - TTL: Time to Live Defines the lifetime of the packet, measured in hops or seconds through routers.
    - ICMP uses IP to deliver Packages.
    - IP is Layer3
    - IP Fragmentation → Divides the IP packet based on the Maximum Transmission Unit(MTU).
    - IPv6: Hop Limit Defines the lifetime of the packet, measured in hops
      through routers. This field replaces the TTL field in IPv4.
    - IPv6 doesn’t support broadcast traffic because broadcast is viewed as an inefficient mechanism for transmission.
    - Neighbor Solicitation, a function of Neighbor Discovery Protocol (NDP), which utilizes ICMPv6(similar to ARP). Is used to find IPv6 devices.
    - IPv6 fragmentation used MTU discovery. Routers do not fragment packets. If the packets sent are too big, an ICMPv6 packet is sent to the initial sender.
    - IPv6 Transitional protocols(encapsulation):
      - 6to4
      - Teredo
      - ISATAP
  - ICMP
    - Relies on IP, it has a checksum.
    - Some firewalls block pings (ICMP).
    - The random text used in an ICMP echo request can be of great interest to a potential attacker. Attackers can use the information in this padding to profile the operating system used on a device.
- Layer4, Transport Layer Protocols:
  - TCP:
    - ports
    - reliable
    - flags (give status)
    - TCP Three-Way Handshake
      - hostA sends a packet with SYN flag active.
      - hostB sends a response with SYN and ACK flag active.
      - hostA responds with ACK flag active.
    - TCP Teardown → FIN/ACK handshake → gracefully end connection.
    - TCP Reset → RST flag → indicates that a connection was closed abruptly.
  - UDP:
    - fast but not reliable
    - No handshake
    - The application-layer protocols DNS and DHCP, which are highly dependent on the speed of packet transmission across a network, use UDP as their transport layer protocol, but they handle error checking and retransmission timers themselves.
- Layer7, Application Layer Protocols.
  - DHCP
    - Allows a device to get an IP address dynamically.
    - DORA process (dev → DHCP server):
      - Discover →
      - Offer ←
      - Request →
      - Acknowledgment ←
    - The DHCP server leases the ip address to the client, when the time passes, the DORA process is performed again(without ‘DO’), the server may offer the same IP if it wasn’t taken.
    - DHCPv6 uses SARR:
      - Solicit →
      - Advertise ←
      - Request →
      - Reply ←
  - DNS
    - Maps an IP to a domain(human readable format)
    - Device → Queries → DNS server
    - Device ← Reply ← DNS server
    - DNS server Recursion, one server may request an IP from another server.
    - DNS zones divide responsibility for namespaces.
      - AXFR and IXFR
      - “The data contained in a zone transfer can be very dangerous in the wrong hands. For example, by enumerating a single DNS server, you can map a network’s entire infrastructure.”
  - HTTP
    - The Hypertext Transfer Protocol is the delivery mechanism of the World Wide Web.
    - An HTTP server is usually on TCP port 80.
    - HTTP header:
      - Header example: “GET / HTTP/1.1\r\n”
      - This information tells us that the client is sending a request to download (GET) the root web directory (/) of the web server using version 1.1 of HTTP.
  - SMTP
    - Simple Mail Transfer Protocol.
    - When you send a message, it’s sent from your MUA to a mail transfer agent (MTA). The MTA is often referred to as the mail server, with popular mail server applications being Microsoft Exchange or Postfix.
    - The MTA must use DNS to find the location address of the recipient mail server.
    - Retrieving email from a mailbox on a SMTP server is done with POP3 or IMAP. TLS encryption is used.
  - Port mirroring is probably not an option on the SOHO switch. Our best bet is to use a cheap tap(hardware) or to perform ARP cache poisoning(man in the middle attack // software) to intercept these packets.
  - A computer will typically use its hosts file as the authoritative source for DNS name-to-IP address mappings, and it will check that file before querying an outside source. `/etc/hosts`
