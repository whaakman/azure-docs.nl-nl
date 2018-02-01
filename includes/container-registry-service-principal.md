## <a name="create-a-service-principal"></a>Een service-principal maken

Als een service-principal maken met toegang tot het register van de container, kunt u het volgende script. Update de `ACR_NAME` variabele met de naam van het register container en desgewenst de `--role` waarde in de [az ad sp maken-voor-rbac] [ az-ad-sp-create-for-rbac] opdracht andere machtigingen te verlenen. U moet hebben de [Azure CLI](/cli/azure/install-azure-cli) dit script is geïnstalleerd.

Nadat u het script uitvoert, dient u de service-principal **ID** en **wachtwoord**. Zodra u de referenties hebt, kunt u uw toepassingen en services om het register van de container als de service-principal te verifiëren.

[!code-azurecli-interactive[acr-sp-create](~/cli_scripts/container-registry/service-principal-create/service-principal-create.sh)]

## <a name="use-an-existing-service-principal"></a>Gebruik een bestaande service-principal

Voor het verlenen van toegang tot het register naar een bestaande service-principal, moet u een nieuwe rol toewijzen aan de service-principal. Net als bij een nieuwe service principal maakt, kunt u de pull-, push- en pull- en eigenaar toegang verlenen.

Het volgende script maakt gebruik van de [az roltoewijzing maken] [ az-role-assignment-create] opdracht toe te kennen *pull* machtigingen voor een service-principal die u opgeeft in de `SERVICE_PRINCIPAL_ID` variabele. Pas de `--role` waarde als u een ander niveau van toegang verlenen.

[!code-azurecli-interactive[acr-sp-role-assign](~/cli_scripts/container-registry/service-principal-assign-role/service-principal-assign-role.sh)]

U vindt deze scripts op GitHub, evenals versies voor PowerShell:

* [Azure CLI][acr-scripts-cli]
* [Azure PowerShell][acr-scripts-psh]

<!-- LINKS - External -->
[acr-scripts-cli]: https://github.com/Azure/azure-docs-cli-python-samples/tree/master/container-registry
[acr-scripts-psh]: https://github.com/Azure/azure-docs-powershell-samples/tree/master/container-registry

<!-- LINKS - Internal -->
[az-ad-sp-create-for-rbac]: /cli/azure/ad/sp#az_ad_sp_create_for_rbac
[az-role-assignment-create]: /cli/azure/role/assignment#az_role_assignment_create
