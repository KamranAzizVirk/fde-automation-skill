# Module: Website

Part of the FDE Automation Skill. The rules in SKILL.md and references/common-standards.md always apply on top of this file.

Covers: business site, landing page, portfolio, blog, or e-commerce site.
Roles: Web Frontend Expert, Security Expert, QA Expert. Add Cloud Automation Expert for hosting and forms. Add a Language Expert for each approved language (SKILL.md, Section 7.5).
Express Mode fits one landing page or a small static site. Use Full Mode for e-commerce, logins, payments, or many pages.

## Extra Intake Questions
1. Type (clickable): landing page, business site, portfolio or blog, online shop.
2. Goal of the site: get calls, get leads, sell products, show work, other.
3. Do you already have a domain name and hosting? (yes, no, not sure)
4. Do you have the text, logo, and photos, or should placeholders be used?
5. Languages of the site (Urdu, English, both). If Urdu is used, the site must support right-to-left (RTL) layout and a readable Urdu font.
6. Who updates the site later: you, the client, or Claude on request?

## Workflow (Added to the Shared Phases)

### A. Pages and Structure (Phase 2)
1. The owner gives pages, or Claude proposes a sitemap. The owner approves it.
2. Content checklist per page: title, short description, headings, text, call-to-action button, image alt text.

### B. Design Options (Phase 7)
1. Show 2 to 3 design previews in the Artifact Panel. The owner chooses one.
2. Mobile first: most visitors use phones. Check the phone view before the desktop view.
3. Colours, fonts, and logo text come from the client brand. If none exist, offer options.
4. Use images the owner has the right to use. Claude does not copy images or text from other sites. Photos of real people need their permission.

### C. Forms and Data
1. Validate on the server and in the browser.
2. Spam defence (for example a hidden field or a simple challenge). Do not rely on the browser alone.
3. Say clearly what data is collected and why. Get consent where the region requires it. Link a privacy policy.
4. Do not send form data to any address that was not approved by the owner.

### D. E-commerce (Only for Online Shops)
1. Product list, prices, stock, delivery, and returns come from the owner. Claude never invents them.
2. Payments use the payment provider's hosted checkout. The site never stores card numbers. Confirm which providers work for the client's country (including local options) from the providers' own pages.
3. If money is involved, read references/finance.md for the control rules, and test only in the provider's test mode.
4. Show prices, delivery cost, and total before payment. Send an order confirmation.

### E. Performance, Accessibility, and SEO
1. Targets: the main content appears in about 2.5 seconds on a normal phone connection, with images compressed and sized properly. Check with a standard tool (for example Lighthouse) and report the numbers.
2. Accessibility: readable contrast, labels on form fields, keyboard use, alt text on images.
3. SEO basics: page titles and descriptions, sitemap, robots file, structured data where it fits, readable web addresses.
4. Analytics only with consent where the region requires it.

### F. Security
1. HTTPS, security headers, no secrets in the code or in the browser.
2. Keep libraries updated. Remove unused ones.
3. Admin or login areas use strong passwords, limits on login attempts, and sessions that expire.

### G. Hosting and Launch
1. Compare 2 or 3 hosting options with pros, cons, and cost (for example a static host, Vercel, or shared hosting). Claude checks current plans on the provider's own page.
2. Domain and DNS steps are given to the owner in simple Urdu. Claude does not buy or change a domain without the owner's approval.
3. Pre-launch check: links, forms, phone view, speed, privacy page, footer, 404 page.
4. Fresh owner confirmation before publishing the site, every time. Public pages carry the footer "Powered by FDE Automation Pakistan, Kamran Aziz" unless the client agreement says otherwise.

### H. After Launch
Agree who updates content, how backups work, and what support is included (common-standards.md, Section 8).

### I. Standards Baseline
1. **Accessibility:** target WCAG 2.2 level AA (the international web accessibility standard). Check colour contrast, keyboard use, form labels, focus visibility, text resizing, and alt text. The owner may change the target in Phase 4.
2. **Core Web Vitals (Google's page experience measures), "good" targets:** Largest Contentful Paint (LCP) 2.5 seconds or less, Interaction to Next Paint (INP) 200 milliseconds or less, Cumulative Layout Shift (CLS) 0.1 or less. Measure with Lighthouse (lab) and, after launch, real-user data where available. Report the numbers in Urdu.
3. **Images and fonts:** modern formats (WebP or AVIF), correct sizes, lazy loading below the fold, and a limited number of font files.
4. **Legal pages:** privacy policy, terms (for shops and services), contact details, and a cookie notice where the region requires it. Claude drafts a plain checklist, and the owner or a lawyer confirms the text.
5. **Ownership of accounts:** the domain, hosting, analytics, and email accounts should be registered in the client's own name, with the owner as an added admin if needed. Keep recovery email and phone with the client. Record who holds each account in `docs/HANDOVER.md`.
6. **Monitoring and backups:** uptime monitoring with an alert, automatic backups of site files and any database, and a reminder for domain and certificate renewal.
7. **Browsers and devices:** test on current Chrome, Safari, Firefox, and Edge, on a low-end Android phone, and at small, medium, and large screen widths.

## Gates Checklist for This Module
- G2: sitemap and content list approved.
- G7: design chosen.
- G13: forms, speed, phone view, and accessibility checks passed.
- Launch: fresh owner confirmation.
