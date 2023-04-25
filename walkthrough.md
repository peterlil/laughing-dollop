# Challenge 3
```PowerShell
$aadAppName="peterlil-DevOpsWithGitHub"

az login
$aadApp = az ad app create --display-name $aadAppName
$aadAppPs = $aadApp | ConvertFrom-Json
$aadSpJson = az ad sp create --id $aadAppPs.appId
$aadSp = $aadSpJson | ConvertFrom-Json

$subscriptionId = az account show --query "[id]" --output tsv

$assigneeObjectId = $aadSp.id
az role assignment create --role contributor --subscription $subscriptionId --assignee-object-id  $assigneeObjectId --assignee-principal-type ServicePrincipal --scope /subscriptions/$subscriptionId

$appObjectID = $aadAppPs.id
az ad app federated-credential create --id $appObjectID --parameters '\l\temp\credential.json' 

#Result:
{
  "@odata.context": "https://graph.microsoft.com/v1.0/$metadata#applications('11a6759d-f2f5-42aa-83c8-75bbb4d2aa0f')/federatedIdentityCredentials/$entity",
  "audiences": [
    "api://AzureADTokenExchange"
  ],
  "description": "Testing",
  "id": "1f74b1e5-ec8f-4e44-9242-ac8a19a261a4",
  "issuer": "https://token.actions.githubusercontent.com",
  "name": "peterlil-DevOpsWithGitHub-creds",
  "subject": "repo:octo-org/octo-repo:environment:Production"
}

```

