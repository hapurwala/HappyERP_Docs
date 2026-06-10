# DatePicker

A standardized date selection input for handling calendar data smoothly.

## 📦 Import
```tsx
import { DatePicker } from '../../components/ui/DatePicker';
```

## 🛠️ Props Reference

| Prop Name | Type | Description |
| :--- | :--- | :--- |
| `value` | `string \| Date` | The selected date. |
| `onChange` | `(date: Date \| null) => void` | Hook fired when a new date is selected. |
| `minDate` | `Date` | Minimum selectable date. |
| `maxDate` | `Date` | Maximum selectable date. |
| `disabled` | `boolean` | Grays out the date picker. |

## 🚀 Basic Example

```tsx
<DatePicker 
  value={issueDate}
  onChange={(date) => setIssueDate(date)}
  minDate={new Date()}
/>
```
