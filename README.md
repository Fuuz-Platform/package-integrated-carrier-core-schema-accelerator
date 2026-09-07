# package-integrated-carrier-core-schema-accelerator

**Version:** 0.0.1
**Spec Version:** 2.0.0

---

## Overview

This package creates the complete data model schema required by the Integrated Carrier shipping system. It is the foundational layer of the Integrated Carrier suite and must be installed before any carrier-specific packages (FedEx, UPS) or the core flows and screens packages.

It installs 9 custom data models that define the carrier abstraction layer — a carrier-agnostic schema that normalizes rate quotes, label generation, shipment tracking, and account configuration across multiple parcel carriers.

---

## Package Contents

```
integrated-carrier-core-schema/
├── manifest.json
├── package-data.json
├── install/                     48 install steps (creates model schemas + fields)
└── preinstall/                  9 preinstall steps (uniqueness verification)
```

---

## Data Models Installed (9)

| Model | Kind | Description |
|-------|------|-------------|
| `IntegratedCarrier` | Reference | Named carrier entity (e.g., FedEx, UPS, USPS). Seed records are created by carrier-specific packages. |
| `IntegratedCarrierAccount` | Reference | Carrier account credentials and configuration. Holds the account number, API keys, and environment (production/test) settings per carrier. |
| `IntegratedCarrierBillingType` | Reference | Billing party options for shipments (e.g., Shipper, Receiver, Third Party). Carrier-specific billing type seed data provided by each carrier package. |
| `IntegratedCarrierConnectionConfiguration` | Reference | Carrier API connection settings — endpoint URLs, timeout values, retry configuration. One record per carrier account. |
| `IntegratedCarrierRequest` | Reference | Shipment request record. Captures the originating shipment details (origin/destination addresses, package dimensions, weight, service type, billing type) before submitting to the carrier API. |
| `IntegratedCarrierRequestFlow` | Reference | Maps a request type to its handler flow. Enables the router to dispatch rate quote, label generation, and void requests to the correct carrier-specific flow. |
| `IntegratedCarrierRequestType` | Reference | Classification of request types — Rate Quote, Label Generation, Void Label, Tracking, End-of-Day Close. |
| `IntegratedCarrierService` | Reference | Carrier service level options (e.g., FedEx Ground, FedEx Standard Overnight, UPS Ground). Seed records provided by carrier-specific packages. |
| *(9th model)* | Reference | Supporting reference model for carrier-specific extended configuration (special services, hazmat flags, customs declarations, etc.) |

---

## Install Process

**Preinstall (9 steps):** Verifies that none of the 9 data model IDs or names already exist in the environment. Throws a descriptive error if any conflict is detected, preventing partial or duplicate schema installation.

**Install (48 steps):** For each of the 9 models:
1. Creates the DataModel header record
2. Creates the model version with full field definitions
3. Deploys the model to make it available in the Application API

---

## Installation

1. This package must be installed FIRST in the Integrated Carrier suite
2. Import via Fuuz Package Manager
3. Verify all 9 models are visible in the Fuuz Data Model Explorer after installation
4. Proceed to install `package-integrated-carrier-core-flows-accelerator` and `package-integrated-carrier-core-screens-accelerator`
5. Then install carrier-specific packages: `package-integrated-carrier-fedex-accelerator`, `package-integrated-carrier-ups-accelerator`

---

## Dependencies

- **Fuuz Platform** — any version with specVersion 2.0.0 package support
- No other Fuuz packages required as prerequisites
- Carrier-specific packages (FedEx, UPS) must be installed AFTER this package

---

## Part of the Integrated Carrier Suite

| Package | Description |
|---------|-------------|
| **integrated-carrier-core-schema** (this) | Data models |
| `integrated-carrier-core-flows` | Router and print flows |
| `integrated-carrier-core-screens` | Shipment management screens |
| `integrated-carrier-fedex` | FedEx seed data and label flows |
| `integrated-carrier-ups` | UPS seed data and label flows |
| `integrated-carrier-addon-plex` | Plex ERP integration extension |

---

*Built on the [Fuuz Industrial Operations Platform](https://fuuz.com)*

## Service levels

No service level agreement applies to anything published here. It becomes a supported
deliverable only once it has been implemented by a Fuuz services professional or an
approved Fuuz partner.
