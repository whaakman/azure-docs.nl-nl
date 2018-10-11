---
title: Uw Azure Key Vault beveiligen | Microsoft Docs
description: Toegangsmachtigingen voor key vault voor het beheren van Azure Key Vault sleutels en geheimen beheren. Model voor verificatie en autorisatie voor key vault en over het beveiligen van uw key vault.
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
ms.date: 10/09/2018
ms.author: ambapat
ms.openlocfilehash: 58307b25c03202fea63be25136c5a9c5a2d32473
ms.sourcegitcommit: 4b1083fa9c78cd03633f11abb7a69fdbc740afd1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/10/2018
ms.locfileid: "49079035"
---
# <a name="secure-your-key-vault"></a>Uw Key Vault beveiligen
Azure Key Vault is een cloudservice die beveiligt uw versleutelingssleutels en geheimen (zoals certificaten, verbindingsreeksen, wachtwoorden). Aangezien deze gegevens vertrouwelijk zijn en bedrijfskritiek, toegang tot uw key vaults moet worden beveiligd, gemachtigde zodat alleen toepassingen en gebruikers om toegang te krijgen. 

Dit artikel bevat een overzicht van het model voor sleutelkluis. Het wordt verificatie en autorisatie worden uitgelegd en wordt beschreven hoe u veilig toegang tot key vault.

## <a name="overview"></a>Overzicht
Toegang tot een Key Vault wordt geregeld via twee afzonderlijke interfaces: de beheerlaag en de gegevenslaag. Voor beide lagen zijn juiste verificatie en autorisatie vereist voordat een aanroeper (een gebruiker of een toepassing) toegang tot key vault kan krijgen. Verificatie wordt de identiteit van de aanroeper vastgesteld, terwijl autorisatie bepaalt de bewerkingen die de aanroeper mag uitvoeren.

Voor verificatie gebruiken zowel de beheerlaag als de gegevenslaag Azure Active Directory. Voor autorisatie gebruikt het beheervlak op rollen gebaseerd toegangsbeheer (RBAC), terwijl de gegevenslaag het toegangsbeleid van Key Vault gebruikt.

Hier volgt een kort overzicht van de onderwerpen die aan bod komen:

[Verificatie met behulp van Azure Active Directory](#authentication-using-azure-active-directory): in deze sectie wordt uitgelegd hoe een aanroeper verificatie uitvoert met Azure Active Directory voor toegang tot een Key Vault via de beheerlaag en de gegevenslaag. 

[Beheerlaag en gegevenslaag](#management-plane-and-data-plane): de beheerlaag en de gegevenslaag zijn twee toegangslagen die worden gebruikt voor toegang tot uw Key Vault. Elke toegangslaag ondersteunt specifieke bewerkingen. In deze sectie worden de toegangseindpunten, ondersteunde bewerkingen en toegangsbeheermethodes beschreven die beide lagen gebruiken. 

[Toegangsbeheer voor de beheerlaag](#management-plane-access-control): in deze sectie laten we zien hoe u toegang geeft aan bewerkingen op de beheerlaag met op rollen gebaseerd toegangsbeheer.

[Toegangsbeheer voor de gegevenslaag](#data-plane-access-control): in deze sectie wordt beschreven hoe u het toegangsbeleid van Key Vault gebruikt om de toegang tot de gegevenslaag te beheren.

[Voorbeeld](#example) -in dit voorbeeld wordt beschreven hoe u voor het instellen van toegangsbeheer voor uw key vault om toe te staan drie verschillende teams (beveiligingsteam, ontwikkelaars/operators en auditors) specifieke taken uitvoeren om te ontwikkelen, beheren en bewaken van een toepassing in Azure.

## <a name="authentication-using-azure-active-directory"></a>Verificatie met Azure Active Directory
Wanneer u een key vault in een Azure-abonnement maakt, het automatisch gekoppeld aan Azure Active Directory-tenant van het abonnement. Alle aanroepers (gebruikers en toepassingen) moeten worden geregistreerd in deze tenant en moeten worden geverifieerd voor toegang tot de key vault. Deze vereiste geldt voor zowel en toegang tot de beheerlaag. In beide gevallen kan een toepassing op twee manieren toegang krijgen tot de Key Vault:

* **voor gebruikers en toegang tot Apps** : wordt gebruikt met toepassingen die toegang hebben tot key vault gebruiken namens een gebruiker is aangemeld. Azure PowerShell en Azure portal zijn voorbeelden van dit type toegang. Er zijn twee manieren om toegang te verlenen aan gebruikers: 
- Toegang verlenen aan gebruikers zodat ze krijgen key vault vanuit elke toepassing tot toegang.
- Een gebruikerstoegang tot key vault geven alleen wanneer ze een bepaalde toepassing (aangeduid als samengestelde identiteit) gebruiken.

* **App-lezentoegang** : wordt gebruikt met toepassingen die worden uitgevoerd als de daemon-services of achtergrondtaken. De identiteit van de toepassing krijgt toegang tot de Key Vault.

Voor beide typen toepassingen wordt de aanvraag geverifieerd met Azure Active Directory met een van de [ondersteunde verificatiemethoden](../active-directory/develop/authentication-scenarios.md) en krijgt de toepassing een token. De gebruikte verificatiemethode is afhankelijk van het type toepassing. Vervolgens gebruikt de toepassing dit token en stuurt deze een REST API-verzoek naar Key Vault. Management vlak-aanvragen worden gerouteerd via een Azure Resource Manager-eindpunt. Bij toegang tot de gegevenslaag gebruiken de toepassingen een eindpunt van Key Vault. Raadpleeg meer informatie over de [gehele verificatiestroom](../active-directory/develop/v1-protocols-oauth-code.md). 

De resourcenaam waarvoor de toepassing een token aanvraagt, is afhankelijk van de laag waartoe de toepassing toegang probeert te krijgen: de beheerlaag of de gegevenslaag. De resourcenaam is daarom het eindpunt van de beheerlaag of gegevenslaag en wordt beschreven in de tabel verderop in dit artikel. Deze is afhankelijk van de Azure-omgeving.

Het gebruik van één enkel verificatiemechanisme voor de beheer- en gegevenslaag heeft zijn eigen voordelen:

* Organisaties kunnen de toegang tot alle Key Vaults in hun organisatie centraal beheren
* Als een gebruiker het bedrijf verlaat, verliest hij onmiddellijk de toegang tot alle Key Vaults in de organisatie
* Organisaties kunnen de verificatie aanpassen via de opties in Azure Active Directory (bijvoorbeeld door Multi-Factor Authentication in te schakelen voor extra beveiliging)

## <a name="management-plane-and-data-plane"></a>De beheer- en gegevenslaag
Azure Key Vault is een Azure-service beschikbaar via de Azure Resource Manager-implementatiemodel. Als u een sleutelkluis maakt, krijgt u een virtuele container voor het opslaan van gevoelige objecten, zoals sleutels, geheimen en certificaten. Specifieke bewerkingen die worden uitgevoerd op een key vault met behulp van de beheerlaag en gegevenslaag beheerinterfaces. De beheerlaag wordt gebruikt voor het beheren van key vault zelf. Dit omvat bewerkingen zoals het beheren van kenmerken en het beleid voor gegevenstoegang vlak instellen. De back-up van de gegevenslaag wordt gebruikt voor het toevoegen, verwijderen, wijzigen en de sleutels, geheimen en certificaten die zijn opgeslagen in key vault gebruiken.

De beheerlaag en gegevenslaag beheerinterfaces toegankelijk zijn via de verschillende eindpunten die hieronder worden vermeld. De tweede kolom beschrijft de DNS-namen voor deze eindpunten in verschillende Azure-omgevingen. De derde kolom beschrijft de bewerkingen die u vanaf elke toegangslaag kunt uitvoeren. Elke toegangslaag heeft ook een eigen mechanisme voor toegangsbeheer. Toegangsbeheer voor Management vlak is ingesteld met behulp van toegangsbeheer toegangsbeheer (RBAC). Toegangsbeheer voor gegevens vlak is ingesteld met behulp van toegangsbeleid voor key vault.

| Toegangslaag | Eindpunten voor toegang | Bewerkingen | Mechanisme voor toegangsbeheer |
| --- | --- | --- | --- |
| Beheerlaag |**Wereldwijd:**<br> management.azure.com:443<br><br> **Azure China 21Vianet:**<br> management.chinacloudapi.cn:443<br><br> **Azure van de Amerikaanse overheid:**<br> management.usgovcloudapi.net:443<br><br> **Azure Duitsland:**<br> management.microsoftazure.de:443 |Key Vault maken/lezen/bijwerken/verwijderen <br> Toegangsbeleid voor Key Vault instellen<br>Tags instellen voor Key Vault |Toegangsbeheer op basis van rollen (RBAC) in Azure Resource Manager |
| Gegevenslaag |**Wereldwijd:**<br> &lt;kluisnaam&gt;.vault.azure.net:443<br><br> **Azure China 21Vianet:**<br> &lt;kluisnaam&gt;.vault.azure.cn:443<br><br> **Azure van de Amerikaanse overheid:**<br> &lt;kluisnaam&gt;.vault.usgovcloudapi.net:443<br><br> **Azure Duitsland:**<br> &lt;kluisnaam&gt;.vault.microsoftazure.de:443 |Voor sleutels: Ontsleutelen, Versleutelen, Sleutel uitpakken, Sleutel inpakken, Controleren, Ondertekenen, Ophalen, Sorteren, Bijwerken, Maken, Importeren, Verwijderen, Back-up maken, Herstellen<br><br> Voor geheimen: Ophalen, Sorteren, Instellen, Verwijderen |Toegangsbeleid van Key Vault |

Beheerlaag en gegevenslaag toegang besturingselementen voor beheer van werken onafhankelijk van elkaar. Bijvoorbeeld, als u de toegang tot een toepassing wilt voor het gebruik van sleutels in een key vault te verlenen, moet alleen u voor het verlenen van toegang tot de gegevenslaag. Toegang verleend tot en met toegangsbeleid van key vault. Een gebruiker die moet worden gelezen kluis eigenschappen en tags, maar geen toegang tot gegevens (sleutels, geheimen of certificaten), moet echter alleen toegang tot de gegevenslaag van besturingselement. Toegang is verleend door toe te wijzen 'lezen' toegang tot de gebruiker, met RBAC.

## <a name="management-plane-access-control"></a>Toegangsbeheer voor de beheerlaag
De beheerlaag bestaat uit bewerkingen die invloed hebben op de key vault zelf, zoals:

- Maken of verwijderen van een key vault.
- Een lijst met kluizen ophalen in een abonnement.
- Bij het ophalen van key vault-eigenschappen (zoals SKU, tags).
- Instellen van toegangsbeleid voor sleutelkluizen die beheer-gebruikers en toepassingen toegang hebben tot sleutels en geheimen.

Het toegangsbeheer voor de beheerlaag gebruikt RBAC. Bekijk de volledige lijst van key vault-bewerkingen die kunnen worden uitgevoerd via de beheerlaag, in de tabel in de voorgaande sectie. 

### <a name="role-based-access-control-rbac"></a>Toegangsbeheer op basis van rollen (RBAC)
Elk Azure-abonnement heeft een Azure Active Directory. Gebruikers, groepen en toepassingen in deze directory kunnen worden gemachtigd om resources te beheren in het Azure-abonnement dat gebruikmaakt van het Azure Resource Manager-implementatiemodel. Dit type toegangsbeheer wordt toegangsbeheer op basis van rollen (RBAC) genoemd. Voor het beheren van deze toegang kunt u [Azure Portal](https://portal.azure.com/), de [Azure CLI-tools](../cli-install-nodejs.md), [PowerShell](/powershell/azureps-cmdlets-docs) of de [Azure Resource Manager REST-API's](https://msdn.microsoft.com/library/azure/dn906885.aspx) gebruiken.

U maakt een key vault in een resource en beheert de toegang tot de beheerlaag met behulp van Azure Active Directory. U kunt bijvoorbeeld gebruikers of een groep de mogelijkheid voor het beheren van sleutelkluizen in een resourcegroep verlenen.

U kunt toegang verlenen aan gebruikers, groepen en toepassingen binnen een bepaald bereik door de juiste RBAC-rollen toewijzen. Bijvoorbeeld, om toegang te verlenen aan een gebruiker voor het beheren van sleutelkluizen, toewijzen u een vooraf gedefinieerde 'Key Vault-Inzender' rol aan deze gebruiker, op een bepaald bereik. Het bereik zou in dit geval zijn een abonnement, een resourcegroep of een specifieke key vault. Een rol die is toegewezen op abonnementsniveau, is van toepassing op alle resourcegroepen en resources in dat abonnement. Een rol die is toegewezen op het niveau van de resourcegroep, is van toepassing op alle resources in die resourcegroep. Een rol die is toegewezen aan een specifieke resource, geldt voor die bron. Er zijn verschillende vooraf gedefinieerde rollen (Zie [RBAC: ingebouwde rollen](../role-based-access-control/built-in-roles.md)). Als een vooraf gedefinieerde rollen niet aansluiten bij uw behoeften kunt u uw eigen rollen definiëren.

> [!IMPORTANT]
> Houd er rekening mee dat een gebruiker die inzendersmachtigingen (RBAC) heeft voor de beheerlaag van een Key Vault, zichzelf toegang kan geven tot de gegevenslaag. Dit kan hij doen door toegangsbeleid voor de Key Vault in te stellen en zo de toegang tot de gegevenslaag te beheren. Daarom is het aanbevolen streng te zijn bij het bepalen wie er inzenderstoegang heeft tot uw Key Vaults en er zo voor te zorgen dat alleen gemachtigde personen toegang hebben tot uw Key Vaults, sleutels, geheimen en certificaten, en deze kunnen beheren.
> 
> 

## <a name="data-plane-access-control"></a>Toegangsbeheer voor de gegevenslaag
Bewerkingen voor sleutelkluis gegevens vlak van toepassing op opgeslagen objecten, zoals sleutels, geheimen en certificaten. Belangrijke bewerkingen zijn onder meer maken, importeren, bijwerken, lijst, back-up en herstellen van sleutels. Cryptografische bewerkingen teken bevatten, controleren, versleutelen, ontsleutelen, inpakken, uitpakken, labels en andere kenmerken voor sleutels instellen. Op deze manier bewerkingen voor geheimen zijn get, set, lijst verwijderen.

U verleent toegang tot de gegevenslaag door toegangsbeleid voor een Key Vault in te stellen. Een gebruiker, groep of toepassing heeft inzendersrechten (RBAC) voor de beheerlaag van een Key Vault nodig om toegangsbeleid voor die Key Vault in te stellen. Een gebruiker, groep of toepassing kan worden gemachtigd om bepaalde bewerkingen voor sleutels of geheimen in een Key Vault uit te voeren. Key Vault ondersteunt tot 1024 toegangsbeleidsitems voor een sleutelkluis. Maak een Azure Active Directory-beveiligingsgroep en voeg gebruikers toe aan die groep om meerdere gebruikers in een Key Vault toegang te geven tot de gegevenslaag.

### <a name="key-vault-access-policies"></a>Toegangsbeleid van Key Vault
Toegangsbeleid van Key vault toekennen machtigingen voor sleutels, geheimen en certificaten afzonderlijk. U kunt bijvoorbeeld een gebruikerstoegang tot sleutels, en er zijn geen machtigingen voor geheimen geven. Machtigingen voor toegang tot sleutels, geheimen of certificaten zijn op het kluisniveau van de. Toegangsbeleid voor Key vault biedt geen ondersteuning voor gedetailleerde machtigingen op objectniveau, zoals een specifieke sleutel/geheim/certificaat. U kunt [Azure Portal](https://portal.azure.com/), de [Azure CLI-hulpprogramma's](../cli-install-nodejs.md), [PowerShell](/powershell/azureps-cmdlets-docs) of de [Key Vault-beheer REST-API's](https://msdn.microsoft.com/library/azure/mt620024.aspx) gebruiken om toegangsbeleid in te stellen voor Key Vault.

> [!IMPORTANT]
> Houd er rekening mee dat toegangsbeleid voor Key Vault wordt toegevoegd op het niveau van de Key Vault. Wanneer een gebruiker bijvoorbeeld is gemachtigd voor het maken en verwijderen van sleutels, kan hij deze bewerkingen uitvoeren op alle sleutels in die Key Vault.

Naast toegangsbeleid kan toegang tot de gegevenslaag ook worden beperkt met behulp van [Virtual Network-Service-eindpunten voor Azure Key Vault](key-vault-overview-vnet-service-endpoints.md) door [firewalls en regels voor virtuele netwerken](key-vault-network-security.md) te configureren voor een extra beveiligingslaag.

## <a name="example"></a>Voorbeeld
Stel dat u bij het ontwikkelen van een toepassing die gebruikmaakt van een certificaat voor SSL, Azure-opslag voor het opslaan van gegevens en een RSA 2048-bits sleutel voor tekenbewerkingen. Stel dat deze toepassing wordt uitgevoerd in een virtuele Machine van Azure (of een virtuele-Machineschaalset). U kunt een key vault gebruiken voor het opslaan van alle geheimen van de toepassing, en opslaan van de bootstrap-certificaat gebruikt door de toepassing voor verificatie met Azure Active Directory.

Hier volgt een samenvatting van de soorten sleutels en geheimen die zijn opgeslagen in een key vault:

* **Een SSL-certificaat** : wordt gebruikt voor SSL
* **Opslagsleutel** : wordt gebruikt voor toegang tot opslagaccount
* **RSA 2048-bits sleutel** : wordt gebruikt voor tekenbewerkingen
* **Bootstrap-certificaat** : wordt gebruikt om te verifiëren met Azure Active Directory. Wanneer toegang is verleend, kunt u de opslagsleutel ophalen en de RSA-sleutel gebruiken voor het ondertekenen.

Nu we voldoen aan de mensen die zijn beheren, implementeren en controleren van deze toepassing. In dit voorbeeld gebruiken we drie rollen.

* **Beveiligingsteam** -gewoonlijk IT-afdeling van het 'kantoor van de CSO (Chief Security Officer)' of gelijk zijn. Dit team is verantwoordelijk voor de juiste beveiliging van geheimen. Bijvoorbeeld, SSL-certificaten, RSA-sleutels gebruikt voor het ondertekenen, tekenreeksen voor databaseverbindingen, en opslagaccountsleutels.
* **Ontwikkelaars/operators** -mensen die de toepassing ontwikkelen en implementeren in Azure. Normaal gesproken ze geen deel uit van het beveiligingsteam bent en kan daarom ze al dan niet mogen hebben toegang tot gevoelige gegevens, zoals SSL-certificaten en RSA-sleutels. Alleen de toepassing die ze implementeren moet toegang tot deze objecten hebben.
* **Auditors** -meestal een andere set personen, geïsoleerd van ontwikkelaars en algemene IT-personeel. Hun verantwoordelijkheid is om te controleren van gebruik en onderhoud van certificaten, sleutels en geheimen te voldoen aan de beveiligingsstandaarden. 

Er is nog een rol die zich buiten het bereik van deze toepassing, maar de relevante hier te vermelden. Deze rol is het abonnement (of resourcegroep) beheerder. De abonnementbeheerder stelt u initiële toegangsmachtigingen voor het beveiligingsteam. Beheerder van het abonnement verleent toegang tot het beveiligingsteam, met behulp van een resourcegroep die de resources die vereist zijn deze toepassing bevat.

We gaan nu kijken welke acties elke rol uitvoert voor deze toepassing.

* **Beveiligingsteam**
  * Key Vaults maken
  * Logboekregistratie van Key Vault inschakelen
  * Sleutels/geheimen toevoegen
  * Back-ups van sleutels maken voor herstel na noodgevallen
  * Toegangsbeleid voor Key Vaults instellen om machtigingen te verlenen aan gebruikers en toepassingen om bepaalde bewerkingen uit te voeren
  * Sleutels/geheimen periodiek terugdraaien
* **Ontwikkelaars/operators**
  * Ophalen van verwijzingen naar bootstrap en SSL-certificaten (vingerafdrukken), opslagsleutel (geheime URI) en de handtekeningsleutel (sleutel-URI) van het beveiligingsteam
  * Toepassingen die programmatisch sleutels en geheimen gebruiken, ontwikkelen en implementeren
* **Auditors**
  * Gebruikslogboeken bekijken om te bevestigen dat sleutels/geheimen op de juiste manier en in overeenstemming met de beveiligingsnormen voor gegevens worden gebruikt

Nu we gaan nu kijken welke machtigingen zijn vereist voor elke rol en de toepassing, voor het uitvoeren van hun toegewezen taken. 

| Gebruikersrol | Machtigingen voor de beheerlaag | Machtigingen voor de gegevenslaag |
| --- | --- | --- |
| Beveiligingsteam |Inzender voor Key Vault |Sleutels: back-ups maken, verwijderen, ophalen, importeren, sorteren, herstellen <br> Geheimen: alle |
| Ontwikkelaars/Operators |Implementatiemachtigingen voor Key Vault, zodat de virtuele machines die ze implementeren, geheimen kunnen ophalen uit de Key Vault |Geen |
| Auditors |Geen |Sleutels: weergeven<br>Geheimen: weergeven |
| Toepassing |Geen |Sleutels: ondertekenen<br>Geheimen: ophalen |

> [!NOTE]
> Auditors hebben weergavemachtigingen voor sleutels en geheimen nodig, zodat ze de kenmerken voor sleutels en geheimen die niet worden weergegeven in de logboeken, kunnen bekijken. Het gaat hierbij bijvoorbeeld om tags en activatie- en vervaldatums.
> 
> 

Naast key vault-machtigingen moeten alle drie de rollen ook toegang tot andere resources. Ze moeten bijvoorbeeld VM’s kunnen implementeren (of web-apps enz.). Ontwikkelaars/Operators hebben ook Inzenderstoegang tot de resourcetypen nodig. Auditors moeten 'gelezen' toegang tot het opslagaccount waar de logboeken van key vault worden opgeslagen.

Omdat de focus van dit artikel is de beveiliging van toegang tot uw key vault, illustreren we alleen concepten met betrekking tot dit onderwerp. Informatie met betrekking tot de implementatie van certificaten, sleutels en geheimen via een programma openen en andere resources, worden elders beschreven. Bijvoorbeeld:

- Implementatie van certificaten die zijn opgeslagen in key vault voor VM's wordt beschreven in een [blogbericht: certificaten implementeren op virtuele machines uit de Sleutelkluis door de klant beheerde](https://blogs.technet.microsoft.com/kv/2016/09/14/updated-deploy-certificates-to-vms-from-customer-managed-key-vault/)
- De [voorbeelden voor Azure Key Vault-client downloaden](https://www.microsoft.com/download/details.aspx?id=45343) laat zien hoe u een bootstrap-certificaat te verifiëren met Azure AD voor toegang tot een key vault.

De meeste toegangsmachtigingen kunnen worden verleend met behulp van Azure portal. Gedetailleerde machtigingen wilt geven, moet u mogelijk Azure PowerShell of CLI gebruiken om het gewenste resultaat te verkrijgen. 

De volgende PowerShell-fragmenten gaan uit van het volgende:

* De Azure Active Directory-beheerder heeft beveiligingsgroepen gemaakt die staan voor de drie functies (de Contoso Security Team, Contoso App Devops en Contoso App Auditors). De beheerder heeft ook gebruikers toegevoegd aan de groepen waartoe ze behoren.
* **ContosoAppRG** is de resourcegroep waarin alle resources zich bevinden. **contosologstorage** is waar de logboeken worden opgeslagen. 
* Key Vault **ContosoKeyVault** en het opslagaccount **contosologstorage**, dat wordt gebruikt voor logboeken van Key Vault, moeten zich in dezelfde Azure-locatie bevinden

De abonnementbeheerder wijst allereerst de rollen Inzender voor Key Vault en Beheerder van gebruikerstoegang toe aan het beveiligingsteam. Deze rollen kunnen het beveiligingsteam toegang tot andere resources beheren en sleutelkluizen in de resourcegroep ContosoAppRG beheren.

```
New-AzureRmRoleAssignment -ObjectId (Get-AzureRmADGroup -SearchString 'Contoso Security Team')[0].Id -RoleDefinitionName "key vault Contributor" -ResourceGroupName ContosoAppRG
New-AzureRmRoleAssignment -ObjectId (Get-AzureRmADGroup -SearchString 'Contoso Security Team')[0].Id -RoleDefinitionName "User Access Administrator" -ResourceGroupName ContosoAppRG
```

Het volgende script toont hoe het beveiligingsteam een key vault maken en instellen van logboekregistratie en toegangsmachtigingen. Zie [over Azure Key Vault sleutels, geheimen en certificaten](about-keys-secrets-and-certificates.md) voor meer informatie over Key Vault-beleid toegangsmachtigingen.

```
# Create key vault and enable logging
$sa = Get-AzureRmStorageAccount -ResourceGroup ContosoAppRG -Name contosologstorage
$kv = New-AzureRmKeyVault -VaultName ContosoKeyVault -ResourceGroup ContosoAppRG -SKU premium -Location 'westus' -EnabledForDeployment
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

De gedefinieerde aangepaste rol kan alleen worden toegewezen aan het abonnement waarin de resourcegroep ContosoAppRG wordt gemaakt. Als dezelfde aangepaste rollen wordt gebruikt voor andere projecten in andere abonnementen, kan meer abonnementen die worden toegevoegd door het bereik hebben.

De toewijzing van de aangepaste rol voor de ontwikkelaars/operators 'implementeren/actie' machtiging is afgestemd op de resourcegroep. Hiermee worden alleen virtuele machines die zijn gemaakt in de resourcegroep 'ContosoAppRG toegang hebben tot de geheimen (SSL-certificaat en bootstrap-certificaat). Virtuele machines die zijn gemaakt in een andere resourcegroep door een teamlid ontwikkelaars/operators geen toegang heeft tot deze geheime gegevens, zelfs als ze de geheime URI's hebben.

Dit voorbeeld toont een eenvoudig scenario. Praktijk zijn scenario's mogelijk ingewikkelder en mogelijk moet u machtigingen voor uw key vault op basis van uw behoeften aanpassen. In ons voorbeeld veronderstellen we dat het beveiligingsteam de sleutel en geheime verwijzingen (URI's en vingerafdrukken), waarmee ontwikkelaars/operators moet verwijzen naar in hun toepassingen. Ontwikkelaars/operators hebben toegang tot de gegevenslaag niet nodig. In dit voorbeeld is gericht op het beveiligen van uw key vault. Vergelijkbare moet worden overwogen voor het beveiligen van [uw virtuele machines](https://azure.microsoft.com/services/virtual-machines/security/), [opslagaccounts](../storage/common/storage-security-guide.md), en andere Azure-resources.

> [!NOTE]
> Opmerking: Dit voorbeeld toont hoe toegang tot de Key Vault wordt vergrendeld tijdens de productie. De ontwikkelaars moeten hun eigen abonnement of resourcegroep hebben waarin ze volledige machtigingen hebben voor het beheren van hun kluizen, VM's en het opslagaccount waarin ze de toepassing ontwikkelen.

Het raadzaam beveiligde toegang tot uw key vault verder door [Key Vault-firewalls en virtuele netwerken configureren](key-vault-network-security.md).

## <a name="resources"></a>Resources
* [Toegangsbeheer op basis van rollen in Azure Active Directory](../role-based-access-control/role-assignments-portal.md)
  
  In dit artikel wordt het toegangsbeheer op basis van rollen in Azure Active Directory uitgelegd.
* [RBAC: ingebouwde rollen](../role-based-access-control/built-in-roles.md)
  
  In dit artikel worden alle beschikbare ingebouwde rollen in RBAC beschreven.
* [Resource Manager-implementatie en klassieke implementatie begrijpen](../azure-resource-manager/resource-manager-deployment-model.md)
  
  In dit artikel worden het Resource Manager-implementatiemodel en het klassieke implementatiemodel uitgelegd en worden de voordelen van het gebruik van Resource Manager en resourcegroepen beschreven
* [Toegangsbeheer op basis van rollen beheren met Azure PowerShell](../role-based-access-control/role-assignments-powershell.md)
  
  In dit artikel wordt uitgelegd hoe u toegangsbeheer op basis van rollen beheert met Azure PowerShell
* [Toegangsbeheer op basis van rollen beheren met de REST-API](../role-based-access-control/role-assignments-rest.md)
  
  Dit artikel beschrijft hoe u de REST-API gebruikt om RBAC te beheren.
* [Toegangsbeheer op basis van rollen voor Microsoft Azure vanuit Ignite](https://channel9.msdn.com/events/Ignite/2015/BRK2707)
  
  Deze 2015 Microsoft Ignite conferentie video bespreekt toegang tot beheer en rapportage in Azure. Het behandelt ook de aanbevolen procedures voor het beveiligen van toegang tot Azure-abonnementen met Azure Active Directory.
* [Toegang verlenen aan webtoepassingen die gebruikmaken van OAuth 2.0 en Azure Active Directory](../active-directory/develop/v1-protocols-oauth-code.md)
  
  Dit artikel beschrijft de volledige OAuth 2.0-stroom voor verificatie met Azure Active Directory.
* [Key Vault-beheer met REST-API’s](https://msdn.microsoft.com/library/azure/mt620024.aspx)
  
  Dit document dient als referentie voor de REST-API's voor het programmatisch beheren van uw Key Vault, inclusief het instellen van het toegangsbeleid voor de Key Vault.
* [REST-API’s van Key Vault](https://msdn.microsoft.com/library/azure/dn903609.aspx)
  
  Koppeling naar de REST API-referentiedocumentatie voor Key Vault.
* [Toegangsbeheer voor sleutels](https://msdn.microsoft.com/library/azure/dn903623.aspx#BKMK_KeyAccessControl)
  
  Koppeling naar de referentiedocumentatie voor toegangsbeheer voor geheimen.
* [Toegangsbeheer voor geheimen](https://msdn.microsoft.com/library/azure/dn903623.aspx#BKMK_SecretAccessControl)
  
  Koppeling naar de referentiedocumentatie voor toegangsbeheer voor sleutels.
* Toegangsbeleid voor Key Vault [instellen](https://docs.microsoft.com/powershell/module/azurerm.keyvault/Set-AzureRmKeyVaultAccessPolicy) en [verwijderen](https://docs.microsoft.com/powershell/module/azurerm.keyvault/Remove-AzureRmKeyVaultAccessPolicy) met behulp van PowerShell
  
  Koppelingen naar referentiedocumentatie voor PowerShell-cmdlets voor het beheren van toegangsbeleid voor Key Vault.

## <a name="next-steps"></a>Volgende stappen
[Key Vault-firewalls en virtuele netwerken configureren](key-vault-network-security.md)

Zie [Aan de slag met Azure Key Vault](key-vault-get-started.md) voor een inleidende zelfstudie voor beheerders.

Zie [Logboekregistratie van Azure Key Vault](key-vault-logging.md) voor meer informatie over de gebruiksregistratie voor Key Vault.

Zie [Over sleutels en geheimen](https://msdn.microsoft.com/library/azure/dn903623.aspx) voor meer informatie over het gebruik van sleutels en geheimen met Azure Key Vault.

Ga naar de [forums van Azure Key Vault](https://social.msdn.microsoft.com/forums/azure/home?forum=AzureKeyVault) als u vragen hebt over Key Vault

