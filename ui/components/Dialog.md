# Dialog

The `Dialog` component (often called a Modal) intercepts user workflows for focused interactions, confirmation prompts, or data-heavy popup forms. It is fully accessible and built using Radix UI primitives.

## 📦 Import
```tsx
import { 
  Dialog, 
  DialogTrigger, 
  DialogContent, 
  DialogHeader, 
  DialogTitle, 
  DialogDescription,
  DialogFooter,
  DialogClose
} from '../../components/ui/Dialog';
```

## 🛠️ Components

- **`Dialog`**: The root state-manager. Accepts `open` and `onOpenChange` props for controlled states.
- **`DialogTrigger`**: Wraps the button/element that opens the modal. (Not needed if controlling state manually).
- **`DialogContent`**: The actual white modal box. Automatically handles portals, overlays, and focus-traps.
- **`DialogHeader`**: Container for the Title and Description.
- **`DialogTitle`**: Accessible `h2` heading for screen readers.
- **`DialogDescription`**: Accessible muted text below the title.
- **`DialogFooter`**: Bottom-aligned container for action buttons (Cancel, Confirm).
- **`DialogClose`**: Wraps a button to automatically close the modal.

## 🚀 Basic Example (Controlled State)

```tsx
import React, { useState } from 'react';
import { Button } from '../../components/ui/Button';
import { 
  Dialog, 
  DialogContent, 
  DialogHeader, 
  DialogTitle, 
  DialogDescription,
  DialogFooter
} from '../../components/ui/Dialog';

export default function DeleteConfirmation() {
  const [isOpen, setIsOpen] = useState(false);

  return (
    <>
      <Button variant="outline" onClick={() => setIsOpen(true)}>
        Delete Record
      </Button>

      <Dialog open={isOpen} onOpenChange={setIsOpen}>
        <DialogContent>
          <DialogHeader>
            <DialogTitle>Are you absolutely sure?</DialogTitle>
            <DialogDescription>
              This action cannot be undone. This will permanently delete the party record from the database.
            </DialogDescription>
          </DialogHeader>
          
          <DialogFooter>
            <Button variant="ghost" onClick={() => setIsOpen(false)}>Cancel</Button>
            <Button variant="primary" style={{background: 'var(--error-600)'}}>Delete</Button>
          </DialogFooter>
        </DialogContent>
      </Dialog>
    </>
  );
}
```
