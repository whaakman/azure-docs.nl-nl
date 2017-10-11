---
title: Uw Key Vault beveiligen | Microsoft Docs
description: Toegangsmachtigingen voor Key Vault voor het beheer van kluizen, sleutels en geheimen beheren. Verificatie- en autorisatiemodel voor Key Vault en informatie over het beveiligen van uw Key Vault
services: key-vault
documentationcenter: 
author: amitbapat
manager: mbaldwin
tags: azure-resource-manager
ms.assetid: e5b4e083-4a39-4410-8e3a-2832ad6db405
ms.service: key-vault
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 01/07/2017
ms.author: ambapat
ms.openlocfilehash: b81791f0bce7e6f57782dfe7bc5fb5fc21369e7d
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/29/2017
---
# <a name="secure-your-key-vault"></a>Uw Key Vault beveiligen
Azure Key Vault is een cloudservice die versleutelingssleutels en geheimen (zoals certificaten, verbindingsreeksen en wachtwoorden) voor uw cloudtoepassingen beveiligt. Omdat deze gegevens vertrouwelijk en essentieel voor uw bedrijf zijn, is het belangrijk om de toegang tot uw Key Vaults te beveiligen, zodat alleen gemachtigde toepassingen en gebruikers toegang hebben tot Key Vault. In dit artikel wordt een overzicht gegeven van het toegangsmodel van Key Vault, verificatie en autorisatie worden uitgelegd en met een voorbeeld wordt veilige toegang tot Key Vault voor uw cloud-toepassingen geïllustreerd.

## <a name="overview"></a>Overzicht
Toegang tot een Key Vault wordt geregeld via twee afzonderlijke interfaces: de beheerlaag en de gegevenslaag. Voor beide lagen is juiste verificatie en autorisatie vereist voordat een aanroeper (een gebruiker of een toepassing) toegang kan krijgen tot Key Vault. Met verificatie wordt de identiteit van de aanroeper vastgesteld, terwijl autorisatie bepaalt welke bewerkingen de aanroeper mag uitvoeren.

Voor verificatie gebruiken zowel de beheerlaag als de gegevenslaag Azure Active Directory. Voor autorisatie gebruikt het beheervlak op rollen gebaseerd toegangsbeheer (RBAC), terwijl de gegevenslaag het toegangsbeleid van Key Vault gebruikt.

Hier volgt een kort overzicht van de onderwerpen die aan bod komen:

[Verificatie met behulp van Azure Active Directory](#authentication-using-azure-active-directory): in deze sectie wordt uitgelegd hoe een aanroeper verificatie uitvoert met Azure Active Directory voor toegang tot een Key Vault via de beheerlaag en de gegevenslaag. 

[Beheerlaag en gegevenslaag](#management-plane-and-data-plane): de beheerlaag en de gegevenslaag zijn twee toegangslagen die worden gebruikt voor toegang tot uw Key Vault. Elke toegangslaag ondersteunt specifieke bewerkingen. In deze sectie worden de toegangseindpunten, ondersteunde bewerkingen en toegangsbeheermethodes beschreven die beide lagen gebruiken. 

[Toegangsbeheer voor de beheerlaag](#management-plane-access-control): in deze sectie laten we zien hoe u toegang geeft aan bewerkingen op de beheerlaag met op rollen gebaseerd toegangsbeheer.

[Toegangsbeheer voor de gegevenslaag](#data-plane-access-control): in deze sectie wordt beschreven hoe u het toegangsbeleid van Key Vault gebruikt om de toegang tot de gegevenslaag te beheren.

[Voorbeeld](#example): in dit voorbeeld wordt beschreven hoe u het toegangsbeheer voor uw Key Vault zo instelt dat drie verschillende teams (beveiligingsteam, ontwikkelaars/operators en auditors) specifieke taken uitvoeren om een toepassing in Azure te ontwikkelen, beheren en controleren.

## <a name="authentication-using-azure-active-directory"></a>Verificatie met Azure Active Directory
Wanneer u een Key Vault maakt in een Azure-abonnement, is het automatisch gekoppeld aan de tenant van Azure Active Directory voor dat abonnement. Alle aanroepers (gebruikers en toepassingen) moeten in deze tenant worden geregistreerd voor toegang tot de Key Vault. Een toepassing of een gebruiker moet worden geverifieerd met Azure Active Directory voor toegang tot de Key Vault. Dit geldt voor toegang tot zowel de beheerlaag als de gegevenslaag. In beide gevallen kan een toepassing op twee manieren toegang krijgen tot de Key Vault:

* **Toegang voor gebruikers en toepassingen**: meestal is dit voor toepassingen die de Key Vault gebruiken namens een aangemelde gebruiker. Azure PowerShell en Azure Portal zijn voorbeelden van dit type toegang. Er zijn twee manieren om toegang te verlenen aan gebruikers: u kunt gebruikers toegang geven tot de Key Vault wanneer ze een willekeurige toepassing gebruiken of alleen wanneer ze een bepaalde toepassing gebruiken (aangeduid als samengestelde identiteit). 
* **Alleen toegang voor toepassingen**: voor toepassingen die daemon-services, achtergrondtaken, enz. uitvoeren. De identiteit van de toepassing krijgt toegang tot de Key Vault.

Voor beide typen toepassingen wordt de aanvraag geverifieerd met Azure Active Directory met een van de [ondersteunde verificatiemethoden](../active-directory/active-directory-authentication-scenarios.md) en krijgt de toepassing een token. De gebruikte verificatiemethode is afhankelijk van het type toepassing. Vervolgens gebruikt de toepassing dit token en stuurt deze een REST API-verzoek naar Key Vault. Bij toegang tot de beheerlaag worden de verzoeken gerouteerd via het eindpunt van Azure Resource Manager. Bij toegang tot de gegevenslaag gebruiken de toepassingen een eindpunt van Key Vault. Raadpleeg meer informatie over de [gehele verificatiestroom](../active-directory/active-directory-protocols-oauth-code.md). 

De resourcenaam waarvoor de toepassing een token aanvraagt, is afhankelijk van de laag waartoe de toepassing toegang probeert te krijgen: de beheerlaag of de gegevenslaag. De resourcenaam is daarom het eindpunt van de beheerlaag of gegevenslaag en wordt beschreven in de tabel verderop in dit artikel. Deze is afhankelijk van de Azure-omgeving.

Het gebruik van één enkel verificatiemechanisme voor de beheer- en gegevenslaag heeft zijn eigen voordelen:

* Organisaties kunnen de toegang tot alle Key Vaults in hun organisatie centraal beheren
* Als een gebruiker het bedrijf verlaat, verliest hij onmiddellijk de toegang tot alle Key Vaults in de organisatie
* Organisaties kunnen de verificatie aanpassen via de opties in Azure Active Directory (bijvoorbeeld door Multi-Factor Authentication in te schakelen voor extra beveiliging)

## <a name="management-plane-and-data-plane"></a>De beheer- en gegevenslaag
Azure Key Vault is een Azure-service die beschikbaar is via het Azure Resource Manager-implementatiemodel. Als u een Key Vault hebt gemaakt, krijgt u een virtuele container waarin u andere objecten (zoals sleutels, geheimen en certificaten) kunt maken. U krijgt vervolgens toegang tot uw Key Vault via de beheer- en gegevenslaag om specifieke bewerkingen uit te voeren. De interface van de beheerlaag wordt gebruikt voor het beheren van uw Key Vault zelf. U kunt bijvoorbeeld kenmerken van de Key Vault maken, verwijderen en bijwerken, en toegangsbeleid voor de gegevenslaag instellen. De gegevenslaag wordt gebruikt voor het toevoegen, verwijderen, wijzigen en gebruiken van sleutels, geheimen en certificaten die zijn opgeslagen in uw Key Vault.

U opent de interfaces van de beheer- en gegevenslaag via verschillende eindpunten (zie tabel). De tweede kolom in de tabel beschrijft de DNS-namen voor deze eindpunten in verschillende Azure-omgevingen. De derde kolom beschrijft de bewerkingen die u vanaf elke toegangslaag kunt uitvoeren. Elke toegangslaag heeft ook een eigen mechanisme voor toegangsbeheer. Het toegangsbeheer voor de beheerlaag wordt ingesteld met behulp van toegangsbeheer op basis van rollen (RBAC) in Azure Resource Manager. Het toegangsbeheer voor de gegevenslaag wordt ingesteld met toegangsbeleid van Key Vault.

| Toegangslaag | Eindpunten voor toegang | Bewerkingen | Mechanisme voor toegangsbeheer |
| --- | --- | --- | --- |
| Beheerlaag |**Wereldwijd:**<br> management.azure.com:443<br><br> **Azure China:**<br> management.chinacloudapi.cn:443<br><br> **Azure van de Amerikaanse overheid:**<br> management.usgovcloudapi.net:443<br><br> **Azure Duitsland:**<br> management.microsoftazure.de:443 |Key Vault maken/lezen/bijwerken/verwijderen <br> Toegangsbeleid voor Key Vault instellen<br>Tags instellen voor Key Vault |Toegangsbeheer op basis van rollen (RBAC) in Azure Resource Manager |
| Gegevenslaag |**Wereldwijd:**<br> &lt;kluisnaam&gt;.vault.azure.net:443<br><br> **Azure China:**<br> &lt;kluisnaam&gt;.vault.azure.cn:443<br><br> **Azure van de Amerikaanse overheid:**<br> &lt;kluisnaam&gt;.vault.usgovcloudapi.net:443<br><br> **Azure Duitsland:**<br> &lt;kluisnaam&gt;.vault.microsoftazure.de:443 |Voor sleutels: Ontsleutelen, Versleutelen, Sleutel uitpakken, Sleutel inpakken, Controleren, Ondertekenen, Ophalen, Sorteren, Bijwerken, Maken, Importeren, Verwijderen, Back-up maken, Herstellen<br><br> Voor geheimen: Ophalen, Sorteren, Instellen, Verwijderen |Toegangsbeleid van Key Vault |

Het toegangsbeheer van de beheerlaag en het toegangsbeheer van de gegevenslaag werken onafhankelijk van elkaar. Als u bijvoorbeeld toegang wilt verlenen aan een toepassing voor het gebruik van sleutels in een Key Vault, hoeft u alleen toegangsmachtigingen toe te kennen aan de gegevenslaag met toegangsbeleid van Key Vault. Er is voor deze toepassing geen toegang tot de beheerlaag vereist. Als u echter wilt dat een gebruiker eigenschappen en tags van de Vault kan lezen, maar geen toegang heeft tot de sleutels, geheimen of certificaten, kunt u deze gebruiker leestoegang geven met RBAC. Hiervoor is geen toegang tot de gegevenslaag vereist.

## <a name="management-plane-access-control"></a>Toegangsbeheer voor de beheerlaag
De beheerlaag bestaat uit bewerkingen die invloed hebben op de Key Vault zelf. U kunt bijvoorbeeld een Key Vault maken of verwijderen. U kunt een lijst met Vaults krijgen in een abonnement. U kunt eigenschappen van de Key Vault (zoals SKU en tags) ophalen en toegangsbeleid voor de Key Vault instellen waarmee de gebruikers en toepassingen die toegang hebben tot sleutels en geheimen in de Key Vault, worden beheerd. Het toegangsbeheer voor de beheerlaag gebruikt RBAC. Zie de volledige lijst met bewerkingen die in Key Vault kunnen worden uitgevoerd via de beheerlaag, in de tabel in de voorgaande sectie. 

### <a name="role-based-access-control-rbac"></a>Toegangsbeheer op basis van rollen (RBAC)
Elk Azure-abonnement heeft een Azure Active Directory. Gebruikers, groepen en toepassingen in deze directory kunnen worden gemachtigd om resources te beheren in het Azure-abonnement dat gebruikmaakt van het Azure Resource Manager-implementatiemodel. Dit type toegangsbeheer wordt toegangsbeheer op basis van rollen (RBAC) genoemd. Voor het beheren van deze toegang kunt u [Azure Portal](https://portal.azure.com/), de [Azure CLI-tools](../cli-install-nodejs.md), [PowerShell](/powershell/azureps-cmdlets-docs) of de [Azure Resource Manager REST-API's](https://msdn.microsoft.com/library/azure/dn906885.aspx) gebruiken.

Met het Azure Resource Manager-model maakt u uw Key Vault in een resourcegroep en beheert u de toegang tot de beheerlaag van deze Key Vault met Azure Active Directory. U kunt bijvoorbeeld gebruikers of een groep machtigen om Key Vaults in een bepaalde resourcegroep te beheren.

U kunt toegang verlenen aan gebruikers, groepen en toepassingen binnen een bepaald bereik door de juiste RBAC-rollen toe te wijzen. Als u bijvoorbeeld een gebruiker wilt machtigen om Key Vaults te beheren, kunt u de vooraf gedefinieerde rol 'Key Vault-inzender' toewijzen aan deze gebruiker voor een bepaald bereik. Het bereik is in dit geval een abonnement, een resourcegroep of alleen een specifieke Key Vault. Een rol die is toegewezen op abonnementsniveau, is van toepassing op alle resourcegroepen en resources in dat abonnement. Een rol die is toegewezen op het niveau van de resourcegroep, is van toepassing op alle resources in die resourcegroep. Een rol die is toegewezen voor een specifieke resource, geldt alleen voor die resource. Er zijn verschillende vooraf gedefinieerde rollen (zie [RBAC: ingebouwde rollen](../active-directory/role-based-access-built-in-roles.md)). Als de vooraf gedefinieerde rollen niet aansluiten bij uw behoeften, kunt u ook uw eigen rollen definiëren.

> [!IMPORTANT]
> Houd er rekening mee dat een gebruiker die inzendersmachtigingen (RBAC) heeft voor de beheerlaag van een Key Vault, zichzelf toegang kan geven tot de gegevenslaag. Dit kan hij doen door toegangsbeleid voor de Key Vault in te stellen en zo de toegang tot de gegevenslaag te beheren. Daarom is het aanbevolen streng te zijn bij het bepalen wie er inzenderstoegang heeft tot uw Key Vaults en er zo voor te zorgen dat alleen gemachtigde personen toegang hebben tot uw Key Vaults, sleutels, geheimen en certificaten, en deze kunnen beheren.
> 
> 

## <a name="data-plane-access-control"></a>Toegangsbeheer voor de gegevenslaag
De gegevenslaag van een Key Vault bestaat uit de bewerkingen die invloed hebben op de objecten in een Key Vault, zoals sleutels, geheimen en certificaten.  Dit zijn onder andere sleutelbewerkingen, zoals sleutels maken, importeren, bijwerken, sorteren, herstellen en er back-ups van maken, cryptografische bewerkingen, zoals ondertekenen, controleren, versleutelen, ontsleutelen, inpakken en uitpakken, en tags en andere kenmerken voor sleutels instellen. Voor geheimen gaat het om bewerkingen zoals ophalen, instellen, sorteren en verwijderen.

U verleent toegang tot de gegevenslaag door toegangsbeleid voor een Key Vault in te stellen. Een gebruiker, groep of toepassing heeft inzendersrechten (RBAC) voor de beheerlaag van een Key Vault nodig om toegangsbeleid voor die Key Vault in te stellen. Een gebruiker, groep of toepassing kan worden gemachtigd om bepaalde bewerkingen voor sleutels of geheimen in een Key Vault uit te voeren. Key Vault ondersteunt maximaal 16 toegangsbeleidsitems voor een sleutelkluis. Maak een Azure Active Directory-beveiligingsgroep en voeg gebruikers toe aan die groep om meerdere gebruikers in een Key Vault toegang te geven tot de gegevenslaag.

### <a name="key-vault-access-policies"></a>Toegangsbeleid van Key Vault
Met toegangsbeleid van Key Vault kunt u machtigingen voor sleutels, geheimen en certificaten afzonderlijk toekennen. U kunt bijvoorbeeld een gebruiker toegang geven tot sleutels, maar geen machtigingen voor geheimen geven. Machtigingen voor toegang tot sleutels, geheimen of certificaten bevinden zich echter op het niveau van de Key Vault. Met andere woorden: het toegangsbeleid voor Key Vault ondersteunt geen machtigingen op objectniveau. U kunt [Azure Portal](https://portal.azure.com/), de [Azure CLI-hulpprogramma's](../cli-install-nodejs.md), [PowerShell](/powershell/azureps-cmdlets-docs) of de [Key Vault-beheer REST-API's](https://msdn.microsoft.com/library/azure/mt620024.aspx) gebruiken om toegangsbeleid in te stellen voor Key Vault.

> [!IMPORTANT]
> Houd er rekening mee dat toegangsbeleid voor Key Vault wordt toegevoegd op het niveau van de Key Vault. Wanneer een gebruiker bijvoorbeeld is gemachtigd voor het maken en verwijderen van sleutels, kan hij deze bewerkingen uitvoeren op alle sleutels in die Key Vault.
> 
> 

## <a name="example"></a>Voorbeeld
Stel dat u een toepassing ontwikkelt die gebruikmaakt van een certificaat voor SSL, Azure Storage voor het opslaan van gegevens, en een RSA 2048 bits-sleutel voor tekenbewerkingen. Stel dat deze toepassing wordt uitgevoerd in een VM (of een schaalset voor VM’s). U kunt een Key Vault gebruiken voor het opslaan van alle geheimen van de toepassing en Key Vault voor het opslaan van het bootstrap-certificaat dat door de toepassing wordt gebruikt voor verificatie met Azure Active Directory.

Hier volgt een overzicht van de sleutels en geheimen die kunnen worden opgeslagen in een Key Vault.

* **SSL-certificaat**: wordt gebruikt voor SSL
* **Opslagsleutel**: wordt gebruikt voor toegang tot het opslagaccount
* **2048-bits RSA-sleutel**: wordt gebruikt voor tekenbewerkingen
* **Bootstrap-certificaat**: wordt gebruikt voor verificatie met Azure Active Directory, om toegang te krijgen tot Key Vault om de opslagsleutel op te halen en om de RSA-sleutel te gebruiken om zaken te ondertekenen.

We gaan nu bekijken wie deze toepassing beheren, implementeren en controleren. In dit voorbeeld gebruiken we drie rollen.

* **Beveiligingsteam**: dit is gewoonlijk IT-personeel dat onder de CSO (Chief Security Officer) staat, of personeel met gelijkwaardige machtigingen, dat verantwoordelijk is voor de juiste beveiliging van geheimen zoals SSL-certificaten, RSA-sleutels die worden gebruikt om te ondertekenen, tekenreeksen voor databases en opslagaccountsleutels.
* **Ontwikkelaars/operators**: dit zijn de mensen die deze toepassing ontwikkelen en vervolgens implementeren in Azure. Doorgaans maken ze geen deel uit van het beveiligingsteam. Ze hebben daarom vaak geen toegang tot gevoelige gegevens, zoals SSL-certificaten en RSA-sleutels, maar ze hebben wel toegang nodig tot de toepassing die ze implementeren.
* **Auditors**: dit is meestal een andere groep mensen die niet bij de ontwikkelaars en het algemene IT-personeel horen. Zij controleren of de certificaten, sleutels, enz. juist worden gebruikt en onderhouden, en zorgen ervoor dat de veiligheidsnormen voor gegevens worden nageleefd. 

Er is nog een rol die deze toepassing niet gebruikt, maar die wel relevant is om hier te vermelden. Dit is de abonnementbeheerder (of resourcegroepbeheerder). De abonnementbeheerder stelt de initiële toegangsmachtigingen voor het beveiligingsteam in. Hier gaan we ervan uit dat de abonnementbeheerder het beveiligingsteam toegang heeft gegeven tot een resourcegroep waarin alle resources zijn geplaatst die nodig zijn voor deze toepassing.

We gaan nu kijken welke acties elke rol uitvoert voor deze toepassing.

* **Beveiligingsteam**
  * Key Vaults maken
  * Logboekregistratie van Key Vault inschakelen
  * Sleutels/geheimen toevoegen
  * Back-ups van sleutels maken voor herstel na noodgevallen
  * Toegangsbeleid voor Key Vaults instellen om machtigingen te verlenen aan gebruikers en toepassingen om bepaalde bewerkingen uit te voeren
  * Sleutels/geheimen periodiek terugdraaien
* **Ontwikkelaars/operators**
  * Verwijzingen naar bootstrap- en SSL-certificaten (vingerafdrukken), de opslagsleutel (geheime URI) en de handtekeningsleutel (sleutel-URI) verkrijgen van het beveiligingsteam
  * Toepassingen die programmatisch sleutels en geheimen gebruiken, ontwikkelen en implementeren
* **Auditors**
  * Gebruikslogboeken bekijken om te bevestigen dat sleutels/geheimen op de juiste manier en in overeenstemming met de beveiligingsnormen voor gegevens worden gebruikt

Nu gaan we bekijken welke toegangsmachtigingen voor Key Vault er nodig zijn zodat elke rol (en de toepassing) de toegewezen taken kan uitvoeren. 

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

Alle drie de rollen hebben niet alleen machtigingen voor Key Vault nodig, maar ook toegang tot andere resources. Ze moeten bijvoorbeeld VM’s kunnen implementeren (of web-apps enz.). Ontwikkelaars/Operators hebben ook Inzenderstoegang tot de resourcetypen nodig. Auditors hebben leestoegang nodig tot het opslagaccount waarin de logboeken van Key Vault worden opgeslagen.

Omdat de focus van dit artikel ligt op de beveiliging van de toegang tot uw Key Vault, illustreren we alleen de relevante onderdelen voor dit onderwerp en slaan we informatie over de implementatie van certificaten, de programmatische toegang tot sleutels en geheimen, enz. over. Deze informatie wordt elders al behandeld. De implementatie van certificaten die zijn opgeslagen in Key Vault voor VM’s, wordt beschreven in een [blogbericht](https://blogs.technet.microsoft.com/kv/2016/09/14/updated-deploy-certificates-to-vms-from-customer-managed-key-vault/) en er is [voorbeeldcode](https://www.microsoft.com/download/details.aspx?id=45343) beschikbaar die laat zien hoe u een bootstrap-certificaat gebruikt voor verificatie met Azure Active Directory om toegang te krijgen tot Key Vault.

De meeste toegangsmachtigingen kunnen worden verleend met behulp van Azure Portal, maar om gedetailleerde machtigingen te verlenen, moet u mogelijk Azure PowerShell (of Azure CLI) gebruiken om het gewenste resultaat te bereiken. 

De volgende PowerShell-fragmenten gaan uit van het volgende:

* De Azure Active Directory-beheerder heeft beveiligingsgroepen gemaakt die staan voor de drie rollen, namelijk Contoso Security Team, Contoso App Devops en Contoso App Auditors. De beheerder heeft ook gebruikers toegevoegd aan de groepen waartoe ze behoren.
* **ContosoAppRG** is de resourcegroep waarin alle resources zich bevinden. **contosologstorage** is waar de logboeken worden opgeslagen. 
* Key Vault **ContosoKeyVault** en het opslagaccount **contosologstorage**, dat wordt gebruikt voor logboeken van Key Vault, moeten zich in dezelfde Azure-locatie bevinden

De abonnementbeheerder wijst allereerst de rollen Inzender voor Key Vault en Beheerder van gebruikerstoegang toe aan het beveiligingsteam. Hiermee kan het beveiligingsteam de toegang tot andere resources beheren en Key Vault in de resourcegroep ContosoAppRG beheren.

```
New-AzureRmRoleAssignment -ObjectId (Get-AzureRmADGroup -SearchString 'Contoso Security Team')[0].Id -RoleDefinitionName "key vault Contributor" -ResourceGroupName ContosoAppRG
New-AzureRmRoleAssignment -ObjectId (Get-AzureRmADGroup -SearchString 'Contoso Security Team')[0].Id -RoleDefinitionName "User Access Administrator" -ResourceGroupName ContosoAppRG
```

Het volgende script geeft weer hoe het beveiligingsteam een Key Vault kan maken, logboekregistratie kan instellen, en toegangsmachtigingen voor andere rollen en de toepassing kan instellen. 

```
# Create key vault and enable logging
$sa = Get-AzureRmStorageAccount -ResourceGroup ContosoAppRG -Name contosologstorage
$kv = New-AzureRmKeyVault -VaultName ContosoKeyVault -ResourceGroup ContosoAppRG -SKU premium -Location 'westus' -EnabledForDeployment
Set-AzureRmDiagnosticSetting -ResourceId $kv.ResourceId -StorageAccountId $sa.Id -Enabled $true -Categories AuditEvent

# Data plane permissions for Security team
Set-AzureRmKeyVaultAccessPolicy -VaultName ContosoKeyVault -ObjectId (Get-AzureRmADGroup -SearchString 'Contoso Security Team')[0].Id -PermissionsToKeys backup,create,delete,get,import,list,restore -PermissionsToSecrets all

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

De gedefinieerde aangepaste rol kan alleen worden toegewezen aan het abonnement waarin de resourcegroep ContosoAppRG wordt gemaakt. Als dezelfde aangepaste rollen worden gebruikt voor andere projecten in andere abonnementen, kunnen er meer abonnementen aan het bereik worden toegevoegd.

De aangepaste rol voor de ontwikkelaars/operators voor de machtiging 'implementeren/actie' kan worden toegewezen aan de resourcegroep. Op deze manier krijgen alleen de VM's die zijn gemaakt in de resourcegroep ContosoAppRG, de geheimen (SSL-certificaat en bootstrap-certificaat). Geen van de VM's die door een lid van het ontwikkelaars/operators-team worden gemaakt in een andere resourcegroep, kunnen deze geheimen ophalen, zelfs niet als de geheime URI's bekend zijn.

In dit voorbeeld ziet u een eenvoudig scenario. In de praktijk zijn scenario's mogelijk ingewikkelder. Mogelijk moet u de machtigingen voor uw Key Vault aanpassen op basis van uw behoeften. In ons voorbeeld gaan we er bijvoorbeeld van uit dat het beveiligingsteam de sleutel en geheime verwijzingen (URI's en vingerafdrukken) levert waarnaar het ontwikkelaars/operators-team in hun toepassingen moet verwijzen. Daarom hoeven ze de ontwikkelaars/operators geen toegang te geven tot de gegevenslaag. Houd er ook rekening mee dat dit voorbeeld is gericht op het beveiligen van uw Key Vault. Het is ook belangrijk om uw [VM’s](https://azure.microsoft.com/services/virtual-machines/security/), [opslagaccounts](../storage/common/storage-security-guide.md) en andere Azure-resources te beveiligen.

> [!NOTE]
> Opmerking: Dit voorbeeld toont hoe toegang tot de Key Vault wordt vergrendeld tijdens de productie. De ontwikkelaars moeten hun eigen abonnement of resourcegroep hebben waarin ze volledige machtigingen hebben voor het beheren van hun kluizen, VM's en het opslagaccount waarin ze de toepassing ontwikkelen.
> 
> 

## <a name="resources"></a>Bronnen
* [Toegangsbeheer op basis van rollen in Azure Active Directory](../active-directory/role-based-access-control-configure.md)
  
  In dit artikel wordt het toegangsbeheer op basis van rollen in Azure Active Directory uitgelegd.
* [RBAC: ingebouwde rollen](../active-directory/role-based-access-built-in-roles.md)
  
  In dit artikel worden alle beschikbare ingebouwde rollen in RBAC beschreven.
* [Resource Manager-implementatie en klassieke implementatie begrijpen](../azure-resource-manager/resource-manager-deployment-model.md)
  
  In dit artikel worden het Resource Manager-implementatiemodel en het klassieke implementatiemodel uitgelegd en worden de voordelen van het gebruik van Resource Manager en resourcegroepen beschreven
* [Toegangsbeheer op basis van rollen beheren met Azure PowerShell](../active-directory/role-based-access-control-manage-access-powershell.md)
  
  In dit artikel wordt uitgelegd hoe u toegangsbeheer op basis van rollen beheert met Azure PowerShell
* [Toegangsbeheer op basis van rollen beheren met de REST-API](../active-directory/role-based-access-control-manage-access-rest.md)
  
  Dit artikel beschrijft hoe u de REST-API gebruikt om RBAC te beheren.
* [Toegangsbeheer op basis van rollen voor Microsoft Azure vanuit Ignite](https://channel9.msdn.com/events/Ignite/2015/BRK2707)
  
  Dit is een koppeling naar een video op Channel 9 van de vergadering over MS Ignite in 2015. In deze sessie wordt ingegaan op de opties voor toegangsbeheer en rapportage in Azure en worden aanbevolen procedures behandeld voor de beveiliging van de toegang tot Azure-abonnementen met Azure Active Directory.
* [Toegang verlenen aan webtoepassingen die gebruikmaken van OAuth 2.0 en Azure Active Directory](../active-directory/active-directory-protocols-oauth-code.md)
  
  Dit artikel beschrijft de volledige OAuth 2.0-stroom voor verificatie met Azure Active Directory.
* [Key Vault-beheer met REST-API’s](https://msdn.microsoft.com/library/azure/mt620024.aspx)
  
  Dit document dient als referentie voor de REST-API's voor het programmatisch beheren van uw Key Vault, inclusief het instellen van het toegangsbeleid voor de Key Vault.
* [REST-API’s van Key Vault](https://msdn.microsoft.com/library/azure/dn903609.aspx)
  
  Koppeling naar de REST API-referentiedocumentatie voor Key Vault.
* [Toegangsbeheer voor sleutels](https://msdn.microsoft.com/library/azure/dn903623.aspx#BKMK_KeyAccessControl)
  
  Koppeling naar de referentiedocumentatie voor toegangsbeheer voor geheimen.
* [Toegangsbeheer voor geheimen](https://msdn.microsoft.com/library/azure/dn903623.aspx#BKMK_SecretAccessControl)
  
  Koppeling naar de referentiedocumentatie voor toegangsbeheer voor sleutels.
* Toegangsbeleid voor Key Vault [instellen](https://msdn.microsoft.com/library/mt603625.aspx) en [verwijderen](https://msdn.microsoft.com/library/mt619427.aspx) met behulp van PowerShell
  
  Koppelingen naar referentiedocumentatie voor PowerShell-cmdlets voor het beheren van toegangsbeleid voor Key Vault.

## <a name="next-steps"></a>Volgende stappen
Zie [Aan de slag met Azure Key Vault](key-vault-get-started.md) voor een inleidende zelfstudie voor beheerders.

Zie [Logboekregistratie van Azure Key Vault](key-vault-logging.md) voor meer informatie over de gebruiksregistratie voor Key Vault.

Zie [Over sleutels en geheimen](https://msdn.microsoft.com/library/azure/dn903623.aspx) voor meer informatie over het gebruik van sleutels en geheimen met Azure Key Vault.

Ga naar de [forums van Azure Key Vault](https://social.msdn.microsoft.com/forums/azure/home?forum=AzureKeyVault) als u vragen hebt over Key Vault

