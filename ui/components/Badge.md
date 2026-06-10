# Badge

Small, colored status pills used extensively for tagging attributes or displaying Workflow Stages.

## 📦 Import
```tsx
import { Badge } from '../../components/ui/Badge';
```

## 🛠️ Props Reference

| Prop Name | Type | Default | Description |
| :--- | :--- | :--- | :--- |
| `variant` | `'default' \| 'success' \| 'warning' \| 'error' \| 'info' \| 'outline'` | `'default'` | Determines the background and text color semantic token. |
| `children` | `React.ReactNode` | **Required** | Text inside the badge. |

## 🚀 Basic Example

```tsx
<div>
  <Badge variant="success">Approved</Badge>
  <Badge variant="warning">Pending Review</Badge>
  <Badge variant="error">Rejected</Badge>
</div>
```
