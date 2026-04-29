#### Lab 8 - SSL Decryption

| Device             | Username/PW        |
| ------------------ | ------------------ |
| FortiAuthenticator | admin/fortinet4A!! |
| FortiManager       | admin/fortinet4A!! |

> [!NOTE]
> **The problem SSL inspection solves**
>
> The vast majority of internet traffic today is encrypted with TLS. This is excellent for privacy — but it also means that without SSL inspection, your firewall is effectively blind to what is inside that traffic. Malware command-and-control, data exfiltration, phishing payloads, and web-based attacks all travel inside HTTPS connections that a standard firewall passes without inspection. Studies consistently show that over 50% of malware now uses encrypted channels — meaning that a network without SSL inspection is missing more than half of its threat surface.
>
> **How SSL inspection works:**
> The FortiGate performs a controlled man-in-the-middle on outbound HTTPS connections. When a client connects to an HTTPS site, the FortiGate intercepts the connection, establishes its own TLS session with the destination server, then re-encrypts the traffic and presents a new certificate to the client — one signed by your internal CA. The client trusts this because the internal CA is installed as a trusted root on all managed devices. The FortiGate can then inspect the decrypted content using application control, antivirus, web filtering, and IPS — and re-encrypt it before forwarding.
>
> **Full SSL Inspection vs. Certificate Inspection:**
> - **Certificate Inspection** — The FortiGate only inspects the certificate presented by the server, not the traffic contents. It can block sites with invalid or expired certificates, or match domain names, but it cannot see inside the encrypted payload. This has zero performance impact and requires no CA deployment.
> - **Full SSL Inspection** — The FortiGate decrypts, inspects, and re-encrypts all traffic. This is what enables antivirus, IPS, and DLP to function on HTTPS traffic. It requires a trusted CA certificate to be deployed to client devices.
>
> **Why a wildcard certificate is recommended:**
> The CA certificate you create here must be installed as a trusted root on every client device that will have its traffic inspected. If you later change the HTTPS certificate on the FortiGate, clients may see certificate errors. Using a wildcard or long-lived dedicated SSL inspection CA certificate avoids this operational overhead — the CA stays stable even as leaf certificates are rotated.
>
> **Privacy and exemptions:**
> Not all traffic should be inspected. Banking sites, health portals, and other sensitive categories can and should be exempted from SSL inspection for privacy reasons. FortiGate's SSL inspection profile supports category-based and domain-based exemptions, allowing you to inspect the bulk of traffic while respecting user privacy for sensitive destinations.

1. On FortiAuthenticator, navigate to Certificate Management > Certificate Authorities > Local CAs and create new
  - Certificate ID: ssld_fortiacme_net
  - Intermediate CA: int_fortiacme_net
  - Name (CN): ssld.fortiacme.net
  - Validity period: 365 days

2. Export key and cert
  - Set password to: fortinet1

3. On FortiManager, navigate to Policy & Objects > Advanced > Dynamic Local Certificate (likely under ...) and click Create New
  - Name: ssld_cert
  - Create Per Device mapping
    - Mapped Device: FGTBr01
    - Click Import Certificate and import the CA you created via PKCS12
    - Click OK
    - Local Certificate: select ssld_fortiacme_net from the dropdown
    - Click OK
  - Click OK and add change notes

> [!TIP]
> **Dynamic Local Certificates** in FortiManager let you use a single named certificate object in your security profiles, with per-device certificate mappings underneath. This means your SSL inspection profile references "ssld_cert" everywhere, and FortiManager handles delivering the correct certificate to each FortiGate — even if different branches use different CAs. This is the right approach for multi-site deployments.

4. Navigate to Policy & Packages > Security Profiles > SSL/SSH Inspection and click Create New
  - Name: SSLD Inspection
  - Inspection Method: Full SSL Inspection
  - CA Certificate: ssld_cert
  - DNS over QUIC: Block
  - Click OK and add change note

> [!NOTE]
> Blocking DNS over QUIC prevents clients from bypassing SSL inspection by using QUIC-based DNS resolution, which would allow them to resolve names without the FortiGate seeing the query. This is an important complementary setting when enabling SSL inspection.

5. Navigate to Policy & Objects > Policy Packages > FGTBranch Policy and edit one of your internet-bound SSIDs (802.1x is suggested)
  - SSL/SSH Inspection: SSLD Inspection
  - Click OK and add change notes

6. Push the configuration to FGTBr01 and verify that HTTPS traffic from a connected client is being inspected

#### Lab complete — move on to Lab 9
