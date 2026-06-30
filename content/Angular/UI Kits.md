# UI Kits

UI kits provide reusable UI components and styling patterns.

In Angular, common choices include component libraries like PrimeNG and styling systems like Tailwind CSS.

## PrimeNG

PrimeNG provides ready-made Angular components.

Examples:

- tables
- dropdowns
- dialogs
- calendars
- menus
- form controls

Good for:

- fast development
- consistent widgets
- complex UI components
- enterprise dashboards/admin apps

Watch for:

- theming strategy
- bundle size
- accessibility behavior
- customization limits
- version compatibility

## Tailwind CSS

Tailwind CSS is a utility-first CSS framework.

It provides small classes for styling:

```html
<button class="px-3 py-2 font-medium">
  Save
</button>
```

Good for:

- custom layouts
- design-system-like spacing/colors
- responsive styling
- avoiding large custom CSS files

Watch for:

- inconsistent class usage
- duplicated styling patterns
- unclear design tokens
- mixing random utilities without rules

## Responsiveness

Responsive design makes UI work across screen sizes.

Important:

- flexible layouts
- readable text
- usable tap targets
- table/card behavior on mobile
- no horizontal overflow
- controls remain reachable

## Theming

A theming strategy defines:

- colors
- spacing
- typography
- dark mode
- component variants
- override rules

Do not customize UI libraries randomly in each component. Centralize theme decisions.

## Integration Rules

- choose which library owns which problem
- avoid mixing multiple systems without rules
- keep overrides centralized
- check accessibility and keyboard behavior
- keep responsive behavior consistent
- document common component patterns

## Short Answer

When integrating UI kits, define theming, spacing, responsive rules, and override strategy clearly. PrimeNG provides ready-made Angular components and behavior, while Tailwind gives utility-based styling control. Avoid random mixing. Decide how dark mode, tokens, responsiveness, and component customization should work across the app.

## Related Notes

- [[Components]]
- [[Accessibility]]
- [[i18n]]
- [[Angular Architecture]]
