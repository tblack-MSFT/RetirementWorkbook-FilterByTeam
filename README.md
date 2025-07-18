# Service Retirement Workbook - Filter by Team

## Purpose

This workbook was created to enable users to filter impacted resources based on organization tags. The workbook helps teams identify and manage resources that will be affected by service retirements, allowing for better planning and resource management across different teams or squads.

> **Note:** This solution includes an **Azure Resource Manager (ARM) template** (`RetirementWorkbook.json`) for automated deployment of the workbook to your Azure environment.

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

## Deployment Instructions

### ARM Template Deployment

The `RetirementWorkbook.json` file is an **Azure Resource Manager (ARM) template** that automates the deployment of the Service Retirement Workbook to your Azure environment.

#### Prerequisites

Before deploying the ARM template, ensure you have:

- **Azure Subscription**: Active Azure subscription with appropriate permissions
- **Resource Group**: A target resource group where the workbook will be deployed
- **Permissions**: At least `Contributor` role on the target resource group
- **Azure Monitor**: Access to Azure Monitor workbooks in your subscription
- **Azure Advisor**: Access to Azure Advisor (the workbook will be linked to Azure Advisor by default)

#### Deployment Methods

##### Method 1: Azure Portal (Recommended for beginners)

1. Navigate to the [Azure Portal](https://portal.azure.com)
2. Search for "Deploy a custom template" in the search bar
3. Click "Build your own template in the editor"
4. Copy the contents of `RetirementWorkbook.json` and paste it into the editor
5. Click "Save"
6. Fill in the required parameters:
   - **Subscription**: Select your target subscription
   - **Resource Group**: Select or create a resource group
   - **Workbook Display Name**: Enter a unique name for the workbook (default: "Service Retirement - Production")
   - **Workbook Source Id**: Leave as default or specify a custom source ID
7. Review the terms and click "Purchase" to deploy

##### Method 2: Azure CLI

```bash
# Login to Azure
az login

# Set your subscription (if needed)
az account set --subscription "your-subscription-name-or-id"

# Deploy the ARM template
az deployment group create \
  --resource-group "your-resource-group-name" \
  --template-file "RetirementWorkbook.json" \
  --parameters workbookDisplayName="Service Retirement - Production"
```

##### Method 3: Azure PowerShell

```powershell
# Login to Azure
Connect-AzAccount

# Set your subscription (if needed)
Set-AzContext -SubscriptionName "your-subscription-name"

# Deploy the ARM template
New-AzResourceGroupDeployment `
  -ResourceGroupName "your-resource-group-name" `
  -TemplateFile "RetirementWorkbook.json" `
  -workbookDisplayName "Service Retirement - Production"
```

##### Method 4: Deploy to Azure Button

You can also create a "Deploy to Azure" button by using the following URL pattern:

```
https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fyour-org%2Fyour-repo%2Fmain%2FRetirementWorkbook.json
```

#### Important Deployment Considerations

⚠️ **Things to Watch Out For:**

1. **Workbook Name Uniqueness**: The workbook display name must be unique within the resource group. If a workbook with the same name already exists, deployment will fail.

2. **Resource Group Permissions**: Ensure you have sufficient permissions in the target resource group. The deployment creates a `microsoft.insights/workbooks` resource.

3. **Region Availability**: Azure Monitor workbooks are available in most Azure regions, but verify your target region supports workbooks.

4. **Tag Policy Requirements**: The workbook relies on proper tagging. Ensure your organization has implemented a tag policy for 'Squad' tags (or customize the tag name within the workbook).

5. **Data Source Access**: The workbook queries Azure Resource Graph. Ensure the user accessing the workbook has appropriate permissions to query resources across the intended scope.

6. **Template Validation**: The ARM template includes parameter validation. Invalid parameters will cause deployment to fail with descriptive error messages.

#### Customization Parameters

The ARM template accepts the following parameters:

- **workbookDisplayName** (string): The friendly name for the workbook (must be unique within resource group)
- **workbookType** (string): Type of workbook (default: "workbook")
- **workbookSourceId** (string): Source identifier that determines where the workbook appears (default: "Azure Advisor" - links the workbook to Azure Advisor)
- **workbookId** (string): Unique identifier for the workbook resource

#### Post-Deployment Steps

After successful deployment:

1. **Access via Azure Advisor**: Navigate to **Azure Advisor** in the Azure Portal and look for the workbook in the workbooks section (default behavior due to workbookSourceId parameter)
2. **Access via Azure Monitor**: Alternatively, navigate to **Azure Monitor** → **Workbooks** in the Azure Portal
3. Find your deployed workbook under "My Workbooks" or the specified category
4. Open the workbook and verify it loads correctly
5. Test the Squad dropdown to ensure it populates with your tag values
6. Verify that resources are properly filtered based on squad selection

#### Troubleshooting Common Issues

**Deployment Fails with "Resource Already Exists"**
- Solution: Use a different workbook display name or delete the existing workbook

**Workbook Shows No Data**
- Solution: Verify resources are tagged with 'Squad' tags and you have permissions to view them

**Squad Dropdown is Empty**
- Solution: Ensure resources in your environment have 'Squad' tags applied

**Permission Denied Errors**
- Solution: Verify you have Contributor role on the resource group and Reader role on resources you want to monitor

## Usage

1. **Deploy the ARM template** to your Azure environment using one of the methods described above
2. **Ensure proper tagging**: Verify your resources are properly tagged with 'Squad' tags
3. **Access the workbook**: Open the workbook in Azure Advisor (default) or Azure Monitor → Workbooks
4. **Filter by team**: Use the Squad dropdown to filter resources by team
5. **Review results**: Analyze the filtered results to understand the impact of service retirements on your specific team's resources

## Benefits

- **Team-focused view**: Filter retirement impacts by specific squads or teams
- **Improved planning**: Teams can focus on resources that directly affect them
- **Better resource management**: Enables targeted action planning for service migrations
- **Organizational alignment**: Supports organizational structures through tag-based filtering