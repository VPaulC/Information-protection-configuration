

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
