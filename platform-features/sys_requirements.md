# System requirements

TokenD is a highly modular system built using the microservices architecture. Such an approach used on top of FBA ensures high level of scalability and fault tolerance. The table below specifies software and hardware requirements for the TokenD system.

| vCPU number | RAM\(GiB\) | Purpose | Required number | Additional requirements |
| :---: | :---: | :---: | :---: | :---: |
| 2 | 8.0 | Core, Horizon | 4 | 200 GB disk and S3-compatible object storage |
| 2 | 4.0 | PSIM | 2 | 500GB NVMe disk storage |
| 2 | 4.0 | API, Frontend | 2 | S3-compatible object storage, Managed DB instance |
| 2 | 4.0 | Events Checker | 1 | 100 GB disk |
| 2 | 4.0 | Monitoring services | 1 | 100 GB disk |
| 1 | 2.0 | VPN gateway | 1 | — |
| 1 | 2.0 | Admin Key Server | 1 | Managed DB instance |
| 2 | 4.0 | Notificator | 1 | Managed DB instance |

