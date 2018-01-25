## <a name="create-a-service-principal"></a>Een service-principal maken

Als een service-principal maken met toegang tot het register van de container, kunt u het volgende script. Update de `ACR_NAME` variabele met de naam van het register container en desgewenst de `--role` waarde in de [az ad sp maken-voor-rbac] [ az-ad-sp-create-for-rbac] opdracht andere machtigingen te verlenen. U moet hebben de [Azure CLI](/cli/azure/install-azure-cli) dit script is geïnstalleerd.

Nadat u het script uitvoert, dient u de service-principal **ID** en **wachtwoord**. Zodra u de referenties hebt, kunt u uw toepassingen en services om het register van de container als de service-principal te verifiëren.

```bash
#!/bin/bash

# Modify for your environment. The ACR_NAME is the name of your Azure Container
# Registry, and the SERVICE_PRINCIPAL_NAME can be any unique name within your
# subscription (you can use the default below).
ACR_NAME=myregistryname
SERVICE_PRINCIPAL_NAME=acr-service-principal

# Populate some values required for subsequent command args
ACR_LOGIN_SERVER=$(az acr show --name $ACR_NAME --query loginServer --output tsv)
ACR_REGISTRY_ID=$(az acr show --name $ACR_NAME --query id --output tsv)

# Create the service principal with rights scoped to the registry.
# Default permissions are for both docker push and pull access. Modify the
# '--role' argument value as desired:
# reader:      pull only
# contributor: push and pull
# owner:       push, pull, and assign roles
SP_PASSWD=$(az ad sp create-for-rbac --name $SERVICE_PRINCIPAL_NAME --scopes $ACR_REGISTRY_ID --role reader --query password --output tsv)
SP_APP_ID=$(az ad sp show --id http://$SERVICE_PRINCIPAL_NAME --query appId --output tsv)

# Output the service principal's credentials; use these in your services and
# applications to authenticate to the container registry.
echo "Service principal ID: $SP_APP_ID"
echo "Service principal password: $SP_PASSWD"
```

## <a name="use-an-existing-service-principal"></a>Gebruik een bestaande service-principal

Voor het verlenen van toegang tot het register naar een bestaande service-principal, moet u een nieuwe rol toewijzen aan de service-principal. Net als bij een nieuwe service principal maakt, kunt u de pull-, push- en pull- en eigenaar toegang verlenen.

Het volgende script maakt gebruik van de [az roltoewijzing maken] [ az-role-assignment-create] opdracht toe te kennen *pull* machtigingen voor een service-principal die u opgeeft in de `SERVICE_PRINCIPAL_ID` variabele. Pas de `--role` waarde als u een ander niveau van toegang verlenen.

```bash
#!/bin/bash

# Modify for your environment. The ACR_NAME is the name of your Azure Container
# Registry, and the SERVICE_PRINCIPAL_ID is the service principal's 'appId' or
# one of its 'servicePrincipalNames' values.
ACR_NAME=myregistryname
SERVICE_PRINCIPAL_ID=<service-principal-ID>

# Populate value required for subsequent command args
ACR_REGISTRY_ID=$(az acr show --name $ACR_NAME --query id --output tsv)

# Assign the desired role to the service principal. Modify the '--role' argument
# value as desired:
# reader:      pull only
# contributor: push and pull
# owner:       push, pull, and assign roles
az role assignment create --assignee $SERVICE_PRINCIPAL_ID --scope $ACR_REGISTRY_ID --role reader
```

<!-- LINKS - Internal -->
[az-ad-sp-create-for-rbac]: /cli/azure/ad/sp#az_ad_sp_create_for_rbac
[az-role-assignment-create]: /cli/azure/role/assignment#az_role_assignment_create