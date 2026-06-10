# EntrySection

The `EntrySection` is a higher-order wrapper built on top of the `Card` component. It is designed to cleanly compartmentalize distinct segments of an Entry form while automatically managing the Read/Edit/Save state machine for that block.

## 📦 Import
```tsx
import { EntrySection } from '../../components/entry/EntrySection';
```

## 🛠️ Props Reference

| Prop Name | Type | Description |
| :--- | :--- | :--- |
| `id` | `string` | Unique identifier for this section. |
| `title` | `string` | The header text displayed for the section. |
| `isEditing` | `boolean` | If true, renders the `Save` and `Cancel` buttons. If false, renders `Edit`. |
| `canEdit` | `boolean` | If true, enables the `Edit` button when in read-only mode. |
| `onEdit` | `() => void` | Hook fired when the user clicks Edit. |
| `onSave` | `() => void` | Hook fired when the user clicks Save. |
| `onCancel` | `() => void` | Hook fired when the user clicks Cancel. |
| `children` | `React.ReactNode` | The internal contents of the section (Forms, DataTables, Text). |

## 🚀 Basic Example

```tsx
import React, { useState } from 'react';
import { EntrySection } from '../../components/entry/EntrySection';

export default function ContactDetails() {
  const [editingId, setEditingId] = useState<string | null>(null);

  const handleSave = () => {
    // Perform API Request here...
    setEditingId(null);
  };

  return (
    <EntrySection
      id="contact_info"
      title="Contact Information"
      isEditing={editingId === 'contact_info'}
      canEdit={true}
      onEdit={() => setEditingId('contact_info')}
      onCancel={() => setEditingId(null)}
      onSave={handleSave}
    >
      {editingId === 'contact_info' ? (
        <form>
          {/* Render editable Input fields */}
        </form>
      ) : (
        <div>
          {/* Render read-only text spans */}
        </div>
      )}
    </EntrySection>
  );
}
```
