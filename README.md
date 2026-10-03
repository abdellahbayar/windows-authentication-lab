# Windows Authentication Lab

A hands-on lab analysing successful and failed authentication attempts through Windows Security logs.

## Environment

Windows 11 Enterprise Evaluation · Hyper-V · Event Viewer  
Local accounts: `LabAdmin` and `LabUser`

## Investigation

I generated controlled authentication attempts using `runas`, then inspected the corresponding events in Event Viewer.

| Time | Event ID | Account | Result |
|---|---|---|---|
| 13:58:39 | 4625 | LabUser | Incorrect password |
| 13:58:52 | 4624 | LabUser | Successful authentication |
| 14:29:46 | 4625 | UtenteInesistente | Unknown username |

*1 October 2026 — local time, UTC+02:00.*

## Findings

- `Subject` identifies the requesting account; the target account identifies the account being authenticated.
- SubStatus `0xC000006A` indicates an incorrect password; `0xC0000064` indicates an unknown username.
- Logon Type `2` and loopback address `::1` are consistent with the local tests.
- LabUser’s failed attempt was followed by a successful authentication approximately 13 seconds later.

These were controlled lab tests. A failure followed by a success does not, by itself, establish a compromise.

## Documentation

- [Analysis report](Windows-Authentication-Log-Analysis-Abdellah-Bayar.pdf)  
  Investigation workflow, event interpretation and conclusions.

- [Lab reproduction guide](Windows-Authentication-Lab-Reproduction-Guide.pdf)  
  Step-by-step setup, authentication tests and verification with screenshots.

## Evidence

Key fields recorded in the three selected events:

| Field | Incorrect password | Successful authentication | Unknown username |
|---|---|---|---|
| Event ID | `4625` | `4624` | `4625` |
| Target account | `LabUser` | `LabUser` | `UtenteInesistente` |
| Logon Type | `2` | `2` | `2` |
| Source address | `::1` | `::1` | `::1` |
| Status | `0xC000006D` | Not applicable | `0xC000006D` |
| SubStatus | `0xC000006A` | Not applicable | `0xC0000064` |

### Screenshots

Open the annotated screenshots to verify the recorded fields:

- [Incorrect password — Event 4625](01-accesso-fallito.png)
- [Successful authentication — Event 4624](02-accesso-riuscito.png)
- [Unknown username — Event 4625](03-utente-inesistente.png)
