# HappyERP Component Inventory

This document provides a categorized breakdown of all the custom React components we have built and standardized within the `src/components/` directory for the HappyERP project. 

## 📝 Form Controls & Inputs

These components are the building blocks for data entry, handling user input, validation states, and complex data types.

- **`Input`**: The fundamental text entry field supporting all standard HTML types (text, email, number) with built-in labels, error state styling, and helper text integration. [📖 View Docs](./components/Input.md)
- **`Select`**: A highly advanced dropdown menu built on Radix UI primitives. It supports single/multi-selection, live searching, custom option rendering, and keyboard accessibility. [📖 View Docs](./components/Select.md)
- **`Checkbox`**: A standardized boolean toggle for forms and table row selections. [📖 View Docs](./components/Checkbox.md)
- **`Switch`**: A visual sliding toggle control (pill-shaped) specifically designed for binary preferences and settings (e.g., "Use WhatsApp"). [📖 View Docs](./components/Switch.md)
- **`DatePicker`**: A standardized date selection input for handling calendar data. [📖 View Docs](./components/DatePicker.md)
- **`RichTextEditor`**: A WYSIWYG text editor powered by ReactQuill, allowing users to write formatted HTML notes and rich descriptions. [📖 View Docs](./components/RichTextEditor.md)
- **`FileUploader`**: A robust drag-and-drop dropzone engineered for multi-file document uploads, displaying a list of staged files and supporting generic attachments. [📖 View Docs](./components/FileUploader.md)
- **`ImagePicker`**: A specialized, layout-bound dropzone optimized exclusively for single-image assets (like Avatars or Cover Photos). Features automatic image preview, circular/rectangular clipping, and hover-to-replace UX. [📖 View Docs](./components/ImagePicker.md)

## 📊 Data Display & Grids

These components govern how large sets of structured data are presented and interacted with on the screen.

- **`Table`**: The foundational, responsive HTML table structure with standardized padding, typography, and hover effects. [📖 View Docs](./components/Table.md)
- **`DataTable`**: Our flagship data grid component. A powerful wrapper around `Table` that injects pagination, column sorting, row selection checkboxes, grouped rows, dynamic Action Menus, and a fully functional **high-density inline edit mode** with built-in Save/Cancel lifecycle hooks. [📖 View Docs](./components/DataTable.md)
- **`Pagination`**: Modular page traversal controls for navigating chunked datasets seamlessly. [📖 View Docs](./components/Pagination.md)
- **`FilterPanel`**: A dynamic off-canvas drawer system that manages complex query parameters, search inputs, and dataset filtering rules without cluttering the main view. [📖 View Docs](./components/FilterPanel.md)

## 🏗️ Layout & Entry Architecture

These components provide structural consistency across pages, specifically standardizing our Data Entry forms (like the Party Masters module).

- **`Card`**: A flexible surface container (exporting `CardHeader`, `CardTitle`, `CardContent`, and `CardFooter`) used to group related information cleanly on bright/dark backgrounds. [📖 View Docs](./components/Card.md)
- **`EntryHeader`**: The standardized top-bar for module pages. It automatically manages the page title, subtitle, the current document `Stage` badge, and back-navigation routing. [📖 View Docs](./components/EntryHeader.md)
- **`EntrySection`**: A structural wrapper for logical segments on Entry forms. It wraps `Card` primitives and natively provides an Edit/Save/Cancel state machine (displaying the appropriate action buttons depending on whether the user is viewing or modifying the section). [📖 View Docs](./components/EntrySection.md)
- **`Dialog`**: Modal overlays used for focused interactions, confirmation prompts, or popup forms. [📖 View Docs](./components/Dialog.md)
- **`Tabs`**: Structural navigation elements for switching between different views or data panes within the same layout context. [📖 View Docs](./components/Tabs.md)

## 💡 Feedback, Visuals, & Utilities

These components provide user feedback, status communication, and atomic interactive elements.

- **`Button`**: The universal action trigger supporting multiple semantic variants (`primary`, `outline`, `ghost`, etc.), sizes, icons, and loading states. [📖 View Docs](./components/Button.md)
- **`Badge`**: Small, colored status pills used extensively for tagging attributes or displaying Workflow Stages (e.g., Draft, Approved, Rejected). [📖 View Docs](./components/Badge.md)
- **`StageHistoryViewer`**: A chronological timeline visualization component that tracks and displays a document's journey through various workflow stages, including timestamps, approvers, and rejection reasons. [📖 View Docs](./components/StageHistoryViewer.md)
- **`AttachmentViewer`**: A dedicated overlay mechanism for previewing uploaded documents, PDFs, and images natively within the ERP without leaving the application. [📖 View Docs](./components/AttachmentViewer.md)
- **`Tooltip`**: Contextual popups that reveal helpful descriptions or hidden metadata when hovering over dense UI elements. [📖 View Docs](./components/Tooltip.md)
- **`Avatar`**: Circular image wrappers or initials-based placeholders for displaying user portraits and contact faces. [📖 View Docs](./components/Avatar.md)
