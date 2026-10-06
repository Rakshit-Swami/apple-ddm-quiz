# Apple Certified IT Professional – Study Notes

> 📖 **How to use this:** Read the MEMORY LINE boxes first. Then drill the comparison tables. Finish with the 2-minute cram page and rapid self-test before your exam.
>
> ⚠️ These notes are for personal revision only. Confirm against the current Apple exam objectives if your exam version differs.

---

## 1. Device Management Fundamentals

> 💡 **MEMORY LINE** — Enrollment establishes management; profiles package settings; payloads configure specific capabilities; commands perform immediate actions; declarative management maintains desired state.

| Term | Remember |
| --- | --- |
| Enrollment | Establishes the relationship between device and MDM/device management service |
| Enrollment profile | Information needed for enrollment and identification of the management service |
| Configuration profile | Legacy XML `.mobileconfig` settings package |
| Configuration | Modern JSON-based payload collection |
| Payload | Specific setting/capability: Wi-Fi, VPN, certificate, account, restriction, etc. |
| Command | Immediate action: lock, erase, restart, update, install/remove app, etc. |
| Query | Requests information: OS, battery, apps, Activation Lock status, etc. |
| Declarative management | Device evaluates desired state, applies it, retries, and reports status |

> 🎯 **Exam trigger:** If the question says "do something now" → **command**. If it says "keep device in a desired state" → **declarative management/configuration**.

> 💡 **MEMORY LINE** — MDM commands act, configurations persist, and queries report.

---

## 2. Enrollment Methods & Ownership

| Method | Typical ownership | Privacy | IT control | Think |
| --- | --- | --- | --- | --- |
| User Enrollment | BYOD | Highest | Limited | Protect personal data |
| Device Enrollment | Personal or org-owned | Moderate | Broader | Manual / broader management |
| Automated Device Enrollment | Organization-owned | Org-controlled | Greatest | Automated + supervised |

> 💡 **MEMORY LINE** — User Enrollment protects BYOD privacy; Device Enrollment provides broader manual management; Automated Device Enrollment automatically supervises organization-owned devices and gives IT the greatest control.

- Automated Device Enrollment: Apple Business/School Manager → assign device to MDM → Setup Assistant → automatic enrollment
- Supervision is the key control concept for organization-owned deployments
- A device can have only **one** enrollment profile at a time

### Platform-specific deployment

| Device | Notes |
| --- | --- |
| Apple TV | Supervised kiosk/conference-room/AirPlay/app deployments |
| Apple Watch | Managed through its paired/supervised iPhone |
| Apple Vision Pro | Privacy-aware device management and organization-controlled configuration |
| Shared iPad | Supervised, organization-owned iPad + Managed Apple Accounts + cloud-backed sessions |
| Mac | Device/user channels, local accounts, Platform SSO, FileVault, bootstrap tokens |
| iPhone/iPad | Generally device-level management (usually one active user) |

---

## 3. Apple Business & Apple Configurator

> 💡 **MEMORY LINE** — Configurator prepares and supervises unsupported-purchase devices, then automates repeatable deployment with Blueprints and workflows.

**Apple Business manages:** devices, users, roles, organizational units, Managed Apple Accounts, apps/books, MDM assignments, suppliers, and API automation.

**Flow:** Supplier → Apple Business → MDM → device assignment → Automated Device Enrollment → Setup Assistant

- Configurator is especially useful for devices **not** purchased through Apple, an authorized reseller, or authorized carrier
- Prepare Assistant can: supervise, enroll, install profiles/apps, configure Wi-Fi, skip Setup Assistant panes, and add devices to Apple Business
- **Blueprints** record repeatable profiles/apps/actions; Shortcuts and cfgutil support automation
- Manually added devices receive mandatory supervision and management enrollment; 30-day provisional release period applies

---

## 4. Network — Numbers to Memorize

| Traffic / Service | Key fact |
| --- | --- |
| Device → APNs | **TCP 5223** |
| MDM server → APNs | **TCP 443 or 2197** |
| Apple network range | **17.0.0.0/8** |
| MDM service | Usually HTTPS / TCP 443 |
| APNs certificate | Keep valid — annual renewal |
| CSR | SHA-2-compatible signing |

> 💡 **MEMORY LINE** — Device 5223, MDM server 2197/443, Apple network 17.0.0.0/8, APNs certificate yearly, CSR uses SHA-2.

### Wi-Fi

- Use few SSIDs; **don't hide SSIDs**
- Use **5 GHz** for high client density
- 2.4 GHz non-overlapping channels: **1 / 6 / 11**
- Survey before and after deployment; measure with the actual target device
- Plan capacity, not just advertised link speed; enable Bonjour where Apple discovery requires it

> 💡 **MEMORY LINE** — Wi-Fi is shared; use ~65% of advertised link rate as a rough real-world throughput estimate, then divide by application demand to estimate clients.

| Term | Meaning |
| --- | --- |
| Link rate | Advertised/negotiated speed |
| Throughput | Useful speed after shared airtime, half-duplex, interference and protocol overhead |

---

## 5. Content Caching

> 💡 **MEMORY LINE** — First device downloads from Apple; later devices download from the Mac cache.

- Use wired Ethernet, preferably Gigabit or faster; provide enough storage
- Monitor CPU/memory/storage/network use; avoid proxy issues and rogue caches
- Large deployments can use Parents and Peers; ListenRanges targets client networks
- DNS TXT records can help with multiple public IPs / advanced cache selection

| Command | Purpose |
| --- | --- |
| `settings` | View configuration |
| `status` | Check operation |
| `activate / deactivate` | Control service |
| `reloadSettings` | Apply updated settings |
| `flush` | Delete cached content |
| `absorb / move` | Transfer cached content |

> 💡 **MEMORY LINE** — Ethernet Mac + USB/Thunderbolt-connected iPhone/iPad + Internet Sharing = tethered content caching.

---

## 6. Identity, SSO & Federation

| Term | Fast meaning |
| --- | --- |
| Authentication | Who you are |
| Authorization | What you can do |
| Federation | Trusted systems share identity |
| IdP | Identity Provider — verifies identity |
| Token | Proof of successful authentication/access |
| SAML | Application sign-in / identity assertions |
| OAuth | Delegated, limited access |
| OIDC | Authentication layer built on OAuth |
| Kerberos | Ticket-based authentication, often for AD resources |

> 💡 **MEMORY LINE** — Authentication = who you are; authorization = what you can do.

> 💡 **MEMORY LINE** — SAML = sign in to apps; OAuth = give apps limited access; OIDC = sign in using OAuth.

### Enrollment SSO vs Platform SSO vs Kerberos

| Technology | Scope / purpose |
| --- | --- |
| Enrollment SSO | Identity app + Extensible SSO + account-driven enrollment; fewer sign-ins; mainly iPhone/iPad/Vision Pro enrollment |
| Platform SSO | macOS identity across login, apps, enrollment, passwords, privileges, Touch ID and shared-user access |
| Kerberos SSO | Authenticates user to AD resources using tickets/TGT; Mac itself does not need to be AD-bound |
| AD binding | Integrates the Mac itself with Active Directory |

> 💡 **MEMORY LINE** — Platform SSO is for macOS; Enrollment SSO is for iPhone, iPad and Apple Vision Pro enrollment.

> 💡 **MEMORY LINE** — Kerberos SSO authenticates the user to AD resources; AD binding integrates the Mac itself with AD.

**Kerberos flow:** Local login → Kerberos SSO → TGT → service tickets → resource checks AD permissions

---

## 7. Apple Business Domains & Managed Apple Accounts

> 💡 **MEMORY LINE** — Verify ownership first; then choose lock, capture, or federation. Capture automatically locks the domain.

| Goal | Action |
| --- | --- |
| Stop new personal Apple Accounts using company domain | Lock domain |
| Make domain accounts Managed Apple Accounts | Capture domain |
| Use company IdP sign-in | Lock first → capture/federate |
| Synchronize IdP users | Federation → optionally directory sync |

- Domain verification: supported IdP verification or DNS TXT record (within 14 calendar days)
- Federation: verified + locked domain → connect IdP → company credentials authenticate Managed Apple Accounts
- Managed Apple Account: organization-owned identity, separate from personal Apple Account
- Personal features (family sharing, personal subscriptions, purchases) are restricted/unavailable

> 💡 **MEMORY LINE** — Domain capture converts domain use to organizational control and automatically locks the domain.

> 💡 **MEMORY LINE** — Managed Apple Account = organization-owned identity, separate from personal data, managed through Apple Business or the IdP.

---

## 8. Setup Assistant, Accounts & Tokens

- Apple Business/School Manager + MDM controls Setup Assistant
- Skip unnecessary panes; **Auto Advance** supports hands-free Mac/Apple TV deployment
- Managed Migration Assistant controls Mac-to-Mac migration during Setup Assistant
- MDM can create local accounts, hidden managed admins, lock usernames, manage admin passwords and privileges

> 💡 **MEMORY LINE** — Secure Token unlocks; Bootstrap Token provisions; Volume Owner authorizes.

| Security concept | Remember |
| --- | --- |
| Secure Token | Authorizes a user to unlock FileVault-protected storage |
| Bootstrap Token | Enables MDM automation for certain security/account operations |
| Volume Owner | On Apple silicon, authorizes important startup/erase/security operations |

---

## 9. Profiles, Payloads & Commands

- Plan by function, platform, OS version, device/user channel, enrollment type, supervision and MDM support
- Avoid duplicate/contradictory settings
- Deleting a profile can remove accounts, certificates, VPN settings, Wi-Fi credentials or access created by it
- Command availability depends on platform, OS, supervision, enrollment, hardware and MDM implementation

> 💡 **MEMORY LINE** — Commands act; configurations persist; queries report.

---

## 10. Software Updates

| Concept | Purpose |
| --- | --- |
| Update | Patch/minor release |
| Upgrade | Major OS release |
| Background Security Improvement | Rapid security fix separate from traditional full OS update |
| Minimum OS version | Protects enrollment/compliance |
| Deferral | Controls when update becomes available |
| Cadence | Controls OS release branch |
| Automatic settings | Controls download/installation behavior |
| Enforcement deadline | Sets latest permitted installation time |
| Declarative management | Keeps retrying until desired state is reached |

> 💡 **MEMORY LINE** — Minimum version protects enrollment, deferral controls availability, cadence controls the OS branch, automatic settings control downloads and installation, enforcement sets the deadline, and declarative management retries until desired state is reached.

---

## 11. Apps, Books & Content

| Content | Key distinction |
| --- | --- |
| App Store app | Publicly available and license-controlled |
| Custom App | Private app made available to specific organizations |
| Unlisted app | Hidden from general App Store discovery; not inherently private |
| In-house app | Organization-developed and self-hosted |
| Book | User-assigned; **non-reassignable** |

> 💡 **MEMORY LINE** — Apps can be reassigned; books are user-only and non-reassignable.

- Apps can generally be assigned to users or devices, revoked, reassigned, installed and removed via MDM
- Content tokens connect Apple Business/School Manager with external MDM; one-year expiry cycle

---

## 12. Wi-Fi, 802.1X, VPN & Filtering

> 💡 **MEMORY LINE** — MDM configures Wi-Fi; the device chooses when to join and roam.

- 802.1X requires a trusted RADIUS server certificate + EAP identity + supported authentication method
- EAP identity can be password-based or certificate-based

| Technology | Scope |
| --- | --- |
| VPN On Demand | Connects automatically when rules require it |
| Per-app VPN | Protects selected managed apps |
| Always On VPN | Tunnels the entire device |
| TLS | Protects application traffic |
| 802.1X | Authenticates network access |
| Network relay | Modern alternative for some VPN use cases |

Content filtering: built-in filters → simple restrictions; DNS → host blocking; proxy → web traffic; advanced filters/VPN → deeper traffic control.

---

## 13. Device Security

> 💡 **MEMORY LINE** — Activation Lock protects lost or stolen devices.

| Security control | Remember |
| --- | --- |
| User-linked Activation Lock | Tied to personal Apple Account + Find My |
| Organization-linked Activation Lock | Controlled through Apple Business/School Manager + MDM |
| Managed Lost Mode | Available for supported supervised iPhone/iPad deployments |
| Managed Device Attestation | Stronger assurance of device identity/hardware/software state |
| Certificates | Authentication, 802.1X, VPN, TLS, smart cards, MDM identity |

---

## 14. Smart Cards & macOS Security

- PIV smart cards support two-factor authentication, signing, encryption and identity authentication
- iPhone/iPad: smart cards via NFC or CCID readers; device generally must be unlocked first
- Mac: smart-card login, directory mapping, Kerberos, Keychain protection and MDM-enforced policies

| macOS security feature | Purpose |
| --- | --- |
| Secure startup | Controls what software is allowed to boot |
| System extensions | Add approved capabilities in user space |
| FileVault | Encrypts data at rest |
| Secure Token | Authorizes user to unlock FileVault |
| Bootstrap Token | Enables MDM automation for certain security operations |
| Volume ownership | Authorizes key Apple silicon startup/erase/security operations |

---

## 15. High-Value "Don't Confuse These" Table

| If you see… | Think… |
| --- | --- |
| User Enrollment | BYOD / privacy |
| Automated Device Enrollment | Org-owned / supervised / automated |
| Configuration profile | Legacy XML settings |
| Configuration | Modern JSON payload collection |
| Payload | Specific setting/capability |
| Command | Immediate action |
| Query | Information/report |
| Declarative management | Desired state / device maintains it |
| Platform SSO | macOS |
| Enrollment SSO | iPhone/iPad/Vision Pro enrollment |
| Kerberos SSO | User → AD resources |
| AD binding | Mac itself → AD |
| Lock domain | Stop new personal accounts on domain |
| Capture domain | Organizational control + automatic lock |
| Federation | Company IdP authenticates Managed Apple Accounts |
| OAuth | Delegated access |
| OIDC | Authentication using OAuth |
| SAML | Application sign-in assertions |
| Per-app VPN | Selected apps |
| Always On VPN | Whole device |
| Link rate | Advertised wireless speed |
| Throughput | Usable speed |
| Secure Token | Unlocks |
| Bootstrap Token | Provisions/automates |
| Volume Owner | Authorizes |
| App license | Generally reassignable |
| Book license | User-assigned / non-reassignable |

---

## 16. Workflows to Recite

### Automated Device Enrollment
Purchase/associate device → Apple Business/School Manager → assign MDM → device starts Setup Assistant → MDM enrollment → supervision/configuration → user completes setup

### Manual Configurator Addition
Required Setup Assistant screen → pair/scan six-digit code → upload serial/hardware info → assign MDM → erase/shut down as directed → user completes Setup Assistant

### Domain + Federation
Verify ownership → lock domain → configure federation with one IdP → users authenticate with company credentials → Managed Apple Accounts are managed through the organization

### Content Caching
First device → Apple → Mac cache → subsequent devices → Mac cache

### Kerberos SSO
Local Mac login → authenticate via Kerberos SSO → receive TGT → request service ticket → internal resource checks permissions

---

## 17. Final 2-Minute Cram

- **Enrollment establishes management**
- **Profiles package settings; payloads configure capabilities**
- **Commands act now; queries report; declarative management maintains desired state**
- **User Enrollment = BYOD privacy**
- **Device Enrollment = broader manual management**
- **Automated Device Enrollment = organization-owned + supervised + greatest control**
- **Configurator = unsupported-purchase devices + prepare/supervise + Blueprints**
- **Device → APNs = 5223**
- **MDM server → APNs = 443 or 2197**
- **Apple network = 17.0.0.0/8**
- **2.4 GHz = channels 1/6/11; Wi-Fi is shared**
- **~65% of advertised rate = rough throughput planning estimate**
- **First cache client gets content from Apple; later clients use cache**
- **Authentication = who; authorization = what**
- **SAML = app sign-in; OAuth = delegated access; OIDC = authentication using OAuth**
- **Platform SSO = macOS; Enrollment SSO = iPhone/iPad/Vision Pro enrollment**
- **Kerberos SSO = AD resources; AD binding = Mac joins AD**
- **Verify domain first; lock/capture/federate afterward**
- **Capture = organizational control + automatic lock**
- **Managed Apple Account = organization-owned identity**
- **Secure Token = unlock; Bootstrap Token = provision; Volume Owner = authorize**
- **Minimum OS = enrollment/compliance; deferral = availability; cadence = OS branch; enforcement = deadline**
- **Apps can be reassigned; books are user-only and non-reassignable**
- **MDM configures Wi-Fi; device decides when to join/roam**
- **802.1X = trusted RADIUS cert + EAP identity**
- **Per-app VPN = selected apps; Always On = whole device**
- **Activation Lock = lost/stolen protection**
- **FileVault = data-at-rest encryption**
- **PIV smart cards = strong authentication, signing, encryption**

---

## 18. Rapid Self-Test

> Cover the right column and test yourself!

| Question | Answer |
| --- | --- |
| Who you are? | Authentication |
| What you can do? | Authorization |
| BYOD privacy? | User Enrollment |
| Org-owned automated setup? | Automated Device Enrollment |
| Mac-wide organizational identity? | Platform SSO |
| AD resources without binding Mac? | Kerberos SSO |
| Mac itself joins AD? | AD binding |
| Immediate action? | MDM command |
| Maintain desired state? | Declarative management |
| Unlock FileVault? | Secure Token |
| MDM provisioning/automation? | Bootstrap Token |
| Apple silicon key authorization? | Volume Owner |
| Selected apps only? | Per-app VPN |
| Whole device tunnel? | Always On VPN |
| APNs device port? | 5223 |
| MDM server APNs port? | 443 / 2197 |
| 2.4 GHz non-overlapping channels? | 1 / 6 / 11 |
| Advertised wireless speed? | Link rate |
| Useful wireless speed? | Throughput |

---

*Based on the Apple IT Training course: [Apple Deployment & Management](https://it-training.apple.com/deployment/tutorials/course/) • For personal revision only • Not official Apple content*
