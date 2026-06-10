# Table

The foundational, responsive HTML table structure with standardized padding, typography, and hover effects.
For advanced grids (pagination, sorting, editable rows), use `DataTable` instead.

## 📦 Import
```tsx
import { Table, TableHeader, TableBody, TableRow, TableHead, TableCell } from '../../components/ui/Table';
```

## 🚀 Basic Example

```tsx
<Table>
  <TableHeader>
    <TableRow>
      <TableHead>Invoice #</TableHead>
      <TableHead>Amount</TableHead>
    </TableRow>
  </TableHeader>
  <TableBody>
    <TableRow>
      <TableCell>INV-001</TableCell>
      <TableCell>$250.00</TableCell>
    </TableRow>
  </TableBody>
</Table>
```
