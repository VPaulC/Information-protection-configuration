<!DOCTYPE html>
<html lang="en-GB" dir="ltr">
<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <meta name="color-scheme" content="light dark">
  <title>Microsoft Purview Information Protection — Common Scenarios &amp; Label Configuration</title>
  <style>
    :root {
      --bg: #f5f6f8;
      --surface: #ffffff;
      --surface-alt: #eef1f5;
      --ink: #1b1d21;
      --ink-soft: #52575e;
      --line: #d9dde3;
      --line-strong: #b9c0c9;
      --accent: #1f4e79;
      --accent-soft: #e6eef6;
      --public: #1e7a45;
      --public-bg: #e4f4ea;
      --general: #1f5da8;
      --general-bg: #e5edf8;
      --conf: #9a5b00;
      --conf-bg: #fbeed8;
      --high: #a32020;
      --high-bg: #fae5e5;
      --code-bg: #eef1f5;
      --shadow: 0 1px 2px rgba(20,25,35,.08), 0 6px 18px rgba(20,25,35,.06);
      --radius: 12px;
      --maxw: 1100px;
      --gutter: 20px;
    }
    @media (prefers-color-scheme: dark) {
      :root {
        --bg: #15171b;
        --surface: #1e2126;
        --surface-alt: #24282e;
        --ink: #e9ebee;
        --ink-soft: #a9b0b9;
        --line: #333941;
        --line-strong: #454c56;
        --accent: #7fb2e5;
        --accent-soft: #1f2a35;
        --public: #63c98d;
        --public-bg: #16301f;
        --general: #7cb0ef;
        --general-bg: #172436;
        --conf: #e0a44a;
        --conf-bg: #33260f;
        --high: #f08b8b;
        --high-bg: #361a1a;
        --code-bg: #262a30;
        --shadow: 0 1px 2px rgba(0,0,0,.4), 0 6px 18px rgba(0,0,0,.3);
      }
    }
    * { box-sizing: border-box; }
    html { scroll-behavior: smooth; }
    body {
      margin: 0;
      background: var(--bg);
      color: var(--ink);
      font-family: "Segoe UI", -apple-system, BlinkMacSystemFont, Roboto, "Helvetica Neue", Arial, sans-serif;
      line-height: 1.6;
      font-size: 16px;
      overflow-wrap: break-word;
    }
    .wrap { max-width: var(--maxw); margin-inline: auto; padding-inline: var(--gutter); }
    header.masthead {
      background: var(--surface);
      border-block-end: 3px solid var(--accent);
      padding-block: 28px 22px;
    }
    h1 { margin: 0 0 6px; font-size: clamp(1.45rem, 2vw, 2rem); }
    .sub { margin: 0; color: var(--ink-soft); }
    nav.toc {
      position: sticky;
      inset-block-start: 0;
      z-index: 10;
      background: var(--surface-alt);
      border-block-end: 1px solid var(--line);
      padding-block: 10px;
    }
    nav.toc ul {
      list-style: none;
      display: flex;
      flex-wrap: wrap;
      gap: 8px;
      margin: 0;
      padding: 0;
    }
    nav.toc a {
      display: inline-block;
      padding: 6px 12px;
      border: 1px solid var(--line);
      border-radius: 999px;
      background: var(--surface);
      color: var(--ink);
      text-decoration: none;
      font-size: 0.8rem;
    }
    main { padding-block: 32px 48px; }
    section {
      background: var(--surface);
      border: 1px solid var(--line);
      border-radius: var(--radius);
      box-shadow: var(--shadow);
      padding: 20px 22px;
      margin-block-end: 20px;
    }
    h2 {
      margin-block: 0 12px;
      padding-block-end: 8px;
      border-block-end: 2px solid var(--line-strong);
      font-size: 1.3rem;
    }
    h3 { margin-block: 16px 8px; font-size: 1.08rem; }
    p, li { font-size: 0.97rem; }
    .badge {
      display: inline-block;
      padding: 2px 10px;
      border-radius: 999px;
      font-size: .75rem;
      font-weight: 600;
      letter-spacing: .02em;
      border: 1px solid currentColor;
      white-space: nowrap;
    }
    .badge.public { color: var(--public); background: var(--public-bg); }
    .badge.general { color: var(--general); background: var(--general-bg); }
    .badge.conf { color: var(--conf); background: var(--conf-bg); }
    .badge.high { color: var(--high); background: var(--high-bg); }
    .taxonomy {
      list-style: none;
      padding: 0;
      margin: 14px 0;
      display: grid;
      gap: 10px;
    }
    .taxonomy li {
      background: var(--surface-alt);
      border-inline-start: 4px solid var(--line-strong);
      border-radius: 6px;
      padding: 8px 12px;
    }
    code {
      font-family: Consolas, "SF Mono", Menlo, monospace;
      background: var(--code-bg);
      padding: 2px 5px;
      border-radius: 4px;
      font-size: .87em;
    }
    ol, ul { padding-inline-start: 20px; }
    .callout {
      border-inline-start: 5px solid var(--accent);
      background: var(--accent-soft);
      padding: 14px 18px;
      border-radius: 8px;
      margin-block: 12px;
    }
    footer {
      padding-block: 18px 32px;
      color: var(--ink-soft);
      font-size: .85rem;
    }
    @media (max-width: 700px) {
      .wrap { padding-inline: 16px; }
      nav.toc ul { overflow-x: auto; white-space: nowrap; }
      section { padding: 16px 14px; }
      .badge { white-space: normal; }
    }
  </style>
</head>
<body>
  <header class="masthead">
    <div class="wrap">
      <h1>Microsoft Purview Information Protection — Common Scenarios</h1>
      <p class="sub">A practical reference for designing sensitivity labels: what each scenario looks like, and how the label should be configured.</p>
    </div>
  </header>

  <nav class="toc" aria-label="Scenario navigation">
    <div class="wrap">
      <ul>
        <li><a href="#taxonomy">Taxonomy</a></li>
        <li><a href="#howto">How to use</a></li>
        <li><a href="#s1">1 · Public</a></li>
        <li><a href="#s2">2 · General</a></li>
        <li><a href="#s3">3 · Confidential</a></li>
        <li><a href="#s4">4 · External</a></li>
        <li><a href="#s5">5 · Highly Confidential</a></li>
        <li><a href="#s6">6 · PII &amp; payment data</a></li>
        <li><a href="#s7">7 · Email-only</a></li>
        <li><a href="#s8">8 · Containers</a></li>
        <li><a href="#s9">9 · Meetings</a></li>
        <li><a href="#s10">10 · Files at rest</a></li>
        <li><a href="#s11">11 · External collaboration</a></li>
      </ul>
    </div>
  </nav>

  <main class="wrap">
    <section id="taxonomy">
      <h2>The taxonomy model</h2>
      <p>A sensitivity label is a single piece of metadata that travels with content and can carry <strong>visual markings</strong>, <strong>encryption</strong>, and <strong>user access controls</strong>.</p>
      <ul class="taxonomy">
        <li><span class="badge public">Public</span> Approved for release outside the organisation. No protection.</li>
        <li><span class="badge general">General</span> Ordinary internal business content. The default label for most work.</li>
        <li><span class="badge conf">Confidential</span> Damage if disclosed. Encrypted. Sublabels such as Internal Only and External Sharing are common.</li>
        <li><span class="badge high">Highly Confidential</span> Severe damage if disclosed. Encrypted, tightly scoped, and often project-specific.</li>
      </ul>
      <p>Design the taxonomy around business language, not technical controls. Users should be able to choose an appropriate label based on its name and tooltip alone.</p>
    </section>

    <section id="howto">
      <h2>How to use</h2>
      <p>This guide is a self-contained HTML document. To view it as a styled page in a browser, use either of the following methods.</p>

      <h3>Option 1: Download the page and open it directly</h3>
      <ol>
        <li>Open the GitHub page for this file.</li>
        <li>Click <strong>Raw</strong> or use the browser’s Save Page As option.</li>
        <li>Save it as <code>README.html</code> (not <code>.md</code>).</li>
        <li>Open the file in a browser by double-clicking it, or use File → Open File…</li>
      </ol>

      <h3>Option 2: Download from the command line</h3>
      <pre><code>curl -L -o README.html https://raw.githubusercontent.com/VPaulC/Information-protection-configuration/main/README.md</code></pre>
      <p>Then open the file in any modern browser. On macOS you can do:</p>
      <pre><code>open README.html</code></pre>
      <p>On Windows PowerShell:</p>
      <pre><code>start README.html</code></pre>
      <p>On Linux:</p>
      <pre><code>xdg-open README.html</code></pre>

      <h3>Option 3: Serve it locally</h3>
      <p>If you prefer to view it using a local web server:</p>
      <pre><code>python -m http.server 8000</code></pre>
      <p>Then open:</p>
      <pre><code>http://localhost:8000/README.html</code></pre>

      <div class="callout">
        <strong>Tip:</strong> If the browser shows raw source text instead of the styled page, the file likely has the wrong extension. Rename it to <code>.html</code> and reopen it.
      </div>
    </section>

    <section id="s1">
      <h2>Scenario 1 — Public</h2>
      <p>Use <span class="badge public">Public</span> for content approved for release outside the organisation. This is content that should not be encrypted or restricted.</p>
      <p>Examples: published marketing content, public web copy, recruitment content, public reports, and other approved external-facing communications.</p>
    </section>

    <section id="s2">
      <h2>Scenario 2 — General</h2>
      <p>Use <span class="badge general">General</span> for ordinary internal business content. This is usually the default label for documents and email.</p>
      <p>Examples: internal project documents, meeting notes, team updates, and routine business communications.</p>
    </section>

    <section id="s3">
      <h2>Scenario 3 — Confidential</h2>
      <p>Use <span class="badge conf">Confidential</span> for information whose disclosure could cause real harm but is still appropriate for the broader workforce.</p>
      <p>Typical examples: internal financials, pricing models, strategy documents, architecture details, and operational plans.</p>
    </section>

    <section id="s4">
      <h2>Scenario 4 — Confidential external sharing</h2>
      <p>Use a confidential sublabel for named external recipients or partner organisations. This is the pattern for controlled disclosures to customers, suppliers, or alliance partners.</p>
      <p>Grant access only to the specific external audience, with limited rights and an appropriate expiry period.</p>
    </section>

    <section id="s5">
      <h2>Scenario 5 — Highly Confidential</h2>
      <p>Use <span class="badge high">Highly Confidential</span> for sensitive matters where disclosure could cause severe business damage or regulatory exposure.</p>
      <p>Examples: M&amp;A information, restructuring plans, litigation strategy, incident response, and restricted project data.</p>
    </section>

    <section id="s6">
      <h2>Scenario 6 — PII and payment data</h2>
      <p>PII, payroll data, health records, national identifiers, and payment card data should be treated as high-risk and automatically identified wherever possible.</p>
      <p>Use DLP and sensitive information types to detect this information early, then apply the appropriate confidentiality label and policy controls.</p>
    </section>

    <section id="s7">
      <h2>Scenario 7 — Email-only controls</h2>
      <p>Mail is a major risk surface. Two common email protection patterns are <strong>Encrypt-Only</strong> and <strong>Do Not Forward</strong>.</p>
      <p>Use encryption for safe recipient-based sharing and use Do Not Forward for sensitive messages that must remain unreadable outside the recipient list.</p>
    </section>

    <section id="s8">
      <h2>Scenario 8 — Containers</h2>
      <p>Container labels define how a group, team, or site is governed. They influence privacy, guest access, external sharing, and default handling for files in the workspace.</p>
      <p>Container labels do not replace file-level labelling; they work alongside it.</p>
    </section>

    <section id="s9">
      <h2>Scenario 9 — Meetings and chat</h2>
      <p>Meeting invites and chat should follow the same sensitivity model as documents and email. Labels can enforce meeting options, recording controls, lobby settings, and participant restrictions.</p>
    </section>

    <section id="s10">
      <h2>Scenario 10 — Files at rest</h2>
      <p>For existing SharePoint and OneDrive content, a service-side auto-labelling policy is the practical way to reach backlogged data.</p>
      <p>Client-side labelling is useful for new content and user guidance, but service-side controls are needed to cover the existing estate.</p>
    </section>

    <section id="s11">
      <h2>Scenario 11 — External collaboration</h2>
      <p>Labels persist with files, so protection travels with the content even when shared outside the tenant. This is a key reason to keep the labelling model consistent and policy-driven.</p>
    </section>
  </main>

  <footer class="wrap">
    Repository: <strong>VPaulC/Information-protection-configuration</strong>
  </footer>
</body>
</html>
