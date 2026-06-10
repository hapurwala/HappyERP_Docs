# Input

The `Input` component is the fundamental text entry field supporting all standard HTML types (text, email, password, number). It includes built-in styling for labels, error states, and generic HTML attributes.

## 📦 Import
```tsx
import { Input } from '../../components/ui/Input';
```

## 🛠️ Props Reference

| Prop Name | Type | Description |
| :--- | :--- | :--- |
| `type` | `string` | Standard HTML input type (`text`, `email`, `number`, etc.). Defaults to `text`. |
| `label` | `string` | Optional text rendered above the input field. |
| `error` | `string` | Error message string. If provided, turns the input borders red and displays the text below. |
| `required` | `boolean` | Adds a mandatory `*` to the label. |
| `...props` | `React.InputHTMLAttributes` | Inherits standard HTML input props like `value`, `onChange`, `placeholder`, `disabled`. |

## 🚀 Basic Example

```tsx
<Input 
  type="email"
  label="Email Address"
  placeholder="john@example.com"
  required
  error="Invalid email format"
/>
```
