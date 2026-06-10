# FilterPanel

A dynamic off-canvas drawer system that manages complex query parameters, search inputs, and dataset filtering rules without cluttering the main view.

## 📦 Import
```tsx
import { FilterPanel } from '../../components/ui/FilterPanel';
```

## 🛠️ Props Reference

| Prop Name | Type | Description |
| :--- | :--- | :--- |
| `isOpen` | `boolean` | Controlled state for opening/closing the drawer. |
| `onClose` | `() => void` | Callback to close the drawer. |
| `filters` | `FilterField[]` | Configuration for dynamically generating inputs (Selects, Dates, etc.). |
| `onApply` | `(values: any) => void` | Callback containing the applied filter map. |

## 🚀 Basic Example

```tsx
<FilterPanel 
  isOpen={showFilters} 
  onClose={() => setShowFilters(false)}
  filters={[
    { key: 'status', label: 'Status', type: 'select', options: [{label: 'Active', value: 'active'}] }
  ]}
  onApply={(results) => console.log(results)}
/>
```
