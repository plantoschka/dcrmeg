v1.0.6.7
=========

Bug Fixes:

1- Error when cloning records with readonly Lookup fields

2- If a top-level record is expanded (+ button) to show related records then the New toolbar button on the top-level record is clicked, 
an error dialog appears. Both records are configured for "Inline create, allow blank required lookup fields" with no default values provided. 
When the top-level record is collapsed, a new record can be created without issue.

3- If you click the New toolbar button and hit the SPACE key immediately afterwards, 
it creates two records and the first column of the grid is duplicated and pushed horizontally. The obvious workaround is to not hit the spacebar. 

HTML5 button default behavior. When button has focus, pressing enter or spacebar simulates a click. Changed behavior to ignore spacebar when button has focus.

4- Issue retrieving GlobalOptionSet using api v8.0 (CRM 2016)

Additions:

1- Text field with URL format: CTRL+Click to open the link like in Outlook

2- Grid API - DCrmEgGrid.GridRowData to retrieve a row data using record guid

3- Grid API - DCrmEgGrid.TotalRecordCount to retrieve total record count. This is not the total records displayed.

4- Replacement for the following native product related editable grids. The grid's (+) create new menu will display "Write-in Product" and "Existing Products" items.

Opportunity-opportunityproduct

Quote-quotedetail

Order-salesorderdetail

Invoice-invoicedetail

5- Set custom title for headers (configuration).

6- Use default view for lookup dropdown menu. Only the condition parts not the attributes.

v1.0.6.6
========

Bug Fixes:
1. When using the wrap text option for the column header. If this results in the header running over more than two lines then this results in a 
scroll bar being added to the grid cutting off part of the header or footer and making it appear that the sticky header/footer isn't working correctly.

Additions:
1. Grid - Added Quick Create to '+' menu (2015 SP1 and up). If the entity IsQuickCreateEnabled metadata is true.
2. Grid - Added DisableGrid API
3. Configuration - Added the ability to sort using fields that are not part of the grid
4. Documentation - Added a new documentation page for the Grid API. [Grid API](https://github.com/mehrgithub/dcrmeg/blob/master/docs/GridApi.md)
5. Switch to use window.parent["APPLICATION_FULL_VERSION"] to determine if WebApi is available and the version in use. A call to RetrieveVersion WebApi is used as a fallback mechanism.

v1.0.6.5
========

Bug fixes:
1. Refresh after save was not firing for single lookup update.
2. After a failed create inline, a row was being added.

Addition:
1. To allow required lookup field to be blank during inline create, an option was added to configuration to by pass the validation logic. "Inline create, allow blank required lookup fields".
2. GetEntitySetName WebApi library, switched to use EntityDefinitions to retreive LogicalCollectionName for an entity.
3. Added rowData parameter to DCrmEgGridOnBeforeLookupFetchRecords JavaScript call back. The rowData parameter contains a structure of row data.
4. New interface to access the grid 'DCrmEgGrid'. The interface has two methods, GridData (returns a JSON object containing grid data), RefreshGrid (refreshs the grid)

Please see [JS call backs documentation](https://github.com/mehrgithub/dcrmeg/blob/master/docs/JavaScript%20Callbacks.md) "SomeFieldChangeHandler" and "DCrmEgGridOnBeforeLookupFetchRecords" functions for details and example of usage.
	
v1.0.6.4
========

Bug Fixes:
1. Unable to get property 'optionsData' of undefined or null reference. CRM 2016 (8.1.1.1005 on premise)
2. Cell formatting wasn't being applied if no conditions were set. Modified the logic to apply cell formatting whether a condition is set or not.

v1.0.6.3
========

Bug fixes:
1. Unable to load grid data - 400 Error. Dynamics CRM 2016 (8.1.1.1005). Retrieving-by-name capability wasn't added until 8.2.
2. Subgrid, two option keyboard issues in IE.
3. Various inline editors keyboard teawks in the grid and the subgrid. IE related.
4. Highlighting of options when moving the mouse over options in an optionset editor.
5. Setting/removing the inline filters wasn't bypassing auto refresh delay.

Addition:
1. Configuration option to allow setting of a subgrid tbody height. Currently, the height of the grid tbody is set to 80 pixels. Example, display accounts and related contacts on an entity. In this scenerio, the contact is the subgrid and in the configuration, tbody height can be set.

v1.0.6.2
========

Bug Fixes:
1. When using nested records, for every record that is expanded the grid ads a lot of extra white space. (with no aggregates present)
2. After a refresh, the grid doesn't navigate to the same page and the selected field. Doesn't keep focus.
3. Scenario: read only column with picklist field (column filter enabled). Issue: when click on column filter, there is js error because self.GridEditors property has null elements
4. Dynamics 365, Create inline record. An undeclared property new_primaryincident which only has property annotations in the payload but no property value was found in the payload. In OData, only declared navigation properties and declared named streams can be represented as properties without values.
5. While editing in grid, Shift+Tab keys used usually on standard forms/grids to focus on previous field not working. Instead it acts as a simple Tab.
6. Keyboard friendly selection from option set. It would be nice if we could select one of the presented options by using arrows keys (up, down), but without triggering the filter as we still need to see available options.
7. Pressing Tab Key is changing the boolean values. (tabbing + with one click checkbox option enabled.)
8. After an auto save and auto refresh, the data wasn't reflecting the changes that was to be made in a workflow triggered by saving a field. Please see the Additions for Auto refresh delay option.
9. Saving to Excel and CSV wasn't adding values from the textual fields.

Additions:
1. "DCrmEgGridOnBeforeLookupFetchRecords" JavaScript callback. Allow additional conditions to be added to the fetch XML prior to fetching data for the Lookup editors.
2. "DCrmEgGridRowOnload" JavaScript callback. Added the means to set the display of the cells via two new attributes. Please see the [JS callbacks documentation](https://github.com/mehrgithub/dcrmeg/blob/master/docs/JavaScript%20Callbacks.md). I am looking for feedback as whether to save the new value or use other means. Please provide feedback.
3. Grid configuration, Auto refresh delay mechanism for Refresh after create and save options. Auto refresh delay (milli seconds) is for scenerios where a workflow is triggered after the grid saves changes to a field. Since the grid immediately fetches data, in most likelihood, the workflow hasn't finished/started. The return data doesn't reflect the changes from workflow. This option allows you to delay the auto refresh till the workflow has finished. Depending on the load, this could be 1000 milli seconds (1 second) or more.
4. If a cell has focus, pressing F5 refreshs the grid.


v1.0.6.1
========

Bug Fixes:
1. Configuration Record does not display list of entities. Dynamics 365 Online version 1612 (8.2.1.207)
2. Synchronuoos Associating records Web API call was missing a parameter

Additions:
1. Added an option to the configuration to set the minimum width of the columns. Default 15 px. The grid will attempt to display all columns within the viewable area. If a column width is specified in the configuration, it will be used as long as it is not less than the minimum column width specified in the configuration. Better browser horizontal resize handling.
2. Added an option to the configuration to allow using a custom identifier for each grid. The custom identifier is passed to all the JS call backs. This is for cases where multiple grids exist on the same form and a need to identify originating grid for the JS call backs.
3. Added borders to grid cells and teawked CSS based on the browser.

v1.0.6.0
========

Bug Fix:

1. DCrmEgGridOnBeforeFetchRecords JS callback was throwing "Unable to get property 'Label' of undefined or null reference".
2. JS error "cas" is undefined or null

v1.0.5.9
========

Bug Fixes:
1. Unable to add more than 27 fields in the configuration.
2. Opening more than one sub grid and refreshing one of them within the main grid was destroying the other sub grids. (V 1.0.5.8 Grid layout is being destroyed on refresh)
3. Issue with window.parent.DCrmEgGridOnload. Unable to set the last option set to read-only.
4. Was unable to remove a selected field from the grid configuration if the field was deleted from the selected entity.

Additions:
1. Web API support for CRM 2016 and Dynamics 365. For CRM versions prior to 2016, the grid uses soap end points (XrmServiceToolkit). For Web API, I have added WEBAPI-SDK.js. Minimum API version supported v8.1. This version number is updated after during the grid initialization to match current Web API version in your CRM environment via a call to versionNumber function.
2. Now you can preset aggregates for numeric fields from the configuration. I have also added the functionality to the grid to auto update aggregate values that were not preset (manually invoked using the aggregate button menu) in the configuration (after an update, delete, and clone operations).
3. Sticky header and footer. The grid header and footer remain static during scrolling.
4. Enforced minimum width for column headers 100px. Horizontal scroll bar is visible if the width of the grid is greater than it's container
