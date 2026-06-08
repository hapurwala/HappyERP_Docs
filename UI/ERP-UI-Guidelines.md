# UI Design Guidelines

## Salient Features

- The UI of the software is designed using `Next.js` (`React`)
- Modern ERP dashboard UI
- Minimal enterprise design
- Responsive layout
- Multi-tab workflow screen
- Drawer + Modal based actions
- Sticky action header
- Grid/table optimized layout
- Consistent layout throughout software

## Common Guidelines

- Look and feel should be consistent throughout the software.

- **Components**: We have created our own UI components. These components are based on standard `React` components. We will use our own components on all pages. A comprehensive list of components is given in the ...

- **Styles**: All styles are defined in common `CSS` files. Inline styling in individual pages will be avoided as much as possible.

- **Themes**: We will provided several themes in the software. User may choose the theme as per his/her choice. At present we have provided 2 themes: Dark and Light.

## Error Handling

Show:

- Inline errors
- Toast notifications
- Modal confirmations

# Layout Structure

## Responsive Layout

| Medium  | Description                                          |
| ------- | ---------------------------------------------------- |
| Desktop | - Multi-column layout<br/>- Side summary panel       |
| Tablet  | - Collapsible side panels                            |
| Mobile  | - Tab accordion layout<br/>- Floating action buttons |

## Main Layout

* **Top Navigation Bar**: It appears on the top of the page. It has:
  * Left Side
    * Launch Application Button (use logo of the software here)
    * Logo and Name of current user’s company
    * Title of current page
  * Right Side
    * Global Search
    * Help Center
    * Notifications
    * Settings
    * User Menu
* **Left side Menu**
  * This menu appears on the left of the screen
  * User may expand/collapse this menu
  * In expanded mode, icons and text of menu items are displayed 
  * In collapsed mode, only icon of the menu items are displayed 
  * Each application has its own menu 
    * First Menu Item is 'Overview'. On clicking this menu item, application dashboard is displayed in main content area.
    * Grouped sub-menu are given for each module in the application.
    * Each group menu has several menu items grouped in accordion style. 
    * These menu items may be separated by a separator, wherever needed. 
* **Main Content Area** 
  * This area shows main content based on selection of current menu item.
* **Bottom Bar** 
  * Copyright Information
  * Selected financial year
  * Selected organisation and branch
  * Name of the Database

## Common Table Features

* This is the most common component of the software.

* It is used to show list of similar data. There are two common usage:
  
  * Any list like PO List, Product List, Customer List etc
  
  * Report data

* Data can be shown in two ways: List or Cards.

* An action menu is given to perform actions on that card or that row.

* All lists have standard features like column selection, resize, sorting and grouping.

* Provision to download as CSV, PDF etc. is also given.

* All lists have provision of Data pagination.

* The list will have a provision of selection of multiple rows and perform an action. For this purpose the first column will have a checkbox. This checkbox will be shown in a light color (grey) so that it does not attract the user's attention.

* Filter to limit data. There will be 3 filters:
  
  * **Common search at the top of the list/table**: It will search currently fetched rows.
  
  * **Search on individual columns**: It will search currently fetched rows.
  
  * **Filter panel**: It will provide filtration on several criteria. It would affect the fetching of the rows from the database.

* Keyboard navigation

* Lazy Loading

* Debounced search

* Virtual Scrolling?

* Cached lookups?
