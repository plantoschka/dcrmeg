## JavaScript Callbacks

**ValidateDCrmEgGrid**
Fired before the value of a field is changed. Cancellable.

**DCrmEgGridSaving**
Fired before saving changed values to records. Cancellable.

**DCrmEgGridDeleting**
Fired before the selected record(s) are deleted. Cancellable.

**DCrmEgGridBeforeCreateNewRecord**
Fired before a new record is created. Cancellable.

**DCrmEgGridCreateNewRecord**
Fired after a new record is created using (+) create new button. Returns Guid of the new record.

**DCrmEgGridOnload**
Fired during population of Option-Sets. You will have the opportunity to set individual options to disabled.

**DCrmEgGridRowOnload**
Fires for every row to be rendered. Allows setting a field (cell) to readonly and change the background and forground colors.

**DCrmEgGridOnBeforeFetchRecords**
Allow additional conditions to be added to the fetch XML prior to fetching data for the grid.

**DCrmEgGridOnBeforeLookupFetchRecords**
Allow additional conditions to be added to the fetch XML prior to fetching data for the Lookup editors. The rowData parameter, a JSON object containing the row (cells) data in both formatted and actual value.

**Set up**

1. Create a new JavaScript resource and add the following code to the it
1. Replace the body of the functions with your code
1. Add the resource to the entity where the grid is being displayed (form editor)
1. For CRM 2015 and 2016, add the FormOnloadHandler function to form onload event (form editor)

Code Blocks

	function Log(s) {
		if (typeof console != "undefined" && typeof console.debug != "undefined") {
			console.log(s);
		}
	}
	// Implement the following only for CRM 2015 and 2016
	// Add the FormOnloadHandler function to form (where grid is to be displayed) onload using form editor
	function FormOnloadHandler(context) {
		window.parent.ValidateDCrmEgGrid = ValidateDCrmEgGrid;
		window.parent.DCrmEgGridSaving = DCrmEgGridSaving;
		window.parent.DCrmEgGridDeleting = DCrmEgGridDeleting;
		window.parent.DCrmEgGridBeforeCreateNewRecord = DCrmEgGridBeforeCreateNewRecord;
		window.parent.DCrmEgGridCreateNewRecord = DCrmEgGridCreateNewRecord;
		window.parent.DCrmEgGridOnload = DCrmEgGridOnload;
		window.parent.DCrmEgGridRowOnload = DCrmEgGridRowOnload;
		window.parent.DCrmEgGridOnBeforeFetchRecords = DCrmEgGridOnBeforeFetchRecords;
		window.parent.DCrmEgGridOnBeforeLookupFetchRecords = DCrmEgGridOnBeforeLookupFetchRecords;
	}
	function ValidateDCrmEgGrid(param, field) {
		var allow = true;
		Log('GridCustomIdentifier [' + field.GridCustomIdentifier + ']');
		Log("FieldSchemaName [" + field.FieldSchemaName 
		+ "] FieldLabel [" + field.FieldLabel 
		+ "] ParentEntitySchemaName [" + field.ParentEntitySchemaName 
		+ "] ParentEntityLabel [" + field.ParentEntityLabel 
		+ "] record guid [" + param.RecordGuid + "]" );
		switch (param.EditorType) {
			// Text
			case 0:
				Log("Text - NewValue [" + param.NewValue 
				+ "] OriginalValue [" + param.OriginalValue + "]");
				break;
			// Whole Number
			case 1:
				Log("Text - NewValue [" + param.NewValue 
				+ "] OriginalValue [" + param.OriginalValue + "]");
				break;
			// Date Picker
			case 2:
				Log("Date - NewDate [" + param.NewValue 
				+ "] OriginalDate [" + param.OriginalValue + "]");
				break;
			// Checkbox (two option)
			case 3:
				Log("Checkbox (TwoOption) - NewText [" + param.NewValue 
				+ "] OriginalText [" + param.OriginalValue 
				+ "] isChecked [" + param.IsChecked + "]");
				break;
			// OptionSet
			case 4:
				Log("OptionSet - NewLabel [" + param.NewLabel 
				+ "] NewValue [" + param.NewValue 
				+ "] OriginalLabel [" + param.OriginalLabel 
				+ "] OriginalValue [" + param.OriginalValue + "]");
				break;
			// Memo (Description)
			case 5:
				Log("Memo (description) - NewValue [" + param.NewValue 
				+ "] OriginalValue [" + param.OriginalValue + "]");
				break;
			// Lookup (single)
			case 6:
				Log("Lookup - NewLabel [" + param.NewLabel 
				+ "] NewGuid [" + param.NewGuid 
				+ "] NewLogicalName [" + param.NewLogicalName 
				+ "] OriginalLabel [" + param.OriginalLabel 
				+ "] OriginalGuid [" + param.OriginalGuid 
				+ "] OriginalLogicalName [" + param.OriginalLogicalName + "]");
				break;
			// Decimal / Float
			case 7:
				Log("Text - NewValue [" + param.NewValue 
				+ "] OriginalValue [" + param.OriginalValue + "]");
				break;
			// Currency
			case 8:
				Log("Text - NewValue [" + param.NewValue 
				+ "] OriginalValue [" + param.OriginalValue + "]");
				break;
			// Date Time Picker
			case 9:
				Log("DateTime - NewDateTime [" + param.NewValue 
				+ "] OriginalDateTime [" + param.OriginalValue + "]");
				break;
			default:
				break;
		}
		return allow;
	}
	function DCrmEgGridSaving(data, entityinfo) {
		var allow = true;
		Log('GridCustomIdentifier [' + entityinfo.GridCustomIdentifier + ']');
		Log("ParentEntityName [" + entityinfo.ParentEntityName 
		+ "] ParentEntitySchemaname [" + entityinfo.ParentEntitySchemaname + "]");
		var item;
		for (var i = 0; i < data.length; i++) {
			item = data[i](i);
			switch (item.InternalEditorType) {
				// Text
				case 0:
				// Whole Number
				case 1:
				// Date Picker
				case 2:
				// Memo (Description)
				case 5:
				// Decimal / Float
				case 7:
				// Currency
				case 8:
				// Date Time Picker
				case 9:
					Log("Record Guid [" + item.RecGuid +
						"] ValueToSave [" + item.ValueToSave +
						"] FieldSchemaName [" + item.FieldSchemaName +
						"] Format [" + item.InternalEditorFormat + "])");
					// Format: url, email, phone, ...
					break;
					// Checkbox (two option)
				case 3:
					Log("Record Guid [" + item.RecGuid +
						"] ValueToSave [" + item.ValueToSave +
						"] FieldSchemaName [" + item.FieldSchemaName +
						"]  [" + item.CheckAttribute + "]");
					break;
				// OptionSet
				case 4:
					Log("Record Guid [" + item.RecGuid +
						"] OptionSetLabel [" + item.ValueToSave +
						"] FieldSchemaName [" + item.FieldSchemaName +
						"] OptionSetValue [" + item.OptionSetValue + "]");
					break;
				// Lookup (single)
				case 6:
					Log("Record Guid [" + item.RecGuid +
						"] LookupText [" + item.ValueToSave +
						"] FieldSchemaName [" + item.FieldSchemaName +
						"] LookupLogicalName [" + item.LookupLogicalName +
						"] LookupGuid [" + item.LookupId + "]");
					break;
				default:
					break;
			}
		}
		return allow;
	}
	function DCrmEgGridDeleting(data, entityinfo) {
		var allow = true;
		Log('GridCustomIdentifier [' + entityinfo.GridCustomIdentifier + ']');
		Log("ParentEntityName [" + entityinfo.ParentEntityName 
		+ "] ParentEntitySchemaname [" + entityinfo.ParentEntitySchemaname + "]");
		for (var i = 0; i < data.length; i++) {
			Log("Record Guid [" + data[i] + "]");
		}
		return allow;
	}
	function DCrmEgGridBeforeCreateNewRecord(newRecStruct, entityinfo) {
		var allow = true;
		Log('GridCustomIdentifier [' + entityinfo.GridCustomIdentifier + ']');
		Log("ParentEntityName [" + entityinfo.ParentEntityName 
		+ "] ParentEntitySchemaname [" + entityinfo.ParentEntitySchemaname + "]");
		Log("New Record Struct", newRecStruct);
		return allow;
	}
	function DCrmEgGridCreateNewRecord(data, entityinfo) {
		Log('GridCustomIdentifier [' + entityinfo.GridCustomIdentifier + ']');
		Log("ParentEntityName [" + entityinfo.ParentEntityName 
		+ "] ParentEntitySchemaname [" + entityinfo.ParentEntitySchemaname + "]");
		Log("Record Guid [" + data.NewRecordGuid + "]");
	}
	function DCrmEgGridOnload(data, entityinfo) {
		Log('GridCustomIdentifier [' + entityinfo.GridCustomIdentifier + ']');
		Log("ParentEntityName [" + entityinfo.ParentEntityName 
		+ "] ParentEntitySchemaname [" + entityinfo.ParentEntitySchemaname + "]");
		//data.Option.readonly = (data.Option.text == "Web");
		Log("Option set - text [" + data.Option.text + "] value [" 
		+ data.Option.value + "] ReadOnly [" + data.Option.readonly + "]");
	}
	var _MyCounter = 1;
	function DCrmEgGridRowOnload(rowData, entityinfo) {
		Log('GridCustomIdentifier [' + entityinfo.GridCustomIdentifier + ']');
		Log("ParentEntityName [" + entityinfo.ParentEntityName 
		+ "] ParentEntitySchemaname [" + entityinfo.ParentEntitySchemaname + "]");
		Log("Record Guid [" + rowData.RecordGuid + "] Row Index [" + rowData.RowIndex + "]");
		if (rowData.InlineCreate) {
			Log("Create inline record is used. One row is being added.");
		}
		if (_MyCounter > 5) {
			_MyCounter = 0;
		}
		if (_MyCounter == 1) {
			rowData.RowBackgroundColor = '#CCCCCC';
		}
		_MyCounter++;
		var CrmFieldTypes = {
			LookupType: "lookup",
			CustomerType: 'customer',
			OwnerType: 'owner',
			BooleanType: "boolean",
			OptionSetType: "picklist",
			DateTimeType: "datetime",
			TextType: "string",
			MemoType: "memo",
			IntegerType: "integer",
			DoubleType: "double",
			DecimalType: "decimal",
			MoneyType: "money",
			State: 'state', // Status statecode
			Status: 'status' // Status Reason statuscode
		};
		for (var i = 0; i < rowData.Fields.length; i++) {
			var field = rowData.Fields[i];
			Log("Field schema name [" + field.SchemaName 
			+ "] field.FieldType [" + field.FieldType + "]");
			switch (field.FieldType) {
				case CrmFieldTypes.TextType:
					Log("Field [" + field.Value + "] Format [" 
					+ field.Format + "]");
					//// To set a different display value
					//field.NewFormattedValue = 'Custom value';
					break;
				case CrmFieldTypes.MemoType:
					Log("Field [" + field.Value + "] Format [" 
					+ field.Format + "]");
					//// To set a different display value
					//field.NewFormattedValue = 'Custom value';
					break;
				case CrmFieldTypes.LookupType:
				case CrmFieldTypes.CustomerType:
				case CrmFieldTypes.OwnerType:
					Log("Field Lookup Guid ["
						+ field.LookupGuid + "] Lookup LogicalName ["
						+ field.LookupLogicalName + "] Lookup Name ["
						+ field.LookupName + "] Value ["
						+ field.Value + "]");
					break;
				case CrmFieldTypes.IntegerType:
					Log("Field FormattedValue [" + field.FormattedValue 
					+ "] Value [" + field.Value + "]");
					// To set a different display value
					//field.NewFormattedValue = 55;
					break;
				case CrmFieldTypes.DoubleType:
				case CrmFieldTypes.DecimalType:
					Log("Field FormattedValue [" + field.FormattedValue 
					+ "] Value [" + field.Value + "]");
					//// To set a different display value
					//field.NewFormattedValue = 65.90;
					break;
				case CrmFieldTypes.MoneyType:
					Log("Field FormattedValue [" + field.FormattedValue 
					+ "] Value [" + field.Value + "]");
					//// To set a different display value
					//field.NewFormattedValue = $65.900;
					//field.NewValue = 65.90;
					break;
				case CrmFieldTypes.DateTimeType:
					Log("Field FormattedValue [" + field.FormattedValue + "]");
					//field.ReadOnly = true;
					//// Optional, set field (cell) background and forground colors
					//field.BackgroundColor = 'lightyellow';
					//field.ForgroundColor = 'black';
					//// To set a different display value
					//field.NewFormattedValue = '03/25/2017';
					break;
				case CrmFieldTypes.OptionSetType:
					Log("Field FormattedValue [" + field.FormattedValue 
					+ "] Value [" + field.Value + "]");
					//// To set a different display value
					//field.NewFormattedValue = 'option three';
					//field.NewValue = 4;
					break;
				case CrmFieldTypes.BooleanType:
					Log("Field FormattedValue [" + field.FormattedValue 
					+ "] Value [" + field.Value + "]");
					//field.ReadOnly = true;
					//field.BackgroundColor = '#CCCCCC';
					//field.ForgroundColor = 'blue';
					//// To set a different display value
					//field.NewFormattedValue = 'Yes';
					//field.NewValue = true;
					//// OR
					//field.NewFormattedValue = 'InActive';
					//field.NewValue = false;
					break;
				case CrmFieldTypes.State:
				case CrmFieldTypes.Status:
					Log("Field FormattedValue [" + field.FormattedValue 
					+ "] Value [" + field.Value + "]");
				default:
					break;
			}
		}
	}
	function DCrmEgGridOnBeforeFetchRecords(entityinfo) {
		Log('GridCustomIdentifier [' + entityinfo.GridCustomIdentifier + ']');
		var additions = null;
		Log("DCrmEgGridOnBeforeFetchRecords - ParentEntityName [" + entityinfo.ParentEntityLabel 
		+ "] ParentEntitySchemaname [" + entityinfo.ParentEntitySchemaName + "]");
		//// Add additional conditions
		//if (entityinfo.ParentEntitySchemaName == 'account') {
			//additions = {};
			//// Example for a sinle value condition
			//additional.Condition = '<condition attribute="primarycontactid" operator="eq" value="{76E339A4-1528-E611-80DD-08002738AA19}" />';
			//// Example for multi value condition
			//additions.Condition = '<condition attribute="primarycontactid" operator="in">' +
			//    '<value>{64E339A4-1528-E611-80DD-08002738AA19}</value>' +
			//    '<value>{76E339A4-1528-E611-80DD-08002738AA19}</value>' +
			//'</condition>';
			//// Example for a link entity
			//additions.LinkEntity = '<link-entity name="incident" from="customerid" to="accountid" alias="aa">' +
			//  '<filter type="and">' +
			//    '<condition attribute="primarycontactidname" operator="like" value="fjghg%" />' +
			//  '</filter>' +
			//'</link-entity>';
		//}
		return additions;
	}
	function DCrmEgGridOnBeforeLookupFetchRecords(entityinfo, rowData) {
		var additions = null;
		//Log('GridCustomIdentifier [' + entityinfo.GridCustomIdentifier + ']');
		//Log("DCrmEgGridOnBeforeLookupFetchRecords - Field Schemname [" + entityinfo.FieldSchemaName + "] ParentEntityName [" + entityinfo.ParentEntityLabel + "] ParentEntitySchemaname [" + entityinfo.ParentEntitySchemaName + "]\r\n\r\n");
		additions = {};
		// Only targetting this lookup with the following logical name
		if (entityinfo.FieldSchemaName != 'new_relatedleveltwoadminid') {
			return additions;
		}
		var attr, guid, dtype = null;
		for (var i = 0; i < rowData.Rows[0].Cells.length; i++) {
			var cell = rowData.Rows[0].Cells[i];
			/*
			rowData.GridEditorTypes
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
			 */
			if ((rowData.Headers[i].EditorType == rowData.GridEditorTypes.Lookup) &&
				(rowData.Headers[i].FieldLogicalName == 'new_relatedleveloneadminid') &&
				(cell.Value) &&
				(cell.Value.EntityLogicalName)) {
					attr = rowData.Headers[i].FieldLogicalName;
					dtype = cell.Value.EntityLogicalName;
					guid = cell.Value.Guid;
					break;
			} else if ((rowData.Headers[i].EditorType == rowData.GridEditorTypes.Text) ||
				(rowData.Headers[i].EditorType == rowData.GridEditorTypes.Currency) ||
				(rowData.Headers[i].EditorType == rowData.GridEditorTypes.OptionSet) ||
				(rowData.Headers[i].EditorType == rowData.GridEditorTypes.Checkbox)) {
					console.log('FormattedValue [' + cell.FormattedValue + '] Value [' + cell.Value + ']');
			}
		}
		if (attr && guid && dtype) {
			additions.Condition = '<condition attribute="' + attr + '" uitype="' + dtype + '" operator="eq" value="{' + guid + '}" />';
		}
		Log('additional.Condition', additions.Condition);
		//// Example for multi value condition
		//additions.Condition = '<condition attribute="primarycontactid" operator="in">' +
		//    '<value>{64E339A4-1528-E611-80DD-08002738AA19}</value>' +
		//    '<value>{76E339A4-1528-E611-80DD-08002738AA19}</value>' +
		//'</condition>';
		//// Example for a link entity
		//additions.LinkEntity = '<link-entity name="incident" from="customerid" to="accountid" alias="aa">' +
		//  '<filter type="and">' +
		//    '<condition attribute="primarycontactidname" operator="like" value="fjghg%" />' +
		//  '</filter>' +
		//'</link-entity>';
		return additions
	}
