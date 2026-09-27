# Microsoft Purview Information Protection: Common Scenarios

An interactive, single-page learning and design reference for Microsoft Purview Information Protection sensitivity labels.

The page translates common business scenarios into practical label-design guidance, including classification, encryption, usage rights, content markings, auto-labelling, collaboration controls, policy settings, deployment risks, and licensing considerations.

> **Important:** This project is a design and workshop reference, not a production configuration export. Microsoft Purview capabilities, portal experiences, licensing, and supported clients change over time. Validate every setting against current Microsoft documentation and test it in your own tenant before deployment.

## What is included

The guide covers 12 common scenarios:

1. Public and marketing material
2. General internal business content
3. Confidential internal-only content
4. Confidential external sharing
5. Highly confidential projects and M&A
6. Personal, health, and payment data
7. Email encryption scenarios
8. Teams, SharePoint, and Microsoft 365 Group container labels
9. Meetings and Teams chat
10. Files at rest and auto-labelling
11. Guest and external collaboration
12. Label policy settings and governance

It also includes:

- A practical four-tier taxonomy model
- A complete example sensitivity-label set
- Scenario filters by tier and workload
- Keyword search
- Expandable configuration guidance
- A responsive summary table
- Common deployment pitfalls
- A five-question self-check quiz
- A design worksheet for an export-controlled engineering scenario
- A detailed model answer
- Print and save-to-PDF support
- Responsive layouts for desktop, tablet, and mobile
- Light and dark colour-scheme support
- Reduced-motion and keyboard-focus support

## Suggested taxonomy

The reference uses the following illustrative model:

| Tier | Purpose | Typical protection |
|---|---|---|
| **Public** | Approved for external release | No encryption |
| **General** | Ordinary internal business content | Marking only; recommended default |
| **Confidential** | Disclosure could cause material harm | Encryption and controlled sharing |
| **Highly Confidential** | Disclosure could cause severe, legal, or strategic harm | Restricted audiences, strong protection, and tighter governance |

The detailed scenarios explain when sublabels may be appropriate, such as:

- `Confidential / Internal Only`
- `Confidential / External Sharing`
- `Confidential / Encrypt-Only`
- `Highly Confidential / Project`
- `Highly Confidential / Do Not Forward`
- `Highly Confidential / Specific People`
- `Highly Confidential / Restricted Personal Data`

## Technology

This project is intentionally self-contained:

- HTML5
- CSS3
- Vanilla JavaScript
- No external libraries
- No package manager
- No build process
- No server-side component
- No telemetry or external data submission

Worksheet responses remain in the browser page and are not stored after the page is closed.

## Run locally

1. Download or clone the repository.
2. Open the HTML file directly in a modern browser.

For example, if the page is named `index.html`:

```text
index.html
```

No installation or build step is required.

For local HTTP testing, you can use any static web server. For example, with Python installed:

```bash
python3 -m http.server 8000
```

Then open `http://localhost:8000` in your browser.

## Publish with GitHub Pages

1. Rename the supplied HTML file to `index.html` if required.
2. Place `index.html` and this `README.md` in the repository root.
3. Push the repository to GitHub.
4. In the repository, open **Settings > Pages**.
5. Under **Build and deployment**, choose **Deploy from a branch**.
6. Select the appropriate branch and the repository root folder.
7. Save the configuration.

GitHub Pages can then serve the project as a static site.

## Repository structure

A minimal repository can use this structure:

```text
.
├── index.html
├── README.md
└── LICENSE
```

## Interactive features

### Search and filters

Users can filter the scenario cards by:

- Keyword
- Label tier
- Email
- Teams and sites
- Files at rest
- Auto-labelling

### Progressive disclosure

Each scenario separates the business context from the recommended configuration. Users can consider the scenario first, then reveal the configuration guidance.

### Self-check quiz

The five-question quiz:

- Tracks answered questions and score
- Shows the correct answer
- Explains why each alternative is unsuitable
- Can be reset without reloading the page

### Design worksheet

The worksheet helps users design a sensitivity label by considering:

- Label and sublabel name
- User-facing tooltip
- Scope
- Encryption method
- Access grant
- Usage rights
- Expiry and offline access
- Content markings
- Auto-labelling
- Label policy settings
- DLP and container controls
- Deployment risk

Use **Print / save my answers** to retain a completed worksheet.

## Accessibility and responsive design

The page includes:

- Semantic headings, sections, forms, fieldsets, and labels
- Keyboard-accessible controls
- Visible focus indicators
- ARIA attributes for expandable regions and status updates
- Minimum touch-target sizing on smaller screens
- Responsive navigation
- Mobile card rendering for the wide summary table
- `prefers-reduced-motion` support
- Light and dark colour-scheme support
- Print styles that reveal collapsed content

## Design principles reflected in the guide

The content follows several practical principles:

- Use business language rather than technical control names.
- Keep the taxonomy small and understandable.
- Do not make an encrypting label the default.
- Treat encryption as a targeted control because it can interrupt business processes.
- Pilot across real clients, devices, applications, recipients, and file types.
- Use simulation before enabling auto-labelling.
- Keep container controls and file-level protection conceptually separate.
- Scope restrictive labels to the users who need them.
- Define ownership, review, recovery, and label-retirement processes.

## Licensing

Licensing is not calculated or enforced by this project.

The guide notes that advanced capabilities may require Microsoft 365 E5, an eligible Microsoft Purview add-on, or an equivalent standalone licence. Examples include some automatic labelling capabilities, advanced classifiers, Endpoint DLP, and sensitivity labels for meetings.

Confirm current entitlement for every user and workload included in the proposed design.

## Security and privacy notes

- The page runs entirely in the browser.
- It does not transmit worksheet answers.
- It does not persist worksheet answers after the browser tab is closed.
- It does not implement Microsoft Purview configuration changes.
- It does not connect to a Microsoft 365 tenant.
- It should not be treated as legal, regulatory, export-control, or licensing advice.

## Browser support

Use a current version of a modern browser such as Microsoft Edge, Google Chrome, Mozilla Firefox, or Safari.

The core content remains visible without JavaScript. JavaScript provides progressive enhancements such as filtering, accordions, quiz scoring, and worksheet controls.

## Customisation

Common customisation points include:

- Organisation name and terminology
- Label and sublabel names
- Tooltips and user guidance
- Colour palette
- Scenario examples
- Sensitive information types
- Rights assignments
- External-sharing assumptions
- Licensing notes
- Internal help links
- Regulatory and industry context

When customising the page, keep implementation-specific values clearly identified as examples until they have been validated in the target tenant.

## Contributing

Contributions are welcome where they improve:

- Technical accuracy
- Accessibility
- Mobile usability
- Scenario coverage
- Clarity for business and technical audiences
- Alignment with current Microsoft Purview documentation

When submitting a change:

1. Describe the scenario or issue addressed.
2. Identify whether the change affects content, behaviour, accessibility, or styling.
3. Include the Microsoft documentation used to validate product-specific claims.
4. Test desktop, mobile, keyboard, print, and dark-mode behaviour where relevant.
5. Avoid adding external dependencies unless there is a clear maintenance benefit.

## Disclaimer

Microsoft Purview evolves continuously. Product names, licensing, settings, supported file types, client behaviour, and administrative experiences may change.

All examples in this project are illustrative. Validate the proposed configuration against current Microsoft documentation, organisational policy, applicable regulation, and the actual tenant and application estate before implementation.

Microsoft, Microsoft 365, Microsoft Purview, Microsoft Teams, Microsoft SharePoint, Microsoft Entra, and related product names are trademarks of Microsoft Corporation. Other names may be trademarks of their respective owners.

## Licence

Add the licence that best fits your intended use. If you want broad reuse with attribution, consider including an MIT licence file. If the content is intended only for internal or controlled partner use, use an appropriate restricted-use notice instead.
