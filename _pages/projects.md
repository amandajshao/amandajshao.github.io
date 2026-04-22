---
layout: page
title: Projects
permalink: /projects/
description: Research organized by theme across AI safety, risk analysis, and trustworthy AI.
nav: true
nav_order: 5
---

<div class="projects">

<h2 id="Attack" style="border-bottom: 1px solid var(--global-divider-color); padding-bottom: 0.5rem; margin-top: 2rem;">Attack</h2>
<div class="publications">
{% bibliography -q @*[topic=attack] --group_by none %}
</div>

<h2 id="Evaluation" style="border-bottom: 1px solid var(--global-divider-color); padding-bottom: 0.5rem; margin-top: 2rem;">Evaluation</h2>
<div class="publications">
{% bibliography -q @*[topic=evaluation] --group_by none %}
</div>

<h2 id="Frontier-AI-Risk-Analysis" style="border-bottom: 1px solid var(--global-divider-color); padding-bottom: 0.5rem; margin-top: 2rem;">Frontier AI Risk Analysis</h2>
<div class="publications">
{% bibliography -q @*[topic=frontier] --group_by none %}
</div>

<h2 id="Risk-Mitigation" style="border-bottom: 1px solid var(--global-divider-color); padding-bottom: 0.5rem; margin-top: 2rem;">Risk Mitigation</h2>
<div class="publications">
{% bibliography -q @*[topic=mitigation] --group_by none %}
</div>

</div>
