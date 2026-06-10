# RichTextEditor

A WYSIWYG text editor powered by ReactQuill, allowing users to write formatted HTML notes and rich descriptions.

## 📦 Import
```tsx
import { RichTextEditor } from '../../components/ui/RichTextEditor';
```

## 🛠️ Props Reference

| Prop Name | Type | Description |
| :--- | :--- | :--- |
| `value` | `string` | The HTML content inside the editor. |
| `onChange` | `(html: string) => void` | Hook fired as the user types. |
| `placeholder` | `string` | Empty state text. |

## 🚀 Basic Example

```tsx
<RichTextEditor 
  value={notes}
  onChange={setNotes}
  placeholder="Type your formatted notes here..."
/>
```
