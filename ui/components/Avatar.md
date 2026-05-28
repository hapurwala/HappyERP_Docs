# Avatar

Circular wrappers or initials-based placeholders for displaying user portraits and contact faces.

## 📦 Import
```tsx
import { Avatar, AvatarImage, AvatarFallback } from '../../components/ui/Avatar';
```

## 🛠️ Props Reference

| Prop Name | Type | Description |
| :--- | :--- | :--- |
| `src` (Image) | `string` | The URL of the image. |
| `delayMs` (Fallback) | `number` | Delay before showing the fallback initials (to prevent flickering on fast loads). |

## 🚀 Basic Example

```tsx
<Avatar>
  <AvatarImage src="https://github.com/shadcn.png" alt="@shadcn" />
  <AvatarFallback delayMs={600}>CN</AvatarFallback>
</Avatar>
```
