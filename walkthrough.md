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
  "id": "fbf54d58-e3bc-409d-900b-2686c769dfea",
  "issuer": "https://token.actions.githubusercontent.com/",
  "name": "peterlil-DevOpsWithGitHub-creds",
  "subject": "repo:peterlil/laughing-dollop:ref:refs/heads/main"
}

az ad app federated-credential delete --federated-credential-id eb6e1890-6977-479b-bb19-50a6d9698453 --id $appObjectID

$aadAppName2="peterlil-DevOpsWithGitHub2"
az ad sp create-for-rbac -n $aadAppName2 --role contributor --scopes "/subscriptions/$subscriptionId"
# Response:
{
  "appId": "9c70cde0-7819-4ff6-93fe-3d4ac2937a0a",
  "displayName": "peterlil-DevOpsWithGitHub2",
  "password": "FfE8Q~pU9iVMdLv5pG1wvrtmAPW9W4iPX5.zva3o",
  "tenant": "16b3c013-d300-468d-ac64-7eda0820b6d3"
}

```

