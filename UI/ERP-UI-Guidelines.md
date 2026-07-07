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

## Entry Screens/Layouts

Entry screens are used for add/modify data. This layout is normally divided in 2 parts:

- **Left Side**: It is the main part of the entry screen. The main part may have one or more sections. A section contains related fields together. For example address section contains fields like street, city, state, country, pin etc.

- **Right Side**: This is a smaller part of the screen. It normally contains profile picture (if applicable), summary sections and system section etc.  

# Documentation Templates

Separate documents are prepared giving description of pages. These documents are created in folder docs/ui. These documents use tables to give details of fields that will be shown along with the data source, validations etc. The blank table templates are given here.

## Fields Information

Following table is used to give details of fields in a section.

| Name                        | Data Source                | Component                    | Component Specific Information                                                                             | Required                                       | Read Only                         | Validations                                            | On Change                                             | Description                                                                           | Tooltip                                                  |
| --------------------------- | -------------------------- | ---------------------------- | ---------------------------------------------------------------------------------------------------------- | ---------------------------------------------- | --------------------------------- | ------------------------------------------------------ | ----------------------------------------------------- | ------------------------------------------------------------------------------------- | -------------------------------------------------------- |
| *Label given for the field* | *Corresponding data field* | *Component used on the page* | *Configuration of Component.<br/><br/>In case of Select component, option values/query is also given here* | *Is it mandatory for user to fill this field?* | *Is this field only for display?* | *Validations to applied on data entered in this field* | *Actions to be taken on entering data in this field.* | *Description related to this field. This information is useful for development team.* | *Tooltip to be shown to the user on hover on this field* |

## DataTable Information

On many entry pages, data is displayed/added/modified in tabular format. For example  product details in a Sales Order. We have a component named DataTable that we use for this purpose. Configuration data is important for this component. The configuration is to be given in a set of multiple tables. These tables are given below:

### Table 1: Columns

| Header          | Data Source                | Format                                                                                                                                                                      | On Click                                                               | Card Placement                                                                                                        | Tooltip / On Hover                                                                            | Inline Edit Component                                                    |
| --------------- | -------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------ |
| *Column Header* | *Corresponding data field* | *Format in which data will be shown<br/>Image<br/>String<br/>Integer (with Comma Formatting Y/N)<br/>Decimal (with number of decimal places and Comma Formatting)<br/>Date* | *Any action to be taken on click. If yes,it would be shown as a link.* | *In card view, indicate where this field will be displayed in the card.<br/>Image<br/>Title<br/>Subtitle<br/>Details* | *Tooltip to be shown to the user on hover on this data. The tooltip may be static or dynamic* | *In case of inline editing, give the name of the component in edit mode* |

### Table 2: Toolbar Config

DataTable component has a toolbar. This toolbar contains buttons to take some common action on the table. Following table gives configuration parameters for this toolbar.  

| Feature          | Settings | On Click                                                                |
| ---------------- | -------- | ----------------------------------------------------------------------- |
| Search           | *Yes/No* | -                                                                       |
| View Toggle      | *Yes/No* | -                                                                       |
| Column Selection | *Yes/No* | -                                                                       |
| Group By         | *Yes/No* | -                                                                       |
| Filter           | *Yes/No* | -                                                                       |
| Export           | *Yes/No* | -                                                                       |
| Share            | *Yes/No* | -                                                                       |
| Full Screen      | *Yes/No* | -                                                                       |
| Add              | *Yes/No* | *Action to take e.g.<br/>Show Popup<br/>Add Blank Row for inline entry* |

### Table 3: Table Config

| Feature        | Settings                        |
| -------------- | ------------------------------- |
| Row Selection  | *Yes/No*                        |
| Bulk Actions   | *Yes/No*                        |
| Sticky Header  | *Yes/No*                        |
| Column Resize  | *Yes/No*                        |
| Column Pinning | *Yes/No*                        |
| Sorting        | *Yes/No*                        |
| Pagination     | *Yes/No<br/>Default Page Size:* |

### Table 4: RowAction Menu

Action Menu is shown when user clicks on the 3 dots icons in last column of the DataTable. 

| Name                        | Action                                           | Visibility Criteria                                                                         | Icon                          | Tooltip                                                      |
| --------------------------- | ------------------------------------------------ | ------------------------------------------------------------------------------------------- | ----------------------------- | ------------------------------------------------------------ |
| *Text shown as Action Menu* | *Action to be taken on clicking this menu item.* | *Some action menu will be shown on specific conditions. That condition will be given here.* | *Icon to use along with text* | *Tooltip to be shown to the user on hover on this menu item* |

### Table 5: Filer Panel Fields

Filter panel will be shown as a drawer, if set to True. This Panel would consists of many criteria fields. User would select/enter values in these fields. Accordingly data will be fetched from the database. 

| Name                        | Component        | Depends On                                                     | Possible Values                                                                                                                      | Default Values                                                                                 | Output                                                                                               | Required                                       | Tooltip                                                  |
| --------------------------- | ---------------- | -------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------ | ---------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------- | ---------------------------------------------- | -------------------------------------------------------- |
| *Label given for the field* | *Component used* | *If this fields depends on another column, give its name here* | *Values / Validations for the component:<br/><br/>**Select**: LoV or Query<br/>**Number**: Min/Max Value<br/>**Date**: Min/Max Date* | *Initial value to be entered in this field. This vaue will also be set again on Reset Filter.* | *This gives the return value based on user selection. This vakue is used in the query to fetch data* | *Is it mandatory for user to fill this field?* | *Tooltip to be shown to the user on hover on this field* |
