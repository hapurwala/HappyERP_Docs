# AttachmentViewer

A dedicated overlay mechanism for previewing uploaded documents, PDFs, and images natively within the ERP without leaving the application.

## 📦 Import

```tsx
import { AttachmentViewer } from '../../components/ui/AttachmentViewer';
```

## 🛠️ Props Reference

| Prop Name     | Type           | Description                               |
|:------------- |:-------------- |:----------------------------------------- |
| `attachments` | `Attachment[]` | Array of file objects or URLs to preview. |
| `isOpen`      | `boolean`      | Controls modal visibility.                |
| `onClose`     | `() => void`   | Callback to close the viewer.             |

## 🚀 Basic Example

```tsx
<AttachmentViewer 
  isOpen={showDocs} 
  onClose={() => setShowDocs(false)} 
  attachments={[
    { id: '1', url: 'https://example.com/invoice.pdf', type: 'application/pdf', name: 'invoice.pdf' }
  ]}
/>
```
