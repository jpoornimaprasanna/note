# note
ui5 snippets
CRUD 

Update :- 

var oModel = this.getOwnerComponent().getModel(); 
oModel.update("/vbakSet('" + vbeln + "')", payLoad, { 
method: "PUT", 
success: function (odata, Response) { 
sap.m.MessageToast.show("Updated Successfully"); 
error: function (cc, vv) {} 
}); 

Create: 
oModel.create("/vbakSet", payLoad, { 
success: function (odata, Response) { 
sap.m.MessageToast.show("Created successfully."); 
}, 
error: function () { 
sap.m.MessageToast.show("Error"); 
} 
});  

Delete: 
oModel.remove("/vbakSet('" + vbelnValue + "')", { 
method: "DELETE", 
success: function (data) { 
sap.m.MessageToast.show("Data got deleted"); 
this.getData(); 
table.rerender(); 
oModel.refresh(); 
}.bind(this), 
error: function (e) { 
sap.m.MessageToast.show("Not able to delete the Data "); 
}.bind(this) 
});  

Get: 
oModel.read("/vbakSet", { 
success: function (oResult) { 
console.log(oResult); 
this.getView().getModel("salesModel").setProperty("/total", oResult.results); 
//this.getView().getModel("salesModel").setProperty("/", oResult); 
}.bind(this) 
}); 

 

Open POPUP 
if (!this.oPopUp) { 
//var oid = this.createId("empFrag");
this.oPopUp = sap.ui.xmlfragment( this.getView().createId(), 
, "CURD.CURDOperations.view.employee", this); 
this.getView().addDependent(this.oPopUp); 
} 
this.oPopUp.open();  

 

m table 
<Table items="{/BATCHSet}" id="idTable" > 
<columns> 
<Column> 
<Label text="ID"></Label> 
</Column> 
<Column> 
<Label text="Action"></Label> 
</Column> 
</columns>
<items> 
<ColumnListItem> 
<cells> 
<ObjectIdentifier title="{Name}" text="{Id}"/> 
<Text text="{Name}"></Text> 
<Text text="{Designation}"></Text>  
</cells> 
</ColumnListItem> 
</items> 
</Table> 


filtering the smart table with the filter 

onBeforeRebindTableExtension 
Let oBindingParams = oEvent.getParameter(“bindingParams”); 
oBindingParams .parameters = oBindingParams.parameters || {}; 
Let oSmartTable = oEvent.getSource(); 
Let oSelectControl = this.getView().byId(“idSelectItem”) 
If(oSelectControl .getSelectedItem()){ 
letsVal  =oSelectControl .getSelectedItem().getText(); 
oBindingParams.filters.push(new sap.ui.model.Filter(‘’, contain, name)) 

filtering multiple fields 
var sQuery = oEvent.getSource().getValue();   
  var oFilter = new Filter({ 
    filters: [ 
      new Filter("value1", FilterOperator.Contains, sQuery), 
      new Filter("value2", FilterOperator.Contains, sQuery) 
    ], 
and: false 
  }); 
  var oBinding = this.byId("list-id").getBinding("items"); 
  oBinding.filter(oFilter); 
or 
var olist = this.getView().byId("list1"), 
arr = [], 
binding, 
filters; 
filters = new Filter({ 
filters: [new Filter("ticketNo", FilterOperator.Contains, oEvent.getSource().getValue()), 
new Filter("Date", FilterOperator.Contains, oEvent.getSource().getValue()) 
], 
and: false 
}); 
binding = olist.getBinding("items"); 
arr.push(filters); 
binding.filter(arr); 
binding.filter(arr); 

single filter  
var sQuery = oEvent.getSource().getValue(); 
  var oFilter = new Filter( 
    "value1",  
    FilterOperator.Contains,  
    sQuery 
  ); 
  var oBinding = this.byId("list-id").getBinding("items"); 
  oBinding.filter([ 
    oFilter  
  ]); 
Service Binding 
function initModel() { 
var sUrl = "/sap/opu/odata/sap/ZUSERDATA_SRV/"; 
var oModel = new sap.ui.model.odata.ODataModel(sUrl, true); 
sap.ui.getCore().setModel(oModel); 
} 


ROUTER 
var oRouter = this.getOwnerComponent().getRouter(); 
oRouter.navTo("Process", { 
obj: }); 
onInit: function () { 
//var oRouter = sap.ui.core.UIComponent.getRouterFor(this); 
var oRouter =  this.getOwnerComponent().getRouter(); 
oRouter.getRoute("View2").attachPatternMatched(this._onObjectMatched, this); 
}, 
_onObjectMatched: function (oEvent) { 
var oArg = oEvent.getParameters("arguments"); 
var oView = this.getView(); 
oView.setModel(this.getOwnerComponent().getModel("details")); 
oView.bindElement("/Employees/" + oArg.arguments.obj); 
} 


onSearch: function (event) { 
                        var olist = this.getView().byId("list1"), 
                        arr = [], 
                        binding, 
                        filters; 
                        filters = new Filter("title", 
     FilterOperator.Contains,event.getSource().getValue()); 
                        binding = olist.getBinding("items"); 
                        arr.push(filters); 
                        binding.filter(arr); 
                }, 
                Edit: function () { 
                        this.getView().getModel("jmodel").setProperty("/editable", true); 
                  }, 
                Save: function(){ 
                        this.getView().getModel("jmodel").setProperty("/editable", false); 
                        var msg = 'Saved'; 
                        MessageToast.show(msg); 
} 
 

 

JSON Model creation 

// get the path to the JSON file 

var sPath = jQuery.sap.getModulePath("your.app.namespace", "/path/to/file.json");  

// initialize the model with the JSON file 

var oModel = new JSONModel(sPath);       

// set the model to the view 

this.getView().setModel(oModel, "jsonFile"); 

      ex:-  var dummyData = [{"fname": "", 

      "lname": "",  

      "desc": "", 

      "removeNew": false, 

      "saveNew": true}]; 

    var oModel = new sap.ui.model.json.JSONModel({data : dummyData});     

    this.getView().setModel(oModel); 

 

Batch call 

var oModel = this.getOwnerComponent().getModel(); 

oModel.setDeferredGroups(["foo"]); 

oModel.setUseBatch(true);  

var aObj = [{EmpId:"125",EmpName:"Swetha",EmpDept:"Assistant Consultant", EmpPhone:"5667867676", EmpAddress:"India",EmpSalary:"90900000"}, 

{EmpId:"126",EmpName:"Swathi",EmpDept:"Assistant Consultant", EmpPhone:"8989898989", EmpAddress:"India",EmpSalary:"234241233"}] 

// oModel.setHeaders({ "content-type": "multipart/mixed; boundary=batch; application/json" });  

for(let i=0; i<aObj.length; i++){ 

oModel.create("/ManagementOpeartionSet", aObj[i],{ groupId: "foo", 

success: function (odata, Response) { 

sap.m.MessageToast.show("Created successfully."); 

oModel.refresh(); 

}, 

error: function () { 

sap.m.MessageToast.show("Error"); 

} 

});  

} 

oModel.submitChanges({ groupId: "foo", 

success: function(oData) {console.log("SUCCESS Batch ODATA"); },  

error: function(oData) { console.log("ERROR Batch ODATA");} 

 

Or 

 

var payload = this.getView().byId("SubsTableId").getModel("oMaintainModel").getProperty("/tableResult"); 

  

var oModel = this.getOwnerComponent().getModel(); 

var aDeferredGroups = oModel.getDeferredGroups(); 

aDeferredGroups = aDeferredGroups.concat(["createGrp"]); 

oModel.setDeferredGroups(aDeferredGroups); 

 

for(var i=0;i<payload.length; i++){ 

payload[i].SERN = parseInt(payload[i].SERN); 

oModel.create("/MatValSet", payload[i], { 

groupId: "createGrp" 

}); 

} 

  

oModel.submitChanges({ 

groupId: "createGrp", 

success: function (oData) { 

console.log("Success"); 

}.bind(this), 

error: function (oEvent) { 

}.bind(this) 

}); 


NavBack 

 

onNavBack: function(oEvent) { 

        let sPreviousHash = History.getInstance().getPreviousHash(), 

          oCrossAppNavigator = sap.ushell 

            ? sap.ushell.Container.getService("CrossApplicationNavigation") 

            : undefined; 

  

        if ( 

          sPreviousHash !== undefined || 

          (oCrossAppNavigator && !oCrossAppNavigator.isInitialNavigation()) 

        ) { 

          history.go(-1); 

        } else { 

          this.getRouter().navTo("main", {}, true); 

        } 

      }, 

 

Cross App Navigation 

 

 

var oCrossAppNavigator = sap.ushell.Container.getService("CrossApplicationNavigation"); // get a handle on the global XAppNav service 

var hash = (oCrossAppNavigator && oCrossAppNavigator.hrefForExternal({ 

target: { 

semanticObject: "ZSEM_MNG_LEAVE_REQ", 

action: "manage" 

}, 

params: { 

"OnBehalfEmployeeNumber": EmpNumber, 

"RequesterNumber": this.loginEmpId  

} 

})) || ""; 

oCrossAppNavigator.toExternal({ 

target: { 

shellHash: hash 

} 

});  

----------------------- 

 

Add Event Deligate 

 

 

var oPicker = this.byId("dateId"); 

oPicker.addEventDelegate({ 

onsapdown: function () { 

var oDateValue = oPicker.getDateValue(); 

oDateValue.setDate(oDateValue.getDate() - 1); 

oPicker.invalidate(); 

}, 

onsapup: function () { 

var oDateValue = oPicker.getDateValue(); 

oDateValue.setDate(oDateValue.getDate() + 1); 

oPicker.invalidate(); 

}, 

onAfterRendering: function(){ 

sap.m.MessageToast.show("control rendered"); 

console.log(this); 

} 

}); 

 

 

 

To get the user from shell in FLP 

 

sap.ushell.services.UserInfo().getId(); 

 

 

Setting the Language in the URL Parameter 

https://<server>:<port>/sap/bc/ui2/flp/FioriLaunchpad.html?sap-language=en 

 

 

 

To Display the document  

 

Would you set in transaction OAG1 the Viewer Settings WEBGUI to 'Call Internet Browser'  

 

 

 

 

 

 

To trigger the data after call 

 

var oContextPath = { 

path: "/StartProcessRecordSet(EmployeeNumber='" + '' + "',WorkItem='" + this.iInstanceId + "')", 

parameters: { 

"expand": "EmployeeEvalSet" 

}, 

events: { 

dataReceived: function (oEvnt) {  

    Your Code 

}.bind(this) 

} 

} 

 

 

 

 

 

 

 

 

 

Export the Excel 

 

ExportExcel: function () { 

            var aColumns = []; 

            aColumns.push({ 

                label: this._getText("VECHILEID"), 

                property: "vehicleId" 

            }); 

            aColumns.push({ 

                label: this._getText("YEAR"), 

                property: "year" 

            }); 

            aColumns.push({ 

                label: this._getText("MAKEANDMODEL"), 

                property: "makeModel" 

            }); 

            aColumns.push({ 

                label: this._getText("MILEAGE"), 

                property: "mileage" 

            }); 

            aColumns.push({ 

                label: this._getText("LOCATION"), 

                property: "location" 

            }); 

            aColumns.push({ 

                label: this._getText("EMPLOYEENAME"), 

                property: "modUser" 

            }); 

            aColumns.push({ 

                label: this._getText("EMPLOYEEID"), 

                property: "employeeId" 

            }); 

            aColumns.push({ 

                label: this._getText("DIVISION"), 

                property: "division" 

            }); 

            aColumns.push({ 

                label: this._getText("DRIVERNAME"), 

                property: "driverName" 

            }); 

            aColumns.push({ 

                label: this._getText("COMMUTEDAYS"), 

                property: "commuterDays" 

            }); 

            aColumns.push({ 

                label: this._getText("WHENSUB"), 

                property: "createdTimeStamp", 

                width: 25 

            }); 

            var dateFormat = DateFormat.getDateInstance({ 

                pattern: "MMM dd, yyyy" 

            }); 

            var offset = new Date().getTimezoneOffset(); 

            var mSettings = { 

                workbook: { 

                    columns: aColumns 

                }, 

                dataSource: this.getView().getModel().getProperty("/CommuteDetails").map(o => { 

                    o.createdTimeStamp = dateFormat.format(new Date(o.createdTimeStamp)); 

                    return o; 

                }), 

                fileName: "Commuters.xlsx" 

            }; 

            var oSpreadsheet = new Spreadsheet(mSettings); 

oSpreadsheet.build(); 

            oSpreadsheet.onprogress = function (iValue) { 

                jQuery.sap.log.debug("Export: %" + iValue + " completed"); 

            }; 

            oSpreadsheet.build().then(function () { 

                    jQuery.sap.log.debug("Export is finished"); 

                }) 

                .catch(function (sMessage) { 

                    jQuery.sap.log.error("Export error: " + sMessage); 

                }); 

        }, 

 

 

 

 

 

 

 

 

To add plugin :- /UI2/FLP_CONF_DEF   , /UI2/FLP_SYS_CONF 

var oSpreadsheet = new Spreadsheet(mSettings); 

oSpreadsheet.build(); 

oSpreadsheet.onprogress = function (iValue) { 

jQuery.sap.log.debug("Export: %" + iValue + " completed"); 

}; 

oSpreadsheet.build().then(function () { 

jQuery.sap.log.debug("Export is finished"); 

}) 

.catch(function (sMessage) { 

jQuery.sap.log.error("Export error: " + sMessage); 

}); 

extHookChangeFooterButtons: function (B) { 

// Place your hook implementation code here  

if (!B.aButtonList) { 

B.aButtonList = []; 

} 

 

 

 

// var OpenTask = this.getOwnerComponent().getModel("i18n").getResourceBundle().getText("OpenTask "); 

if (this._oHeaderFooterOptions.sDetailTitle === "Transfer Process") { 

B.aButtonList.pop(); 

var extendedButton = { 

sId: "EXT_BUTTON", 

sI18nBtnTxt: this.getView().getModel("i18n").getResourceBundle().getText("OpenTask"), //make sure you add texts in respective i18n files 

bEnabled: true, 

onBtnPressed: function (evt) { 

this.navigateToTransferProcess(); 

}.bind(this) 

}; 

B.aButtonList.push(extendedButton); 

} 

} 

Deep Entity   - Sending header and item from the front end 

 

 

 

var aTtableData = this.getView().byId("SubsTableId").getModel("oMaintainModel").getProperty("/tableResult"); 

  

for (var j = 0; j < aTtableData.length; j++) { 

aTtableData[j].SERN = parseInt(aTtableData[j].SERN); 

aTtableData[j].SUBT = aTtableData[j].SUBT.toUpperCase(); 

if (!aTtableData[j].Mat1 || !aTtableData[j].SERN || !aTtableData[j].SDAT || !aTtableData[j].EDAT || !aTtableData[j].MAT2) { 

MessageToast.show("plese enter all the Mandatory fields"); 

return; 

} 

} 

var sMarerial = this._getModel("oMaintainModel").getProperty("/Article"); 

  

var oStartDate = this._dateFormat().format(this.getView().byId("dateRangeId").getDateValue()); 

var oEndDate = this._dateFormat().format(this.getView().byId("dateRangeId").getSecondDateValue()); 

var payload = { 

Mat1: sMarerial, 

SDAT: oStartDate, 

EDAT: oEndDate, 

MatValSet: aTtableData 

}; 

  

oModel.create("/HeaderDetSet", payload, { 

success: function (oData) { 

console.log("Success"); 

}.bind(this), 

error: function (Err) { 

console.log(Err); 

}.bind(this) 

}); 

Sorting from the XML and controller 

 

items="{ 
         path : '/Invoices', 
         sorter : { 
            path : 'ProductName'  
         } 
      }" 

 

 

var oSorter = new sap.ui.model.Sorter('name'); 
var oBindingInfo = { 
    path: '/users', 
    template: oItemTemplate, 
    sorter: oSorter 
}; 

 

var oList = new sap.m.List(); 
oList.setModel(oModel); 
 
oList.bindItems(oBindingInfo); 

 

handleConfirmPA: function(oEvent) { 

          // Reset sorting criteria 

          let aSorters = [ 

            new Sorter( 

              oEvent.getParameter("sortItem").getProperty("key"), 

              oEvent.getParameter("sortDescending") 

            ) 

          ]; 

  

          // Apply sorters 

          this.byId("paymentAdviceTable") 

            .getBinding("items") 

            .sort(aSorters); 

        }, 

 

 

 

 

 

 

Filter or and add condetions both  

 

var andFilter = []; 

var orFilter = [];  

andFilter.push(new sap.ui.model.Filter("DeliDate", sap.ui.model.FilterOperator.BT, finalFromDate, finalToDate, false)); 

if(storecode.length !== 0){ 

storecode.forEach(function (sItem) { 

orFilter.push( 

new Filter({ 

path: "SupSite", 

operator: FilterOperator.EQ, 

value1: sItem.getKey(), 

and:true 

}) 

); 

}, this); 

andFilter.push(new sap.ui.model.Filter(orFilter, true)); 

} 

Steps to translate catalog titles , group titles .....in fiori launch pad 
 

 
 

(getting text id and config ID) 
In the system in which you run the SAP Fiori launchpad (on the front-end server for SAP Fiori launchpad), start Fiori Launchpad Texts (transaction /UI2/FLT). 
In the Text Filter field, enter the text string you want to change. 
(Optional) In the Catalog ID field, enter the ID of the catalog. 
In the Adaptation Layer section, choose the relevant scope (customization or configuration). 
Choose Execute. 
Open the link of the relevant entry in the Text column.(Click the text to get the config id and text id) 
Note 
If the object containing the text refers to another object, choose the link icon in the Reference column to navigate to the object containing the text definition (e.g. navigate from a group tile to the catalog containing the tile or from a reference tile to original tile from which it was copied). 
A table displaying the text ID and configuration ID is displayed, highlighting the relevant text. 

 
 

Note the entry of the Text ID and Configuration ID fields. 

 
 

 
Go to se63 --> short text --> 00 Meta objects(expand) --> TABL (Tables Meta) ---> 

 
 

in the object name enter --> *WDY_CONF_USERT2* 

 
 

select source and destination laungues and then click edit 

 
 

In the edit screen enter below 

 
 

user scope : A 
Enter config Id copies previously 
enter config type : 07 
 
click execute then maintain traslations 
 
Clear the cache 

 

HTML Library 

 

xmlns:html="http://www.w3.org/1999/xhtml" 

downloadDataFromTable: function (oConfig) { 

let oData = []; 

if (Array.isArray(oConfig.oContext)) { 

oData = oConfig.oContext.map(oItem => 

oItem.getModel().getProperty(oItem.getPath()) 

); 

  

this.downloadDataFromModel({ 

sPage: oConfig.sPage, 

oData: oData 

}); 

} else { 

Log.error( 

`Fatal error! Contexts invalid for param sPage = ${oConfig.sPage}` 

); 

} 

}, 

 

 

 

 

 

Redirect  

sap.m.URLHelper.redirect(modURL, false); 

 

 

 

 

 

 

onNavBack: function (oEvent) { 

let sPreviousHash = History.getInstance().getPreviousHash(), 

oCrossAppNavigator = sap.ushell ? sap.ushell.Container.getService("CrossApplicationNavigation") : undefined; 

  

if ( 

sPreviousHash !== undefined || 

(oCrossAppNavigator && !oCrossAppNavigator.isInitialNavigation()) 

) { 

history.go(-1); 

} else { 

this.getRouter().navTo("main", {}, true); 

} 

}, 
