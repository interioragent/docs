---
title: Home
layout: home
nav_order: 1
---

# InteriorAgent-IDSL

InteriorAgent-IDSDL is an Interior-Design-aware Scene Description Language for composing structured 3D indoor scenes through high-level object registration, group-based placement, and scene optimization.

This documentation is intended to help readers understand what the language can express and how to use it to build scenes efficiently. 

## What this documentation covers

The language is organized around three main ideas:

- **Object registration**  
  Adding scene assets and highlighting the various functionalities available on them.

- **Object placement via groups**  
  Arranging objects using functional groups as reusable spatial abstractions. We provide several implementations, including relative groups, grid groups, around groups, and room groups.

- **Scene optimization**  
  Refining layouts with geometric and higher-level constraints to improve plausibility and spatial coherence. These include both gradient-based and VLM-based constraints.

Together, these components allow users to describe indoor scenes at a level closer to design intent than low-level 3D manipulation.

Additionally, the language is designed to accommodate advanced tools, including LLMs, diffusion models, and 3D generation repositories.

Towards the end of this documentation, you will also find several complete examples of scene creation.

## Start here

If you are new to the language, follow these pages in order:

1. [Installation](installation)  
   Set up the environment and dependencies needed to run InteriorAgent-IDSDL.

2. [Getting Started](getting-started)  
   Walk through a complete example scene program and learn the overall workflow of writing and exporting a scene.

## About the project

InteriorAgent-IDSDL is part of the Ph.D. thesis work of **Kunal Gupta**, a **CSE Ph.D. student at UC San Diego**. His research investigates how to codify the expertise of artists and designers in computational form, with the broader goal of enabling generative AI systems to perform better on creative tasks.

Within that larger research agenda, InteriorAgent-IDSDL serves as a structured language for expressing interior design intent in a way that is both human-authored and machine-actionable.

## Next step

Continue to [Installation](installation) to set up the system, and then move on to [Getting Started](getting-started) to build your first scene.
