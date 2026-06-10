# Tabs

The `Tabs` component allows users to navigate between different views or data panes without leaving the current layout context. It uses React Context to manage active states effortlessly.

## 📦 Import
```tsx
import { Tabs, TabsList, TabsTrigger, TabsContent } from '../../components/ui/Tabs';
```

## 🛠️ Components & Props

### `Tabs`
The root provider that manages the active tab state.
| Prop Name | Type | Description |
| :--- | :--- | :--- |
| `defaultValue` | `string` | **Required.** The `value` of the tab that should be open by default. |

### `TabsList`
The flexbox container that wraps the clickable tab buttons.

### `TabsTrigger`
The clickable tab button.
| Prop Name | Type | Description |
| :--- | :--- | :--- |
| `value` | `string` | **Required.** The unique identifier for this tab. |

### `TabsContent`
The wrapper around the content that displays when its corresponding trigger is active.
| Prop Name | Type | Description |
| :--- | :--- | :--- |
| `value` | `string` | **Required.** The unique identifier matching the trigger. |

## 🚀 Basic Example

```tsx
import React from 'react';
import { Tabs, TabsList, TabsTrigger, TabsContent } from '../../components/ui/Tabs';

export default function ProfileView() {
  return (
    <Tabs defaultValue="overview">
      <TabsList>
        <TabsTrigger value="overview">Overview</TabsTrigger>
        <TabsTrigger value="settings">Settings</TabsTrigger>
        <TabsTrigger value="billing">Billing History</TabsTrigger>
      </TabsList>
      
      <TabsContent value="overview">
        <p>This is the overview dashboard.</p>
      </TabsContent>
      
      <TabsContent value="settings">
        <form>
          {/* Settings form */}
        </form>
      </TabsContent>
      
      <TabsContent value="billing">
        <p>No past invoices found.</p>
      </TabsContent>
    </Tabs>
  );
}
```
