# Select

The `Select` component is a highly advanced dropdown menu built on Radix UI primitives. It supports single/multi-selection, live searching, custom option rendering, and keyboard accessibility.

## 📦 Import
```tsx
import { Select } from '../../components/ui/Select';
```

## 🛠️ Props Reference

| Prop Name | Type | Description |
| :--- | :--- | :--- |
| `options` | `{ label: string; value: string; description?: string }[]` | **Required.** The list of selectable options. |
| `value` | `string \| string[]` | The currently selected value(s). |
| `onChange` | `(value: any) => void` | Callback fired when selection changes. |
| `isMulti` | `boolean` | Enables multi-select pill tags. |
| `isSearchable` | `boolean` | Enables the internal search input to filter options. |
| `isLoading` | `boolean` | Shows a loading spinner in the dropdown trigger. |
| `label` | `string` | Label rendered above the dropdown. |
| `error` | `string` | Error message rendering. |

## 🚀 Basic Example

```tsx
<Select 
  label="User Role"
  options={[
    { label: 'Admin', value: 'admin', description: 'Full access' },
    { label: 'User', value: 'user', description: 'Limited access' }
  ]}
  value={selectedRole}
  onChange={setSelectedRole}
  isSearchable
/>
```
