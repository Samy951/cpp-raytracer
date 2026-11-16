# Discovery report

## Problem statement

A film studio needs a renderer it can extend and debug itself, because an unexplained rendering defect delayed a film by three weeks, but with commercial tools like Maya a new feature takes six months to a year, or is refused.

## Client interview summary

The interview was a whole-class round robin. I did not record who asked each question or its exact wording, so questions are given by topic and answers are paraphrased from my notes. Words in quotation marks are the client's own.

### Extensibility

**Need (non-functional):** The studio must be able to add its own features to the renderer. The client said this is the main need.

> **Q (class):** About adding the studio's own features.
>
> **A (paraphrased):** This is the main need. With Maya, a new feature takes six months to a year, or is refused.

### Diagnosing defects

**Need (non-functional):** When an image has a defect, the team must find the cause within the same day.

> **Q (class):** About problems with the current tools.
>
> **A (paraphrased):** Six months ago, black dots appeared in the images and rendering became twice as slow, and nobody knew why. The studio had to buy Maya licences and delivered three weeks late. When an image has a defect, they must find out why within the day.

### Determinism

**Need (non-functional):** The same input must always produce the same image.

> **Q (class):** About how reliable the results must be.
>
> **A (paraphrased):** The same input data must always give the same image.

### Preview and final render

**Need (functional):** Designers need fast, small previews (about 10 per image), and the studio needs a high-quality final render.

> **Q (class):** About how the renderer will be used.
>
> **A (paraphrased):** Designers need a quick, small preview, about 10 previews per image. The studio also needs a high-quality final render.

### Rendering speed

**Need (non-functional):** A 90-minute HD film must render in two months at most. For previews, the client gave no figure.

> **Q (class):** About rendering time.
>
> **A (paraphrased):** A 90-minute film in HD must render in two months at most. Previews must be fast, with no figure given.

**My conclusion:** 90 min × 60 s × 24 frames/s = 129,600 frames. Two months ≈ 60 days × 24 h × 100 machines = 144,000 machine-hours. So one HD frame must render in about 67 minutes at most on one render-farm machine. This assumes 24 frames/s, machines running 24/7 and no re-renders.

### Inputs and outputs

**Need (functional):** The renderer reads a scene file and writes images. It has no user interface: it runs as a black box on the render farm.

> **Q (class):** About how the renderer is run.
>
> **A (paraphrased):** A scene file goes in and images come out. There is no interface: it is a black box launched on the render farm.

### Hardware

**Need (constraint):** CPU only, no graphics card, on up to 100 machines.

> **Q (class):** About the available hardware.
>
> **A (paraphrased):** CPUs only, no graphics cards, on "up to 100" machines.

### Shapes

**Need (functional):** Spheres and planes for now. The needs will change with each project.

> **Q (class):** About which shapes the renderer must draw.
>
> **A (paraphrased):** Spheres and planes for now. The needs will change with every project.

### Maintainability

**Need (non-functional):** The team must be able to maintain the renderer if its author leaves.

> **Q (class):** About maintaining the software.
>
> **A (paraphrased):** The team must be able to maintain the software if its author leaves. Last time, nobody knew why some architecture choices had been made.

**My conclusion:** This is what architecture decision records (ADRs) are for: each choice is written down in the repository with its reasons.

### Open source

**Need (constraint and non-functional):** All the code is open source, and partner studios must be able to contribute cleanly.

> **Q (class):** About the licence of the code.
>
> **A (paraphrased):** All the code is open source, and partner studios must be able to contribute cleanly.

### Documentation

**Need (functional):** Technical documentation for developers, and a guide for artists.

> **Q (class):** About documentation.
>
> **A (paraphrased):** Technical documentation for developers, and a guide for artists.

### Budget and delivery

**Need (constraint):** A budget of at most 500,000, and delivery on 5 January. Last year the studio paid more than $2M in licences, priced per machine.

> **Q (class):** About budget and deadline.
>
> **A (paraphrased):** Last year the studio paid more than $2M in licences, priced per machine. The maximum budget is 500,000, and delivery is on 5 January.

### Open questions

- Number of machines: my notes say 1,000, but a classmate's transcript says "up to 100". With 1,000 machines, the budget per frame would be about 11 hours instead of 67 minutes.
- Budget currency: euros or dollars?
- "HD": I assume 1920×1080, but the client did not confirm it.
- Preview speed: the client said "fast" with no figure. How long may one preview take?
- Do the two months include the previews (about 10 per image) and any re-renders?

## State of the art

| Solution | Maker | Licence | Pricing (date checked) | Capabilities | Sources |
|---|---|---|---|---|---|
| Arnold | Autodesk | Commercial, subscription | $430/year for 1 user (annual plan), or $55/month (monthly plan). US site, USD (checked 2026-11-16) | CPU and GPU; Monte Carlo ray tracing; OSL; denoising; USD Hydra render delegate through the separate open-source arnold-usd project (Apache-2.0) | [Buy page](https://www.autodesk.com/products/arnold/buy), [Product details](https://www.autodesk.com/products/arnold/product-details), [arnold-usd](https://github.com/Autodesk/arnold-usd) |
| RenderMan | Pixar | Commercial; a Non-Commercial licence is offered after registration | Commercial: **not published**, a quote form is used instead (checked 2026-11-16) | XPU (GPU-accelerated) and CPU; unidirectional and bidirectional path tracing; USD and OSL listed as supported standards; ML denoiser; Linux, macOS, Windows | [Tech specs](https://renderman.pixar.com/tech-specs), [Quote form](https://renderman.pixar.com/quote) |
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
