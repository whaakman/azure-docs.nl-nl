---
title: Uw Azure Key Vault - Azure Key Vault beveiligen | Microsoft Docs
description: Toegangsmachtigingen voor Azure Key Vault sleutels en geheimen beheren. Bevat informatie over het model voor verificatie en autorisatie voor Key Vault en over het beveiligen van uw key vault.
services: key-vault
documentationcenter: ''
author: amitbapat
manager: mbaldwin
tags: azure-resource-manager
ms.assetid: e5b4e083-4a39-4410-8e3a-2832ad6db405
ms.service: key-vault
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 01/02/2019
ms.author: ambapat
ms.openlocfilehash: ff831d117edc708303be1d9665b2c97bc248f6c2
ms.sourcegitcommit: da69285e86d23c471838b5242d4bdca512e73853
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/03/2019
ms.locfileid: "53999758"
---
# <a name="secure-your-key-vault"></a>Uw Key Vault beveiligen

Azure Key Vault is een cloudservice die beveiligt uw versleutelingssleutels en geheimen (zoals certificaten, verbindingsreeksen en wachtwoorden). Omdat deze gegevens vertrouwelijk zijn en bedrijfskritiek, moet u toegang tot uw key vaults beveiligen, gemachtigde zodat alleen toepassingen en gebruikers. Dit artikel bevat een overzicht van het model voor Key Vault. Het wordt verificatie en autorisatie worden uitgelegd en wordt beschreven hoe u veilige toegang.

## <a name="overview"></a>Overzicht

Toegang tot een Key Vault wordt geregeld via twee afzonderlijke interfaces: de beheerlaag en de gegevenslaag. 
**Beheerlaag** omgaat met het beheren van de kluis, bijvoorbeeld - het maken van een kluis, het bijwerken van een kluis, een kluis verwijderen. 
**Gegevenslaag** deals met geheimen in een kluis, die is gemaakt, bijwerken, verwijderen en een geheim lezen in de kluis. Voor beide lagen zijn juiste verificatie en autorisatie vereist voordat een aanroeper (een gebruiker of een toepassing) toegang tot key vault kan krijgen. Verificatie wordt de identiteit van de aanroeper vastgesteld, terwijl autorisatie bepaalt de bewerkingen die de aanroeper mag uitvoeren.

Voor verificatie gebruiken zowel de beheerlaag als de gegevenslaag Azure Active Directory. Voor autorisatie gebruikt het beheervlak op rollen gebaseerd toegangsbeheer (RBAC), terwijl de gegevenslaag het toegangsbeleid van Key Vault gebruikt.

Hier volgt een kort overzicht van de onderwerpen die aan bod komen:

[Verificatie met behulp van Azure Active Directory](#authentication-using-azure-active-directory): in deze sectie wordt uitgelegd hoe een aanroeper verificatie uitvoert met Azure Active Directory voor toegang tot een Key Vault via de beheerlaag en de gegevenslaag. 

Beide lagen gebruiken voor verificatie, Azure Active Directory (Azure AD). Voor autorisatie gebruikt het beheervlak op rollen gebaseerd toegangsbeheer (RBAC), terwijl de gegevenslaag met toegangsbeleid van Key Vault.

## <a name="authenticate-by-using-azure-active-directory"></a>Verifiëren met behulp van Azure Active Directory

Wanneer u een key vault in een Azure-abonnement maakt, het automatisch gekoppeld aan Azure AD-tenant van het abonnement. Alle aanroepers moeten zijn geregistreerd in deze tenant en moeten worden geverifieerd voor toegang tot de key vault. Deze vereiste geldt voor zowel en toegang tot de beheerlaag. In beide gevallen wordt een toepassing toegang tot Key Vault op twee manieren:

* **voor gebruikers en toegang tot Apps**: Gebruikt met toepassingen die toegang Key Vault namens een gebruiker is aangemeld tot. Azure PowerShell en Azure portal zijn voorbeelden van dit type toegang. Er zijn twee manieren om toegang te verlenen aan gebruikers:

  - Toegang tot Key Vault vanuit elke toepassing.
  - Toegang tot Key Vault alleen wanneer ze een bepaalde toepassing (aangeduid als samengestelde identiteit) gebruiken.

* **App-lezentoegang**: Wordt gebruikt met toepassingen die worden uitgevoerd als de daemon-services of achtergrondtaken. De identiteit van de toepassing krijgt toegang tot de Key Vault.

In beide typen toepassingen, de aanvraag wordt geverifieerd met Azure AD met behulp van de [ondersteunde verificatiemethoden](../active-directory/develop/authentication-scenarios.md), en krijgt een token. De verificatiemethode die wordt gebruikt, is afhankelijk van het toepassingstype. Vervolgens wordt de toepassing dit token wordt gebruikt en verzendt een REST-API-aanvraag naar de Key Vault. Management vlak-aanvragen worden gerouteerd via een Azure Resource Manager-eindpunt. Bij het openen van het vlak van gegevens, wordt de toepassing communiceert rechtstreeks naar een eindpunt van Key Vault. Zie voor meer informatie de [gehele verificatiestroom](../active-directory/develop/v1-protocols-oauth-code.md). 

De naam van de resource waarvoor de toepassing worden aangevraagd dat een token is afhankelijk van welke vlak van de toepassing toegang heeft tot. Naam van de resource is een vlak beheereindpunt, of een eindpunt van het vlak van gegevens, afhankelijk van de Azure-omgeving. Zie de tabel verderop in dit artikel voor meer informatie.

Één enkel verificatiemechanisme voor verificatie van beide lagen heeft enkele voordelen:

* Organisaties kunnen toegang tot alle key vaults in hun organisatie centraal beheren.
* Als een gebruiker verlaat, verliest hij of zij direct de toegang tot alle key vaults in de organisatie.
* Organisaties kunnen verificatie aanpassen via de opties in Azure AD (bijvoorbeeld door multi-factor authentication voor extra beveiliging).

## <a name="the-management-plane-and-the-data-plane"></a>Het beheer en de gegevenslaag

Gebruik het beheervlak voor het beheren van Key Vault zelf. Dit omvat bewerkingen zoals het beheren van kenmerken en het beleid voor gegevenstoegang vlak instellen. Gebruik het gegevensvlak toevoegen, verwijderen, wijzigen en de sleutels, geheimen en certificaten die zijn opgeslagen in Key Vault gebruiken.

Toegang tot de beheerlaag en gegevenslaag beheerinterfaces via de verschillende eindpunten die worden vermeld in de volgende tabel. De tweede kolom van de tabel beschrijft de DNS-namen voor deze eindpunten in verschillende Azure-omgevingen. De derde kolom beschrijft de bewerkingen die u vanaf elke toegangslaag kunt uitvoeren. Elke toegangslaag heeft ook een eigen mechanisme voor toegangsbeheer. Toegangsbeheer voor Management vlak is ingesteld met behulp van Azure Resource Manager-rol gebaseerd toegangsbeheer (RBAC). Toegangsbeheer voor gegevens vlak is ingesteld met behulp van toegangsbeleid voor Key Vault.

| Toegangslaag | Eindpunten voor toegang | Bewerkingen | Mechanisme voor toegangsbeheer |
| --- | --- | --- | --- |
| Beheerlaag |**Wereldwijd:**<br> management.azure.com:443<br><br> **Azure China 21Vianet:**<br> management.chinacloudapi.cn:443<br><br> **Azure van de Amerikaanse overheid:**<br> management.usgovcloudapi.net:443<br><br> **Azure Duitsland:**<br> management.microsoftazure.de:443 |Key Vault maken/lezen/bijwerken/verwijderen <br> Toegangsbeleid voor Key Vault instellen<br>Tags instellen voor Key Vault |RBAC met Azure Resource Manager |
| Gegevenslaag |**Wereldwijd:**<br> &lt;kluisnaam&gt;.vault.azure.net:443<br><br> **Azure China 21Vianet:**<br> &lt;kluisnaam&gt;.vault.azure.cn:443<br><br> **Azure van de Amerikaanse overheid:**<br> &lt;kluisnaam&gt;.vault.usgovcloudapi.net:443<br><br> **Azure Duitsland:**<br> &lt;kluisnaam&gt;.vault.microsoftazure.de:443 |Voor sleutels: Ontsleutelen, versleutelen, sleutel uitpakken, sleutel inpakken, controleren, ondertekenen, ophalen, weergeven, bijwerken, maken, importeren, verwijderen, back-up terugzetten<br><br> Voor geheimen: Ophalen, weergeven, instellen en verwijderen |Toegangsbeleid voor Key Vault |

Beheerlaag en gegevenslaag toegang besturingselementen voor beheer van werken onafhankelijk van elkaar. Bijvoorbeeld, als u de toegang tot een toepassing wilt voor het gebruik van sleutels in een key vault te verlenen, moet alleen u voor het verlenen van toegang tot de gegevenslaag. U verleent toegang tot en met toegangsbeleid van Key Vault. Een gebruiker die moet worden gelezen Key Vault-eigenschappen en tags, maar geen toegang tot gegevens (sleutels, geheimen of certificaten), moet echter alleen toegang tot de beheerlaag. U kunt toegang verlenen door leestoegang toe te wijzen aan de gebruiker met RBAC.

## <a name="management-plane-access-control"></a>Toegangsbeheer voor de beheerlaag

De beheerlaag bestaat uit bewerkingen die invloed hebben op de key vault zelf, zoals:

- Maken of verwijderen van een key vault.
- Een lijst met kluizen ophalen in een abonnement.
- Bij het ophalen van Key Vault-eigenschappen (zoals SKU en tags).
- Instelling voor Key Vault-beleidsregels die gebruiker en de toepassing toegang tot sleutels en geheimen.

Het toegangsbeheer voor de beheerlaag gebruikt RBAC.  

### <a name="role-based-access-control-rbac"></a>Toegangsbeheer op basis van rollen (RBAC)

Elk Azure-abonnement heeft een Azure AD-instantie. U kunt toegang verlenen aan gebruikers, groepen en toepassingen in deze directory voor het beheren van resources in het Azure-abonnement met het Azure Resource Manager-implementatiemodel. Dit type toegangsbeheer wordt aangeduid als RBAC. Voor het beheren van deze toegang kunt u [Azure Portal](https://portal.azure.com/), de [Azure CLI-tools](../cli-install-nodejs.md), [PowerShell](/powershell/azureps-cmdlets-docs) of de [Azure Resource Manager REST-API's](https://msdn.microsoft.com/library/azure/dn906885.aspx) gebruiken.

U een key vault in een resourcegroep maken en beheren van toegang tot de beheerlaag met behulp van Azure AD. U kunt bijvoorbeeld gebruikers of een groep de mogelijkheid voor het beheren van sleutelkluizen in een resourcegroep verlenen.

U kunt toegang verlenen aan gebruikers, groepen en toepassingen binnen een bepaald bereik door de juiste RBAC-rollen toewijzen. Bijvoorbeeld, om toegang te verlenen aan een gebruiker voor het beheren van sleutelkluizen, toewijzen u een vooraf gedefinieerde Key Vault-Inzender rol aan deze gebruiker, op een bepaald bereik. Het bereik zou in dit geval zijn een abonnement, een resourcegroep of een specifieke key vault. Een rol die is toegewezen op het abonnementsniveau van het is van toepassing op alle resourcegroepen en resources in dat abonnement. Een rol die is toegewezen op het niveau van de resource is van toepassing op alle resources in die resourcegroep. Een rol die is toegewezen aan een specifieke resource, geldt voor die bron. Er zijn verschillende vooraf gedefinieerde rollen (Zie [RBAC: Ingebouwde rollen](../role-based-access-control/built-in-roles.md)). Als een vooraf gedefinieerde rollen niet aansluiten bij uw behoeften, kunt u uw eigen rollen definiëren.

> [!IMPORTANT]
> Houd er rekening mee dat als een gebruiker heeft Inzender machtigingen voor een beheerlaag voor Key Vault, ze kan zichzelf verlenen toegang tot de gegevenslaag door toegangsbeleid voor Key Vault instellen. Daarom moet u nauw bepalen wie Inzender-toegang tot uw key vaults heeft. Zorg ervoor dat alleen gemachtigde personen kunnen openen en beheren van uw sleutelkluizen, sleutels, geheimen en certificaten.
>

## <a name="data-plane-access-control"></a>Toegangsbeheer voor de gegevenslaag

Key Vault-bewerkingen voor het vlak van gegevens van toepassing op opgeslagen objecten, zoals sleutels, geheimen en certificaten. Belangrijke bewerkingen zijn onder meer maken, importeren, bijwerken, lijst, back-up en herstellen van sleutels. Cryptografische bewerkingen opnemen ondertekenen, controleren, versleutelen, ontsleutelen, inpakken, uitpakken, labels instellen en andere kenmerken voor sleutels instellen. Op deze manier bewerkingen voor geheimen zijn get, set, lijst verwijderen.

U verleent toegang tot de gegevenslaag door het instellen van toegangsbeleid voor een key vault. Een gebruiker, groep of toepassing moet Inzender-rechten voor het beheervlak voor een key vault om het instellen van toegangsbeleid voor die key vault te kunnen hebben. U kunt een gebruiker, groep of toegang tot toepassingen voor het uitvoeren van specifieke bewerkingen voor sleutels of geheimen in een key vault verlenen. Key Vault ondersteunt maximaal 1024 vermeldingen van het toegangsbeleid voor key vault. Als u wilt verlenen toegang tot de gegevenslaag om meerdere gebruikers, een Azure AD-beveiligingsgroep maken en gebruikers toevoegen aan die groep.

### <a name="key-vault-access-policies"></a>Toegangsbeleid voor Key Vault

Toegangsbeleid voor Key Vault toekennen machtigingen voor sleutels, geheimen en certificaten afzonderlijk. U kunt bijvoorbeeld een gebruikerstoegang tot sleutels, en er zijn geen machtigingen voor geheimen geven. Machtigingen voor toegang tot sleutels, geheimen of certificaten zijn op het kluisniveau van de. Toegangsbeleid voor Key Vault biedt geen ondersteuning voor gedetailleerde, op objectniveau machtigingen, zoals een specifieke sleutel, het geheim of het certificaat. U kunt de [Azure-portal](https://portal.azure.com/), wordt de [Azure CLI-hulpprogramma's](../cli-install-nodejs.md), [PowerShell](/powershell/azureps-cmdlets-docs), of de [Key Vault Management REST-API's](https://msdn.microsoft.com/library/azure/mt620024.aspx) toegangsbeleid instellen voor een key vault.

> [!IMPORTANT]
> Key Vault-toegangsbeleid toepassen op het kluisniveau van de. Wanneer een gebruiker bijvoorbeeld is gemachtigd voor het maken en verwijderen van sleutels, kan hij deze bewerkingen uitvoeren op alle sleutels in die Key Vault.

Naast het gebruik van beleidsregels voor toegang, kunt u ook toegang tot de gegevenslaag beperken met behulp van [Virtual network-service-eindpunten voor Azure Key Vault](key-vault-overview-vnet-service-endpoints.md). U configureert [Firewalls en virtuele netwerkregels](key-vault-network-security.md) voor een extra beveiligingslaag.

## <a name="example"></a>Voorbeeld

Stel dat u bij het ontwikkelen van een toepassing die gebruikmaakt van een certificaat voor SSL, Azure Storage voor het opslaan van gegevens en een RSA 2048-bits sleutel voor tekenbewerkingen. Stel dat deze toepassing wordt uitgevoerd in een virtuele machine van Azure (of een virtuele-machineschaalset). U kunt een key vault gebruiken voor het opslaan van alle geheimen van de toepassing, en opslaan van de bootstrap-certificaat gebruikt door de toepassing om te verifiëren met Azure AD.

Hier volgt een samenvatting van de soorten sleutels en geheimen opgeslagen:

* **Een SSL-certificaat**: Wordt gebruikt voor SSL.
* **Opslagsleutel**: Toegang krijgen tot het Opslagaccount dat wordt gebruikt.
* **RSA 2048-bits sleutel**: Gebruikt voor tekenbewerkingen.
* **Bootstrap-certificaat**: Gebruikt voor verificatie met Azure AD. Nadat er toegang is verleend, kunt u de opslagsleutel ophalen en de RSA-sleutel gebruiken voor het ondertekenen.

Nu we voldoen aan de mensen die zijn beheren, implementeren en controleren van deze toepassing. In dit voorbeeld gebruiken we drie rollen.

* **Beveiligingsteam**: Normaal gesproken IT-afdeling van het kantoor van de CSO (Chief Security Officer) of iets vergelijkbaars. Dit team is verantwoordelijk voor de juiste beveiliging van geheimen. Voorbeelden zijn SSL-certificaten, RSA-sleutels gebruikt voor het ondertekenen, tekenreeksen voor databaseverbindingen, en opslagaccountsleutels.
* **Ontwikkelaars/operators**: Mensen die de toepassing ontwikkelen en implementeren in Azure. Normaal gesproken ze geen deel uit van het beveiligingsteam bent en kan daarom ze al dan niet mogen hebben toegang tot gevoelige gegevens, zoals SSL-certificaten en RSA-sleutels. Alleen de toepassing die ze implementeren moet toegang tot deze objecten hebben.
* **Auditors**: Meestal een andere set personen, geïsoleerd van ontwikkelaars en algemene IT-personeel. Hun verantwoordelijkheid is om te controleren van gebruik en onderhoud van certificaten, sleutels en geheimen te voldoen aan de beveiligingsstandaarden. 

Er is nog een rol die zich buiten het bereik van deze toepassing, maar de relevante hier te vermelden. Deze rol is het abonnement (of resourcegroep) beheerder. De abonnementbeheerder stelt u initiële toegangsmachtigingen voor het beveiligingsteam. Beheerder van het abonnement verleent toegang tot het beveiligingsteam, met behulp van een resourcegroep die de resources die vereist voor deze toepassing bevat.

We gaan nu kijken welke acties elke rol uitvoert voor deze toepassing.

* **Beveiligingsteam**
  * Hiermee maakt u sleutelkluizen.
  * Hiermee schakelt u logboekregistratie van Key Vault.
  * Sleutels/geheimen toevoegen
  * Hiermee maakt u back-up van de sleutels voor herstel na noodgevallen.
  * Hiermee stelt u toegangsbeleid voor Key Vault om machtigingen te verlenen aan gebruikers en toepassingen specifieke bewerkingen uit te voeren.
  * Sleutels/geheimen totaliseert periodiek.
* **Ontwikkelaars/operators**
  * Ophalen van verwijzingen naar bootstrap en de SSL-certificaten (vingerafdrukken), de opslagsleutel (geheime URI) en de handtekeningsleutel (sleutel-URI) van de security-team.
  * Ontwikkel en implementeer toepassingen die programmatisch toegang heeft tot sleutels en geheimen.
* **Auditors**
  * Gebruikslogboeken bekijken om te bevestigen van de juiste sleutel en-geheim voor gebruik en de naleving van beveiligingsnormen voor gegevens.

Nu we gaan nu kijken welke machtigingen zijn vereist voor elke rol en de toepassing, voor het uitvoeren van hun toegewezen taken. 

| Gebruikersrol | Machtigingen voor de beheerlaag | Machtigingen voor de gegevenslaag |
| --- | --- | --- |
| Beveiligingsteam |Inzender voor Key Vault |Sleutels: back-ups maken, verwijderen, ophalen, importeren, sorteren, herstellen <br> Geheimen: alle |
| Ontwikkelaars/Operators |Key Vault implementeren machtiging, zodat de virtuele machines die ze implementeren geheimen kunnen ophalen uit de key vault. |Geen |
| Auditors |Geen |Sleutels: weergeven<br>Geheimen: weergeven |
| Toepassing |Geen |Sleutels: ondertekenen<br>Geheimen: ophalen |

> [!NOTE]
> Auditors moeten een lijst met machtigingen voor sleutels en geheimen, zodat ze de kenmerken voor sleutels en geheimen die niet worden weergegeven in de logboeken kunnen inspecteren. Deze kenmerken bevatten labels, activering en vervaldatums.
> 
> 

Naast de machtigingen die Key Vault moeten alle drie de rollen ook toegang tot andere resources. Bijvoorbeeld, als u virtuele machines (of de functie Web Apps van Azure App Service) implementeren, moeten ontwikkelaars en operators ook Inzender-toegang tot de resourcetypen. Auditors hebben leestoegang nodig tot het opslagaccount waar de Key Vault-logboeken worden opgeslagen.

Omdat de focus van dit artikel is de beveiliging van toegang tot uw key vault, illustreren we alleen concepten met betrekking tot dit onderwerp. Informatie met betrekking tot implementatie van certificaten en toegang tot sleutels en geheimen programmatisch worden elders beschreven. Bijvoorbeeld:

- Implementatie van certificaten die zijn opgeslagen in Key Vault voor VM's wordt beschreven in [certificaten implementeren op virtuele machines uit de Sleutelkluis door de klant beheerde](https://blogs.technet.microsoft.com/kv/2016/09/14/updated-deploy-certificates-to-vms-from-customer-managed-key-vault/) (blogbericht).
- De [voorbeelden voor Azure Key Vault-client downloaden](https://www.microsoft.com/download/details.aspx?id=45343) laat zien hoe u een bootstrap-certificaat te verifiëren met Azure AD voor toegang tot een key vault.

U kunt de meeste van de machtigingen voor toegang verlenen met behulp van de Azure-portal. Gedetailleerde machtigingen wilt geven, moet u mogelijk Azure PowerShell of de CLI gebruiken om het gewenste resultaat te verkrijgen. 

De volgende PowerShell-fragmenten gaan uit van het volgende:

* De Azure AD-beheerder heeft beveiligingsgroepen gemaakt die staan voor de drie functies (de Contoso Security Team, Contoso App Devops en Contoso App Auditors). De beheerder heeft ook gebruikers toegevoegd aan de groepen waartoe ze behoren.
* **ContosoAppRG** is de resourcegroep waarin alle resources zich bevinden. **contosologstorage** is waar de logboeken worden opgeslagen. 
* De key vault **ContosoKeyVault**, en het opslagaccount dat wordt gebruikt voor Sleutelkluis-logboeken **contosologstorage**, moeten zich in dezelfde Azure-locatie.

Eerst de abonnementbeheerder wijst `key vault Contributor` en `User Access Administrator` rollen aan het beveiligingsteam. Deze rollen kunnen het beveiligingsteam voor het beheren van toegang tot andere resources en beheren van sleutelkluizen, in de resourcegroep ContosoAppRG.

```PowerShell
New-AzureRmRoleAssignment -ObjectId (Get-AzureRmADGroup -SearchString 'Contoso Security Team')[0].Id -RoleDefinitionName "key vault Contributor" -ResourceGroupName ContosoAppRG
New-AzureRmRoleAssignment -ObjectId (Get-AzureRmADGroup -SearchString 'Contoso Security Team')[0].Id -RoleDefinitionName "User Access Administrator" -ResourceGroupName ContosoAppRG
```

Het volgende script toont hoe het beveiligingsteam een key vault maken en instellen van logboekregistratie en toegangsmachtigingen. Zie voor meer informatie over Key Vault-beleid toegangsmachtigingen [over Azure Key Vault sleutels, geheimen en certificaten](about-keys-secrets-and-certificates.md).

```PowerShell
# Create key vault and enable logging
$sa = Get-AzureRmStorageAccount -ResourceGroup ContosoAppRG -Name contosologstorage
$kv = New-AzureRmKeyVault -Name ContosoKeyVault -ResourceGroup ContosoAppRG -SKU premium -Location 'westus' -EnabledForDeployment
Set-AzureRmDiagnosticSetting -ResourceId $kv.ResourceId -StorageAccountId $sa.Id -Enabled $true -Categories AuditEvent

# Data plane permissions for Security team
Set-AzureRmKeyVaultAccessPolicy -VaultName ContosoKeyVault -ObjectId (Get-AzureRmADGroup -SearchString 'Contoso Security Team')[0].Id -PermissionsToKeys backup,create,delete,get,import,list,restore -PermissionsToSecrets get,list,set,delete,backup,restore,recover,purge

# Management plane permissions for Dev/ops
# Create a new role from an existing role
$devopsrole = Get-AzureRmRoleDefinition -Name "Virtual Machine Contributor"
$devopsrole.Id = $null
$devopsrole.Name = "Contoso App Devops"
$devopsrole.Description = "Can deploy VMs that need secrets from key vault"
$devopsrole.AssignableScopes = @("/subscriptions/<SUBSCRIPTION-GUID>")

# Add permission for dev/ops so they can deploy VMs that have secrets deployed from key vaults
$devopsrole.Actions.Add("Microsoft.KeyVault/vaults/deploy/action")
New-AzureRmRoleDefinition -Role $devopsrole

# Assign this newly defined role to Dev ops security group
New-AzureRmRoleAssignment -ObjectId (Get-AzureRmADGroup -SearchString 'Contoso App Devops')[0].Id -RoleDefinitionName "Contoso App Devops" -ResourceGroupName ContosoAppRG

# Data plane permissions for Auditors
Set-AzureRmKeyVaultAccessPolicy -VaultName ContosoKeyVault -ObjectId (Get-AzureRmADGroup -SearchString 'Contoso App Auditors')[0].Id -PermissionsToKeys list -PermissionsToSecrets list
```

De gedefinieerde aangepaste rol kan alleen worden toegewezen aan het abonnement waarin de `ContosoAppRG` resourcegroep wordt gemaakt. Als dezelfde aangepaste rollen wordt gebruikt voor andere projecten in andere abonnementen, hebben het bereik meer abonnementen die worden toegevoegd.

De toewijzing van de aangepaste rol voor de machtiging ' implementeren/actie' voor ontwikkelaars en operators is afgestemd op de resourcegroep. Hiermee wordt alleen virtuele machines die zijn gemaakt in de resourcegroep `ContosoAppRG` toegang hebben tot de geheimen (SSL-certificaat en bootstrap-certificaat). Virtuele machines die zijn gemaakt in een andere resourcegroep door een ontwikkelaars en teamlid operators geen toegang heeft tot deze geheime gegevens, zelfs als ze de geheime URI's hebben.

Dit voorbeeld toont een eenvoudig scenario. Praktijk zijn scenario's kunnen complexe, en kunt u machtigingen voor uw key vault op basis van uw behoeften aanpassen. In ons voorbeeld veronderstellen we dat het beveiligingsteam de sleutel en geheime verwijzingen (URI's en vingerafdrukken) die ontwikkelaars en operators moeten verwijzen naar in hun toepassingen biedt. Ontwikkelaars en operators nodig geen voor alle toegang tot de gegevenslaag. In dit voorbeeld is gericht op het beveiligen van uw key vault. U geeft belangrijk voor het beveiligen van [uw virtuele machines](https://azure.microsoft.com/services/virtual-machines/security/), [opslagaccounts](../storage/common/storage-security-guide.md), en andere Azure-resources.

> [!NOTE]
> In dit voorbeeld laat zien hoe u toegang tot Key Vault wordt vergrendeld in de productieomgeving. De ontwikkelaars moeten hebben hun eigen abonnement of resourcegroep waar ze hebben volledige machtigingen voor het beheren van hun kluizen, VM's en storage-account waar ze de toepassing ontwikkelen.

We raden u aan dat u beveiligde toegang tot uw key vault door verder [Key Vault-firewalls en virtuele netwerken configureren](key-vault-network-security.md).

## <a name="resources"></a>Resources

* [Azure Active Directory-rollen gebaseerd toegangsbeheer](../role-based-access-control/role-assignments-portal.md)
  
* [RBAC: Ingebouwde rollen](../role-based-access-control/built-in-roles.md)
  
* [Resource Manager-implementatie en klassieke implementatie begrijpen](../azure-resource-manager/resource-manager-deployment-model.md)
  
* [Op rollen gebaseerd toegangsbeheer met Azure PowerShell beheren](../role-based-access-control/role-assignments-powershell.md)
  
* [Op rollen gebaseerd toegangsbeheer beheren met de REST-API](../role-based-access-control/role-assignments-rest.md)
  
* [Op rollen gebaseerd toegangsbeheer voor Microsoft Azure vanuit Ignite](https://channel9.msdn.com/events/Ignite/2015/BRK2707)
  
  Deze 2015 Microsoft Ignite conferentie video bespreekt toegang tot beheer en rapportage in Azure. Het behandelt ook de aanbevolen procedures voor het beveiligen van toegang tot Azure-abonnementen met behulp van Azure AD.
* [Toegang verlenen aan webtoepassingen die gebruikmaken van OAuth 2.0 en Azure AD](../active-directory/develop/v1-protocols-oauth-code.md)
  
* [Key Vault-beheer REST-API 's](https://msdn.microsoft.com/library/azure/mt620024.aspx)
  
  In dit document dient als referentie voor de REST-API's voor het beheren van uw key vault via een programma, inclusief het instellen van toegangsbeleid voor Key Vault.
* [Key Vault REST-API 's](https://msdn.microsoft.com/library/azure/dn903609.aspx)
  
* [Toegangsbeheer voor sleutels](https://msdn.microsoft.com/library/azure/dn903623.aspx#BKMK_KeyAccessControl)
  
* [Toegangsbeheer voor geheimen](https://msdn.microsoft.com/library/azure/dn903623.aspx#BKMK_SecretAccessControl)
  
* [Stel](https://docs.microsoft.com/powershell/module/azurerm.keyvault/Set-AzureRmKeyVaultAccessPolicy) en [verwijderen](https://docs.microsoft.com/powershell/module/azurerm.keyvault/Remove-AzureRmKeyVaultAccessPolicy) toegangsbeleid van Key Vault met behulp van PowerShell
  
## <a name="next-steps"></a>Volgende stappen

[Key Vault-firewalls en virtuele netwerken configureren](key-vault-network-security.md)

Zie [Aan de slag met Azure Key Vault](key-vault-get-started.md) voor een inleidende zelfstudie voor beheerders.

Zie [Logboekregistratie van Azure Key Vault](key-vault-logging.md) voor meer informatie over de gebruiksregistratie voor Key Vault.

Zie voor meer informatie over het gebruik van sleutels en geheimen met Azure Key Vault [over sleutels en geheimen](https://msdn.microsoft.com/library/azure/dn903623.aspx).

Als u vragen over Key Vault hebt, gaat u naar de [forums](https://social.msdn.microsoft.com/forums/azure/home?forum=AzureKeyVault).

