# OpalMinds Design System — Usage Conventions

## Setup

Link `styles.css` and `_ds_bundle.js` in every HTML page. No provider or theme wrapper needed — components resolve tokens from CSS custom properties that `styles.css` imports automatically.

```html
<link rel="stylesheet" href="../../styles.css">
<script src="../../_ds_bundle.js"></script>
<script src="https://unpkg.com/react@18.3.1/umd/react.development.js"></script>
<script src="https://unpkg.com/react-dom@18.3.1/umd/react-dom.development.js"></script>
<script src="https://unpkg.com/@babel/standalone@7.29.0/babel.min.js"></script>
<script src="https://unpkg.com/lucide@latest/dist/umd/lucide.min.js"></script>
```

Access all components from the global namespace:

```js
const { Button, Card, Badge, Input, Select, Checkbox, Switch, Tabs, Avatar, IconButton,
        AgentCard, ConfidenceIndicator, ExplainabilityPanel }
  = window.OpalMindsDesignSystem_ef853c;
```

Render JSX with `<script type="text/babel" data-presets="react">`.

## Styling idiom

No CSS classes. Style via component props; use CSS custom properties for layout glue.

| Purpose | Tokens |
|---------|--------|
| Surfaces | `--surface-page` (bg), `--surface-card` (white), `--surface-sunken`, `--surface-inverse` (dark) |
| Text | `--text-strong`, `--text-body`, `--text-muted`, `--text-subtle`, `--text-brand` |
| Spacing | `--space-1` (4px) → `--space-10` (96px); `--gutter` = 24px |
| Type size | `--text-h1` – `--text-h5`, `--text-base` (16px), `--text-caption` (14px), `--text-small` (12px) |
| Fonts | `--font-sans` (DM Sans — all UI), `--font-mono` (IBM Plex Mono — AI outputs & values) |
| Radius | `--radius-sm` (8px), `--radius-md` (10px), `--radius-lg` (16px), `--radius-xl` (20px) |
| Shadows | `--shadow-1` (card default), `--shadow-2` (hover), `--shadow-3` (elevated), `--shadow-brand` |
| Primary | `--primary-500` (#00AEEF) — buttons, links, active states |
| AI/agent | `--accent-purple` (#6F4EF6) — reserve for AI/agent moments only |

## Component API

- **Button** — `variant`: primary | secondary | tertiary | danger; `size`: sm | md | lg; `disabled`, `loading`, `fullWidth`, `iconLeft`, `iconRight`
- **Card** — `padding`: none | sm | md | lg; `elevation`: 0–3; `interactive`, `header`, `footer`
- **Badge** — `tone`: neutral | primary | success | warning | error | info | ai; `variant`: soft | solid; `dot`
- **Input** — `label`, `hint`, `error`, `iconLeft`, `size`, `type`, `placeholder`, `value`, `disabled`, `onChange`
- **AgentCard** — `name`, `role`, `status`: running | idle | review | complete | error; `confidence` (0–100), `lastAction`, `executionTime`, `actions`
- **ConfidenceIndicator** — `score` (0–100), `variant`: bar | ring | badge; `showValue`, `size`
- **ExplainabilityPanel** — `output`, `confidence`, `sources`, `validation`: passed | pending | failed; `review`: approved | required | rejected | auto; `onApprove`, `onReject`

## Sources

Per-component usage: `components/<group>/<Name>.prompt.md`. TypeScript contracts: `<Name>.d.ts`. Full brand guide: `readme.md`. Stylesheets: `styles.css` → `tokens/colors.css`, `tokens/typography.css`, `tokens/spacing.css`, `tokens/radius.css`.

## Example

```jsx
const { Button, Card, Badge, ConfidenceIndicator } = window.OpalMindsDesignSystem_ef853c;

function ExtractionCard({ clause, confidence, status }) {
  return (
    <Card interactive elevation={1}
      header={
        <div style={{ display:'flex', justifyContent:'space-between', alignItems:'center' }}>
          <span style={{ fontFamily:'var(--font-sans)', fontSize:'var(--text-caption)',
                         fontWeight:600, color:'var(--text-strong)' }}>
            {clause}
          </span>
          <Badge tone={status === 'review' ? 'warning' : 'success'} dot>{status}</Badge>
        </div>
      }
    >
      <ConfidenceIndicator score={confidence} variant="bar" showValue />
      <div style={{ display:'flex', gap:'var(--space-3)', marginTop:'var(--space-4)' }}>
        <Button size="sm" variant="primary">Approve</Button>
        <Button size="sm" variant="tertiary">Flag for review</Button>
      </div>
    </Card>
  );
}
```
