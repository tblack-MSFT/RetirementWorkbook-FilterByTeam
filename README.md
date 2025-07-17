# Service Retirement Workbook - Filter by Team

## Purpose

This workbook was created to enable users to filter impacted resources based on organization tags. The workbook helps teams identify and manage resources that will be affected by service retirements, allowing for better planning and resource management across different teams or squads.

**Requirements:**
- A proper tag policy must be implemented in your Azure environment ([Learn more about tagging Azure resources](https://learn.microsoft.com/en-us/azure/azure-resource-manager/management/tag-resources))
- Resources must be tagged with appropriate squad/team identifiers (if resources are not tagged, the workbook will list them as missing tag, missing-Squad in this case)
- The workbook relies on the presence of 'Squad' tags on Azure resources, but this can be changed within the workbook. Images below show where to change the tag name.

## Background

This workbook is based on the original **Service Retirement Workbook**. We simply add a dropdown that queries all the tags and only returns the Squad tags. This drop down filters based on the 'Squad' tag. This way teams/departments are able to filter for the resources that are impacted for their team/department.

## Key Changes Made

This modified version includes the following enhancements:

### 1. Squad Dropdown Parameter

A new dropdown parameter has been added to allow users to filter resources by squad/team:

- **Parameter name**: Squad
- **Display name**: Squad  
- **Parameter type**: Drop down
- **Allow multiple selections**: Yes
- **Explanation**: Filter by Squad tags
- **Data source**: Query-based, extracting squad values from resource tags

![Squad Parameter Configuration](https://github.com/user-attachments/assets/2e773b71-ef8e-41a3-8998-af663ffb0d3c)

### 2. Squad Column and Filtering Logic

The query has been enhanced to include squad-based filtering:

- **Squad column**: Added to display the squad tag value for each resource
- **Filtering logic**: Implemented `where` statement to filter the resource list based on the selected squad from the dropdown
- **Tag extraction**: The query now extracts and displays squad information from resource tags using `tostring(tags['Squad'])`
- **Dynamic filtering**: Resources are filtered based on the squad parameter selection

![Squad Column and Filtering](https://github.com/user-attachments/assets/83de5100-4f8a-4028-a774-44267beb06bc)

## Usage

1. Deploy the workbook to your Azure environment
2. Ensure your resources are properly tagged with 'Squad' tags
3. Open the workbook in Azure Monitor
4. Use the Squad dropdown to filter resources by team
5. Review the filtered results to understand the impact of service retirements on your specific team's resources

## Benefits

- **Team-focused view**: Filter retirement impacts by specific squads or teams
- **Improved planning**: Teams can focus on resources that directly affect them
- **Better resource management**: Enables targeted action planning for service migrations
- **Organizational alignment**: Supports organizational structures through tag-based filtering