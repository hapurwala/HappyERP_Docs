# FileUploader

A robust drag-and-drop dropzone engineered for multi-file document uploads, displaying a list of staged files and supporting generic attachments.

## 📦 Import
```tsx
import { FileUploader } from '../../components/ui/FileUploader';
```

## 🛠️ Props Reference

| Prop Name | Type | Description |
| :--- | :--- | :--- |
| `onUpload` | `(files: File[]) => void` | Callback receiving the array of dropped files. |
| `maxFiles` | `number` | Maximum number of files allowed (default: unlimited). |
| `accept` | `Record<string, string[]>` | Supported MIME types. |

## 🚀 Basic Example

```tsx
<FileUploader 
  onUpload={(files) => console.log('Uploaded files:', files)}
  maxFiles={5}
  accept={{ 'application/pdf': ['.pdf'] }}
/>
```
