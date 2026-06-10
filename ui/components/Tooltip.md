# Tooltip

Contextual popups that reveal helpful descriptions or hidden metadata when hovering over dense UI elements.

## 📦 Import
```tsx
import { Tooltip, TooltipTrigger, TooltipContent, TooltipProvider } from '../../components/ui/Tooltip';
```

## 🛠️ Basic Example

```tsx
<TooltipProvider>
  <Tooltip>
    <TooltipTrigger asChild>
      <Button variant="ghost">Hover me</Button>
    </TooltipTrigger>
    <TooltipContent>
      <p>This explains what the button does.</p>
    </TooltipContent>
  </Tooltip>
</TooltipProvider>
```
