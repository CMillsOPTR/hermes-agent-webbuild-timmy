# Wix Velo product-assignment debugging

## Verified workflow

1. Keep the Fundraisers dataset in Read & Write mode and preserve the button's native `Submit` dataset connection.
2. Use the dataset `onAfterSave` hook to obtain the new fundraiser item's `_id`; product assignment persistence cannot run if dataset validation fails first.
3. Call a backend web method after save and insert one `CampaignProductAssignments` record per selected product.
4. Verify the new rows in CMS before cleaning up failed test data.

## Validation failure diagnosis

If Preview reports `datasetApi 'save' operation failed` and `Some of the elements validation failed`, inspect red form controls before debugging backend code. Required uploads and valid email formatting can block the dataset save. Repeated async-listener/message-channel warnings are generally preview noise unless they identify a field.

## Case-sensitive CMS field IDs

The collection display label is not the field key. Open the field's Edit field dialog and copy the exact **Field ID**. In the validated pilot, the display label `Wix Product ID` used the CMS field ID `textWxProductId`. Similar-looking keys such as `textWXProductID` and `wixProductId` created undefined warning fields and did not populate the intended column.

Do not delete undefined fields until records using them have been reviewed or migrated. A successful correction is a new test record whose product ID appears under the intended existing field.
