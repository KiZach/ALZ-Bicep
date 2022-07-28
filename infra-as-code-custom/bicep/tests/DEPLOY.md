az account set --subscription 1959e71d-fd70-4e34-aa9d-5487117ed4f5

Deploy
az deployment group create --resource-group alzcanary-westeurope-hub-networking --template-file .\infra-as-code\bicep\modules\hubNetworking\hubNetworking.bicep --parameters @infra-as-code\bicep\modules\hubNetworking\hubNetworking.parameters.canary.json

az deployment mg create --location "westeurope" --management-group-id "alz" --template-file .\infra-as-code\bicep\orchestration\hubPeeredSpoke\hubPeeredSpoke.bicep --parameters @infra-as-code\bicep\orchestration\hubPeeredSpoke\hubPeeredSpoke.parameters.example.z4it.json

az deployment group create --resource-group alzcanary-westeurope-hub-networking --template-file .\infra-as-code\bicep\modules\vwanConnectivity\vwanConnectivity.bicep --parameters @infra-as-code\bicep\modules\vwanConnectivity\vwanConnectivity.parameters.canary.json

az deployment mg create --location "westeurope" --management-group-id "alz" --template-file .\infra-as-code\bicep\orchestration\hubPeeredSpoke\hubPeeredSpoke.bicep --parameters @infra-as-code\bicep\orchestration\hubPeeredSpoke\hubPeeredSpoke.vwan.parameters.example.z4it.json

CleanUp
az deployment group create --resource-group alzcanary-westeurope-spoke-networking --template-file .\infra-as-code-custom\bicep\tests\empty.bicep --mode Complete
az deployment group create --resource-group alzcanary-westeurope-hub-networking --template-file .\infra-as-code-custom\bicep\tests\empty.bicep --mode Complete

TEST
