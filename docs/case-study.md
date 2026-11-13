# Case study: a connected lead-to-delivery workflow

## Context

The reviewed application supports a service business that helps customers form and maintain companies. Its scope includes sales, quoting, payments, onboarding, operations, client progress, and management reporting.

The challenge is continuity: the next person in the process needs the context collected by the previous team. A consultant's deal should become an operations file without losing its service selection, payment context, or customer progress.

## Workflow

1. Capture and qualify an enquiry from an intake channel.
2. Match a repeat open enquiry and route the lead to a consultant.
3. Record activity and maintain a next follow-up.
4. Prepare a proposal for the selected service.
5. Reconcile payment with the provider and the internal ledger.
6. Close the deal with the service sold made explicit.
7. Collect onboarding details and required documents.
8. Track the filing and delivery stages.
9. Expose appropriate progress to the client.

## Decisions worth discussing in an interview

### Shared intake

The reviewed contact-normalization code makes the matching key tolerant of formatting and country prefixes. Matching remains scoped to an open lead and brand. This avoids treating every repeated contact as a new opportunity while allowing future purchases to become new leads.

### Stage validation

The stage-guard code checks the facts required by the next status, including activity and next-action dates. The public demo recreates a much smaller version: follow-up requires a date; Proposal requires a quote; Won requires a simulated full payment. These simplified rules illustrate the pattern rather than reproducing production behavior.

### Received money comes from the ledger

The reviewed ledger helper sums received payment rows, including currency conversion where applicable. Close-time logic determines what amount, if any, is actually new money. This illustrates a practical consistency issue: payment can arrive before the deal closes, and the closing action should not duplicate it.

### Server-side staff checks

The reviewed authorization helper verifies the authenticated user's staff record and allowed roles. A portal user can have a valid session without being a staff member. This is why a session check alone is insufficient for a privileged server action. Database policies complement these application checks; this case study does not constitute a security audit.

## Review evidence

These paths in the private handover informed the presentation:

| Topic | Private source reference |
| --- | --- |
| System use case and process | `HANDOVER.md`, sections 1–8 |
| Application dependencies | `package.json` |
| Intake normalization | `src/lib/phone-dedupe.ts` |
| Transition rules | `src/lib/stage-guard.ts` |
| Ledger reconciliation | `src/lib/payments-ledger.ts` |
| Staff and money permissions | `src/lib/require-staff.ts` |

The public repository does not reproduce the source files, confidential comments, identifiers, live domains, or customer examples used in those files.

## Honest scope

This is a project-level technical presentation, not a claim that one person authored every module. There are no verified production outcome metrics in this case study. The interactive artifact is a separate, fictional-data implementation for evaluation and conversation.
