---
layout: page
title: Bayesian Signal Updating
permalink: /ibbu/
description: An interactive app for Bayesian belief updating with binary signals
nav: false
nav_order: 4
---

<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=Libre+Baskerville:wght@400;700&family=Inter:wght@300;400;500;600&display=swap" rel="stylesheet">

<script>
  window.MathJax = {
    tex: {
      inlineMath: [['\\(', '\\)'], ['$', '$']],
      displayMath: [['$$', '$$']]
    },
    svg: { fontCache: 'global' }
  };
</script>
<script defer src="https://cdn.jsdelivr.net/npm/mathjax@3/es5/tex-svg.js"></script>

<style>
  :root {
    --bg: #fff0db;
    --blue: #0039a6;
    --red: #aa0000;
    --text: #111111;
    --muted: #555555;
    --line: #c9b89d;
    --soft: #f7ead4;
  }

  .bayes-wrap {
    background: var(--bg);
    padding: 28px 24px 48px;
    border-radius: 16px;
  }

  .bayes-inner {
    max-width: 980px;
    margin: 0 auto;
  }

  .bayes-title {
    font-family: var(--serif);
    font-size: 2rem;
    line-height: 1.25;
    color: var(--text);
    margin: 0 0 12px;
  }

  .bayes-subtitle {
    font-family: var(--sans);
    font-size: 1.05rem;
    color: var(--muted);
    margin: 0 0 28px;
    font-weight: 300;
  }

  .bayes-card {
    background: rgba(255,255,255,0.18);
    border: 1px solid rgba(0,0,0,0.06);
    border-radius: 16px;
    padding: 20px 20px 18px;
    margin-bottom: 24px;
  }

  .bayes-card-title {
    font-family: var(--serif);
    font-size: 1.15rem;
    color: var(--text);
    margin: 0 0 12px;
  }

  .tree-wrap {
    background: #fff8ee;
    border: 1px solid rgba(0,0,0,0.08);
    border-radius: 16px;
    padding: 14px;
  }

  .tree-caption {
    font-family: var(--sans);
    font-size: 0.92rem;
    color: var(--muted);
    margin-top: 8px;
    line-height: 1.5;
  }

  .summary-table-wrap {
    background: #fff8ee;
    border: 1px solid rgba(0,0,0,0.08);
    border-radius: 16px;
    padding: 14px;
    margin-top: 16px;
  }

  .summary-table {
    width: 100%;
    border-collapse: collapse;
    font-family: var(--sans);
    font-size: 0.96rem;
    color: var(--text);
  }

  .summary-table th,
  .summary-table td {
    padding: 10px 12px;
    border-bottom: 1px solid rgba(0,0,0,0.08);
    text-align: center;
  }

  .summary-table th:first-child,
  .summary-table td:first-child {
    text-align: left;
  }

  .summary-table tr:last-child td {
    border-bottom: none;
  }

  .summary-table thead th {
    font-weight: 600;
    background: rgba(255,255,255,0.35);
  }

  .bayes-control {
    margin: 0 0 20px;
  }

  .bayes-label {
    display: flex;
    justify-content: space-between;
    align-items: baseline;
    gap: 12px;
    font-family: var(--sans);
    font-size: 1rem;
    font-weight: 600;
    color: var(--text);
    margin-bottom: 10px;
  }

  .bayes-value {
    font-weight: 500;
    color: var(--text);
  }

  .bayes-help {
    font-family: var(--sans);
    font-size: 0.92rem;
    color: var(--muted);
    margin-top: 6px;
    line-height: 1.5;
  }

  .bayes-slider {
    width: 100%;
    appearance: none;
    height: 8px;
    border-radius: 999px;
    background: #d6d6d6;
    outline: none;
  }

  .bayes-slider::-webkit-slider-thumb {
    appearance: none;
    width: 20px;
    height: 20px;
    border-radius: 50%;
    background: var(--red);
    border: 2px solid #8b0000;
    cursor: pointer;
  }

  .bayes-slider::-moz-range-thumb {
    width: 20px;
    height: 20px;
    border-radius: 50%;
    background: var(--red);
    border: 2px solid #8b0000;
    cursor: pointer;
  }

  .bayes-ticks {
    display: flex;
    justify-content: space-between;
    font-family: var(--sans);
    font-size: 0.8rem;
    color: var(--muted);
    margin-top: 7px;
  }

  .signal-toggle {
    display: flex;
    gap: 10px;
    flex-wrap: wrap;
  }

  .signal-btn {
    border: 1px solid rgba(0,0,0,0.15);
    background: #fff8ee;
    color: var(--text);
    padding: 10px 14px;
    border-radius: 999px;
    font-family: var(--sans);
    font-size: 0.95rem;
    font-weight: 500;
    cursor: pointer;
  }

  .signal-btn.active {
    background: var(--red);
    color: white;
    border-color: #8b0000;
  }

  .posterior-boxes {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 12px;
    margin-top: 14px;
  }

  .posterior-box {
    background: #fff8ee;
    border: 1px solid rgba(0,0,0,0.08);
    border-radius: 14px;
    padding: 14px 14px 12px;
  }

  .posterior-box.active {
    border: 2px solid var(--red);
    box-shadow: 0 0 0 3px rgba(170, 0, 0, 0.08);
  }

  .posterior-label {
    font-family: var(--sans);
    font-size: 0.95rem;
    color: var(--muted);
    margin-bottom: 6px;
  }

  .posterior-number {
    font-family: var(--sans);
    font-size: 1.45rem;
    font-weight: 700;
    color: var(--text);
  }

  .posterior-note {
    font-family: var(--sans);
    font-size: 0.88rem;
    color: var(--muted);
    margin-top: 6px;
    line-height: 1.45;
  }

  .formula-box {
    background: #fff8ee;
    border: 1px solid rgba(0,0,0,0.08);
    border-radius: 14px;
    padding: 14px 16px;
    margin-top: 14px;
    font-family: var(--serif);
    color: var(--text);
  }

  .formula-box .math {
    text-align: center;
    margin: 8px 0;
  }
</style>

<div class="bayes-wrap">
  <div class="bayes-inner">
    <h1 class="bayes-title">Bayesian Signal Updating</h1>
    <p class="bayes-subtitle">
      A small interactive page for exploring Bayesian belief updating with a binary state, a noisy binary signal, and live posterior calculations.
    </p>

<div class="bayes-card">
  <h2 class="bayes-card-title">Signal structure</h2>

  <div class="tree-wrap">
    <svg id="signal-tree" viewBox="0 0 760 420" width="100%" aria-label="Signal tree diagram">
      <!-- main branches -->
      <line x1="70" y1="210" x2="260" y2="120" stroke="#444" stroke-width="2"/>
      <line x1="70" y1="210" x2="260" y2="300" stroke="#444" stroke-width="2"/>

      <!-- theta = 1 branches -->
      <line x1="260" y1="120" x2="560" y2="70" stroke="#444" stroke-width="2"/>
      <line x1="260" y1="120" x2="560" y2="170" stroke="#444" stroke-width="2"/>

      <!-- theta = 0 branches -->
      <line x1="260" y1="300" x2="560" y2="250" stroke="#444" stroke-width="2"/>
      <line x1="260" y1="300" x2="560" y2="350" stroke="#444" stroke-width="2"/>

      <!-- nodes -->
      <circle cx="70" cy="210" r="9" fill="#fff" stroke="#222" stroke-width="2"/>
      <circle cx="260" cy="120" r="7" fill="#0039a6"/>
      <circle cx="260" cy="300" r="7" fill="#0039a6"/>

      <circle cx="560" cy="70" r="7" fill="#aa0000"/>
      <circle cx="560" cy="170" r="7" fill="#aa0000"/>
      <circle cx="560" cy="250" r="7" fill="#aa0000"/>
      <circle cx="560" cy="350" r="7" fill="#aa0000"/>

      <!-- labels -->
      <text x="232" y="102" font-family="Inter, sans-serif" font-size="21" fill="#111">θ = 1</text>
      <text x="232" y="322" font-family="Inter, sans-serif" font-size="21" fill="#111">θ = 0</text>

      <text x="590" y="76" font-family="Inter, sans-serif" font-size="21" fill="#111">s = 1</text>
      <text x="590" y="176" font-family="Inter, sans-serif" font-size="21" fill="#111">s = 0</text>
      <text x="590" y="256" font-family="Inter, sans-serif" font-size="21" fill="#111">s = 1</text>
      <text x="590" y="356" font-family="Inter, sans-serif" font-size="21" fill="#111">s = 0</text>

      <!-- branch labels -->
      <text id="lbl-prior-pass" x="145" y="130" font-family="Inter, sans-serif" font-size="18" fill="#111">π₀</text>
      <text id="lbl-prior-fail" x="140" y="285" font-family="Inter, sans-serif" font-size="18" fill="#111">1 − π₀</text>

      <text id="lbl-a1" x="392" y="84" font-family="Inter, sans-serif" font-size="18" fill="#111">1 − α</text>
      <text id="lbl-a0" x="395" y="154" font-family="Inter, sans-serif" font-size="18" fill="#111">α</text>

      <text id="lbl-b1" x="400" y="244" font-family="Inter, sans-serif" font-size="18" fill="#111">β</text>
      <text id="lbl-b0" x="395" y="344" font-family="Inter, sans-serif" font-size="18" fill="#111">1 − β</text>

      <!-- highlights -->
      <line id="path-s1-pass" x1="260" y1="120" x2="560" y2="70" stroke="#aa0000" stroke-width="6" opacity="0.12"/>
      <line id="path-s1-fail" x1="260" y1="300" x2="560" y2="250" stroke="#aa0000" stroke-width="6" opacity="0.12"/>

      <line id="path-s0-pass" x1="260" y1="120" x2="560" y2="170" stroke="#0039a6" stroke-width="6" opacity="0.12"/>
      <line id="path-s0-fail" x1="260" y1="300" x2="560" y2="350" stroke="#0039a6" stroke-width="6" opacity="0.12"/>
    </svg>

    <div class="tree-caption">
      The state is \( \theta \in \{0,1\} \), where \( \theta=1 \) means “passed” and \( \theta=0 \) means “failed.”
      The signal is \( s \in \{0,1\} \), where \( s=1 \) is a signal in favor of passing and \( s=0 \) is a signal in favor of failing.
    </div>
  </div>

  <div class="summary-table-wrap">
    <table class="summary-table">
      <thead>
        <tr>
          <th>True state</th>
          <th>Signal \(s=1\)</th>
          <th>Signal \(s=0\)</th>
        </tr>
      </thead>
      <tbody>
        <tr>
          <td>\( \theta=1 \) (passed)</td>
          <td id="tbl-pass-s1">\(1-\alpha\)</td>
          <td id="tbl-pass-s0">\(\alpha\)</td>
        </tr>
        <tr>
          <td>\( \theta=0 \) (failed)</td>
          <td id="tbl-fail-s1">\(\beta\)</td>
          <td id="tbl-fail-s0">\(1-\beta\)</td>
        </tr>
      </tbody>
    </table>
  </div>
</div>

<div class="bayes-card">
  <h2 class="bayes-card-title">Controls</h2>

  <div class="bayes-control">
    <div class="bayes-label">
      <span>Prior \( \pi_0 = \mathbb{P}(\theta=1) \)</span>
      <span id="prior-display" class="bayes-value">50.0%</span>
    </div>
    <input id="prior-slider" class="bayes-slider" type="range" min="0.5" max="99.5" step="0.5" value="50">
    <div class="bayes-ticks">
      <span>0</span><span>10</span><span>20</span><span>30</span><span>40</span><span>50</span><span>60</span><span>70</span><span>80</span><span>90</span><span>100</span>
    </div>
  </div>

  <div class="bayes-control">
    <div class="bayes-label">
      <span>\( \alpha \) (false negative)</span>
      <span id="alpha-display" class="bayes-value">20.0%</span>
    </div>
    <input id="alpha-slider" class="bayes-slider" type="range" min="0.0" max="99.5" step="0.5" value="20">
    <div class="bayes-help">
      \( \alpha = \mathbb{P}(s=0 \mid \theta=1) \), so \( 1-\alpha = \mathbb{P}(s=1 \mid \theta=1) \).
    </div>
  </div>

  <div class="bayes-control">
    <div class="bayes-label">
      <span>\( \beta \) (false positive)</span>
      <span id="beta-display" class="bayes-value">20.0%</span>
    </div>
    <input id="beta-slider" class="bayes-slider" type="range" min="0.0" max="99.5" step="0.5" value="20">
    <div class="bayes-help">
      \( \beta = \mathbb{P}(s=1 \mid \theta=0) \), so \( 1-\beta = \mathbb{P}(s=0 \mid \theta=0) \).
    </div>
  </div>

  <div class="bayes-control">
    <div class="bayes-label">
      <span>Observed signal</span>
    </div>
    <div class="signal-toggle">
      <button id="signal-one" class="signal-btn active" type="button">\( s = 1 \) (“passed”)</button>
      <button id="signal-zero" class="signal-btn" type="button">\( s = 0 \) (“failed”)</button>
    </div>
  </div>

  <div class="posterior-boxes">
    <div id="box-s1" class="posterior-box active">
      <div class="posterior-label">Posterior if \( s=1 \)</div>
      <div id="post-s1" class="posterior-number">80.0%</div>
      <div class="posterior-note">
        \( \pi_1(s=1)=\mathbb{P}(\theta=1\mid s=1) \)
      </div>
    </div>

    <div id="box-s0" class="posterior-box">
      <div class="posterior-label">Posterior if \( s=0 \)</div>
      <div id="post-s0" class="posterior-number">20.0%</div>
      <div class="posterior-note">
        \( \pi_1(s=0)=\mathbb{P}(\theta=1\mid s=0) \)
      </div>
    </div>
  </div>

  <div class="formula-box">
    <div style="font-family: Inter, sans-serif; font-size: 0.95rem; color: #555;">
      Active posterior for the selected signal:
    </div>
    <div id="active-posterior" class="math" style="font-size: 1.35rem;">
      \( \pi_1 = 80.0\% \)
    </div>
  </div>
</div>

    <h2 class="section-title">Setup</h2>
    <p class="section-text">
      The environment is characterized by a binary outcome, corresponding to whether the decision-maker passes or fails the exam.
      Formally, the state is \( \theta \in \{0,1\} \), where \( \theta=1 \) means “passed” and \( \theta=0 \) means “failed.”
      The prior probability of passing is denoted by \( \pi_0 = \mathbb{P}(\theta=1) \), with \( \pi_0 \in (0,1) \).
    </p>

    <p class="section-text">
      Before making a decision, the decision-maker receives a binary signal \( s \in \{0,1\} \).
      We interpret \( s=1 \) as a signal in favor of passing, and \( s=0 \) as a signal in favor of failing.
      The signal is noisy rather than perfectly revealing, with likelihoods
    </p>

    <div class="math-block">
      $$
      \mathbb{P}(s=1\mid \theta=1)=1-\alpha,\qquad
      \mathbb{P}(s=0\mid \theta=1)=\alpha,
      $$
      $$
      \mathbb{P}(s=1\mid \theta=0)=\beta,\qquad
      \mathbb{P}(s=0\mid \theta=0)=1-\beta,
      $$
    </div>

    <p class="section-text">
      where \( \alpha,\beta\in[0,1] \) are the false-negative and false-positive rates, respectively.
    </p>

    <h2 class="section-title">Posterior beliefs</h2>
    <p class="section-text">
      By Bayes’ rule, the posterior probability of passing after signal \( s=1 \) is
    </p>

    <div class="math-block">
      $$
      \pi_1(s=1)
      \equiv
      \mathbb{P}(\theta=1\mid s=1)
      =
      \frac{(1-\alpha)\pi_0}{(1-\alpha)\pi_0+\beta(1-\pi_0)}.
      $$
    </div>

    <p class="section-text">
      Similarly, the posterior probability of passing after signal \( s=0 \) is
    </p>

    <div class="math-block">
      $$
      \pi_1(s=0)
      \equiv
      \mathbb{P}(\theta=1\mid s=0)
      =
      \frac{\alpha\pi_0}{\alpha\pi_0+(1-\beta)(1-\pi_0)}.
      $$
    </div>

    <p class="section-text">
      Use the sliders to see how the prior \( \pi_0 \), the false-negative rate \( \alpha \), and the false-positive rate \( \beta \)
      jointly determine posterior beliefs. In particular, a more accurate signal corresponds to lower values of both \( \alpha \) and \( \beta \).
    </p>
  </div>
</div>

<script>
  const priorSlider = document.getElementById('prior-slider');
  const alphaSlider = document.getElementById('alpha-slider');
  const betaSlider = document.getElementById('beta-slider');

  const priorDisplay = document.getElementById('prior-display');
  const alphaDisplay = document.getElementById('alpha-display');
  const betaDisplay = document.getElementById('beta-display');

  const postS1 = document.getElementById('post-s1');
  const postS0 = document.getElementById('post-s0');
  const activePosterior = document.getElementById('active-posterior');

  const btnS1 = document.getElementById('signal-one');
  const btnS0 = document.getElementById('signal-zero');
  const boxS1 = document.getElementById('box-s1');
  const boxS0 = document.getElementById('box-s0');

  const lblPriorPass = document.getElementById('lbl-prior-pass');
  const lblPriorFail = document.getElementById('lbl-prior-fail');
  const lblA1 = document.getElementById('lbl-a1');
  const lblA0 = document.getElementById('lbl-a0');
  const lblB1 = document.getElementById('lbl-b1');
  const lblB0 = document.getElementById('lbl-b0');

  const pathS1Pass = document.getElementById('path-s1-pass');
  const pathS1Fail = document.getElementById('path-s1-fail');
  const pathS0Pass = document.getElementById('path-s0-pass');
  const pathS0Fail = document.getElementById('path-s0-fail');

  const tblPassS1 = document.getElementById('tbl-pass-s1');
  const tblPassS0 = document.getElementById('tbl-pass-s0');
  const tblFailS1 = document.getElementById('tbl-fail-s1');
  const tblFailS0 = document.getElementById('tbl-fail-s0');

  let activeSignal = 1;

  function pct(x) {
    return (100 * x).toFixed(1) + '%';
  }

  function fmtProb(x) {
    return x.toFixed(3);
  }

  function bayesPosteriorS1(pi0, alpha, beta) {
    const num = (1 - alpha) * pi0;
    const den = (1 - alpha) * pi0 + beta * (1 - pi0);
    return num / den;
  }

  function bayesPosteriorS0(pi0, alpha, beta) {
    const num = alpha * pi0;
    const den = alpha * pi0 + (1 - beta) * (1 - pi0);
    return num / den;
  }

  function setActiveSignal(signal) {
    activeSignal = signal;

    if (signal === 1) {
      btnS1.classList.add('active');
      btnS0.classList.remove('active');
      boxS1.classList.add('active');
      boxS0.classList.remove('active');

      pathS1Pass.setAttribute('opacity', '0.95');
      pathS1Fail.setAttribute('opacity', '0.95');
      pathS0Pass.setAttribute('opacity', '0.10');
      pathS0Fail.setAttribute('opacity', '0.10');
    } else {
      btnS0.classList.add('active');
      btnS1.classList.remove('active');
      boxS0.classList.add('active');
      boxS1.classList.remove('active');

      pathS0Pass.setAttribute('opacity', '0.95');
      pathS0Fail.setAttribute('opacity', '0.95');
      pathS1Pass.setAttribute('opacity', '0.10');
      pathS1Fail.setAttribute('opacity', '0.10');
    }

    updateAll();
  }

  function updateAll() {
    const pi0 = parseFloat(priorSlider.value) / 100.0;
    const alpha = parseFloat(alphaSlider.value) / 100.0;
    const beta = parseFloat(betaSlider.value) / 100.0;

    const pS1 = bayesPosteriorS1(pi0, alpha, beta);
    const pS0 = bayesPosteriorS0(pi0, alpha, beta);

    priorDisplay.textContent = pct(pi0);
    alphaDisplay.textContent = pct(alpha);
    betaDisplay.textContent = pct(beta);

    postS1.textContent = pct(pS1);
    postS0.textContent = pct(pS0);

    if (activeSignal === 1) {
      activePosterior.innerHTML = '\\( \\pi_1(s=1) = ' + pct(pS1) + ' \\)';
    } else {
      activePosterior.innerHTML = '\\( \\pi_1(s=0) = ' + pct(pS0) + ' \\)';
    }

    lblPriorPass.textContent = fmtProb(pi0);
    lblPriorFail.textContent = fmtProb(1 - pi0);

    lblA1.textContent = fmtProb(1 - alpha);
    lblA0.textContent = fmtProb(alpha);

    lblB1.textContent = fmtProb(beta);
    lblB0.textContent = fmtProb(1 - beta);

    tblPassS1.innerHTML = '\\(' + fmtProb(1 - alpha) + '\\)';
    tblPassS0.innerHTML = '\\(' + fmtProb(alpha) + '\\)';
    tblFailS1.innerHTML = '\\(' + fmtProb(beta) + '\\)';
    tblFailS0.innerHTML = '\\(' + fmtProb(1 - beta) + '\\)';

    if (window.MathJax && window.MathJax.typesetPromise) {
      MathJax.typesetPromise([
        activePosterior,
        tblPassS1,
        tblPassS0,
        tblFailS1,
        tblFailS0
      ]).catch(() => {});
    }
  }

  priorSlider.addEventListener('input', updateAll);
  alphaSlider.addEventListener('input', updateAll);
  betaSlider.addEventListener('input', updateAll);

  btnS1.addEventListener('click', () => setActiveSignal(1));
  btnS0.addEventListener('click', () => setActiveSignal(0));

  window.addEventListener('load', () => {
    setActiveSignal(1);
    updateAll();
  });
</script>
