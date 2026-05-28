# Card

The `Card` is the fundamental surface container for grouping related content, data, or settings. It provides standardized padding, borders, shadows, and flexbox structures for HappyERP layouts.

## 📦 Import
```tsx
import { 
  Card, 
  CardHeader, 
  CardTitle, 
  CardDescription, 
  CardContent, 
  CardFooter 
} from '../../components/ui/Card';
```

## 🛠️ Components & Props

### `Card`
The outermost wrapper.
| Prop Name | Type | Default | Description |
| :--- | :--- | :--- | :--- |
| `variant` | `'default' \| 'outline' \| 'ghost'` | `'default'` | Defines the border and background styling. |
| `className` | `string` | `undefined` | Custom CSS classes. |

### `CardHeader`
Container for titles and actions. Applies standard spacing above the content.

### `CardTitle`
An `h3` component with standardized font weights and sizes.

### `CardDescription`
A `p` component for muted subtext.

### `CardContent`
The primary data pane containing your custom forms, grids, or text.

### `CardFooter`
Bottom-aligned container for actions, pagination, or secondary links.

## 🚀 Basic Example

```tsx
<Card variant="default">
  <CardHeader>
    <CardTitle>Organization Details</CardTitle>
    <CardDescription>Manage the company name and identifiers.</CardDescription>
  </CardHeader>
  <CardContent>
    <form>
      {/* Input fields go here */}
    </form>
  </CardContent>
  <CardFooter>
    <Button variant="primary">Save Changes</Button>
  </CardFooter>
</Card>
```
