# Pagination

Modular page traversal controls for navigating chunked datasets seamlessly.

## 📦 Import
```tsx
import { Pagination } from '../../components/ui/Pagination';
```

## 🛠️ Props Reference

| Prop Name | Type | Description |
| :--- | :--- | :--- |
| `currentPage` | `number` | The current active page (1-indexed). |
| `totalPages` | `number` | Total number of available pages. |
| `onPageChange` | `(page: number) => void` | Hook fired when user clicks a new page. |

## 🚀 Basic Example

```tsx
<Pagination 
  currentPage={1} 
  totalPages={10} 
  onPageChange={(page) => fetchPage(page)} 
/>
```
