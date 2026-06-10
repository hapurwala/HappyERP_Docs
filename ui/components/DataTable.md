# DataTable

The `DataTable` is our flagship, ultra-dense data grid component. It wraps the core HTML `<Table>` primitive and injects advanced business-logic features like Pagination, Column Sorting, Row Selection, Action Menus, and a fully functional **High-Density Inline Edit Mode**.

## 📦 Import
```tsx
import { DataTable, type DataTableColumn } from '../../components/ui/DataTable';
```

## 🛠️ Key Interfaces

### `DataTableColumn`
Defines the structure and behavior of individual columns.

| Property | Type | Description |
| :--- | :--- | :--- |
| `key` | `string` | The object key mapped to your dataset. |
| `header` | `string` | The text displayed in the table header. |
| `type` | `ColumnType` | Formats data based on type (`Amount`, `Date`, `Image`, `Boolean`, etc.) |
| `sortable` | `boolean` | Enables clicking the header to alphabetically/numerically sort the column. |
| `editable` | `boolean` | Flag indicating if this column can be modified when the row enters Edit Mode. |
| `editType` | `'text' \| 'number' \| 'date' \| 'select' \| 'boolean'` | What form control to render during Edit Mode. |
| `editOptions` | `{ label: string; value: string }[]` | Options array if `editType` is set to `'select'`. |
| `render` | `(value: any, row: any) => ReactNode` | Override the entire cell with custom JSX. |

### `DataTableProps`
Top-level configuration for the data grid.

| Prop Name | Type | Description |
| :--- | :--- | :--- |
| `columns` | `DataTableColumn[]` | Array of column configurations. |
| `data` | `any[]` | Array of row objects. **Must contain a unique `id` field for selection and inline editing.** |
| `rowSelection` | `boolean` | Enables multi-select checkboxes on the left side of the table. |
| `onRowSelectionChange` | `(selectedIds: string[]) => void` | Callback fired when rows are checked. |
| `rowActions` | `(row: any) => DataTableRowAction[]` | Function returning an array of action configurations (e.g. Delete, View). Renders as a 3-dot popover menu. |
| `pagination` | `boolean` | Enables bottom pagination controls to traverse large datasets natively. |
| `pageSize` | `number` | Determines how many rows are rendered per page when pagination is active. |
| `toolbarConfig` | `DataTableToolbarConfig` | Toggles the visibility of native top-bar utilities (search, export, column toggles). |
| `editConfig` | `DataTableEditConfig` | Advanced configuration to enable Inline Editing capabilities. |

### `DataTableToolbarConfig`
Fine-grained control over the top utility bar rendered above the grid.

| Property | Type | Description |
| :--- | :--- | :--- |
| `search` | `boolean` | Shows the global search input box. |
| `columns` | `boolean` | Enables a dropdown to let users dynamically toggle column visibility. |
| `viewToggle` | `boolean` | Enables a switcher between standard List view and Card grid view. |
| `download` | `boolean` | Renders an export/download button. |
| `add` | `boolean` | Shows the primary "+ Add" action button. |
| `onAdd` | `() => void` | Hook fired when the user clicks the Add button. |

## 🚀 Inline Editing Example

The `DataTable` can dynamically swap Text strings for dense form controls (Inputs, Selects, Checkboxes) on a row-by-row basis. When `editConfig` is supplied, the standard Action Menu is automatically intercepted and swapped out for `Save` and `Cancel` hooks.

```tsx
import React, { useState } from 'react';
import { DataTable } from '../../components/ui/DataTable';
import { Trash2 } from 'lucide-react';

export default function RegulatoryTable() {
  const [data, setData] = useState([
    { id: '1', class: 'GSTIN', number: '27AABCU9603R1ZM', issue_date: '2022-01-01' }
  ]);

  return (
    <DataTable 
      data={data}
      columns={[
        { 
          key: 'class', 
          header: 'Class', 
          editable: true, 
          editType: 'select', 
          editOptions: [
            {label: 'GSTIN', value: 'GSTIN'}, 
            {label: 'PAN', value: 'PAN'}
          ] 
        },
        { key: 'number', header: 'Number', editable: true, editType: 'text' },
        { key: 'issue_date', header: 'Issue Date', editable: true, editType: 'date' }
      ]}
      editConfig={{
        mode: 'inline',
        onSave: async (updatedRow) => {
          // This fires when the user clicks the green Checkmark
          const newData = data.map(row => 
            row.id === updatedRow.id ? updatedRow : row
          );
          setData(newData);
        }
      }}
      rowActions={(row) => [
        { 
          label: 'Delete', 
          icon: <Trash2 size={14}/>, 
          onClick: () => setData(data.filter(r => r.id !== row.id)) 
        }
      ]}
    />
  );
}
```

## 🧠 Technical Details
- **Portals**: The `rowActions` popover menu utilizes `ReactDOM.createPortal`. This allows the Action dropdowns to visually escape the `overflow: hidden` bounding boxes of high-density grids without being cropped by scrolling viewports.
- **Form Controls**: When a column enters Edit Mode, the injected form controls (`Input`, `Select`, `DatePicker`) are dynamically compressed using a custom `style` override to maintain a minimal `32px` height profile, ensuring the row doesn't vertically stretch and break your UI layout.
