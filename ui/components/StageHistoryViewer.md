# StageHistoryViewer

A chronological timeline visualization component that tracks and displays a document's journey through various workflow stages, including timestamps, approvers, and rejection reasons.

## 📦 Import
```tsx
import { StageHistoryViewer } from '../../components/ui/StageHistoryViewer';
```

## 🛠️ Props Reference

| Prop Name | Type | Description |
| :--- | :--- | :--- |
| `history` | `StageHistoryRecord[]` | Array of timeline events. |

## 🚀 Basic Example

```tsx
<StageHistoryViewer 
  history={[
    { stage: 'Draft', timestamp: '2023-01-01T10:00:00Z', user: 'Alice' },
    { stage: 'Approved', timestamp: '2023-01-02T14:30:00Z', user: 'Bob' }
  ]}
/>
```
