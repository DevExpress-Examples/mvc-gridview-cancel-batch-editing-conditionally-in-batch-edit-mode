<!-- default badges list -->
![](https://img.shields.io/endpoint?url=https://codecentral.devexpress.com/api/v1/VersionRange/128549632/24.2.1%2B)
[![](https://img.shields.io/badge/Open_in_DevExpress_Support_Center-FF7200?style=flat-square&logo=DevExpress&logoColor=white)](https://supportcenter.devexpress.com/ticket/details/T115116)
[![](https://img.shields.io/badge/📖_How_to_use_DevExpress_Examples-e9f6fc?style=flat-square)](https://docs.devexpress.com/GeneralInformation/403183)
[![](https://img.shields.io/badge/💬_Leave_Feedback-feecdd?style=flat-square)](#does-this-example-address-your-development-requirementsobjectives)
<!-- default badges end -->

# GridView for ASP.NET MVC - How to cancel editing conditionally in batch edit mode


You can cancel grid data editing in batch edit mode on the client or server side.

In this example, users can modify data only in even rows (except the **ID** column). Each column is protected in a different way, listed below.

## Implementation Details

On the client, handle the [BatchEditStartEditing](https://docs.devexpress.com/AspNet/js-ASPxClientGridView.BatchEditStartEditing) event and set its [e.cancel](https://docs.devexpress.com/AspNet/js-ASPxClientCancelEventArgs.cancel) property to `true` to cancel editing:

```js
function OnBatchStartEdit(s, e) {
  if (condition) e.cancel = true;
}
```

Another way to cancel editing is to use an editor's [SetReadOnly](https://docs.devexpress.com/AspNet/js-ASPxClientEdit.SetReadOnly%28readOnly%29) method. The code sample below demonstrates how to disable an editor conditionally:

```js
function OnBatchStartEdit(s, e) {
   if (condition) {
       editor = s.GetEditor(e.focusedColumn.fieldName);
       editor.SetReadOnly(true);
 }
```

You can implement custom server-side logic in the [CustomJSProperties](https://docs.devexpress.com/AspNetMvc/DevExpress.Web.Mvc.GridViewSettings.CustomJSProperties) event handler:

```cs
settings.CustomJSProperties += (s, e) => {
    var clientData = new Dictionary<int,object>();
    var grid = s as MVCxGridView;
    for (int i = 0; i < grid.VisibleRowCount; i++) {
        var rowValues = grid.GetRowValues(i, new string[] {"ID", "ServerSideExample"}) as object[];

        var key = Convert.ToInt32(rowValues[0]);
        if (key % 2 != 0)
            clientData.Add(key, "ServerSideExample");
    }
    e.Properties["cp_cellsToDisable"] = clientData;
};
```

## Files to Look At

- [Index.cshtml](./CS/BatchEditCancel/Views/Home/Index.cshtml) (VB: [Index.vbhtml](./VB/BatchEditCancel/Views/Home/Index.vbhtml))
- [GridViewPartial.cshtml](./CS/BatchEditCancel/Views/Home/GridViewPartial.cshtml) (VB: [GridViewPartial.vbhtml](./VB/BatchEditCancel/Views/Home/GridViewPartial.vbhtml))

## Documentation

- [Batch Edit Mode](https://docs.devexpress.com/AspNetMvc/16147/components/grid-view/concepts/data-editing-and-validation/batch-edit)

## More Examples

- [Grid View for ASP.NET Web Forms - Prevent the cell edit action on the client in batch edit mode](https://github.com/DevExpress-Examples/aspxgridview-prevent-batch-edit-action)
<!-- feedback -->
## Does this example address your development requirements/objectives?

[<img src="https://www.devexpress.com/support/examples/i/yes-button.svg"/>](https://www.devexpress.com/support/examples/survey.xml?utm_source=github&utm_campaign=asp-net-mvc-grid-cancel-editing-conditionally-in-batch-mode&~~~was_helpful=yes) [<img src="https://www.devexpress.com/support/examples/i/no-button.svg"/>](https://www.devexpress.com/support/examples/survey.xml?utm_source=github&utm_campaign=asp-net-mvc-grid-cancel-editing-conditionally-in-batch-mode&~~~was_helpful=no)

(you will be redirected to DevExpress.com to submit your response)
<!-- feedback end -->
