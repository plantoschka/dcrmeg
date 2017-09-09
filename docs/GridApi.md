## Grid API

**GridData(GridCustomIdentifier, EntityLogicalName)**
Returns a JSON object containing grid headers, rows (cells), and an enum to determine the type of the field (text, nummeric, ...)

**RefreshGrid(GridCustomIdentifier, EntityLogicalName)**
Refreshes the grid data.

**DisbaleGrid(GridCustomIdentifier, EntityLogicalName, Disable)**
Disable or enable grid.

**Parameters**

1. GridCustomIdentifier: Grid Custom Identifier set in the grid configuration. Pass null if only one grid exists.
2. EntityLogicalName: The LogicalName of the grid entity where data is retreived. In the following example, I am displaying accounts on the grid.
3. Disable: Boolean type. true to enable and false to disable the grid.

**Grid data returned structure**

	{ Headers: [], Rows: [], GridEditorTypes: DCrmEditableGrid.Editors };

Headers: Array of header objects
	{
		EditorType: Numeric value corresponding to one of GridEditorTypes
		FieldLogicalName: Field logical name
		Label: Field label
	}

Rows: Array of grid row objects
	{
		RecordGuid: Record Guid
		RowIndex: Row index
		Cells: [] Array of cells
	}

Cells: Array of cell objects. null value indicates an empty cell (no data)
	{
		FormattedValue: formatted value. The value that is displayed
		Value: for lookups, the Value is a JSON object. All other types, Value will have a single value. Example:
		//
		// Option set -> Value = numeric
		// Numeric, currency, decimal, double -> Value = numeric
		// Two Option -> Value = true/false
		// DateTimePicker, DatePicker -> Value = Date object
		// Lookup, Customer ->  Value = {
		//                                EntityLogicalName: Lookup Entity logical name
		//                                Guid: Lookup Guid
		//                             }
	}

GridEditorTypes: Helper enum
	{
		"Text": 0,
		"Numeric": 1,
		"DatePicker": 2,
		"Checkbox": 3,
		"OptionSet": 4,
		"Description": 5,
		"Lookup": 6,
		"Decimal": 7,
		"Currency": 8,
		"DateTimePicker": 9,
		"Status": 10,
		"Double": 12,
		"Customer": 13,
		"Owner": 14,
	}

**The following function contains an example of how to access the grid and subsequently the grid API**

	function SomeFieldChangeHandler() {
		// If you are calling this function during the form onload event, you need to use a timer.
		setTimeout(function () {
			// replace WebResource_xxxxxxxxx with the the grid webresource name
			var IFrame = Xrm.Page.getControl("WebResource_xxxxxxxxx").getObject();
			if (IFrame) {
				var frameWindow = IFrame.contentWindow;
				if (frameWindow) {
					if (frameWindow.DCrmEgGrid) {
						var gridData = frameWindow.DCrmEgGrid.GridData(null, 'account');
						for (var i = 0; i < gridData.Headers.length; i++) {
							var header = gridData.Headers[i];
							console.log('EditorType [' + header.EditorType + '] FieldLogicalName [' + header.FieldLogicalName + ']  [' + header.Label + ']');
						}
						for (var i = 0; i < gridData.Rows.length; i++) {
							var row = gridData.Rows[i];
							console.log('RecordGuid [' + row.RecordGuid + '] RowIndex [' + row.RowIndex + ']');
							for (var ii = 0; ii < row.Cells.length; ii++) {
								var cell = row.Cells[ii];
								if ((gridData.Headers[ii].EditorType == gridData.GridEditorTypes.Lookup) ||
									(gridData.Headers[ii].EditorType == gridData.GridEditorTypes.Customer)) {
									console.log('FormattedValue [' + cell.FormattedValue
										+ '] EntityLogicalName [' + ((cell.Value) ? cell.Value.EntityLogicalName : '')
										+ '] Guid [' + ((cell.Value) ? cell.Value.Guid : '') + ']');
								} else {
									console.log('FormattedValue [' + cell.FormattedValue
										+ '] Value [' + cell.Value + ']');
								}
							}
						}
						//
						// Refresh grid:
						// frameWindow.DCrmEgGrid.RefreshGrid(null, 'account');
						//
						// Enable/Disable the grid:
						// frameWindow.DCrmEgGrid.DisableGrid(null, 'account', true);
					} else {
						console.log("No DCrmEgGrid");
					}
				} else {
					console.log("No Frame content window");
				}
			} else {
				console.log("No IFRAME");
			}
		}, 2000);
	}