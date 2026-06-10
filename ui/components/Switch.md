# Switch

A visual sliding toggle control (pill-shaped) specifically designed for binary preferences and settings. Unlike Checkbox, Switch provides immediate visual feedback of state changes.

## 📦 Import
```tsx
import { Switch } from '../../components/ui/Switch';
```

## 🛠️ Props Reference

| Prop Name | Type | Description |
| :--- | :--- | :--- |
| `checked` | `boolean` | Controlled state of the switch. |
| `onCheckedChange` | `(checked: boolean) => void` | Callback fired when the user toggles the switch. |
| `disabled` | `boolean` | Disables interaction and grays out the switch. |

## 🚀 Basic Example

```tsx
<div style={{ display: 'flex', justifyContent: 'space-between' }}>
  <span>Enable WhatsApp Notifications</span>
  <Switch 
    checked={notify} 
    onCheckedChange={setNotify} 
  />
</div>
```
