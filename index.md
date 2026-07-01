---
layout: default
layout_variant: home
title: Home
---

<!-- index.md keeps its front matter (layout: default, layout_variant: home, title: Home). Body below. -->

<section class="hero">
  <div class="hero__copy">
    <span class="eyebrow">Spec language for LLMs</span>
    <h1 class="display hero__title">The specification language that <em>talks back</em></h1>
    <p class="lead hero__lead">Allium clarifies your intent into a precise, durable artefact that keeps your AI grounded, conversation after conversation.</p>
    <div class="cta-row">
      <a href="{{ site.baseurl }}/installation" class="btn btn-accent">Get started <span class="arrow" aria-hidden="true">&rarr;</span></a>
      <a href="{{ site.baseurl }}/usage" class="btn btn-secondary">See it in action</a>
    </div>
  </div>
  <div class="hero__panel">
    <div class="code-window">
      <div class="code-tab">
        <span class="code-dot"></span>
        <span class="code-file">orders.allium</span>
        <span class="code-lang">allium</span>
      </div>
      <pre class="code-body"><code class="language-allium">rule OrderConfirmed {
    when: CheckoutCompleted(cart, payment)

    requires: cart.items.all(in_stock)
    requires: payment.authorised

    ensures:
        let order = Order.created(
            items: cart.items,
            total: cart.total
        )
        PaymentCaptured(order: order)
}</code></pre>
    </div>
  </div>
</section>

<hr class="rule">

<section class="section section-block section-tint">
  <div class="terminal" data-reveal>
    <div class="terminal-titlebar">
      <div class="terminal-dots">
        <span class="terminal-dot"></span>
        <span class="terminal-dot"></span>
        <span class="terminal-dot"></span>
      </div>
      <span class="terminal-title">Claude Code</span>
    </div>
    <div class="terminal-body">
      <div class="turn user">
        <span class="prompt">&gt;</span>We need backorder support. Let customers order items that are currently out of stock.
      </div>
      <div class="turn llm contradiction">
        <span class="marker">&#10043;</span>Your Allium says <code>OrderConfirmed</code> triggers <code>PaymentCaptured</code>, which charges the customer immediately. A backorder would charge before the item ships. Should we defer payment capture until the item is available, or capture immediately and refund if we can't fulfil?
      </div>
      <div class="turn user">
        <span class="prompt">&gt;</span>Capture payment details upfront but don't charge until we can fulfil. Update the Allium and implement.
      </div>
      <div class="turn llm">
        <span class="marker">&#10043;</span>Updated <code>orders.allium</code>. <code>BackorderPlaced</code> authorises payment and defers capture until <code>ReadyToFulfil</code>. Implementing now.
      </div>
    </div>
  </div>
</section>

<hr class="rule">

<section class="section section-block">
  <div class="section-head" data-reveal>
    <span class="eyebrow">Why a spec language at all?</span>
    <h2 class="h2 section-head__title">A formal language for what a system should do, separate from how.</h2>
    <p class="lead section-head__lead">There are four reasons to capture intent this way.</p>
  </div>

  <div class="why-grid why-grid-spaced" data-reveal>
    <div class="why-card">
      <span class="why-index">01</span>
      <h3>Stops the drift</h3>
      <p>Within a session, meaning slips: by prompt twenty, the model is pattern-matching its own outputs rather than your original intent. A spec anchors it.</p>
    </div>
    <div class="why-card">
      <span class="why-index">02</span>
      <h3>Persists between sessions</h3>
      <p>Constraints and decisions captured in conversation disappear when the chat ends. An Allium spec is a durable artefact the next session, or the next engineer, can read.</p>
    </div>
    <div class="why-card">
      <span class="why-index">03</span>
      <h3>Surfaces contradictions</h3>
      <p>Markdown lets two requirements quietly disagree. Allium's structure exposes the conflict instead of letting a capable model paper over it.</p>
    </div>
    <div class="why-card">
      <span class="why-index">04</span>
      <h3>Separates what from how</h3>
      <p>Code can't tell you whether a behaviour is intended. Allium says what the system should do; the code says how it does it. The gap between them is information.</p>
    </div>
  </div>
</section>

<hr class="rule">

<!-- ===== [1] CODE vs INTENT — editorial spread ===== -->
<section class="section section-block spread section-tint" data-reveal>
  <div class="spread__copy">
    <span class="eyebrow">Code vs intent</span>
    <h2 class="h2 spread__title">Why not just point the LLM at the code?</h2>
    <p class="lead spread__lead">Modern LLMs navigate codebases effectively, and many engineers find this sufficient. The limit appears when you need to distinguish what the code <em>does</em> from what it <em>should do</em>.</p>
    <p>Code captures implementation, including bugs and expedient decisions, and the model treats all of it as intended behaviour. Precise prompting helps, but precise prompting <em>is</em> specifying intent: which behaviours are deliberate, which constraints must be preserved.</p>
    <p>You end up writing descriptions of intent scattered across your prompts. Allium captures it in a form that persists, so the next engineer, the next model, or you next week can read not just what the system does, but what it was meant to do.</p>
  </div>

  <div class="spread__exhibit">
    <span class="exhibit-tag">what the code does</span>
    <div class="code-window exhibit-window">
      <div class="code-tab">
        <span class="code-dot"></span>
        <span class="code-file">orders.py</span>
        <span class="code-lang">python</span>
      </div>
      <pre class="code-body"><code><span class="allium-keyword">def</span> confirm_order(cart, payment):
    <span class="allium-comment"># expedient: VIP carts skip the stock check</span>
    <span class="allium-keyword">if</span> cart.vip <span class="allium-keyword">or</span> all(i.in_stock <span class="allium-keyword">for</span> i <span class="allium-keyword">in</span> cart.items):
        capture(payment)         <span class="allium-comment"># charges immediately</span>
        <span class="allium-keyword">return</span> Order.created(cart)</code></pre>
    </div>

    <div class="gap-tag">
      <span class="gap-line"></span>
      <span class="gap-label">intent &ne; implementation</span>
      <span class="gap-line"></span>
    </div>

    <span class="exhibit-tag exhibit-tag--accent">what it should do</span>
    <div class="code-window exhibit-window">
      <div class="code-tab">
        <span class="code-dot"></span>
        <span class="code-file">orders.allium</span>
        <span class="code-lang">allium</span>
      </div>
      <pre class="code-body"><code class="language-allium">rule OrderConfirmed {
    when: CheckoutCompleted(cart, payment)

    requires: cart.items.all(in_stock)
    requires: payment.authorised

    ensures: PaymentCaptured(order)
}</code></pre>
    </div>
  </div>
</section>

<hr class="rule">

<!-- ===== [2] STRUCTURE OVER PROSE — compare panels + diagnostic ===== -->
<section class="section section-block">
  <div class="section-head" data-reveal>
    <span class="eyebrow">Structure over prose</span>
    <h2 class="h2 section-head__title">Why not capture requirements in markdown?</h2>
    <p class="lead section-head__lead">Markdown gives you no framework for surfacing ambiguities and contradictions. You can require authentication in one section and allow guest checkout in another, and the format never flags the tension. A capable model may resolve it silently in a way you didn&rsquo;t intend; a weaker one may never notice it exists.</p>
  </div>

  <div class="compare" data-reveal>
    <div class="compare__col">
      <span class="exhibit-tag">prose &middot; conflict hidden</span>
      <div class="code-window exhibit-window">
        <div class="code-tab">
          <span class="code-dot"></span>
          <span class="code-file">requirements.md</span>
          <span class="code-lang">markdown</span>
        </div>
        <pre class="code-body md-body"><code><span class="md-h">## Authentication</span>
<span class="md-flag">- users must be authenticated to check out</span>

<span class="md-h">## Checkout</span>
<span class="md-flag">- guest checkout is supported</span></code></pre>
      </div>
    </div>

    <div class="compare__col">
      <span class="exhibit-tag exhibit-tag--accent">allium &middot; conflict exposed</span>
      <div class="code-window exhibit-window">
        <div class="code-tab">
          <span class="code-dot"></span>
          <span class="code-file">checkout.allium</span>
          <span class="code-lang">allium</span>
        </div>
        <pre class="code-body"><code class="language-allium">rule Checkout {
    when: CheckoutStarted(session)
    requires: session.authenticated
}

rule GuestCheckout {
    when: CheckoutStarted(session)
    requires: not session.authenticated
}</code></pre>
      </div>
    </div>
  </div>

  <div class="diagnostic" data-reveal role="status">
    <span class="diagnostic__mark" aria-hidden="true">&#10043;</span>
    <div class="diagnostic__body">
      <span class="diagnostic__cmd">$ allium check checkout.allium</span>
      <span class="diagnostic__line"><span class="diagnostic__tag">conflict</span> <code>Checkout</code> and <code>GuestCheckout</code> share trigger <code>CheckoutStarted</code></span>
      <span class="diagnostic__detail">requires: session.authenticated &nbsp;&middot;&nbsp; requires: not session.authenticated &nbsp;&rarr;&nbsp; preconditions can never both hold</span>
    </div>
  </div>

  <p class="section-coda" data-reveal>The model doesn&rsquo;t need to be clever enough to spot the issue in prose &mdash; the structure does that work. Markdown can capture robust behaviour with enough diligence, but that diligence falls entirely on the author. Allium&rsquo;s constraints guide you toward completeness and consistency.</p>
</section>

<hr class="rule">

<!-- ===== [3] TWO DIRECTIONS — bidirectional flow rail ===== -->
<section class="section section-block section-tint">
  <div class="section-head" data-reveal>
    <span class="eyebrow">Two directions of travel</span>
    <h2 class="h2 section-head__title">The specification and the code evolve together.</h2>
    <p class="lead section-head__lead">Refining a behavioural model alongside implementation deepens your understanding of both the problem and your solution. LLMs generate code from descriptions, shifting where design thinking happens &mdash; the specification becomes the site of that thinking, and the code its expression.</p>
  </div>

  <div class="flow-rail" data-reveal>
    <div class="flow-node">
      <span class="flow-node__tag">source</span>
      <span class="flow-node__title">Intent</span>
      <p class="flow-node__desc">What you meant the system to do &mdash; goals, constraints, the behaviour you actually intended.</p>
    </div>

    <div class="flow-arrow flow-arrow--fwd">
      <span class="flow-arrow__label">elicitation <i class="flow-arrow__glyph" aria-hidden="true"></i></span>
      <span class="flow-arrow__track"></span>
    </div>

    <div class="flow-node flow-node--spec">
      <span class="flow-node__tag">model.allium</span>
      <span class="flow-node__title">Specification</span>
      <p class="flow-node__desc">The durable artefact both directions meet in &mdash; the shared site where design thinking now lives.</p>
    </div>

    <div class="flow-arrow flow-arrow--back">
      <span class="flow-arrow__label"><i class="flow-arrow__glyph flow-arrow__glyph--back" aria-hidden="true"></i> distillation</span>
      <span class="flow-arrow__track"></span>
    </div>

    <div class="flow-node">
      <span class="flow-node__tag">source</span>
      <span class="flow-node__title">Implementation</span>
      <p class="flow-node__desc">What the code actually does today, including behaviours that were never explicitly decided.</p>
    </div>
  </div>

  <div class="flow-foot" data-reveal>
    <p>Elicitation works forward from intent through structured conversation; distillation works backward from implementation. Distillation reveals what you <em>built</em>; elicitation clarifies what you <em>meant</em>. When the two diverge, you&rsquo;ve found something worth investigating.</p>
    <div class="guide-links">
      <a class="guide-link" href="https://github.com/juxt/allium/blob/main/skills/elicit/SKILL.md">elicitation guide <span class="arrow" aria-hidden="true">&rarr;</span></a>
      <a class="guide-link" href="https://github.com/juxt/allium/blob/main/skills/distill/SKILL.md">distillation guide <span class="arrow" aria-hidden="true">&rarr;</span></a>
    </div>
  </div>
</section>

<hr class="rule">

<!-- ===== [4] SINGLE SOURCES OF TRUTH — precedent trio + pull-quote ===== -->
<section class="section section-block">
  <div class="section-head section-head--center" data-reveal>
    <span class="eyebrow">On single sources of truth</span>
    <h2 class="h2 section-head__title">Redundancy here isn&rsquo;t duplication. It&rsquo;s a check.</h2>
    <p class="lead section-head__lead">A common objection: a behavioural model beside the code violates single-source-of-truth. But code captures both intentional and accidental behaviour, with no mechanism to tell them apart &mdash; is that authentication quirk a feature or a bug? The code can&rsquo;t tell you. You need something outside the code to even say &ldquo;this is wrong.&rdquo; Engineers already accept this elsewhere.</p>
  </div>

  <div class="trio" data-reveal>
    <div class="trio-card">
      <span class="why-index">01</span>
      <h3>Type systems</h3>
      <p>Express intent the implementation must satisfy. Nobody calls a signature a duplicate of its body.</p>
    </div>
    <div class="trio-card">
      <span class="why-index">02</span>
      <h3>Tests</h3>
      <p>Assert expected behaviour against actual behaviour. The gap between them is the whole point.</p>
    </div>
    <div class="trio-card trio-card--accent">
      <span class="why-index">03</span>
      <h3>Allium</h3>
      <p>The same pattern, raised to behaviour: a model of <em>what</em>, and under which conditions, beside the code that says <em>how</em>.</p>
    </div>
  </div>

  <blockquote class="pull-quote" data-reveal>
    <p>When code and model disagree, that disagreement is <em>information</em>. Redundancy, in this context, isn&rsquo;t overhead. It&rsquo;s <em>resilience</em>.</p>
  </blockquote>
</section>

<hr class="rule">

<!-- ===== [5] TESTIMONIALS — sliding carousel ===== -->
<section class="section section-block section-tint">
  <div class="section-head section-head--center" data-reveal>
    <span class="eyebrow">From the field</span>
    <h2 class="h2 section-head__title">What early users are saying.</h2>
  </div>

  <div class="testi" data-carousel data-reveal>
    <div class="testi__viewport">
      <ul class="testi__track">
        <li class="testi__slide is-active">
          <figure class="testi-card">
            <span class="testi-card__mark" aria-hidden="true">&ldquo;</span>
            <blockquote class="testi-card__quote">
              <p>I&rsquo;m definitely enjoying using Allium, and finding driving Claude Code with spec to be <em>highly effective</em> both in brownfield and greenfield developments.</p>
            </blockquote>
            <figcaption class="testi-card__cite">
              <span class="testi-card__name">Jim Downing</span>
              <span class="testi-card__role">CPTO, Cyclops Workout</span>
            </figcaption>
          </figure>
        </li>
        <li class="testi__slide">
          <figure class="testi-card">
            <span class="testi-card__mark" aria-hidden="true">&ldquo;</span>
            <blockquote class="testi-card__quote">
              <p>This is a <em>game changer</em> for agentic pair planning. I run Allium on all new planning sessions &mdash; it catches so many of my blind spots.</p>
            </blockquote>
            <figcaption class="testi-card__cite">
              <span class="testi-card__name">Ben Ritchie</span>
              <span class="testi-card__role">CTO, Stealth Startup</span>
            </figcaption>
          </figure>
        </li>
        <li class="testi__slide">
          <figure class="testi-card">
            <span class="testi-card__mark" aria-hidden="true">&ldquo;</span>
            <blockquote class="testi-card__quote">
              <p>Thanks for this wonderful contribution! Early days, but I&rsquo;ve <em>distilled a large amount of knowledge</em> from a heavily WIP backoffice system.</p>
            </blockquote>
            <figcaption class="testi-card__cite">
              <span class="testi-card__name">John Grimsey</span>
              <span class="testi-card__role">Group CTO, New Age Partners</span>
            </figcaption>
          </figure>
        </li>
      </ul>
    </div>

    <div class="testi__dots">
      <button class="testi__dot is-active" type="button" aria-label="Show testimonial 1" aria-current="true"></button>
      <button class="testi__dot" type="button" aria-label="Show testimonial 2"></button>
      <button class="testi__dot" type="button" aria-label="Show testimonial 3"></button>
    </div>
  </div>
</section>

<section class="section">
  <div class="cta-banner" data-reveal>
    <span class="eyebrow">Velocity through clarity</span>
    <h2 class="h2 cta-banner-title">Ready to give your AI a clearer brief?</h2>
    <p>Install Allium in your editor and turn your next conversation into a spec your future self can still read.</p>
    <div class="cta-row">
      <a href="{{ site.baseurl }}/installation" class="btn btn-accent">Install Allium <span class="arrow" aria-hidden="true">&rarr;</span></a>
      <a href="https://github.com/juxt/allium" class="btn btn-secondary">View on GitHub</a>
    </div>
  </div>
</section>
