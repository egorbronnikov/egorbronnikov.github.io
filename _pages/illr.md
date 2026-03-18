---
layout: page
title: Interactive LLR
permalink: /illr/
description: An interactive app for exploring the Log-Likelihood Ratio
nav: false
nav_order: 3
---

<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=Libre+Baskerville:wght@400;700&family=Inter:wght@300;400;500&display=swap" rel="stylesheet">

<script src="https://cdn.plot.ly/plotly-2.35.2.min.js"></script>
<script>
  window.MathJax = {
    tex: { inlineMath: [['\\(', '\\)'], ['$', '$']], displayMath: [['$$', '$$']] },
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
    --serif: "Libre Baskerville", serif;
    --sans: "Inter", sans-serif;
  }

  .illr-wrap {
    background: var(--bg);
    padding: 28px 24px 48px;
    border-radius: 16px;
  }

  .illr-inner {
    max-width: 960px;
    margin: 0 auto;
  }

  .illr-title {
    font-family: var(--serif);
    font-size: 2rem;
    line-height: 1.25;
    color: var(--text);
    margin: 0 0 12px;
  }

  .illr-subtitle {
    font-family: var(--sans);
    font-size: 1.05rem;
    color: var(--muted);
    margin: 0 0 24px;
    font-weight: 300;
  }

  .illr-graph {
    width: 100%;
    height: 560px;
    margin-bottom: 28px;
  }

  .illr-control {
    margin: 0 0 24px;
  }

  .illr-label {
    display: block;
    font-family: var(--sans);
    font-size: 1rem;
    font-weight: 500;
    color: var(--text);
    margin-bottom: 10px;
  }

  .illr-value {
    margin-left: 8px;
    font-weight: 500;
  }

  .illr-slider {
    width: 100%;
    appearance: none;
    height: 8px;
    border-radius: 999px;
    background: #d9d9d9;
    outline: none;
  }

  .illr-slider::-webkit-slider-thumb {
    appearance: none;
    width: 20px;
    height: 20px;
    border-radius: 50%;
    background: var(--red);
    border: 2px solid #8b0000;
    cursor: pointer;
  }

  .illr-slider::-moz-range-thumb {
    width: 20px;
    height: 20px;
    border-radius: 50%;
    background: var(--red);
    border: 2px solid #8b0000;
    cursor: pointer;
  }

  .illr-ticks {
    display: flex;
    justify-content: space-between;
    font-family: var(--sans);
    font-size: 0.82rem;
    color: var(--muted);
    margin-top: 8px;
  }

  .illr-output {
    margin: 28px 0 42px;
    font-family: var(--sans);
    font-size: 1.05rem;
    color: var(--text);
    font-weight: 500;
  }

  .illr-section-title {
    font-family: var(--serif);
    font-size: 1.2rem;
    color: var(--text);
    margin: 30px 0 14px;
  }

  .illr-text {
    font-family: var(--serif);
    font-size: 1.02rem;
    line-height: 1.8;
    color: var(--text);
    margin: 0 0 16px;
  }

  .illr-math {
    text-align: center;
    margin: 20px 0 28px;
    font-family: var(--serif);
    color: var(--text);
  }

  .illr-bullets {
    margin: 0 0 24px 0;
    padding-left: 1.25rem;
  }

  .illr-bullets li {
    font-family: var(--serif);
    font-size: 1.02rem;
    line-height: 1.8;
    color: var(--text);
    margin-bottom: 8px;
  }

  @media (max-width: 700px) {
    .illr-wrap {
      padding: 20px 14px 34px;
    }
    .illr-graph {
      height: 420px;
    }
  }
</style>

<div class="illr-wrap">
  <div class="illr-inner">
    <h1 class="illr-title">Log-Likelihood Ratio</h1>
    <p class="illr-subtitle">
      This interactive page lets you explore how changes in prior and posterior beliefs affect the Log-Likelihood Ratio (LLR).
    </p>

    <div id="illr-plot" class="illr-graph"></div>

    <div class="illr-control">
      <label class="illr-label" for="posterior">
        Posterior value:
        <span id="posterior-value" class="illr-value">50.00</span>
      </label>
      <input id="posterior" class="illr-slider" type="range" min="0.5" max="99.5" step="0.5" value="50">
      <div class="illr-ticks">
        <span>0</span><span>10</span><span>20</span><span>30</span><span>40</span><span>50</span><span>60</span><span>70</span><span>80</span><span>90</span><span>100</span>
      </div>
    </div>

    <div class="illr-control">
      <label class="illr-label" for="prior">
        Prior value:
        <span id="prior-value" class="illr-value">50.00</span>
      </label>
      <input id="prior" class="illr-slider" type="range" min="0.5" max="99.5" step="0.5" value="50">
      <div class="illr-ticks">
        <span>0</span><span>10</span><span>20</span><span>30</span><span>40</span><span>50</span><span>60</span><span>70</span><span>80</span><span>90</span><span>100</span>
      </div>
    </div>

    <div class="illr-output">
      LLR value at (Posterior, Prior):
      <span id="llr-value">0.00</span>
    </div>

    <h2 class="illr-section-title">What is the LLR?</h2>
    <p class="illr-text">
      The log-likelihood ratio (LLR) is a standard tool used to describe how beliefs change when new information arrives.
      It measures both the direction and the strength of belief updating.
    </p>

    <h2 class="illr-section-title">How is the LLR constructed?</h2>
    <p class="illr-text">
      Suppose the outcome of an exam is binary: a student either passes or fails. Let the prior probability of passing be
      \( \mathbb{P}(\text{Passed}) = \pi \). Since the outcome is binary, the probability of failing is
      \( \mathbb{P}(\text{Failed}) = 1 - \pi \).
    </p>

    <p class="illr-text">
      Now suppose the student receives a signal about their performance. The signal is informative but not perfect.
      Conditional on the true outcome, it is generated with probabilities
      \( \mathbb{P}(\text{Signal} \mid \text{Passed}) \) and
      \( \mathbb{P}(\text{Signal} \mid \text{Failed}) \).
    </p>

    <p class="illr-text">Using Bayes’ rule, the posterior probability of passing is:</p>
    <div class="illr-math">
      $$ \mathbb{P}(\text{Passed} \mid \text{Signal}) =
      \frac{
        \mathbb{P}(\text{Signal} \mid \text{Passed}) \cdot \mathbb{P}(\text{Passed})
      }{
        \mathbb{P}(\text{Signal} \mid \text{Passed}) \cdot \mathbb{P}(\text{Passed})
        +
        \mathbb{P}(\text{Signal} \mid \text{Failed}) \cdot \mathbb{P}(\text{Failed})
      } $$
    </div>

    <p class="illr-text">Similarly, the posterior probability of failing is:</p>
    <div class="illr-math">
      $$ \mathbb{P}(\text{Failed} \mid \text{Signal}) =
      \frac{
        \mathbb{P}(\text{Signal} \mid \text{Failed}) \cdot \mathbb{P}(\text{Failed})
      }{
        \mathbb{P}(\text{Signal} \mid \text{Failed}) \cdot \mathbb{P}(\text{Failed})
        +
        \mathbb{P}(\text{Signal} \mid \text{Passed}) \cdot \mathbb{P}(\text{Passed})
      } $$
    </div>

    <p class="illr-text">Taking the ratio of posterior odds and then taking logs gives:</p>
    <div class="illr-math">
      $$ \log \left(
      \frac{\mathbb{P}(\text{Passed} \mid \text{Signal})}
      {\mathbb{P}(\text{Failed} \mid \text{Signal})}
      \right)
      =
      \log \left(
      \frac{\mathbb{P}(\text{Passed})}
      {\mathbb{P}(\text{Failed})}
      \right)
      +
      \log \left(
      \frac{\mathbb{P}(\text{Signal} \mid \text{Passed})}
      {\mathbb{P}(\text{Signal} \mid \text{Failed})}
      \right) $$
    </div>

    <p class="illr-text">This leads to the definition of the log-likelihood ratio (LLR):</p>
    <div class="illr-math">
      $$ \text{LLR} =
      \log \left( \frac{\text{Posterior}}{1-\text{Posterior}} \right)
      -
      \log \left( \frac{\text{Prior}}{1-\text{Prior}} \right) $$
    </div>

    <h2 class="illr-section-title">Interpretation of LLR</h2>
    <ul class="illr-bullets">
      <li>If LLR &gt; 0, the signal provides evidence in favor of passing.</li>
      <li>If LLR &lt; 0, the signal provides evidence in favor of failing.</li>
      <li>Larger absolute values of LLR correspond to stronger evidence.</li>
    </ul>

    <h2 class="illr-section-title">Example</h2>
    <p class="illr-text">
      Suppose your prior belief of passing is 50%. After receiving a positive signal, your belief increases to 75%.
      The LLR is then:
    </p>

    <div class="illr-math">
      $$ \text{LLR} =
      \log \left( \frac{0.75}{0.25} \right)
      -
      \log \left( \frac{0.50}{0.50} \right)
      =
      \log(3) \approx 1.10 $$
    </div>

    <p class="illr-text">
      This positive value means the signal supports passing. If instead your posterior dropped to 25%,
      the LLR would be negative, indicating evidence against passing.
    </p>
  </div>
</div>

<script>
  const posteriorSlider = document.getElementById('posterior');
  const priorSlider = document.getElementById('prior');
  const posteriorValue = document.getElementById('posterior-value');
  const priorValue = document.getElementById('prior-value');
  const llrValue = document.getElementById('llr-value');
  const plotDiv = document.getElementById('illr-plot');

  function linspace(start, end, n) {
    const arr = [];
    const step = (end - start) / (n - 1);
    for (let i = 0; i < n; i++) arr.push(start + step * i);
    return arr;
  }

  function computeLLR(posterior, prior) {
    return Math.log((posterior / prior) * ((100 - prior) / (100 - posterior)));
  }

  function updatePlot() {
    const posterior = Math.min(99.5, Math.max(0.5, parseFloat(posteriorSlider.value)));
    const prior = Math.min(99.5, Math.max(0.5, parseFloat(priorSlider.value)));

    posteriorValue.textContent = posterior.toFixed(1);
    priorValue.textContent = prior.toFixed(1);

    const x = linspace(0.5, 99.5, 500);
    const y = x.map(px => computeLLR(px, prior));
    const pointLLR = computeLLR(posterior, prior);

    llrValue.textContent = pointLLR.toFixed(2);

    const trace = {
      x: x,
      y: y,
      type: 'scatter',
      mode: 'lines',
      name: 'LLR',
      line: { color: '#0039a6', width: 3 }
    };

    const verticalLine = {
      x: [posterior, posterior],
      y: [Math.min(...y), Math.max(...y)],
      type: 'scatter',
      mode: 'lines',
      name: 'Posterior value',
      line: { color: 'darkred', dash: 'dash', width: 2 }
    };

    const layout = {
      paper_bgcolor: '#fff0db',
      plot_bgcolor: '#fff0db',
      margin: { l: 90, r: 50, t: 20, b: 70 },
      xaxis: {
        title: { text: 'Posterior', font: { family: 'Inter, sans-serif', size: 18, color: '#111111' } },
        tickfont: { family: 'Inter, sans-serif', size: 13, color: '#111111' },
        range: [0, 100],
        tickvals: [0,10,20,30,40,50,60,70,80,90,100]
      },
      yaxis: {
        title: { text: 'Log-Likelihood Ratio (LLR)', font: { family: 'Inter, sans-serif', size: 18, color: '#111111' } },
        tickfont: { family: 'Inter, sans-serif', size: 13, color: '#111111' }
      },
      legend: {
        font: { family: 'Inter, sans-serif', size: 13, color: '#111111' },
        orientation: 'h',
        y: 1.08
      }
    };

    Plotly.newPlot(plotDiv, [trace, verticalLine], layout, { responsive: true, displayModeBar: false });
  }

  posteriorSlider.addEventListener('input', updatePlot);
  priorSlider.addEventListener('input', updatePlot);
  window.addEventListener('load', updatePlot);
  window.addEventListener('resize', () => Plotly.Plots.resize(plotDiv));
</script>
