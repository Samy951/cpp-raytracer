# Discovery report

## Problem statement

A film studio needs a renderer it can extend and debug itself, because an unexplained rendering defect delayed a film by three weeks, but with commercial tools like Maya a new feature takes six months to a year, or is refused.

## Client interview summary

Interview held with the whole class on 16 November 2026. Quotes are taken from the shared class notes; [...] marks words left out.

### Extensibility

**Need (non-functional):** The studio must be able to add its own features to the renderer. The client said this is the main need.

> **Q (class):** "What is the main thing you need?"
>
> **A:** "The ability to add our own features. With Maya [...] it takes 6 months or a year to add a feature."

### Diagnosing defects

**Need (non-functional):** When an image has a defect, the team must find the cause within the same day.

> **Q (class):** "Walk me through the last time you rendered with your previous tool."
>
> **A:** "We kept finding black artifacts [...]. Nobody could tell us why. [...] We delivered 3 weeks late."

### Determinism

**Need (non-functional):** The same input must always produce the same image.

> **Q (class):** "What will show you that the software meets your needs?"
>
> **A:** "If I give it some inputs, I can always expect the same output. [...] If there is an anomaly in an image, we can work out within the same day why it happened."

### Preview and final render

**Need (functional):** Designers need fast, small previews (about 10 per image), and the studio needs a high-quality final render.

> **Q (class):** "How many creative renders do you do?"
>
> **A:** "At least 10 different pre-renders per image."

> **Q (class):** "What is a reasonable render time for you?"
>
> **A:** "They need to get images really quickly."

### Rendering speed

**Need (non-functional):** A 90-minute HD film must render in two months at most. For previews, the client gave no figure.

> **Q (class):** "How long can final rendering take?"
>
> **A:** "We don't want to spend more than 2 months on final rendering."

> **Q (class):** "How long is the film?"
>
> **A:** "It is a 90-minute film" (HD, 24 fps in my notes).

**My conclusion:** 90 min × 60 s × 24 frames/s = 129,600 frames. Two months ≈ 60 days × 24 h × 100 machines = 144,000 machine-hours. So one HD frame must render in about 67 minutes at most on one render-farm machine. This assumes 24 frames/s, machines running 24/7 and no re-renders.

### Inputs and outputs

**Need (functional):** The renderer reads a scene file and writes images. It has no user interface: it runs as a black box on the render farm.

> **Q (class):** "Do you need an interface, or is a file enough?"
>
> **A:** "A standard file is created and saved by another tool, [...] put onto the render farm. No interface at this point."

### Hardware

**Need (constraint):** CPU only, no graphics card, on up to 100 machines.

> **Q (class):** "How big is the farm?"
>
> **A:** "We have up to 100 machines." The client also said: "We don't have graphics cards."

### Shapes

**Need (functional):** Spheres and planes for now. The needs will change with each project.

> **Q (class):** "Which features do you need?"
>
> **A:** "For the moment, [...] spheres and planes."

### Maintainability

**Need (non-functional):** The team must be able to maintain the renderer if its author leaves.

> **Q (class):** "Is there anything we have missed?"
>
> **A:** "What happens when the person who wrote the software leaves. [...] Nobody could tell me why [these choices] had been made."

**My conclusion:** This is what architecture decision records (ADRs) are for: each choice is written down in the repository with its reasons.

### Open source

**Need (constraint and non-functional):** All the code is open source, and partner studios must be able to contribute cleanly.

> **Q (class):** About the licence of the code.
>
> **A (paraphrased):** All the code is open source, and partner studios must be able to contribute cleanly.

### Documentation

**Need (functional):** Technical documentation for developers, and a guide for artists.

> **Q (class):** "What kind of documentation do you need?"
>
> **A:** "Technical people will be reading it. [...] We expect some kind of user guide for the artists."

### Budget and delivery

**Need (constraint):** A budget of at most 500,000, and delivery on 5 January. Last year the studio paid more than $2M in licences, priced per machine.

> **Q (class):** About budget and deadline.
>
> **A (paraphrased, except the quote):** Last year the studio paid more than $2M in licences, priced per machine. The budget is "no more than 500,000" euros, and delivery is on 5 January.

### Open questions

- "HD": I assume 1920×1080, but the client did not confirm it.
- Preview speed: the client said "fast" with no figure. How long may one preview take?
- Do the two months include the previews (about 10 per image) and any re-renders?

## State of the art

| Solution | Maker | Licence | Pricing (date checked) | Capabilities | Sources |
|---|---|---|---|---|---|
| Arnold | Autodesk | Commercial, subscription | $430/year for 1 user (annual plan), or $55/month (monthly plan). US site, USD (checked 2026-11-16) | CPU and GPU; Monte Carlo ray tracing; OSL; denoising; USD Hydra render delegate through the separate open-source arnold-usd project (Apache-2.0) | [Buy page](https://www.autodesk.com/products/arnold/buy), [Product details](https://www.autodesk.com/products/arnold/product-details), [arnold-usd](https://github.com/Autodesk/arnold-usd) |
| RenderMan | Pixar | Commercial; a Non-Commercial licence is offered after registration | $595 per licence, plus $250 per licence for annual maintenance. USD (checked 2026-11-16) | XPU (GPU-accelerated) and CPU; unidirectional and bidirectional path tracing; USD and OSL listed as supported standards; ML denoiser; Linux, macOS, Windows | [Tech specs](https://renderman.pixar.com/tech-specs), [Store](https://renderman.pixar.com/store/) |
| V-Ray | Chaos | Commercial, subscription | Annual billing, list prices: Solo €39.50/month, Premium €58.90/month, Collection €94.90/month. EUR, excluding taxes (checked 2026-11-16) | CPU, GPU or both; USD support and a Hydra delegate for Houdini Solaris; MaterialX; distributed rendering | [Pricing](https://www.chaos.com/pricing#vray), [V-Ray for Houdini](https://www.chaos.com/vray/houdini) |
| Redshift | Maxon | Commercial, subscription | €24.90/month, €298.80 billed annually per licence. EUR, VAT included (checked 2026-11-16) | GPU-accelerated, biased renderer; up to 8 GPUs on one computer; native in Cinema 4D, plugins for Maya, 3ds Max, Houdini, Katana | [Plans and pricing](https://www.maxon.net/en/buy), [Redshift](https://www.maxon.net/en/redshift) |
| Karma | SideFX (Houdini) | Commercial, bundled with Houdini | Extra Karma licence: $195 per 1-year rental. 5 Karma licences come with Houdini Core ($1,475) and Houdini FX ($3,505) 1-year rentals. USD (checked 2026-11-16) | Karma CPU and Karma XPU (GPU and CPU together); Hydra delegate in Solaris; MaterialX; bundled licences usable on a render farm | [Houdini store](https://www.sidefx.com/buy/), [Karma](https://www.sidefx.com/products/karma/) |
| Cycles | Blender project | Open source: Cycles is Apache-2.0, Blender is GPL | No licence fee (checked 2026-11-16) | Path tracing; one kernel for CPU and GPU (CUDA, OptiX, HIP, oneAPI, Metal); OSL (CPU only); denoising; standalone or Hydra render delegate | [cycles-renderer.org](https://www.cycles-renderer.org/), [Blender manual: licence](https://docs.blender.org/manual/en/latest/getting_started/about/license.html), [Repository](https://github.com/blender/cycles) |
| OctaneRender | OTOY | Commercial, subscription, online licence check | Studio+ annual: €19.99/month, billed yearly (checked 2026-11-16) | GPU renderer (NVIDIA CUDA, Apple Silicon); "unbiased, spectrally correct"; OSL; AI denoiser; render node licences included | [Purchase page](https://home.otoy.com/render/octane-render/purchase/), [Product page](https://home.otoy.com/render/octane-render/) |
| pbrt-v4 | Matt Pharr, Wenzel Jakob, Greg Humphreys | Open source, Apache-2.0 | No licence fee (checked 2026-11-16) | Companion renderer of the *Physically Based Rendering* book; CPU, or GPU with CUDA and OptiX; teaching and reference renderer, not a production tool | [Repository](https://github.com/mmp/pbrt-v4) |
| Mitsuba 3 | Realistic Graphics Lab, EPFL | Open source, BSD-style | No licence fee (checked 2026-11-16) | Research renderer; forward and differentiable (inverse) rendering; CPU or GPU through Dr.Jit; RGB or spectral; C++17 | [mitsuba-renderer.org](https://www.mitsuba-renderer.org/), [LICENSE](https://github.com/mitsuba-renderer/mitsuba3/blob/master/LICENSE) |
| LuxCoreRender | LuxCoreRender project | Open source, Apache-2.0 | No licence fee (checked 2026-11-16) | Physically based, unbiased engine; CPU path and bidirectional engines plus OpenCL engines; denoisers; C++ and Python API | [README](https://github.com/LuxCoreRender/LuxCore/blob/master/README.md) |
| Hyperion / Manuka | Walt Disney Animation Studios / Wētā FX | Proprietary, in-house | **Not published**: not offered for sale (checked 2026-11-16) | Physically based path tracers built and used inside the studios | [Hyperion](https://www.disneyanimation.com/technology/hyperion/), [Manuka](https://www.wetafx.co.nz/research-and-tech/technology/manuka) |

Notes:

- Intel [Embree](https://www.embree.org/) (Apache-2.0) and NVIDIA [OptiX](https://developer.nvidia.com/rtx/ray-tracing/optix) are ray-tracing libraries, not complete renderers: they provide the ray-intersection core that renderers are built on.
- Prices are shown as each vendor displayed them: Autodesk and SideFX in USD, Chaos, Maxon and OTOY in EUR, so they cannot be compared directly.

These renderers are mature, and several support production standards such as USD and OSL. The commercial ones are closed and paid per user or per machine: the studio cannot fix a defect or add a feature itself, and licences cost it more than $2M last year. The open-source ones can be modified, but pbrt-v4 and Mitsuba 3 are teaching and research tools rather than production renderers.

## What is ray tracing?

[One paragraph, in your own words.]

**Sources:** I started from [*Ray Tracing in One Weekend*](https://raytracing.github.io/books/RayTracingInOneWeekend.html) (Peter Shirley, Trevor David Black, Steve Hollasch). The reference I would come back to is [*Physically Based Rendering: From Theory to Implementation*, 4th edition](https://pbr-book.org/4ed/contents) (Matt Pharr, Wenzel Jakob, Greg Humphreys).

## What is C++?

### Programming paradigms

[Your first understanding.]

### What to watch out for

[Your first understanding.]

### Shaping the domain model

[Your first understanding.]

**Sources:** I started from [Learn C++](https://www.learncpp.com/) and Bjarne Stroustrup's [C++ Style and Technique FAQ](https://www.stroustrup.com/bs_faq.html#Object-Oriented-language). The references I would come back to are the [C++ Core Guidelines](https://isocpp.github.io/CppCoreGuidelines/CppCoreGuidelines) (Bjarne Stroustrup, Herb Sutter), [cppreference.com](https://en.cppreference.com/cpp) and [*A Tour of C++*, 3rd edition](https://www.stroustrup.com/tour3.html) (Bjarne Stroustrup).
