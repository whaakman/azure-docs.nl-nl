---
title: Handleiding voor ontwikkelaars van Azure Key Vault
description: Ontwikkelaars kunnen Azure Key Vault gebruiken om cryptografische sleutels binnen de Microsoft Azure-omgeving te beheren.
services: key-vault
author: bryanla
manager: mbaldwin
ms.service: key-vault
ms.topic: article
ms.workload: identity
ms.date: 09/05/2018
ms.author: bryanla
ms.openlocfilehash: b4522d9eb2e9af782918f021c79bbcb155c5c383
ms.sourcegitcommit: ebd06cee3e78674ba9e6764ddc889fc5948060c4
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/07/2018
ms.locfileid: "44053618"
---
# <a name="azure-key-vault-developers-guide"></a>Handleiding voor ontwikkelaars van Azure Key Vault

Key Vault kunt u veilig toegang krijgen tot gevoelige gegevens in uw toepassingen:

- Sleutels en geheimen worden beveiligd zonder te hoeven schrijven van de code zelf en u bent eenvoudig kunnen worden gebruikt in uw toepassingen.
- U zijn kunt uw eigen klanten en hun eigen sleutels te beheren, zodat u zich concentreren kunt op het leveren van de belangrijkste softwarefuncties. Op deze manier wordt de verantwoordelijk of potentiële aansprakelijkheid voor uw klanten tenantsleutels en geheimen geen eigenaar van uw toepassingen.
- Uw toepassing kunt sleutels gebruiken voor het ondertekenen en versleuteling nog houdt het Sleutelbeheer externe van uw toepassing, zodat de oplossing geschikt is als een geografisch gedistribueerde app.
- Vanaf de release van September 2016 van Key Vault, kunnen uw toepassingen nu Key Vault-certificaten beheren. Zie voor meer informatie, [over sleutels, geheimen en certificaten](https://docs.microsoft.com/rest/api/keyvault/about-keys--secrets-and-certificates).

Zie voor meer algemene informatie over Azure Key Vault [wat is Key Vault](key-vault-whatis.md).

## <a name="public-previews"></a>Openbare preview-versies

Periodiek, vrijgeven we een openbare preview-versie van een nieuwe Key Vault-functie. Probeer deze en laat ons weten wat u ervan vindt azurekeyvault@microsoft.com, onze feedback-e-mailadres.

### <a name="storage-account-keys---july-10-2017"></a>Opslagaccountsleutels - 10 juli 2017

>[!NOTE]
>Voor deze update van Azure Key Vault de **Opslagaccountsleutels** functie is beschikbaar als preview.

In dit voorbeeld bevat onze nieuwe Opslagaccountsleutels functie beschikbaar via deze interfaces; [.NET / C#](https://docs.microsoft.com/dotnet/api/microsoft.azure.keyvault/), [REST](https://docs.microsoft.com/rest/api/keyvault/) en [PowerShell](https://docs.microsoft.com/powershell/module/azurerm.keyvault/). 

Zie voor meer informatie over de nieuwe functie voor Storage-Accountsleutels [overzicht van Azure Key Vault storage account sleutels](key-vault-ovw-storage-keys.md).

## <a name="videos"></a>Video's

In deze video ziet u hoe u uw eigen key vault maakt en hoe u deze gebruikt in de 'Hello Key Vault-voorbeeldtoepassing.

- [Key Vault-ontwikkelaars - aan de slag](https://channel9.msdn.com/Blogs/Azure/Azure-Key-Vault-Developer-Quick-Start/player)

Resources in bovenstaande video:

- [Azure PowerShell](http://go.microsoft.com/fwlink/p/?linkid=320376&clcid=0x409)
- [Voorbeeld van Azure Key Vault-Code](http://go.microsoft.com/fwlink/?LinkId=521527&clcid=0x409)

## <a name="creating-and-managing-key-vaults"></a>Het maken en beheren van Sleutelkluizen

Azure Key Vault biedt een manier voor het veilig opslaan van referenties en andere sleutels en geheimen, maar uw code moet worden geverifieerd voor Key Vault om ze op te halen. Beheerde identiteiten voor Azure-resources maakt het oplossen van dit probleem eenvoudiger door middel van Azure-services een automatisch beheerde identiteit in Azure Active Directory (Azure AD). U kunt deze identiteit gebruiken voor verificatie bij alle services die ondersteuning bieden voor Azure AD-verificatie, inclusief Key Vault, zonder dat u referenties in uw code hoeft te hebben. 

Zie voor meer informatie over beheerde identiteiten voor Azure-resources [het overzicht van beheerde identiteiten](../active-directory/managed-identities-azure-resources/overview.md). Zie voor meer informatie over het werken met AAD [toepassingen integreren met Azure Active Directory](/azure/active-directory/develop/active-directory-integrating-applications).

Voordat u met de sleutels, geheimen of certificaten werken in uw key vault, moet u maken en beheren van uw key vault via de CLI, PowerShell, Resource Manager-sjablonen of REST, zoals beschreven in de volgende artikelen:

- [Maken en beheren van Sleutelkluizen met CLI](key-vault-manage-with-cli2.md)
- [Maken en beheren van Sleutelkluizen met PowerShell](key-vault-get-started.md)
- [Een sleutelkluis maken en een geheim via een Azure Resource Manager-sjabloon toevoegen](../azure-resource-manager/resource-manager-template-keyvault.md)
- [Maken en beheren van Sleutelkluizen met REST](https://docs.microsoft.com/rest/api/keyvault/)


## <a name="coding-with-key-vault"></a>Coderen met Key Vault

Het beheersysteem voor Key Vault voor programmeurs bestaat uit meerdere interfaces. In deze sectie bevat koppelingen naar alle talen, evenals enkele exampls code. 

### <a name="supported-programming-and-scripting-languages"></a>Ondersteunde programmeren en scripttalen

#### <a name="rest"></a>REST

Al uw Key Vault-resources zijn toegankelijk via de REST-interface; kluizen, sleutels, geheimen, enz. 

[Key Vault REST API-verwijzing](https://docs.microsoft.com/rest/api/keyvault/). 

#### <a name="net"></a>.NET

[.NET API-verwijzing voor Key Vault](https://docs.microsoft.com/dotnet/api/microsoft.azure.keyvault) 

Zie voor meer informatie over de versie 2.x van de .NET SDK, de [opmerkingen bij de Release](key-vault-dotnet2api-release-notes.md).

#### <a name="java"></a>Java

[Java SDK voor Key Vault](https://docs.microsoft.com/java/api/overview/azure/keyvault)

#### <a name="nodejs"></a>Node.js

In Node.js zijn de API voor Key Vault-beheer en de Key Vault-object API afzonderlijke. Het volgende overzichtsartikel hebt u toegang tot beide. 

[Azure Key Vault-modules voor Node.js](https://docs.microsoft.com/nodejs/api/overview/azure/key-vault)

#### <a name="python"></a>Python

[Azure Key Vault-bibliotheken voor Python](https://docs.microsoft.com/python/api/overview/azure/key-vault)

#### <a name="azure-cli-2"></a>Azure CLI 2

[Azure CLI voor Key Vault](https://docs.microsoft.com/cli/azure/keyvault)

#### <a name="azure-powershell"></a>Azure PowerShell 

[Azure PowerShell voor Key Vault](https://docs.microsoft.com/powershell/module/azurerm.keyvault)

### <a name="quick-start-guides"></a>Quick start-handleidingen

- [Key Vault maken](https://github.com/Azure/azure-quickstart-templates/tree/master/101-key-vault-create)
- [Aan de slag met Key Vault in Node.js](https://github.com/Azure-Samples/key-vault-node-getting-started)

### <a name="code-examples"></a>Voorbeelden van code

Voor volledige voorbeelden met Key Vault met uw toepassingen, Zie:

- [Azure Key Vault-codevoorbeelden](https://azure.microsoft.com/resources/samples/?service=key-vault) -codevoorbeelden voor Azure Key Vault. 
- [Azure Key Vault vanuit een webtoepassing gebruiken](quick-create-net.md) -zelfstudie voor meer informatie over het gebruik van Azure Key Vault vanuit een webtoepassing in Azure. 

## <a name="how-tos"></a>Procedures

Geef taak-specifieke richtlijnen voor het werken met Azure Key Vault de volgende scenario's en artikelen:

- [Sleutelkluis tenant-ID wijzigen na abonnement verplaatsen](key-vault-subscription-move-fix.md) : wanneer u uw Azure-abonnement van tenant A naar tenant B verplaatst uw bestaande key vaults zijn niet toegankelijk door de principals (gebruikers en toepassingen) in tenant B. oplossing deze met behulp van deze handleiding.
- [Toegang tot Key Vault achter een firewall](key-vault-access-behind-firewall.md) - toegang heeft tot een sleutelkluis die uw key vault-clienttoepassing moet toegang hebben tot meerdere eindpunten voor verschillende functies.
- [Het genereren en sleutels voor Azure Key Vault Transfer HSM-Protected](key-vault-hsm-protected-keys.md) -dit helpt u bij het plannen, te genereren en vervolgens over te dragen van uw eigen HSM beveiligde sleutels gebruiken met Azure Key Vault.
- [Beveiligde waarden (zoals wachtwoorden) doorgeven tijdens de implementatie van het](../azure-resource-manager/resource-manager-keyvault-parameter.md) : wanneer u moet een beveiligde waarde (zoals een wachtwoord) als een parameter doorgeven tijdens de implementatie, kunt u die waarde als een geheim in een Azure Key Vault opslaan en verwijzen naar de waarde in een andere bron Manager-sjablonen.
- [Over het gebruik van Key Vault voor uitbreidbaar Sleutelbeheer met SQL Server](https://msdn.microsoft.com/library/dn198405.aspx) -de SQL Server-Connector voor Azure Key Vault kunnen SQL Server en SQL-in-a-VM gebruikmaken van de Azure Key Vault-service als een provider Extensible Key Management (EKM) om te beveiligen de versleutelingssleutels voor de verbinding van toepassingen. Transparent Data Encryption, back-versleuteling en versleuteling op blokniveau kolom.
- [Certificaten implementeren op virtuele machines uit de Sleutelkluis](https://blogs.technet.microsoft.com/kv/2015/07/14/deploy-certificates-to-vms-from-customer-managed-key-vault/) - een cloudtoepassing in een virtuele machine wordt uitgevoerd op de behoeften voor Azure een certificaat. Hoe krijgt u dit certificaat in deze virtuele machine vandaag?
- [Key Vault instellen met end-to-end sleutelrotatie en controle](key-vault-key-rotation-log-monitoring.md) : Dit helpt u bij het instellen van sleutelrotatie en controle met Azure Key Vault.
- [Implementatie van Azure Web App Certificate via Key Vault]( https://blogs.msdn.microsoft.com/appserviceteam/2016/05/24/deploying-azure-web-app-certificate-through-key-vault/) vindt u stapsgewijze instructies voor het implementeren van certificaten die zijn opgeslagen in Key Vault als onderdeel van [App Service Certificate](https://azure.microsoft.com/blog/internals-of-app-service-certificate/) bieden.
- [Veel toepassingen om een key vault toegang te verlenen tot](key-vault-group-permissions-for-apps.md) toegangsbeleid voor Key Vault biedt alleen ondersteuning voor 16 vermeldingen. U kunt echter een Azure Active Directory-beveiligingsgroep maken. Alle gekoppelde service-principals toevoegen aan deze beveiligingsgroep en vervolgens toegang verlenen aan deze beveiligingsgroep aan Key Vault.
- Zie voor meer taakspecifieke begeleiding voor het integreren van en het gebruik van Sleutelkluizen met Azure, [Ryan Jones Azure Resource Manager-sjabloonvoorbeelden voor Key Vault](https://github.com/rjmax/ArmExamples/tree/master/keyvaultexamples).
- [Key Vault-functie voor voorlopig verwijderen gebruiken met CLI](key-vault-soft-delete-cli.md) begeleidt u bij het gebruik en de levenscyclus van een key vault en verschillende key vault-objecten met voorlopig verwijderen ingeschakeld.
- [Key Vault-functie voor voorlopig verwijderen gebruiken met PowerShell](key-vault-soft-delete-powershell.md) begeleidt u bij het gebruik en de levenscyclus van een key vault en verschillende key vault-objecten met voorlopig verwijderen ingeschakeld.

## <a name="integrated-with-key-vault"></a>Geïntegreerd met Key Vault

Deze artikelen zijn voor andere scenario's en services die gebruikmaken van of integreren met Key Vault.

- [Azure Disk Encryption](../security/azure-security-disk-encryption.md) maakt gebruik van de industrienorm [BitLocker](https://technet.microsoft.com/library/cc732774.aspx) functie van Windows en de [DM-Crypt](https://en.wikipedia.org/wiki/Dm-crypt) -functie van Linux om volumeversleuteling voor het besturingssysteem en de gegevensschijven te bieden. De oplossing is geïntegreerd met Azure Key Vault om te controleren en beheren van de sleutels en geheimen in uw key vault-abonnement, terwijl u ervoor te zorgen dat alle gegevens in de virtuele-machineschijven zijn versleuteld in rust in uw Azure-opslag.
- [Azure Data Lake Store](../data-lake-store/data-lake-store-get-started-portal.md) biedt de mogelijkheid voor het versleutelen van gegevens die zijn opgeslagen in het account. Voor sleutelbeheer biedt Data Lake Store twee modi voor het beheren van uw hoofdversleutelingssleutels (MEK's), die vereist zijn voor het ontsleutelen van gegevens die zijn opgeslagen in de Data Lake Store. U kunt ofwel Data Lake Store beheren van de MEK's voor u, of kies eigendom van de MEK's met uw Azure Key Vault-account behouden. U kunt de modus van Sleutelbeheer opgeven tijdens het maken van een Data Lake Store-account. 
- [Azure Information Protection](/azure/information-protection/plan-implement-tenant-key) kunt u naar de manager uw eigen tenantsleutel. Bijvoorbeeld, in plaats van Microsoft voor het beheren van uw tenantsleutel (de standaardinstelling), kunt u uw eigen tenantsleutel om te voldoen aan specifieke regels die voor uw organisatie gelden beheren. Uw eigen tenantsleutel beheren wordt ook wel aangeduid als bring your own key of BYOK.

## <a name="key-vault-overviews-and-concepts"></a>Key Vault overzichten en concepten

- [Gedrag voor Key Vault-functie voor voorlopig verwijderen](key-vault-ovw-soft-delete.md) een functie waarmee herstellen van verwijderde objecten beschrijft of de verwijdering per ongeluk of opzettelijk is.
- [Key Vault-client beperking](key-vault-ovw-throttling.md) oriënteert u op de basisconcepten van beperking en biedt een benadering voor uw app.
- [Overzicht van Key Vault storage account sleutels](key-vault-ovw-storage-keys.md) beschrijving van de sleutels van Key Vault-integratie Azure Storage-Accounts.
- [Key Vault beveiligingswerelden](key-vault-ovw-security-worlds.md) beschrijft de relaties tussen regio's en aspecten van de beveiliging.

## <a name="social"></a>Sociaal

- [Key Vault-Blog](http://aka.ms/kvblog)
- [Key Vault-Forum](http://aka.ms/kvforum)

## <a name="supporting-libraries"></a>Ondersteunende bibliotheken

- [Microsoft Azure Key Vault-Kernbibliotheek](http://www.nuget.org/packages/Microsoft.Azure.KeyVault.Core) biedt **IKey** en **IKeyResolver** interfaces voor sleutels van id's zoeken en uitvoeren van bewerkingen met sleutels.
- [Microsoft Azure Key Vault-extensies](http://www.nuget.org/packages/Microsoft.Azure.KeyVault.Extensions) biedt uitgebreide mogelijkheden voor Azure Key Vault.


