# Winter '27 readiness assessment

Date: 2026-09-29, Australia/Sydney. Target: tradelink (verified by live Organization query); Developer Edition, non-sandbox, USA876.

## Outcome

No breaking defect was identified in the inspected baseline. This is a limited pre-upgrade assessment, not certification that nothing can break. The org remains on Summer '26; post-upgrade regression and interactive user journeys are not tested here.

## Notice and exact schedule

Read both messages in Gmail titled Winter '27 Release 1 Week Notification, sent by Salesforce on September 29 at 10:11 and 10:14 Sydney time, addressed respectively to Ironbark and TradeLink project aliases. They say approximately one week and refer to Trust for timing; no exact date is stated in the emails. No email addresses or tracking URLs are reproduced in evidence.

The live Salesforce Trust detail for USA876 shows confirmed Winter '27 Major Release, maintenance 20136502, October 3, 2026, 15:00–15:30 AEST (05:00–05:30 UTC). This is Saturday this week, not the following week. Availability: generally available during the window. The generic email's five-minute interruption applies only to non-Hyperforce orgs; do not assume it describes this instance.

Source: https://status.salesforce.com/maintenances/20136502

## Actual validation

- Queried Organization, ApexClass, ApexTrigger, FlowDefinition and ReleaseUpdate using explicit target alias. All succeeded. Exact SOQL and sanitized responses are in adjacent JSON files.
- Four Apex classes, all packaged devedapp at API 64; no custom Apex or triggers. One inactive standard report-export protection Flow; no active Flow.
- Metadata API inventories: no ConnectedApp, NamedCredential, ExternalDataSource, ApexPage or AuraDefinitionBundle components returned. Seven Lightning web components, all devedapp. No custom __c objects returned among 376 listed object components.
- Workflow metadata listed Case; a separate Tooling WorkflowRule query returned zero rules. These are distinct results, not a claim that the Workflow metadata file is absent.
- Ran all org Apex tests using sf apex run test --target-org tradelink --test-level RunAllTestsInOrg --wait 1 --json. Two packaged tests passed, zero failed or skipped. Test-run ID and individual test outcomes are in tests.json. These tests do not exercise a custom business implementation.
- Initial CLI calls failed certificate validation (UNABLE_TO_VERIFY_LEAF_SIGNATURE). Retried successfully with process-local NODE_OPTIONS=--use-system-ca, retaining TLS verification. No global configuration or certificate validation bypass was applied.

## Release-update exposure

Live ReleaseUpdate records have Winter '27 entries Pending for instanced API URLs, profile filtering, three accessibility changes and order fee-tax behavior. Their generic DueDate is September 1; it is not the instance's maintenance date. Presence in this list alone does not prove a used feature or a failure.

- Instanced API URLs: the authenticated CLI uses each org's My Domain URL and current API 67. No source integration implementation exists in the inspected repository, and no ConnectedApp metadata was returned. External clients and centrally distributed OAuth applications were not exhaustively inventoried; any such clients must use My Domain, not an incorrect instance hostname.
- Profile filtering: ordinary users may no longer see other profile names without View All Profiles. No custom code dependency was found. Actual non-admin UI behavior remains untested; no broad permission was granted.
- Accessibility: page headers, modals, date pickers and related layouts may change. No custom Aura/LWC was found, but zoom and standard screen regression were not exercised.
- Order fee-tax: Order count is recorded separately; zero orders were observed. No operational tax-calculation scenario was tested.
- Official current release notes describe later OAuth and email enforcement dates, while org metadata still includes older informational labels. Do not infer next-week enforcement from an old title. Future integration/authentication changes need their own assessment if those features are introduced.

Official reference: https://help.salesforce.com/s/articleView?id=release-notes.rn_ru.htm&release=264&type=5

## Changes and limits

Only dated evidence was added to this repository. No Salesforce deployments, configuration toggles, activations, business-data edits or credential changes were made. Apex tests were executed in the named org.

Still required for full assurance: post-upgrade test rerun; browser checks of core Sales/Service screens and high zoom; verification of any external integration clients, managed-package vendor compatibility, email delivery and non-admin profile visibility. No Winter '27 preview org was available in the authenticated org inventory, so these current-release tests cannot establish future-release behavior.

Supporting files contain only configuration names, counts, release information and sanitized test outcomes. Reviewed for credentials, tokens, private keys, email addresses and customer records before publication. Repository diff whitespace check performed before commit.
