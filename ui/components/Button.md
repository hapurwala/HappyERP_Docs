# Button

The universal action trigger supporting multiple semantic variants, sizes, icons, and loading states.

## 📦 Import
```tsx
import { Button } from '../../components/ui/Button';
```

## 🛠️ Props Reference

| Prop Name | Type | Default | Description |
| :--- | :--- | :--- | :--- |
| `variant` | `'primary' \| 'secondary' \| 'outline' \| 'ghost' \| 'danger'` | `'primary'` | Defines the color semantics and background style. |
| `size` | `'sm' \| 'md' \| 'lg'` | `'md'` | Defines the padding and height geometry. |
| `isLoading` | `boolean` | `false` | Renders a spinning loader and disables interaction. |
| `leftIcon` | `React.ReactNode` | `undefined` | Icon injected before the text. |
| `rightIcon` | `React.ReactNode` | `undefined` | Icon injected after the text. |

## 🚀 Basic Example

```tsx
<Button 
  variant="primary" 
  size="md" 
  leftIcon={<Save size={16} />}
  isLoading={isSaving}
  onClick={handleSubmit}
>
  Save Document
</Button>
```
