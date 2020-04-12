# Azure custom roles

Custom role that allows read permissions on the subscription and as well open tickets.

```ps1
# Get and copy the subscription id
Get-AzureRmSubscription

# This will list all the provider operations
Get-AzureRmProviderOperation "Microsoft.Support/*" | FT Operation, Description -AutoSize

Get-AzureRmRoleDefinition -Name "Reader" | ConvertTo-Json | Out-File $home/clouddrive/ReaderSupportRole.json

code $home/clouddrive/ReaderSupportRole.json
```

The original file will look like this:

```json
{
  "Name": "Reader",
  "Id": "########################",
  "IsCustom": false,
  "Description": "Lets you view everything, but not make any changes.",
  "Actions": [
    "*/read"
  ],
  "NotActions": [],
  "DataActions": [],
  "NotDataActions": [],
  "AssignableScopes": [
    "/"
  ]
}
```

Edit the file:

```json
{
  "Name": "Reader Support Ticket",
  "IsCustom": true,
  "Description": "Lets you view everything in the subscription and open support tickets.",
  "Actions": [
    "*/read",
    "Microsoft.Support/*"
  ],
  "NotActions": [],
  "DataActions": [],
  "NotDataActions": [],
  "AssignableScopes": [
    "/subscriptions/###################"
  ]
}
```

Create new role:

```ps1
New-AzureRmRoleDefinition -InputFile $home/clouddrive/ReaderSupportRole.json
Get-AzureRmRoleDefinition | ? {$_.IsCustom -eq $true} | FT Name, IsCustom
```