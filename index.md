---
layout: default
layout_variant: home
title: Home
---

<section class="hero-section">
  <span class="hero-eyebrow">Spec language for LLMs</span>
  <h1 class="hero-title">The specification language<br>that <em>talks back</em></h1>
  <p class="hero-lead">Allium clarifies your intent into a precise, durable artefact that keeps your AI grounded, conversation after conversation.</p>
  <div class="cta-row">
    <a href="{{ site.baseurl }}/installation" class="btn btn-accent">Get started <span class="arrow">&rarr;</span></a>
    <a href="{{ site.baseurl }}/usage" class="btn btn-secondary">See it in action</a>
  </div>
</section>

<section class="section">
<div class="terminal">
  <div class="terminal-titlebar">
    <div class="terminal-dots">
      <span class="terminal-dot red"></span>
      <span class="terminal-dot yellow"></span>
      <span class="terminal-dot green"></span>
    </div>
    <span class="terminal-title">Claude Code</span>
  </div>
  <div class="terminal-body">
    <div class="turn user">
      <span class="prompt">&gt;</span> We need backorder support. Let customers order items that are currently out of stock.
    </div>
    <div class="turn llm">
      <span class="marker">✻</span> Your Allium says <code>OrderConfirmed</code> triggers <code>PaymentCaptured</code>, which charges the customer immediately. A backorder would charge before the item ships. Should we defer payment capture until the item is available, or capture immediately and refund if we can't fulfil?
    </div>
    <div class="turn user">
      <span class="prompt">&gt;</span> Capture payment details upfront but don't charge until we can fulfil. Update the Allium and implement.
    </div>
    <div class="turn llm">
      <span class="marker">✻</span> Updated <code>orders.allium</code>. <code>BackorderPlaced</code> authorises payment and defers capture until <code>ReadyToFulfil</code>. Implementing now.
    </div>
  </div>
</div>
</section>

<section class="band">
<div class="section">
<h2 class="section-title">Why a spec language at all?</h2>
<p class="section-lead">Allium is a formal language for capturing what a system should do, separate from how. There are four reasons to do this:</p>

<div class="feature-grid">
  <div class="feature-card feature-card-coral">
    <h3>Stops the drift</h3>
    <p>Within a session, meaning slips: by prompt twenty, the model is pattern-matching its own outputs rather than your original intent. A spec anchors it.</p>
  </div>
  <div class="feature-card feature-card-purple">
    <h3>Persists between sessions</h3>
    <p>Constraints and decisions captured in conversation disappear when the chat ends. An Allium spec is a durable artefact the next session, or the next engineer, can read.</p>
  </div>
  <div class="feature-card feature-card-amber">
    <h3>Surfaces contradictions</h3>
    <p>Markdown lets two requirements quietly disagree. Allium's structure exposes the conflict instead of letting a capable model paper over it.</p>
  </div>
  <div class="feature-card feature-card-mint">
    <h3>Separates what from how</h3>
    <p>Code can't tell you whether a behaviour is intended. Allium says what the system should do; the code says how it does it. The gap between them is information.</p>
  </div>
</div>
</div>
</section>

<section class="section section-narrow" markdown="1">

## Why not just point the LLM at the code?

Modern LLMs navigate codebases effectively, and many engineers find this sufficient. The limitation appears when you need to distinguish what the code *does* from what it *should do*. Code captures implementation, including bugs and expedient decisions. The model treats all of it as intended behaviour.

Precise prompting helps, but precise prompting means specifying intent: which behaviours are deliberate, which constraints must be preserved. You end up writing descriptions of intent distributed across your prompts. Allium captures this in a form that persists. The next engineer, or the next model, or you next week, can understand not just what the system does but what it was meant to do.

## Why not capture requirements in markdown?

Markdown provides no framework for surfacing ambiguities and contradictions. You can write "users must be authenticated" in one section and "guest checkout is supported" in another without the format highlighting the tension. Capable models may resolve such ambiguity silently in ways you didn't intend. Weaker models may not notice that there's an ambiguity at all.

Allium's structure makes contradictions visible. When two rules have incompatible preconditions, the formal syntax exposes the conflict. The model doesn't need to be clever enough to spot the issue in prose; the structure does that work. Markdown can capture robust behaviour with sufficient diligence, but that diligence falls entirely on the author. Allium's constraints guide you toward completeness and consistency.

## Iterating on specifications

The specification and the code evolve together. Writing and refining a behavioural model alongside implementation deepens your understanding of both the problem and your solution. Questions surface that you wouldn't have thought to ask; constraints emerge that only become visible when you try to formalise them.

Manual coding embedded this discovery in the act of implementation. LLMs generate code from descriptions, shifting where design thinking occurs. Allium captures it explicitly: the specification becomes the site of that thinking, the code its expression.

Two processes feed this growth: **elicitation** works forward from intent through structured conversations with stakeholders, while **distillation** works backward from implementation to capture what the system actually does, including behaviours that were never explicitly decided. Distillation reveals what you built; elicitation clarifies what you meant. When these diverge, you've found something worth investigating.

See the [elicitation guide](https://github.com/juxt/allium/blob/main/skills/elicit/SKILL.md) and the [distillation guide](https://github.com/juxt/allium/blob/main/skills/distill/SKILL.md) for detailed approaches.

## On single sources of truth

A common objection is that maintaining behavioural models alongside code violates the single source of truth principle. But code captures both intentional and accidental behaviour, with no mechanism to distinguish them. Is that authentication quirk a feature or a bug? The code can't tell you. You need something outside the code to even articulate "this behaviour is wrong". Engineers already accept this in other contexts: type systems express intent that code must satisfy, tests assert expected behaviour against actual behaviour. These aren't duplication.

Allium applies the same pattern. Code excels at expressing *how*; behavioural models excel at expressing *what* and *under which conditions*. When these disagree, that disagreement is information. Perhaps the implementation drifted from intent, or perhaps the model was naive. Either might need to change. The gap between them surfaces questions you need to answer. Redundancy, in this context, isn't overhead. It's resilience.

</section>

<section class="cta-banner">
  <h2>Ready to give your AI a clearer brief?</h2>
  <p>Install Allium in your editor and turn your next conversation into a spec your future self can still read.</p>
  <div class="cta-row">
    <a href="{{ site.baseurl }}/installation" class="btn btn-accent">Install Allium <span class="arrow">&rarr;</span></a>
    <a href="https://github.com/juxt/allium" class="btn btn-secondary">View on GitHub</a>
  </div>
</section>
