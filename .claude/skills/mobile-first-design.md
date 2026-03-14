---
name: mobile-first-design
description: Design and build mobile-first responsive web experiences. Start with the smallest screen as baseline, progressively enhance for larger viewports. Covers touch targets, thumb zones, performance budgets, content hierarchy, and responsive patterns for landing pages and web applications.
---

# Mobile-First Design Skill

Design for the smallest screen first, then progressively enhance for larger viewports. This ensures core functionality works everywhere while taking advantage of additional screen real estate when available.

---

## Core Philosophy

**Mobile-first means:**
- Start with 320-375px as the baseline design
- Add complexity and features as viewport grows
- Essential content and actions work without enhancement
- Performance is optimized for slower mobile networks

**Mobile-first does NOT mean:**
- Design for desktop, then "adapt" for mobile
- Hide content on mobile that exists on desktop
- Treat mobile as a degraded experience

---

## Viewport Breakpoints

| Range | Target Devices | Design Approach |
|-------|----------------|-----------------|
| 320-480px | iPhone SE, older phones | Single column, essential content only, touch-optimized |
| 481-768px | Large phones, small tablets | Minor enhancements, still primarily single column |
| 769-1024px | Tablets, small laptops | Two-column layouts, secondary content appears |
| 1025-1440px | Laptops, desktops | Full layouts, sidebars, advanced interactions |
| 1441px+ | Large monitors | Max-width constraints, generous whitespace |

### Tailwind Mapping

```css
/* Mobile-first: no prefix = mobile baseline */
/* sm: 640px+ */
/* md: 768px+ */
/* lg: 1024px+ */
/* xl: 1280px+ */
/* 2xl: 1536px+ */
```

---

## Touch Targets & Thumb Zones

### Minimum Touch Targets

- **Minimum size:** 44×44px (Apple HIG) or 48×48px (Material Design)
- **Minimum spacing:** 8px between interactive elements
- **Preferred size:** 48×48px with 12px spacing

```tsx
// Good: Adequate touch target
<button className="min-h-[48px] min-w-[48px] px-6 py-3">
  Submit
</button>

// Bad: Too small for reliable touch
<button className="px-2 py-1 text-sm">
  Submit
</button>
```

### Thumb Zone Placement

On mobile, users hold phones with one hand and navigate with their thumb. The **bottom 60% of the screen** is the easiest to reach.

```
┌─────────────────────────┐
│                         │  ← Hard to reach (stretch zone)
│    Logo / Branding      │
│                         │
├─────────────────────────┤
│                         │
│    Main Content         │  ← Natural reach
│                         │
│                         │
├─────────────────────────┤
│                         │
│    CTAs / Navigation    │  ← Easy thumb access (comfort zone)
│                         │
└─────────────────────────┘
```

**Implications:**
- Primary CTAs should be in lower portion of viewport
- Sticky bottom bars work well for key actions
- Hamburger menus in top corners require stretch

---

## Content Hierarchy for Mobile

### Above-the-Fold Priorities

On mobile, "above the fold" is ~500-600px. Include:
1. Clear headline (what is this?)
2. Value proposition (why should I care?)
3. Primary CTA or clear next action
4. Trust indicator (optional but helpful)

### Progressive Disclosure Patterns

Don't dump everything on mobile users. Use:

**Accordion/Collapse:**
```tsx
<details>
  <summary>See more details</summary>
  <p>Extended content here...</p>
</details>
```

**Read More Truncation:**
```tsx
<p className="line-clamp-3 md:line-clamp-none">
  Long content that truncates on mobile...
</p>
<button className="md:hidden">Read more</button>
```

**Swipe Carousels (use sparingly):**
- Show partial next item to indicate scrollability
- Include pagination dots
- Consider if stacking is better for discoverability

---

## Performance Budget

Mobile users often have slower connections. Set strict targets:

| Metric | Target | Critical |
|--------|--------|----------|
| First Contentful Paint (FCP) | < 1.8s | < 3s |
| Largest Contentful Paint (LCP) | < 2.5s | < 4s |
| Cumulative Layout Shift (CLS) | < 0.1 | < 0.25 |
| Time to Interactive (TTI) | < 3.8s | < 5s |
| Total JS (gzipped) | < 100KB | < 150KB |
| Total CSS (gzipped) | < 50KB | < 75KB |

### Performance Optimization Techniques

**Image Optimization:**
```tsx
// Use Next.js Image with responsive sizes
<Image
  src="/hero.jpg"
  alt="Hero"
  sizes="(max-width: 768px) 100vw, 50vw"
  priority // Only for above-fold images
/>
```

**Lazy Loading:**
```tsx
// Defer below-fold content
<section className="min-h-screen">
  {/* Critical content */}
</section>

<Suspense fallback={<Skeleton />}>
  <BelowFoldSection />
</Suspense>
```

**Animation Deferral:**
```tsx
// Load heavy animation libraries after initial paint
const [mounted, setMounted] = useState(false);
useEffect(() => setMounted(true), []);

{mounted && <ParticleCanvas />}
```

**CSS-First Animations:**
```css
/* Prefer CSS over JS for simple animations */
@keyframes fadeIn {
  from { opacity: 0; transform: translateY(20px); }
  to { opacity: 1; transform: translateY(0); }
}

.reveal {
  animation: fadeIn 0.6s ease-out forwards;
}
```

---

## Responsive Layout Patterns

### Single Column → Multi-Column

```tsx
// Mobile: single column, Desktop: 3 columns
<div className="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-4 md:gap-6">
  <Card />
  <Card />
  <Card />
</div>
```

### Stacked → Side-by-Side

```tsx
// Mobile: stacked, Desktop: split screen
<div className="flex flex-col lg:flex-row gap-8">
  <div className="lg:w-1/2">{/* Left content */}</div>
  <div className="lg:w-1/2">{/* Right content */}</div>
</div>
```

### Hidden on Mobile → Visible on Desktop

```tsx
// Progressive enhancement
<aside className="hidden lg:block lg:w-64">
  {/* Sidebar only on desktop */}
</aside>

// Show simplified version on mobile
<nav className="lg:hidden">
  {/* Mobile-specific nav */}
</nav>
```

---

## Mobile Navigation Patterns

### Bottom Tab Bar
Best for apps with 3-5 primary destinations.

```tsx
<nav className="fixed bottom-0 left-0 right-0 h-16 bg-surface border-t md:hidden">
  <div className="flex justify-around items-center h-full">
    <TabButton icon={<Home />} label="Home" />
    <TabButton icon={<Search />} label="Search" />
    <TabButton icon={<Profile />} label="Profile" />
  </div>
</nav>
```

### Sticky CTA Bar
Best for landing pages with single conversion goal.

```tsx
<div className="fixed bottom-0 left-0 right-0 p-4 bg-surface/90 backdrop-blur border-t md:hidden">
  <button className="w-full py-4 bg-gold text-black font-semibold rounded-lg">
    Get Started
  </button>
</div>
```

### Smart-Hide Header
Hide on scroll down, show on scroll up.

```tsx
const [hidden, setHidden] = useState(false);
const lastScroll = useRef(0);

useEffect(() => {
  const onScroll = () => {
    const y = window.scrollY;
    setHidden(y > 100 && y > lastScroll.current);
    lastScroll.current = y;
  };
  window.addEventListener('scroll', onScroll, { passive: true });
  return () => window.removeEventListener('scroll', onScroll);
}, []);
```

---

## Typography Scaling

Use fluid typography that scales with viewport:

```css
/* Fluid type scale */
--text-xs: clamp(0.75rem, 0.7rem + 0.25vw, 0.875rem);
--text-sm: clamp(0.875rem, 0.8rem + 0.35vw, 1rem);
--text-base: clamp(1rem, 0.9rem + 0.5vw, 1.125rem);
--text-lg: clamp(1.125rem, 1rem + 0.6vw, 1.25rem);
--text-xl: clamp(1.25rem, 1rem + 1vw, 1.5rem);
--text-2xl: clamp(1.5rem, 1.2rem + 1.5vw, 2rem);
--text-3xl: clamp(1.875rem, 1.4rem + 2vw, 2.5rem);
--text-4xl: clamp(2.25rem, 1.6rem + 3vw, 3.5rem);
--text-5xl: clamp(3rem, 2rem + 4vw, 4.5rem);
```

Or with Tailwind:

```tsx
<h1 className="text-3xl sm:text-4xl md:text-5xl lg:text-6xl xl:text-7xl">
  Responsive Headline
</h1>
```

---

## Forms on Mobile

### Best Practices

1. **Single column layout** — Never side-by-side fields on mobile
2. **Large input fields** — min-height 48px
3. **Appropriate input types** — `type="email"`, `type="tel"`, `inputmode="numeric"`
4. **Sticky submit button** — Keep CTA visible
5. **Inline validation** — Show errors next to fields
6. **Minimal fields** — Every field reduces conversion

```tsx
<form className="space-y-4">
  <input
    type="email"
    inputMode="email"
    autoComplete="email"
    className="w-full min-h-[48px] px-4 rounded-lg"
    placeholder="Email address"
  />
  <button
    type="submit"
    className="w-full min-h-[48px] bg-gold text-black font-semibold rounded-lg"
  >
    Subscribe
  </button>
</form>
```

---

## Testing Checklist

### Device Testing
- [ ] Test on real devices (not just DevTools)
- [ ] Test on slow 3G network simulation
- [ ] Test with large text / accessibility settings
- [ ] Test in both portrait and landscape

### Functionality Testing
- [ ] All touch targets are 44×44px minimum
- [ ] No horizontal scroll on any viewport
- [ ] Forms work with mobile keyboards
- [ ] No hover-dependent interactions
- [ ] CTAs are reachable with thumb

### Performance Testing
- [ ] Lighthouse mobile score > 90
- [ ] FCP < 1.8s on 4G
- [ ] LCP < 2.5s on 4G
- [ ] No layout shift on load
- [ ] Images use responsive sizes

### Accessibility Testing
- [ ] Text is readable without zooming
- [ ] Color contrast meets WCAG AA
- [ ] Focus states are visible
- [ ] Screen reader announces content correctly

---

## Anti-Patterns to Avoid

### DON'T:
- Use hover states as the only interaction feedback
- Hide critical content behind "See more" on mobile
- Use tiny text links instead of buttons
- Require pinch-to-zoom to read content
- Disable zoom (`user-scalable=no`)
- Put important CTAs in top corners (hard to reach)
- Use complex multi-step interactions
- Auto-play video with sound
- Use carousels for critical content

### DO:
- Design for touch first, add mouse interactions as enhancement
- Test on real devices with real network conditions
- Use native form controls (they're optimized for mobile)
- Provide clear feedback for all interactions
- Consider offline/poor connectivity states
- Make text large enough to read (16px+ body)
- Stack content vertically by default

---

## Progressive Enhancement Layers

**Layer 1: HTML (Works everywhere)**
- Semantic markup
- Content is accessible
- Links work, forms submit

**Layer 2: CSS (Visual design)**
- Layout and spacing
- Typography and color
- Responsive breakpoints

**Layer 3: JavaScript (Enhanced interactions)**
- Animations and transitions
- Dynamic content loading
- Advanced interactions

Test each layer independently:
1. Disable JavaScript — does core functionality work?
2. Disable CSS — is content still readable?
3. Throttle to slow 3G — does it load acceptably?

---

## Resources

- [Apple Human Interface Guidelines - Mobile](https://developer.apple.com/design/human-interface-guidelines/)
- [Material Design - Mobile](https://m3.material.io/)
- [web.dev - Mobile Performance](https://web.dev/mobile/)
- [Lighthouse - Performance Auditing](https://developers.google.com/web/tools/lighthouse)

---

*Remember: If it doesn't work well on a 320px screen with a slow connection, it doesn't work well.*
