---
name: "Trueup"
description: "A refined, soft-warm modern dashboard design system blending gentle cloud-toned surfaces with crimson and obsidian accents."
---

## Overview

Trueup combines a serene light canvas, pill-shaped tactile geometry, and high-clarity typography. Inspired by the soft warmth, pill controls, and subtle crimson highlights of contemporary executive fintech tools, it delivers an approachable yet deeply professional experience across desktop and mobile form factors.

The design philosophy balances soft neutral containment cards (`#FFFFFF`, `#F7F7F8`, `#F3F4F6`), subtle border definitions, and stark obsidian contrasts (`#111111`) with vivid rose-crimson accent moments (`#D82E5A`, `#B52E51`).

## Colors

The color palette directly inherits the visual DNA of the reference interface: warm-tinted light background canvases, pure white raised cards with ultra-soft outlines, deep dark pills for primary interactive focus, and energetic crimson accents for key metrics, badges, and active states.

- **Primary:** Deep obsidian charcoal for confident contrast, headers, and primary pills.
- **Accent / Brand:** Vivid rose-crimson for highlights, positive/key badges, and standout tags.
- **Surface:** Soft cool-warm tinted grey canvas with crisp pure white card layers.

```yaml
colors:
  accent: "#D82E5A"
  accent-dark: "#B52E51"
  accent-subtle: "#FCECEF"
  surface-canvas: "#F4F5F7"
  surface-card: "#FFFFFF"
  surface-subtle: "#F8F9FA"
  surface-muted: "#ECEFF2"
  border-soft: "#E5E7EB"
  border-subtle: "#F0F2F5"
  text-heading: "#111827"
  text-body: "#4B5563"
  text-muted: "#9CA3AF"
  success: "#10B981"
  success-subtle: "#ECFDF5"
  warning: "#F59E0B"
  light:
    primary: "#111827"
    on-primary: "#FFFFFF"
    secondary: "#CE3554"
    on-secondary: "#FFFFFF"
    surface: "#FFFFFF"
    on-surface: "#111827"
    surface-container: "#F4F5F7"
    on-surface-container: "#374151"
    surface-container-high: "#E5E7EB"
    on-surface-container-high: "#1F2937"
    outline: "#D1D5DB"
    outline-variant: "#E5E7EB"
    error: "#DC2626"
    on-error: "#FFFFFF"
  dark:
    primary: "#F9FAFB"
    on-primary: "#111827"
    secondary: "#FB7185"
    on-secondary: "#4C0519"
    surface: "#18181B"
    on-surface: "#F4F4F5"
    surface-container: "#27272A"
    on-surface-container: "#D4D4D8"
    surface-container-high: "#3F3F46"
    on-surface-container-high: "#E4E4E7"
    outline: "#52525B"
    outline-variant: "#3F3F46"
    error: "#F87171"
    on-error: "#18181B"
```

## Typography

Trueup uses **Plus Jakarta Sans** for clear, modern geometric headlines and body text, with **JetBrains Mono** for financial tabular numbers and timestamps.

```yaml
typography:
  display-lg:
    fontFamily: "Plus Jakarta Sans"
    fontSize: 40px
    fontWeight: 700
    lineHeight: 48px
    letterSpacing: -0.75px
  display-lg-mobile:
    fontFamily: "Plus Jakarta Sans"
    fontSize: 28px
    fontWeight: 700
    lineHeight: 36px
    letterSpacing: -0.4px
  headline-lg:
    fontFamily: "Plus Jakarta Sans"
    fontSize: 32px
    fontWeight: 700
    lineHeight: 38px
    letterSpacing: -0.5px
  headline-lg-mobile:
    fontFamily: "Plus Jakarta Sans"
    fontSize: 24px
    fontWeight: 700
    lineHeight: 30px
    letterSpacing: -0.3px
  headline-md:
    fontFamily: "Plus Jakarta Sans"
    fontSize: 22px
    fontWeight: 600
    lineHeight: 28px
    letterSpacing: -0.2px
  headline-sm:
    fontFamily: "Plus Jakarta Sans"
    fontSize: 18px
    fontWeight: 600
    lineHeight: 24px
    letterSpacing: -0.1px
  body-lg:
    fontFamily: "Plus Jakarta Sans"
    fontSize: 16px
    fontWeight: 400
    lineHeight: 24px
    letterSpacing: 0px
  body-md:
    fontFamily: "Plus Jakarta Sans"
    fontSize: 14px
    fontWeight: 400
    lineHeight: 20px
    letterSpacing: 0px
  body-sm:
    fontFamily: "Plus Jakarta Sans"
    fontSize: 12px
    fontWeight: 400
    lineHeight: 16px
    letterSpacing: 0px
  label-lg:
    fontFamily: "Plus Jakarta Sans"
    fontSize: 14px
    fontWeight: 600
    lineHeight: 20px
    letterSpacing: 0.1px
  label-md:
    fontFamily: "Plus Jakarta Sans"
    fontSize: 12px
    fontWeight: 600
    lineHeight: 16px
    letterSpacing: 0.1px
  label-sm:
    fontFamily: "Plus Jakarta Sans"
    fontSize: 11px
    fontWeight: 600
    lineHeight: 14px
    letterSpacing: 0.2px
  num-hero:
    fontFamily: "Plus Jakarta Sans"
    fontSize: 36px
    fontWeight: 700
    lineHeight: 42px
    letterSpacing: -0.5px
  num-hero-mobile:
    fontFamily: "Plus Jakarta Sans"
    fontSize: 28px
    fontWeight: 700
    lineHeight: 34px
    letterSpacing: -0.3px
  num-stat:
    fontFamily: "Plus Jakarta Sans"
    fontSize: 24px
    fontWeight: 700
    lineHeight: 30px
    letterSpacing: -0.2px
```

## Layout & Spacing

Layout follows an ergonomic dashboard structure based on the reference layout:
- **Top Navigation Bar:** Integrated brand identity, search/filter pill, global pill navigation items (`Dashboard`, `Transactions`, `Analytics`, `Budgets`), status indicators, and user profile avatar.
- **Main Container:** Fluid with max-width bounding (`max-w-7xl`), organized into a balanced multi-column grid on desktop and clean single-column stacked hierarchy on mobile.
- **Spacing Rhythm:** Standard 8px grid foundation.

```yaml
spacing:
  gutter-xs: 4px
  gutter-sm: 8px
  gutter-md: 16px
  gutter-lg: 24px
  gutter-xl: 32px
  section: 40px
  page-margin: 24px
```

## Elevation & Depth

Surfaces maintain an airy, tactile presence using subtle hairline borders (`border border-[#E5E7EB]/70`), soft ambient shadows (`0 8px 24px -4px rgba(0, 0, 0, 0.04)`), and solid pure white panels floating effortlessly on the soft canvas (`#F4F5F7`). High-emphasis widgets like savings goals or dark contrast cards use solid dark obsidian backgrounds (`#111827`) to anchor visual priority.

## Shapes

Shapes feature generous pill contours and rounded card geometry:
- Primary buttons and filter pills are fully rounded (`rounded-full` / `9999px`).
- Widget cards and panels use smooth rounded corners (`rounded-2xl` / `rounded-3xl` / `16px` to `24px`).
- Inner indicators, badges, and progress pills mirror the outer pill curves.

```yaml
rounded:
  sm: 6px
  DEFAULT: 10px
  md: 14px
  lg: 18px
  xl: 24px
  full: 9999px
```

## Motion & Micro-interaction

Interactions feel crisp and fluid, providing immediate tactile response without decorative excess.

```yaml
motion:
  state-change:
    duration: 120ms
    delay: 0ms
    timingFunction: ease-out
  surface-enter:
    duration: 220ms
    delay: 0ms
    timingFunction: cubic-bezier(0.16, 1, 0.3, 1)
  surface-exit:
    duration: 140ms
    delay: 0ms
    timingFunction: cubic-bezier(0.7, 0, 0.84, 0)
```

## Voice & Content Strategy

- **Tone:** Concise, transparent, executive, and empowering.
- **Numbers:** Always formatted with standard currency notation (`$128,430.50`), currency codes, and precise change deltas (`+12.4% vs Last Week`).
- **Action Verbs:** Explicit action verbs on interactive pills: `Receive Money`, `Send Money`, `Export CSV`, `Add Goal`.

## Navigation Menu Specifications

The navigation system uses floating pill-shaped containers tailored for both desktop and mobile viewports:

### Desktop Navigation (`nav-desktop-pill`)
- **Ubicación:** Barra superior del contenedor principal (`<header>`), centrado entre el bloque de marca/búsqueda (izquierda) y las acciones de usuario/perfil (derecha).
- **Diseño & Estructura:** 
  - Contenedor pill flotante con fondo gris suave (`#F4F5F7`), borde sutil (`border border-[#E5E7EB]/70`), padding interno mínimo (`p-1`) y esquinas redondeadas completas (`rounded-full`).
  - **Item Activo:** Cápsula con fondo oscuro sólido (`bg-[#111827]`), texto blanco (`text-white`), sombra sutil (`shadow-xs`), y radio `rounded-full`. Puede incluir un icono decorativo o funcional.
  - **Items Inactivos:** Texto atenuado (`text-[#4B5563]`), sin fondo, con hover interactivo a texto más oscuro (`hover:text-[#111827]`) o fondo translúcido suave.
- **Código HTML de Referencia:**
```html
<nav class="flex items-center bg-[#F4F5F7] p-1 rounded-full border border-[#E5E7EB]/70 shadow-xs">
  <!-- Active Navigation Item -->
  <a href="#" class="flex items-center gap-1.5 px-4 py-2 rounded-full bg-[#111827] text-white text-xs font-semibold shadow-xs transition-all">
    <span class="material-symbols-outlined text-base">dashboard</span>
    <span>Dashboard</span>
  </a>
  <!-- Inactive Navigation Items -->
  <a href="#" class="flex items-center gap-1.5 px-4 py-2 rounded-full text-[#4B5563] hover:text-[#111827] text-xs font-medium transition-all">
    <span class="material-symbols-outlined text-base">swap_horiz</span>
    <span>Transactions</span>
  </a>
  <a href="#" class="px-4 py-2 rounded-full text-[#4B5563] hover:text-[#111827] text-xs font-medium transition-all">
    Analytics
  </a>
  <a href="#" class="px-4 py-2 rounded-full text-[#4B5563] hover:text-[#111827] text-xs font-medium transition-all">
    Wallet
  </a>
</nav>
```

### Mobile Navigation Dock (`nav-mobile-bottom-dock`)
- **Ubicación:** Anclado de forma fija o pegajosa en la parte inferior de la pantalla (`sticky bottom-0` / `fixed bottom-0 left-0 right-0 z-20`).
- **Diseño & Estructura:**
  - Barra inferior con fondo blanco o translúcido con desenfoque de fondo (`bg-white/95 backdrop-blur`), borde superior sutil (`border-t border-[#E5E7EB]/60`), padding ergonómico táctil (`px-6 py-2.5`) y distribución uniforme (`flex justify-between items-center`).
  - **Item Activo:** Icono y etiqueta coloreados en el tono de acento carmesí (`text-[#D82E5A]`) y peso tipográfico bold (`font-bold`).
  - **Items Inactivos:** Icono y etiqueta en tono neutro atenuado (`text-[#9CA3AF]`), con hover hacia `#111827`.
- **Código HTML de Referencia:**
```html
<nav class="border-t border-[#E5E7EB]/60 bg-white/95 backdrop-blur px-6 py-2.5 flex items-center justify-between sticky bottom-0 z-20">
  <!-- Active Item -->
  <a href="#" class="flex flex-col items-center gap-1 text-[#D82E5A]">
    <span class="material-symbols-outlined text-xl">dashboard</span>
    <span class="text-[10px] font-bold">Home</span>
  </a>
  <!-- Inactive Items -->
  <a href="#" class="flex flex-col items-center gap-1 text-[#9CA3AF] hover:text-[#111827]">
    <span class="material-symbols-outlined text-xl">swap_horiz</span>
    <span class="text-[10px] font-medium">Transactions</span>
  </a>
  <a href="#" class="flex flex-col items-center gap-1 text-[#9CA3AF] hover:text-[#111827]">
    <span class="material-symbols-outlined text-xl">account_balance_wallet</span>
    <span class="text-[10px] font-medium">Wallet</span>
  </a>
  <a href="#" class="flex flex-col items-center gap-1 text-[#9CA3AF] hover:text-[#111827]">
    <span class="material-symbols-outlined text-xl">tune</span>
    <span class="text-[10px] font-medium">Budgets</span>
  </a>
</nav>
```