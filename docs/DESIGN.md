# Design system

Owner: Designer. Anything with pixels must satisfy these rules and the design requirements in [RULES.md](RULES.md).

## Philosophy

Minimal, but **crafted** — never empty or lazy. Minimalism here means: remove until only the essential remains, then perfect what's left. Generous space, strong hierarchy, restraint in color, precision in alignment. The user should feel calm and in control, and sense that every corner was considered. "Simple" is the hardest thing to make look effortless; we put the work in.

Rules of thumb:
- If an element isn't earning its place, cut it.
- One primary action per screen. Everything else recedes.
- Contrast comes from space and weight, not from lines and boxes.
- Nothing decorative. Every pixel has a reason.

## Design tokens

Tokens are the single source of visual truth. **No hardcoded values in product components.** Platform-specific token adapters consume the values in `packages/design-tokens`; web uses CSS variables and mobile uses React Native values.

### Spacing — 4pt base scale
```
space.0  = 0
space.1  = 4
space.2  = 8
space.3  = 12
space.4  = 16   ← default gutter
space.5  = 20
space.6  = 24
space.8  = 32
space.10 = 40
space.12 = 48
space.16 = 64
```
Layout uses these only. Screen horizontal padding = `space.4` (16) as the default.

### Type scale
System font first (SF Pro on iOS, Roboto on Android) — native, fast, familiar. One typeface, weight and size do the work.
```
display   32 / 40   weight 600
title     24 / 32   weight 600
heading   18 / 26   weight 600
body      16 / 24   weight 400   ← default
callout   15 / 22   weight 500
caption   13 / 18   weight 400
```
Line-height is baked in. No more than 3 sizes visible on one screen.

### Color — restrained, semantic
Semantic tokens, not raw hex, in components. Light and dark from day one (dark is not an afterthought).
```
bg            surface background
surface       raised card / sheet
text          primary text
textMuted     secondary text
border        hairline separators (used sparingly)
accent        the single brand/action color
accentText    text on accent
danger        destructive
success       confirmation
```
Discipline: **one accent color.** The UI is near-monochrome; the accent marks the one thing that matters on a screen. Semantic names mean dark mode is a token swap, not a rewrite.

### Radius & elevation
```
radius.sm = 8     radius.md = 12    radius.lg = 16    radius.full = 999
```
Elevation via soft shadow + surface, not hard borders. Shadows are subtle; if you notice the shadow, it's too strong.

### Iconography
One icon set, consistent stroke weight, sized to the type scale. Icons support text, rarely replace it.

## Motion

Owner: Designer. Motion has a spec ([RULES.md](RULES.md) #8). It should feel physical and calm — never showy, never janky.

### Durations & easing
```
motion.fast     120ms   micro-feedback (press, toggle)
motion.base     220ms   most transitions (enter/exit, expand)
motion.slow     320ms   larger surfaces (sheets, screens)
easing.standard cubic-bezier(0.2, 0, 0, 1)     enter
easing.exit     cubic-bezier(0.4, 0, 1, 1)      exit
easing.spring   for gestures — critically damped, no wobble
```
- Prefer platform-native, compositor-friendly animations. Add an animation library only when platform capabilities cannot meet a specified interaction.
- **Proximity / spatial motion:** elements animate *from where they came* — a sheet rises from the button, a detail expands from its list row. Motion explains the spatial relationship, it isn't garnish.
- Respect "reduce motion" OS setting — fall back to a fast fade.
- Every animation must hold 60fps. A dropped frame is a bug.

## Interaction & feedback

- Every tappable target ≥ 44×44pt.
- Every action gives immediate feedback (press state within `motion.fast`), even before the network.
- Draft input updates locally and immediately. Network submission never blocks typing, and a failed request preserves the draft.
- Empty states are designed, not blank — one line of guidance + the primary action.
- Errors are calm and recoverable, never a dead end.

## Responsiveness

- Design the mobile workflow at **375pt** first, then verify at small (320) and large (430+) phones and tablet width.
- Verify the web workflow at narrow mobile, tablet, laptop, and wide desktop widths; constrain reading/editing measure instead of stretching content indefinitely.
- Layout adapts by available space using documented layout tokens and a small breakpoint set.
- Text respects Dynamic Type / font scaling — never truncate meaning.
- Safe areas honored on every screen.

## Accessibility (baseline, not optional)

- Contrast meets WCAG AA against tokens.
- Every control has an accessible label.
- Works with screen readers and larger font sizes.
- Not color-alone for meaning.

## Definition of visually done
Uses tokens only · one primary action · motion follows the spec without jank · deliberate across required web/mobile widths · light + dark · accessible labels + AA contrast · loading, empty, error, offline, permission, and success states designed.
