# Handoff: website page for the Lexware Office Migrator

For the agent that builds the page. Everything on this page comes from the
module source, its README, the migration plan and a live Odoo 20 database
with the migrated Lexware demo account. Do not add a claim that is not here.

## 1. The task

Build one product page for the **Lexware Office Migrator**, a free Odoo
module of bloopark systems. The page explains what the module does, shows it
with screenshots, and gives one call to action: contact bloopark for help
with a migration.

## 2. Where the material is

| What | Where |
|---|---|
| Repository | `https://github.com/blooparksystems/lexware` (private) |
| Branch with this handoff | `docs/LXM-7_website-handoff` |
| This file | `docs/website-handoff/HANDOFF.md` |
| Screenshots (13 PNG, 1440 x 900) | `docs/website-handoff/screenshots/` |
| Module icon | `addons_bp/lexware_migrator/static/description/icon.svg`, `icon.png` |
| bloopark logo | `addons_bp/lexware_migrator/static/description/images/bloopark-logo.svg` |
| Feature icons (SVG) | `addons_bp/lexware_migrator/static/description/images/*.svg` |
| Odoo Apps store page (layout and copy reference) | `addons_bp/lexware_migrator/static/description/index.html` |
| Module README | `addons_bp/lexware_migrator/README.rst` |
| Migration plan | `docs/migration-plan.md` |

## 3. Product facts

| Fact | Value |
|---|---|
| Name | Lexware Office Migrator |
| Technical name | `lexware_migrator` |
| One line | Migrate the data of a Lexware Office account into Odoo |
| Maker | bloopark systems GmbH & Co. KG, Magdeburg, official Odoo partner |
| Version | 1.0 |
| Odoo versions | 18.0, 19.0, 20.0 (one branch per version) |
| Editions | Community and Enterprise, on-premise and Odoo.sh |
| Price | Free. No paid tier. No feature behind a payment. |
| Licence | LGPL-3 |
| Odoo dependencies | Accounting (`account`), Sales (`sale_management`), `mail` |
| Source system | Lexware Office (formerly lexoffice), Public API `https://api.lexware.io/v1` |
| Contact | sales@bloopark.de |
| Category | Tools, data migration |

What it is: a **one-time migration** from Lexware Office into Odoo.
What it is not: a connector or a live sync. Do not write "sync",
"integration" or "connector" on the page.

Target reader: a German company (or its Odoo partner or tax advisor) that
leaves Lexware Office and starts its accounting in Odoo.

## 4. Messages, in order of weight

1. Move from Lexware Office (formerly lexoffice) to Odoo.
2. The documents keep their Lexware numbers. The open amounts in Odoo are the
   same as in Lexware.
3. The migrator only reads from Lexware. It sends GET requests only. It never
   writes into the Lexware account.
4. A second run makes no duplicate. It updates contacts, articles and draft
   documents, and skips posted documents.
5. Nothing wrong is posted. A document whose total differs from Lexware stays
   a draft, and the log lists it.
6. It runs inside Odoo. No separate service.
7. It is free.

## 5. Features

### What is migrated

| Lexware Office | Odoo |
|---|---|
| Contacts (customers, vendors, both) | Contacts: company or person, customer or vendor number, VAT ID, email, phone, billing address. The shipping address becomes a delivery contact. A contact person becomes a child contact. |
| Articles | Products and services: number, GTIN, net price, unit, sales and purchase tax. |
| Open quotations | Quotations with the Lexware number. |
| Open order confirmations | Confirmed sales orders with the Lexware number. |
| Invoices and down-payment invoices | Posted customer invoices with the Lexware number, lines, discounts, taxes, payment term and the PDF. A closing invoice gets one negative line per down payment. A voided invoice is cancelled and keeps its number. |
| Credit notes | Posted credit notes, linked to their invoice, with the PDF. |
| Bookkeeping vouchers (purchase and sales receipts) | Vendor bills, refunds or customer invoices. One line per voucher item with posting category, tax rate and amount. The receipt file is attached. |
| Payments | Bank or cash payments, reconciled with the document. A cash discount or an irrecoverable receivable becomes a credit note that corrects the VAT (§ 17 UStG). |

### How it works

1. **Connect.** Enter the Lexware Office API key. *Test Connection* shows the
   Lexware organization and each setup problem before the first run.
2. **Map.** *Fill Mappings* proposes the Odoo tax for each Lexware tax type
   and rate on SKR03 or SKR04. Add an account for each posting category of
   the vouchers. Common units map automatically.
3. **Run.** *Run Migration* starts a scheduled job. It commits after each
   batch of 25 records. After a server restart or an API rate limit, it
   continues at its position.
4. **Check.** The chatter shows the result of each step (created, updated,
   skipped, failed, warnings). The log shows each failure and warning.

### Scope options

- Open items only (default): only the documents with an open amount.
- Full history: all finalized documents.
- *Documents From*: only documents of a start date and later, for example the
  first day of the fiscal year.
- Document files: attach the PDF of each document and the files of each
  voucher.
- Open quotations and orders: on or off.

### Standard Odoo flows

Invoices are posted, payments are reconciled, and cancelled invoices are
cancelled with the standard Odoo methods. The module has its own access
group (*Lexware Migration / Administrator*).

### Quality assurance

- Automated tests replay a recorded Lexware Office account. They check each
  document type, the taxes, the payments, the cash discounts, the bad debts
  and the rerun. The 20.0 branch has 23 test methods. The store page says 25.
  On the website, write "more than 20 automated tests".
- Before each release, bloopark migrates a live Lexware Office test account
  into a new Odoo database and compares each document with Lexware.

## 6. Requirements

- German localization (`l10n_de`) with SKR03 or SKR04.
- Odoo Accounting and Sales.
- Company currency EUR. Tax rounding "Round Globally".
- A Lexware Office plan with the Public API. The API key comes from
  Add-ons > Public API in Lexware Office.
- A separate sales journal for the migration, because the documents keep
  their Lexware numbers.

## 7. Limits (say them on the page, do not hide them)

- EUR only.
- Lexware Office (cloud) only. Lexware desktop products (buchhalter,
  faktura+auftrag) have no API and are out of scope.
- The Lexware API has no chart of accounts, no journal entries, no bank
  transactions and no opening balance. The opening balance comes from the tax
  advisor (DATEV).
- Dunnings, delivery notes, recurring templates and drafts are not migrated.
- Lexware allows 2 requests per second and at most 10,000 rows per list. A
  larger account is migrated in parts with *Documents From*.
- A total can differ by one cent on documents with gross prices. Such a
  document stays a draft and the log lists it.
- A negative payment item (for example a returned debit) is booked by hand.

## 8. Screenshots

All screenshots show Odoo 20 Enterprise in dark mode, 1440 x 900 pixels, with
the Lexware demo account. The data is fictitious (Muster Gastro GmbH, Erika
Musterfrau). The API key is masked. The company name on the screen is
"bloopark demo GmbH".

| File | Shows | Suggested caption | Use for |
|---|---|---|---|
| `00-home-app.png` | Odoo home screen with the Lexware Migration app | The migrator is an app inside Odoo. | Intro, "runs inside Odoo" |
| `01-backend-form.png` | Backend form: connection, scope, journals, buttons Test Connection, Fill Mappings, Run Migration, state Done, chatter with the result of each step | One form for the connection, the scope and the run. | **Hero image** |
| `02-mappings.png` | Mappings tab: Lexware tax keys (`de:19`, `intraCommunitySupply:0`) and their Odoo taxes | Each Lexware tax type and rate maps to an Odoo tax. | Step "Map" |
| `03-log.png` | Log list: one line per step with the counts | The log shows each step, failure and warning. | Step "Check" |
| `04-invoices.png` | Customer invoices RE0001 to RE0008 with states Paid, Partially paid, Posted, Reversed, Cancelled | Invoices keep their Lexware numbers and their payment state. | "Documents keep their numbers" |
| `05-invoice-RE0005.png` | Invoice RE0005, partially paid, with the Lexware PDF beside it | The invoice in Odoo, with the original Lexware PDF attached. | Invoices feature |
| `06-credit-notes.png` | Credit notes GS0001, GS0002 | Credit notes are linked to their invoices. | Credit notes feature |
| `07-vendor-bills.png` | Vendor bills from bookkeeping vouchers | Bookkeeping vouchers become vendor bills. | Vouchers feature |
| `08-vendor-bill.png` | Vendor bill with posting category "Wareneinkauf", 7 % tax and the receipt file | The voucher with its posting category and its receipt. | Vouchers feature |
| `09-contacts.png` | Customers with email, phone, country, delivery contact and contact person | Customers and vendors with addresses and contact persons. | Contacts feature |
| `10-products.png` | Products with price and article number | Articles become products with price and number. | Articles feature |
| `11-sales-order-AB0001.png` | Confirmed sales order AB0001 from a Lexware order confirmation | Open order confirmations become confirmed sales orders. | Quotations and orders feature |
| `12-payments.png` | Customer payments, Paid and Reconciled | Payments are reconciled with their documents. | Payments feature |

Alt text: use the "Shows" column.

## 9. Suggested page structure

1. **Hero.** Headline, one sentence, call to action, `01-backend-form.png`.
   Badges: Free, Odoo 18 / 19 / 20, LGPL-3.
2. **Flow strip.** Lexware Office (Public API, read only) → Lexware Office
   Migrator (mapping, batches, log, rerun) → Odoo (contacts, products,
   invoices, bills, payments).
3. **Six promises.** Runs inside Odoo. Reads only. Documents keep their
   numbers. Open items or full history. Rerun without duplicates. Every
   failure in a log.
4. **What is migrated.** Six cards: Contacts, Articles, Quotations and
   orders, Invoices and credit notes, Bookkeeping vouchers, Payments. One
   screenshot each.
5. **How it works.** Four steps: Connect, Map, Run, Check. Screenshots 01,
   02, 03.
6. **Quality assurance.** Automated tests and the live test.
7. **Requirements and limits.** Two lists, from sections 6 and 7.
8. **Price.** "The module is free." If you install Odoo modules yourself, you
   do not need us. If you want help with the mapping, the run or the check of
   the migrated data, write to us.
9. **Call to action.** "Request help with the migration", sales@bloopark.de.
10. **About bloopark.** bloopark systems is an official Odoo partner based in
    Magdeburg. It supports mid-sized and larger companies across the DACH
    region on ERP, commerce and integration projects.

## 10. Copy

Headline options:

- Move from Lexware Office to Odoo.
- Lexware Office to Odoo, with your invoice numbers and your open items.

Subline:

> The migrator reads your contacts, articles, invoices, credit notes,
> bookkeeping vouchers and payments through the Lexware Office Public API and
> creates them in Odoo. Your documents keep their Lexware numbers, and the
> open amounts are the same as in Lexware.

Search terms (the readers are German): Lexware Office Odoo Migration,
lexoffice zu Odoo, lexoffice Odoo Umstieg, Lexware Office Daten nach Odoo
übertragen, lexoffice Rechnungen importieren Odoo, Odoo SKR03 SKR04
Migration.

## 11. Rules for the page

- State facts only. No superlative and no number that is not in this file.
- "Lexware", "Lexware Office" and "lexoffice" are trademarks of their owner.
  Write "Lexware Office (formerly lexoffice)" at the first mention. Do not
  use the Lexware logo. Do not suggest a partnership with Lexware.
- Do not promise a complete accounting migration. The opening balance, the
  chart of accounts and the bank transactions are not in the Lexware API.
- The call to action is contact with bloopark, not a purchase.
- The repository is private. Do not link to it from the public page.

## 12. Open points

- Page language: not decided. The copy here is English. The readers are
  German, so a German version is likely needed.
- The link to the Odoo Apps store listing: not available yet.
- The screenshots are in dark mode. Ask for light mode screenshots if the
  website design needs them.
