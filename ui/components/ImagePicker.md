# ImagePicker

The `ImagePicker` is a specialized layout-bound dropzone optimized exclusively for single-image assets (like Avatars, Profile Pictures, or Cover Photos). It features automatic image previewing, circular or rectangular clipping bounds, and a fluid hover-to-replace UX.

## 📦 Import
```tsx
import { ImagePicker } from '../../components/ui/ImagePicker';
```

## 🛠️ Props Reference

| Prop Name | Type | Default | Description |
| :--- | :--- | :--- | :--- |
| `value` | `string \| File \| null` | `null` | The currently selected image. Can be an existing URL from your database, or a newly dropped native `File` object. |
| `onChange` | `(file: File \| null) => void` | **Required** | Callback fired when the user drops a new image or clicks the "Remove" trash icon. |
| `shape` | `'rect' \| 'circle'` | `'rect'` | Determines the clipping bounds. `circle` will mathematically enforce a 50% border radius. |
| `height` | `number \| string` | `200` | The CSS height of the dropzone. |
| `width` | `number \| string` | `'100%'` | The CSS width of the dropzone. *Note: If `shape="circle"`, width is automatically forced to match height to maintain a perfect aspect ratio.* |
| `label` | `string` | `undefined` | Optional top-level label for the input. |
| `helperText` | `string` | `undefined` | Descriptive text placed underneath the bounding box. |
| `error` | `string` | `undefined` | If provided, forces the bounding box into a Red validation state and displays the error message. |
| `required` | `boolean` | `false` | Appends a mandatory asterisk `*` to the label. |

## 🚀 Basic Example (Square Cover Photo)

```tsx
import React, { useState } from 'react';
import { ImagePicker } from '../../components/ui/ImagePicker';

export default function CoverPhotoEditor() {
  const [coverPhoto, setCoverPhoto] = useState<File | string | null>(null);

  return (
    <ImagePicker
      label="Company Cover Photo"
      shape="rect"
      height={250}
      value={coverPhoto}
      onChange={(file) => setCoverPhoto(file)}
      helperText="Recommended size: 1200x250. Max 5MB."
    />
  );
}
```

## ⭕ Circular Avatar Example

When `shape="circle"` is applied, the component automatically forces a 1:1 aspect ratio based on the `height` prop, ensuring the dropzone is perfectly round.

```tsx
import React, { useState } from 'react';
import { ImagePicker } from '../../components/ui/ImagePicker';

export default function ProfileEditor() {
  const [avatar, setAvatar] = useState<File | string | null>('https://i.pravatar.cc/150');

  return (
    <ImagePicker
      label="Profile Picture"
      shape="circle"
      height={150} // Width is ignored and automatically set to 150
      value={avatar}
      onChange={(file) => setAvatar(file)}
    />
  );
}
```

## 🧠 Technical Details
- Under the hood, this component heavily relies on `react-dropzone` for handling the drag-and-drop HTML5 API.
- If a native `File` object is provided in the `value` prop, the component automatically manages a temporary `URL.createObjectURL` map to display the preview before the file is uploaded to your backend servers.
- The "Remove" button uses complex trigonometry CSS overrides to ensure the icon is never clipped outside of the `border-radius: 50%` wrapper when in `circle` mode.
