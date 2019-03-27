---
title: Toegang tot een key vault - Azure Key Vault beveiligen | Microsoft Docs
description: Toegangsmachtigingen voor Azure Key Vault sleutels en geheimen beheren. Bevat informatie over het model voor verificatie en autorisatie voor Key Vault en over het beveiligen van uw key vault.
services: key-vault
documentationcenter: ''
author: amitbapat
manager: barbkess
tags: azure-resource-manager
ms.assetid: e5b4e083-4a39-4410-8e3a-2832ad6db405
ms.service: key-vault
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 01/07/2019
ms.author: ambapat
ms.openlocfilehash: 20c58647b8a6283de4ca2b90c830fe54db927095
ms.sourcegitcommit: 0dd053b447e171bc99f3bad89a75ca12cd748e9c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/26/2019
ms.locfileid: "58484183"
---
# <a name="secure-access-to-a-key-vault"></a>Beveiligde toegang tot een key vault

Azure Key Vault is een cloudservice die beveiligt uw versleutelingssleutels en geheimen, zoals certificaten, verbindingsreeksen en wachtwoorden. Omdat deze gegevens vertrouwelijk zijn en bedrijfskritiek, u moet voor het beveiligen van toegang tot uw key vaults door toe te staan alleen gemachtigde toepassingen en gebruikers. Dit artikel bevat een overzicht van het model voor Key Vault. Het wordt verificatie en autorisatie worden uitgelegd en wordt beschreven hoe u veilig toegang tot uw key vaults.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="access-model-overview"></a>Overzicht van Access-model

Toegang tot een key vault wordt geregeld via twee interfaces: de **beheerlaag** en de **gegevenslaag**. Het beheervlak is waar u Key Vault beheren zelf. Bewerkingen op dit vlak zijn onder meer het maken en verwijderen van sleutelkluizen, bij het ophalen van de eigenschappen van de Key Vault en toegangsbeleid wordt bijgewerkt. Het vlak van gegevens is waar u werkt met de gegevens die zijn opgeslagen in een key vault. U kunt toevoegen, verwijderen en wijzigen van sleutels, geheimen en certificaten.

Voor toegang tot een key vault in een van beide vlak, moet alle aanroepers (gebruikers of toepassingen) juiste verificatie en autorisatie. Verificatie wordt de identiteit van de oproepende functie. Autorisatie bepaalt welke bewerkingen de aanroeper kan worden uitgevoerd. 

Beide lagen gebruiken Azure Active Directory (Azure AD) voor verificatie. Het beheervlak op rollen gebaseerd toegangsbeheer (RBAC) wordt gebruikt voor autorisatie, en de gegevenslaag maakt gebruik van een toegangsbeleid voor Key Vault.

## <a name="active-directory-authentication"></a>Active Directory-authenticatie

Wanneer u een key vault in een Azure-abonnement maakt, het automatisch gekoppeld aan de Azure AD-tenant van het abonnement. Alle aanroepers in beide lagen moeten registreren in deze tenant en verifiëren voor toegang tot de key vault. In beide gevallen toepassingen hebben toegang tot Key Vault op twee manieren:

- **Gebruikers- en toepassing toegang**: De toepassing heeft toegang tot Key Vault namens een gebruiker is aangemeld. Voorbeelden van dit type toegang zijn Azure PowerShell en Azure portal. Gebruikerstoegang wordt verleend op twee manieren. Gebruikers hebben toegang tot Key Vault vanuit elke toepassing, of ze een bepaalde toepassing moeten gebruiken (aangeduid als _samengestelde identiteit_).
- **Alleen de toepassing toegang**: De toepassing wordt uitgevoerd als een daemon-service of achtergrond-taak. De toepassings-id krijgt toegang tot de key vault.

Voor beide typen toegang, wordt de toepassing verifieert met Azure AD. De toepassing gebruikmaakt van een [ondersteund verificatiemethode](../active-directory/develop/authentication-scenarios.md) op basis van het toepassingstype. De toepassing krijgt een token voor een resource in het vlak van om toegang te verlenen. De resource heeft een eindpunt in het vlak van beheer of gegevens, op basis van de Azure-omgeving. De toepassing gebruikt het token en stuurt een REST-API-aanvraag naar Key Vault. Voor meer informatie, Controleer de [gehele verificatiestroom](../active-directory/develop/v1-protocols-oauth-code.md).

Het model van een enkel verificatiemechanisme voor beide lagen heeft een aantal voordelen:

- Organisaties kunnen de toegang tot alle key vaults in hun organisatie centraal beheren.
- Als een gebruiker verlaat, verliest ze direct toegang tot alle key vaults in de organisatie.
- Organisaties kunnen de verificatie aanpassen met behulp van de opties in Azure AD, zoals meervoudige verificatie voor extra beveiliging in te schakelen.

## <a name="resource-endpoints"></a>Resource-eindpunt

Toepassingen toegang tot de vlakken via-eindpunten. De besturingselementen voor toegang voor de twee vlakken werken onafhankelijk van elkaar. Een toepassing om toegang te verlenen voor het gebruik van sleutels in een key vault, kunt u toegang tot de gegevenslaag verlenen met behulp van een toegangsbeleid voor Key Vault. Als u wilt verlenen aan een gebruiker alleen toegang tot Key Vault-eigenschappen en tags, maar niet de toegang tot gegevens (sleutels, geheimen of certificaten), verleent u toegang tot de beheerlaag met RBAC.

De volgende tabel ziet u de eindpunten voor het beheer en de gegevens vlakken.

| Toegang tot&nbsp;vlak | Eindpunten voor toegang | Bewerkingen | Toegang tot&nbsp;mechanisme beheren |
| --- | --- | --- | --- |
| Beheerlaag | **Wereldwijd:**<br> management.azure.com:443<br><br> **Azure China 21Vianet:**<br> management.chinacloudapi.cn:443<br><br> **Azure van de Amerikaanse overheid:**<br> management.usgovcloudapi.net:443<br><br> **Azure Duitsland:**<br> management.microsoftazure.de:443 | Maken, lezen, bijwerken en verwijderen van sleutelkluizen<br><br>Toegangsbeleid voor Key Vault instellen<br><br>Labels voor Key Vault instellen | Azure Resource Manager RBAC |
| Gegevenslaag | **Wereldwijd:**<br> &lt;kluisnaam&gt;.vault.azure.net:443<br><br> **Azure China 21Vianet:**<br> &lt;kluisnaam&gt;.vault.azure.cn:443<br><br> **Azure van de Amerikaanse overheid:**<br> &lt;kluisnaam&gt;.vault.usgovcloudapi.net:443<br><br> **Azure Duitsland:**<br> &lt;kluisnaam&gt;.vault.microsoftazure.de:443 | Sleutels: ontsleutelen, versleutelen,<br> uitpakken, inpakken, controleren, ondertekenen,<br> ophalen, lijsten, bijwerken, maken,<br> importeren, verwijderen, back-up en herstel<br><br> Geheimen: ophalen, weergeven, instellen en verwijderen | Toegangsbeleid voor Key Vault |

## <a name="management-plane-and-rbac"></a>Beheer- en RBAC

In het beheervlak gebruikt u RBAC (op rollen gebaseerd toegangsbeheer) voor het autoriseren van de bewerkingen die een aanroeper kunt uitvoeren. In de RBAC-model heeft elk Azure-abonnement een exemplaar van Azure AD. U kunt toegang verlenen aan gebruikers, groepen en toepassingen in deze directory. Toegang te krijgen tot het beheren van resources in het Azure-abonnement die gebruikmaken van de Azure Resource Manager-implementatiemodel. Om toegang te verlenen, gebruikt u de [Azure-portal](https://portal.azure.com/), wordt de [Azure CLI](../cli-install-nodejs.md), [Azure PowerShell](/powershell/azureps-cmdlets-docs), of de [Azure Resource Manager REST API's](https://msdn.microsoft.com/library/azure/dn906885.aspx).

U maakt een key vault in een resourcegroep en toegang beheren met behulp van Azure AD. U verleent gebruikers of groepen van de mogelijkheid voor het beheren van de sleutelkluizen in een resourcegroep. In dat geval verleent u de toegang op het niveau van een bepaald bereik in juiste RBAC-rollen toe te wijzen. Om toegang te verlenen aan een gebruiker voor het beheren van sleutelkluizen, u een vooraf gedefinieerde toewijzen `key vault Contributor` rol aan de gebruiker op een bepaald bereik. Het niveau van de volgende bereiken kunnen worden toegewezen aan een RBAC-rol:

- **Abonnement**: Een RBAC-rol op het abonnementsniveau van het is van toepassing op alle resourcegroepen en resources in dat abonnement.
- **Resourcegroep**: Een RBAC-rol die is toegewezen op het niveau van de resource is van toepassing op alle resources in die resourcegroep.
- **Specifieke resource**: Een RBAC-rol die is toegewezen aan een specifieke resource, geldt voor die bron. In dit geval heeft de resource een specifieke key vault.

Er zijn verschillende vooraf gedefinieerde rollen. Als een vooraf gedefinieerde rollen niet aansluiten bij uw behoeften, kunt u uw eigen rollen definiëren. Zie voor meer informatie, [RBAC: Ingebouwde rollen](../role-based-access-control/built-in-roles.md).

> [!IMPORTANT]
> Als een gebruiker heeft `Contributor` machtigingen voor de beheerlaag van een sleutelkluis, de gebruiker kan zich toegang verlenen tot de gegevenslaag door in te stellen van een toegangsbeleid voor Key Vault. U moet zorgvuldig beheren wie heeft `Contributor` rollen gebaseerde toegang tot uw key vaults. Zorg ervoor dat alleen gemachtigde personen kunnen openen en beheren van uw sleutelkluizen, sleutels, geheimen en certificaten.
>

<a id="data-plane-access-control"></a> 
## <a name="data-plane-and-access-policies"></a>Gegevensbeleid voor vlak- en toegangsbeheer

U verleent toegang tot de gegevenslaag door het instellen van toegangsbeleid voor Key Vault voor een key vault. Als u wilt deze toegangsbeleid instellen, een gebruiker, groep of toepassing moet hebben `Contributor` machtigingen voor de beheerlaag voor die key vault.

Verleent u een gebruiker, groep of toegang tot toepassingen voor het uitvoeren van bepaalde bewerkingen voor sleutels of geheimen in een key vault. Key Vault ondersteunt maximaal 1.024 vermeldingen van het toegangsbeleid voor key vault. Als u wilt verlenen toegang tot de gegevenslaag om meerdere gebruikers, een Azure AD-beveiligingsgroep maken en gebruikers toevoegen aan die groep.

<a id="key-vault-access-policies"></a> Toegangsbeleid voor Key Vault verlenen machtigingen afzonderlijk voor sleutels, geheimen en certificaten. U kunt een gebruikerstoegang alleen voor sleutels en geheimen mogen verlenen. Toegangsmachtigingen voor sleutels, geheimen en certificaten zijn op het kluisniveau van de. Toegangsbeleid voor Key Vault bieden geen ondersteuning voor gedetailleerde, op objectniveau machtigingen, zoals een specifieke sleutel, het geheim of het certificaat. Als u wilt dat het toegangsbeleid van key vault, gebruikt u de [Azure-portal](https://portal.azure.com/), wordt de [Azure CLI](../cli-install-nodejs.md), [Azure PowerShell](/powershell/azureps-cmdlets-docs), of de [Key Vault Management REST API's](https://msdn.microsoft.com/library/azure/mt620024.aspx).

> [!IMPORTANT]
> Key Vault-toegangsbeleid toepassen op het kluisniveau van de. Wanneer een gebruiker is gemachtigd om te maken en verwijderen van sleutels, ze kunnen deze bewerkingen uitvoeren op alle sleutels in die key vault.
>

U kunt toegang tot de gegevenslaag beperken met behulp van [virtual network-service-eindpunten voor Azure Key Vault](key-vault-overview-vnet-service-endpoints.md). U kunt configureren [firewalls en virtuele netwerkregels](key-vault-network-security.md) voor een extra beveiligingslaag.

## <a name="example"></a>Voorbeeld

In dit voorbeeld wordt er gewerkt aan een toepassing die gebruikmaakt van een certificaat voor SSL, Azure Storage voor het opslaan van gegevens en een RSA 2048-bits sleutel voor tekenbewerkingen. De toepassing wordt uitgevoerd in een Azure virtuele machine (VM) (of een virtuele-machineschaalset). We kunnen een key vault gebruiken voor het opslaan van geheimen van de toepassing. We kunnen de bootstrap-certificaat dat door de toepassing wordt gebruikt voor verificatie met Azure AD op te slaan.

We moeten toegang hebben tot de volgende opgeslagen sleutels en geheimen:
- **SSL-certificaat**: Wordt gebruikt voor SSL.
- **Opslagsleutel**: Gebruikt voor toegang tot het opslagaccount.
- **RSA 2048-bits sleutel**: Gebruikt voor tekenbewerkingen.
- **Bootstrap-certificaat**: Gebruikt voor verificatie met Azure AD. Nadat er toegang is verleend, kunnen we de opslagsleutel ophalen en de RSA-sleutel gebruiken voor het ondertekenen van.

We moeten voor het definiëren van de volgende rollen als u wilt implementeren, beheren en controleren van de toepassing opgeven:
- **Beveiligingsteam**: IT-afdeling van het kantoor van de CSO (Chief Security Officer) of soortgelijke bijdragers. Het beveiligingsteam is verantwoordelijk voor de juiste beveiliging van geheimen. De geheimen kunnen bevatten SSL-certificaten, RSA-sleutels voor het ondertekenen, tekenreeksen voor databaseverbindingen en opslagaccountsleutels.
- **Ontwikkelaars en operators**: Het personeel dat de toepassing ontwikkelen en implementeren in Azure. De leden van dit team geen deel uitmaken van de medewerkers van de beveiliging. Deze mag geen toegang tot gevoelige gegevens, zoals SSL-certificaten en RSA-sleutels bevatten. Alleen de toepassing die ze implementeren moet toegang tot gevoelige gegevens hebben.
- **Auditors**: Deze rol is voor inzenders die geen lid zijn van de ontwikkeling of de algemene IT-personeel. Ze Controleer het gebruik en onderhoud van certificaten, sleutels en geheimen te voldoen aan de beveiligingsstandaarden. 

Er is een andere rol die zich buiten het bereik van de toepassing: het abonnement (of de resourcegroep) beheerder. De beheerder van abonnement stelt u initiële toegangsmachtigingen voor het beveiligingsteam. Ze verlenen toegang tot het security team met behulp van een resourcegroep die de resources die door de toepassing nodig heeft.

We wilt toestaan dat de volgende bewerkingen voor onze functies:

**Beveiligingsteam**
- Key vaults maken.
- Schakel logboekregistratie van Key Vault.
- Sleutels en geheimen toevoegen.
- Maak back-ups van sleutels voor herstel na noodgevallen.
- Instellen van toegangsbeleid voor Key Vault om machtigingen te verlenen aan gebruikers en toepassingen voor specifieke bewerkingen.
- De sleutels en geheimen periodiek te herstellen.

**Ontwikkelaars en operators**
- Ophalen van verwijzingen van de security-team voor de bootstrap en de SSL-certificaten (vingerafdrukken), de opslagsleutel (geheime URI) en de RSA-sleutel (sleutel-URI) voor het ondertekenen.
- Ontwikkel en implementeer de toepassing voor toegang tot sleutels en geheimen via een programma.

**Auditors**
- Bekijk de logboeken van Key Vault om te bevestigen dat de juiste gebruik van sleutels en geheimen en voldoet aan de beveiligingsnormen voor gegevens.

De volgende tabel geeft een overzicht van de toegangsmachtigingen voor onze functies en de toepassing. 

| Rol | Machtigingen voor de beheerlaag | Machtigingen voor de gegevenslaag |
| --- | --- | --- |
| Beveiligingsteam | Inzender voor Key Vault | Sleutels: back-up maken, maken, verwijderen, ophalen, importeren, lijsten, herstellen<br>Geheimen: alle bewerkingen |
| Ontwikkelaars en&nbsp;operators | Implementatiemachtigingen voor Key Vault<br><br> **Opmerking**: Deze machtiging kan geïmplementeerde VM's voor het ophalen van geheimen in een key vault. | Geen |
| Auditors | Geen | Sleutels: weergeven<br>Geheimen: weergeven<br><br> **Opmerking**: Deze machtiging kunt auditors kenmerken (tags, activering datums, vervaldatums) voor sleutels en geheimen die niet worden weergegeven in de logboeken controleren. |
| Toepassing | Geen | Sleutels: ondertekenen<br>Geheimen: ophalen |

De drie teamrollen voor het moeten toegang tot andere resources samen met Key Vault-machtigingen. Implementatie van virtuele machines (of de functie Web Apps van Azure App Service) nodig voor ontwikkelaars en operators `Contributor` toegang tot de resourcetypen. Auditors hebben leestoegang nodig tot het opslagaccount waar de Key Vault-logboeken worden opgeslagen.

Toegang tot sleutels en geheimen via een programma, Zie de volgende bronnen voor meer informatie over het implementeren van certificaten:
- Meer informatie over het [certificaten aan virtuele machines implementeren vanaf een door de klant beheerde key vault](https://blogs.technet.microsoft.com/kv/2016/09/14/updated-deploy-certificates-to-vms-from-customer-managed-key-vault/) (blogbericht).
- Download de [voorbeelden voor Azure Key Vault-client](https://www.microsoft.com/download/details.aspx?id=45343). Deze inhoud ziet u hoe u een bootstrap-certificaat gebruiken om u te verifiëren met Azure AD voor toegang tot een key vault.

U kunt de meeste van de machtigingen voor toegang verlenen met behulp van de Azure-portal. Gedetailleerde om machtigingen te verlenen, dat kunt u Azure PowerShell of Azure CLI.

De PowerShell-fragmenten in deze sectie zijn gebouwd met de volgende veronderstellingen:
- De Azure AD-beheerder heeft beveiligingsgroepen om weer te geven van de drie functies gemaakt: Contoso Security Team, Contoso App DevOps en Contoso App Auditors. De beheerder heeft gebruikers toegevoegd aan de bijbehorende groepen.
- Alle resources bevinden zich in de **ContosoAppRG** resourcegroep.
- De Key Vault-logboeken worden opgeslagen de **contosologstorage** storage-account. 
- De **ContosoKeyVault** sleutelkluis en de **contosologstorage** storage-account zich in dezelfde Azure-locatie.

De beheerder van abonnement wijst de `key vault Contributor` en `User Access Administrator` rollen aan het beveiligingsteam. Deze rollen kunnen het beveiligingsteam voor het beheren van toegang tot andere resources en sleutelkluizen, beide in de **ContosoAppRG** resourcegroep.

```powershell
New-AzRoleAssignment -ObjectId (Get-AzADGroup -SearchString 'Contoso Security Team')[0].Id -RoleDefinitionName "key vault Contributor" -ResourceGroupName ContosoAppRG
New-AzRoleAssignment -ObjectId (Get-AzADGroup -SearchString 'Contoso Security Team')[0].Id -RoleDefinitionName "User Access Administrator" -ResourceGroupName ContosoAppRG
```

Het beveiligingsteam een key vault maakt en logboekregistratie en toegangsmachtigingen ingesteld. Zie voor meer informatie over Key Vault-beleid toegangsmachtigingen [over Azure Key Vault sleutels, geheimen en certificaten](about-keys-secrets-and-certificates.md).

```powershell
# Create a key vault and enable logging
$sa = Get-AzStorageAccount -ResourceGroup ContosoAppRG -Name contosologstorage
$kv = New-AzKeyVault -Name ContosoKeyVault -ResourceGroup ContosoAppRG -SKU premium -Location 'westus' -EnabledForDeployment
Set-AzDiagnosticSetting -ResourceId $kv.ResourceId -StorageAccountId $sa.Id -Enabled $true -Category AuditEvent

# Set up data plane permissions for the Contoso Security Team role
Set-AzKeyVaultAccessPolicy -VaultName ContosoKeyVault -ObjectId (Get-AzADGroup -SearchString 'Contoso Security Team')[0].Id -PermissionsToKeys backup,create,delete,get,import,list,restore -PermissionsToSecrets get,list,set,delete,backup,restore,recover,purge

# Set up management plane permissions for the Contoso App DevOps role
# Create the new role from an existing role
$devopsrole = Get-AzRoleDefinition -Name "Virtual Machine Contributor"
$devopsrole.Id = $null
$devopsrole.Name = "Contoso App DevOps"
$devopsrole.Description = "Can deploy VMs that need secrets from a key vault"
$devopsrole.AssignableScopes = @("/subscriptions/<SUBSCRIPTION-GUID>")

# Add permissions for the Contoso App DevOps role so members can deploy VMs with secrets deployed from key vaults
$devopsrole.Actions.Add("Microsoft.KeyVault/vaults/deploy/action")
New-AzRoleDefinition -Role $devopsrole

# Assign the new role to the Contoso App DevOps security group
New-AzRoleAssignment -ObjectId (Get-AzADGroup -SearchString 'Contoso App Devops')[0].Id -RoleDefinitionName "Contoso App Devops" -ResourceGroupName ContosoAppRG

# Set up data plane permissions for the Contoso App Auditors role
Set-AzKeyVaultAccessPolicy -VaultName ContosoKeyVault -ObjectId (Get-AzADGroup -SearchString 'Contoso App Auditors')[0].Id -PermissionsToKeys list -PermissionsToSecrets list
```

Onze gedefinieerde aangepaste rollen zijn toegewezen alleen aan het abonnement waarin de **ContosoAppRG** resourcegroep wordt gemaakt. Voor het gebruik van een aangepaste rol voor andere projecten in andere abonnementen, moet u andere abonnementen toevoegen aan het bereik voor de rol.

Voor onze DevOps-personeel, de toewijzing van de aangepaste rol voor de key vault `deploy/action` machtiging is afgestemd op de resourcegroep. Alleen virtuele machines die zijn gemaakt in de **ContosoAppRG** resourcegroep toegang hebben tot de geheimen (SSL en bootstrap certificaten). Virtuele machines die in andere resourcegroepen zijn gemaakt door een DevOps-lid heeft geen toegang tot deze geheime gegevens, zelfs als de virtuele machine de geheime URI's heeft.

In ons voorbeeld wordt een eenvoudig scenario beschreven. Concrete scenario's mogelijk meer complex zijn. U kunt machtigingen voor uw key vault op basis van uw behoeften aanpassen. Wordt ervan uitgegaan dat het beveiligingsteam de sleutel en geheime verwijzingen (URI's en vingerafdrukken), die worden gebruikt door de DevOps-personeel in hun toepassingen biedt. Ontwikkelaars en operators nodig geen voor alle toegang tot de gegevenslaag. Richten we ons op over het beveiligen van uw key vault. Geven belangrijk als u beveiligt [uw virtuele machines](https://azure.microsoft.com/services/virtual-machines/security/), [opslagaccounts](../storage/common/storage-security-guide.md), en andere Azure-resources.

> [!NOTE]
> Dit voorbeeld laat zien hoe toegang tot Key Vault wordt vergrendeld in de productieomgeving. Ontwikkelaars moeten hebben hun eigen abonnement of resourcegroep groep met volledige machtigingen voor het beheren van hun kluizen, VM's en het opslagaccount waar ze de toepassing ontwikkelen.

Is het raadzaam dat u instelt van extra beveiligde toegang tot uw key vault door [Key Vault-firewalls en virtuele netwerken configureren](key-vault-network-security.md).

## <a name="resources"></a>Resources

* [Azure AD RBAC](../role-based-access-control/role-assignments-portal.md)

* [RBAC: Ingebouwde rollen](../role-based-access-control/built-in-roles.md)

* [Resource Manager-implementatie en klassieke implementatie begrijpen](../azure-resource-manager/resource-manager-deployment-model.md) 

* [RBAC met Azure PowerShell beheren](../role-based-access-control/role-assignments-powershell.md) 

* [RBAC met de REST-API beheren](../role-based-access-control/role-assignments-rest.md)

* [RBAC voor Microsoft Azure](https://channel9.msdn.com/events/Ignite/2015/BRK2707)

    Deze 2015 Microsoft Ignite conferentie video bespreekt toegang tot beheer en rapportage in Azure. Het behandelt ook de aanbevolen procedures voor het beveiligen van toegang tot Azure-abonnementen met behulp van Azure AD.

* [Toegang verlenen aan webtoepassingen met behulp van OAuth 2.0 en Azure AD](../active-directory/develop/v1-protocols-oauth-code.md)

* [Key Vault-beheer REST-API 's](https://msdn.microsoft.com/library/azure/mt620024.aspx)

    De verwijzing voor de REST-API's voor het beheren van uw key vault via een programma, inclusief het instellen van toegangsbeleid voor Key Vault.

* [Key Vault REST-API 's](https://msdn.microsoft.com/library/azure/dn903609.aspx)

* [Toegangsbeheer voor sleutels](https://msdn.microsoft.com/library/azure/dn903623.aspx#BKMK_KeyAccessControl)
  
* [Toegangsbeheer voor geheimen](https://msdn.microsoft.com/library/azure/dn903623.aspx#BKMK_SecretAccessControl)
  
* [Stel](/powershell/module/az.keyvault/Set-azKeyVaultAccessPolicy) en [verwijderen](/powershell/module/az.keyvault/Remove-azKeyVaultAccessPolicy) toegangsbeleid van Key Vault met behulp van PowerShell.
  
## <a name="next-steps"></a>Volgende stappen

Configureer [Key Vault-firewalls en virtuele netwerken](key-vault-network-security.md).

Zie voor een aan de slag-zelfstudie voor beheerders, [wat is Azure Key Vault?](key-vault-overview.md).

Zie [Logboekregistratie van Azure Key Vault](key-vault-logging.md) voor meer informatie over de gebruiksregistratie voor Key Vault.

Zie voor meer informatie over het gebruik van sleutels en geheimen met Azure Key Vault [over sleutels en geheimen](https://msdn.microsoft.com/library/azure/dn903623.aspx).

Als u vragen over Key Vault hebt, gaat u naar de [forums](https://social.msdn.microsoft.com/forums/azure/home?forum=AzureKeyVault).
