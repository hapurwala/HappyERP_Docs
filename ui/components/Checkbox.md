# Checkbox

A standardized boolean toggle for forms and table row selections, fully styled to match the ERP design system.

## 📦 Import
```tsx
import { Checkbox } from '../../components/ui/Checkbox';
```

## 🛠️ Props Reference

| Prop Name | Type | Description |
| :--- | :--- | :--- |
| `checked` | `boolean` | Controlled state of the checkbox. |
| `onChange` | `(e: React.ChangeEvent<HTMLInputElement>) => void` | Event handler. |
| `disabled` | `boolean` | Disables interaction. |
| `className` | `string` | Custom CSS injection. |

## 🚀 Basic Example

```tsx
<div style={{ display: 'flex', gap: '8px', alignItems: 'center' }}>
  <Checkbox 
    checked={isActive} 
    onChange={(e) => setIsActive(e.target.checked)} 
  />
  <label>Mark as Active</label>
</div>
```
