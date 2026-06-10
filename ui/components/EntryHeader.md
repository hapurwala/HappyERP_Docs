# EntryHeader

The `EntryHeader` is a standardized top-bar component designed explicitly for module entry pages (like Party Master, Customer Entry, etc.). It manages page routing back-links, titles, and workflow stage badges in a consistent format.

## 📦 Import
```tsx
import { EntryHeader } from '../../components/entry/EntryHeader';
```

## 🛠️ Props Reference

| Prop Name | Type | Default | Description |
| :--- | :--- | :--- | :--- |
| `title` | `string` | **Required** | The primary headline of the page. |
| `subtitle` | `string` | `undefined` | Optional secondary description text below the title. |
| `stage` | `string` | `undefined` | The current workflow stage of the document (e.g., 'Draft', 'Approved'). |
| `stageVariant` | `'default' \| 'success' \| 'warning' \| 'error' \| 'info' \| 'outline'` | `'default'` | The color semantic applied to the Stage badge. |
| `onBack` | `() => void` | **Required** | Callback fired when the user clicks the left back-arrow. |

## 🚀 Basic Example

```tsx
import React from 'react';
import { useNavigate } from 'react-router-dom';
import { EntryHeader } from '../../components/entry/EntryHeader';

export default function PurchaseOrderEntry() {
  const navigate = useNavigate();

  return (
    <EntryHeader 
      title="Purchase Order #PO-2023-085"
      subtitle="Vendor: Acme Corp"
      stage="Pending Approval"
      stageVariant="warning"
      onBack={() => navigate('/purchase-orders')}
    />
  );
}
```
