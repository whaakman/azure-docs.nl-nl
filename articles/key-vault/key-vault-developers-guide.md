---
title: Ontwikkelaarshandleiding Azure Sleutelkluis
description: Ontwikkelaars kunnen Azure Key Vault gebruiken voor het beheren van de cryptografische sleutels die binnen de Microsoft Azure-omgeving.
services: key-vault
author: BrucePerlerMS
manager: mbaldwin
ms.service: key-vault
ms.topic: article
ms.workload: identity
ms.date: 08/04/2017
ms.author: bruceper
ms.openlocfilehash: fec4769c0bd571edea84dd2f766bb907d8819be5
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/18/2017
---
# <a name="azure-key-vault-developers-guide"></a>Ontwikkelaarshandleiding Azure Sleutelkluis

Sleutelkluis kunt u veilig toegang krijgen tot gevoelige informatie in uw toepassingen:

- Sleutels en geheimen worden beveiligd zonder de code zelf schrijven en u gemakkelijk kunnen worden gebruikt vanuit uw toepassingen.
- U zijn kunt uw eigen klanten hebben en hun eigen sleutels beheren zodat u zich concentreren kunt op het leveren van de belangrijkste softwarefuncties. Op deze manier wordt de verantwoordelijk of potentiële aansprakelijkheid voor uw klanten tenant-sleutels en geheimen geen eigenaar van uw toepassingen.
- Toepassingen kunnen gebruikmaken van sleutels voor ondertekening en versleuteling nog houdt de Sleutelbeheer externe van uw toepassing, waardoor de oplossing geschikt is als een geografisch verspreide app.
- Vanaf de release van September 2016 van Sleutelkluis uw toepassingen kunnen nu gebruikmaken van Sleutelkluis [certificaten](https://docs.microsoft.com/rest/api/keyvault/certificate-operations). Zie voor meer informatie [over sleutels, geheimen en certificaten](https://docs.microsoft.com/rest/api/keyvault/about-keys--secrets-and-certificates).

Zie voor meer algemene informatie over Azure Sleutelkluis [wat is Sleutelkluis](key-vault-whatis.md).

## <a name="public-previews"></a>Openbare Previews

Tijd tot tijd brengen we een openbare preview van een nieuwe functie van de Sleutelkluis. Probeer deze en laat ons weten wat u denkt dat azurekeyvault@microsoft.com, ons feedback e-mailadres.

### <a name="storage-account-keys---july-10-2017"></a>Opslagaccountsleutels - 10 juli 2017

>[!NOTE]
>Voor deze update van Azure Sleutelkluis de **Opslagaccountsleutels** functie is in preview.

Dit voorbeeld bevat onze nieuwe toegangscodes voor opslag functie beschikbaar via deze interfaces; [.NET / C#](https://docs.microsoft.com/dotnet/api/microsoft.azure.keyvault/), [REST](https://docs.microsoft.com/rest/api/keyvault/) en [PowerShell](https://docs.microsoft.com/powershell/module/azurerm.keyvault/). 

Zie voor meer informatie over de nieuwe functie voor Opslagaccountsleutels [overzicht van Azure Sleutelkluis storage account sleutels](key-vault-ovw-storage-keys.md).

## <a name="videos"></a>Video's

In deze video ziet u het maken van uw eigen sleutelkluis en het gebruik ervan in de voorbeeldtoepassing 'Hello Sleutelkluis'.

- [Sleutelkluis developer - snelstartgids](https://channel9.msdn.com/Blogs/Azure/Azure-Key-Vault-Developer-Quick-Start/player)

Resources in bovenstaande video:

- [Azure PowerShell](http://go.microsoft.com/fwlink/p/?linkid=320376&clcid=0x409)
- [Voorbeeldcode voor Azure Sleutelkluis](http://go.microsoft.com/fwlink/?LinkId=521527&clcid=0x409)

## <a name="creating-and-managing-key-vaults"></a>Maken en beheren van Sleutelkluizen

Voordat u met Azure Sleutelkluis in uw code werkt, kunt u maken en beheren van kluizen via REST, Resource Manager-sjablonen, PowerShell of CLI, zoals beschreven in de volgende artikelen:

- [Maken en beheren van Sleutelkluizen met REST](https://docs.microsoft.com/rest/api/keyvault/)
- [Maken en beheren van Sleutelkluizen met PowerShell](key-vault-get-started.md)
- [Maken en beheren van Sleutelkluizen met CLI](key-vault-manage-with-cli2.md)
- [Een sleutelkluis maken en toevoegen van een geheim via een Azure Resource Manager-sjabloon](../azure-resource-manager/resource-manager-template-keyvault.md)

> [!NOTE]
> Bewerkingen op sleutelkluizen worden geverifieerd via AAD en geautoriseerd via Sleutelkluis van eigen-beleid, per kluis gedefinieerd.

## <a name="coding-with-key-vault"></a>Codering met Sleutelkluis

Het beheersysteem Sleutelkluis voor programmeurs bestaat uit meerdere interfaces met REST als basis. Via de REST-interface zijn al uw resources sleutelkluizen toegankelijk; sleutels, geheimen en certificaten. [Sleutel kluis REST API-verwijzing](https://docs.microsoft.com/rest/api/keyvault/). 

### <a name="supported-programming-languages"></a>Welke programmeertalen worden ondersteund

#### <a name="net"></a>.NET

- [.NET API-verwijzing voor Sleutelkluis](https://docs.microsoft.com/dotnet/api/microsoft.azure.keyvault) 

Zie voor meer informatie over de 2.x-versie van de .NET SDK de [Release-opmerkingen](key-vault-dotnet2api-release-notes.md).

#### <a name="java"></a>Java

- [Java SDK voor Sleutelkluis](https://docs.microsoft.com/java/api/com.microsoft.azure.keyvault)

#### <a name="nodejs"></a>Node.js

In Node.js, zijn de kluis management API en de object-API van de kluis zijn gescheiden. Key Vault Management kunt maken en bijwerken van de sleutelkluis. Sleutelkluis Operations API is voor het werken met de kluis objecten zoals; sleutels, geheimen en certificaten. 

- [Node.js-API-verwijzing voor sleutelbeheer kluis](http://azure.github.io/azure-sdk-for-node/azure-arm-keyvault/latest/)
- [Node.js-API-verwijzing voor Sleutelkluis bewerkingen](http://azure.github.io/azure-sdk-for-node/azure-keyvault/latest/) 

### <a name="quick-start"></a>Snel starten

- [Sleutelkluis maken](https://github.com/Azure/azure-quickstart-templates/tree/master/101-key-vault-create)
- [Aan de slag met Sleutelkluis in Node.js](https://azure.microsoft.com/en-us/resources/samples/key-vault-node-getting-started/)

### <a name="code-examples"></a>Codevoorbeelden

Voor volledige voorbeelden met Sleutelkluis met uw toepassingen, Zie:

- [Azure Sleutelkluis-codevoorbeelden](http://www.microsoft.com/download/details.aspx?id=45343) -voorbeeldtoepassing .NET *HelloKeyVault* en een voorbeeld van Azure-web-service. 
- [Gebruik van Azure Sleutelkluis in een webtoepassing](key-vault-use-from-web-application.md) -zelfstudie waarmee u kunt informatie over het gebruik van Azure Sleutelkluis in een webtoepassing in Azure. 

## <a name="how-tos"></a>Procedures

De volgende artikelen en scenario's bieden richtlijnen voor het werken met Azure Key Vault taakspecifieke:

- [Wijziging sleutelkluis tenant-ID na abonnement verplaatsen](key-vault-subscription-move-fix.md) : wanneer u uw Azure-abonnement van de tenant A naar B, tenant verplaatst uw bestaande sleutelkluizen zijn niet toegankelijk door de principals (gebruikers en toepassingen) in de tenant B. Fix met deze handleiding.
- [Sleutelkluis achter de firewall openen](key-vault-access-behind-firewall.md) - toegang tot een sleutelkluis uw clienttoepassing sleutelkluis moet toegang kunnen krijgen tot meerdere eindpunten voor verschillende functies.
- [Het genereren en sleutels voor Azure Sleutelkluis Transfer HSM-Protected](key-vault-hsm-protected-keys.md) -dit helpt u bij het plannen, te genereren en brengt u uw eigen HSM beveiligde sleutels voor gebruik met Azure Sleutelkluis.
- [Hoe beveiligde waarden (zoals wachtwoorden) tijdens de implementatie van doorgeven](../azure-resource-manager/resource-manager-keyvault-parameter.md) : wanneer u moet een beveiligde waarde (zoals een wachtwoord) als een parameter doorgeven tijdens de implementatie en kunnen worden opgeslagen die waarde als een geheim in een Azure Key Vault verwijzen naar de waarde in een andere bron Manager-sjablonen.
- [Het gebruik van de Sleutelkluis voor extensible key management met SQL Server](https://msdn.microsoft.com/library/dn198405.aspx) -de SQL Server-Connector voor Azure Sleutelkluis kunt u SQL Server en SQL in een VM voor het benutten van de Azure Sleutelkluis-service als een Extensible Key Management (EKM)-provider te beschermen de de versleutelingssleutels voor toepassingen koppeling; Transparante gegevensversleuteling, back-versleuteling en kolom: versleuteling op bestandsniveau.
- [Certificaten implementeren op virtuele machines van de Sleutelkluis](https://blogs.technet.microsoft.com/kv/2015/07/14/deploy-certificates-to-vms-from-customer-managed-key-vault/) - een cloudtoepassing uitgevoerd in een VM op Azure moet een certificaat. Hoe krijgt u dit certificaat in deze virtuele machine vandaag?
- [Het instellen van de Sleutelkluis met end-to-end sleutel worden gedraaid en controle](key-vault-key-rotation-log-monitoring.md) - dit helpt bij het instellen van de sleutel worden gedraaid en controle met Azure Sleutelkluis.
- [Implementatie van Azure Web App certificaat via Sleutelkluis]( https://blogs.msdn.microsoft.com/appserviceteam/2016/05/24/deploying-azure-web-app-certificate-through-key-vault/) bevat stapsgewijze instructies voor het implementeren van certificaten die zijn opgeslagen in de Sleutelkluis als onderdeel van [App Service-certificaat](https://azure.microsoft.com/blog/internals-of-app-service-certificate/) aanbieden.
- [Machtiging verlenen om te veel toepassingen voor toegang tot een sleutelkluis](key-vault-group-permissions-for-apps.md) beleid voor toegangsbeheer Sleutelkluis biedt alleen ondersteuning voor 16 vermeldingen. U kunt echter een Azure Active Directory-beveiligingsgroep maken. Alle gekoppelde service-principals toevoegen aan deze beveiligingsgroep en vervolgens toegang verlenen aan deze beveiligingsgroep voor Sleutelkluis.
- Zie voor meer taakspecifieke instructies over het integreren en sleutel kluizen gebruiken met Azure [Ryan Jones Azure Resource Manager-sjabloon voorbeelden voor Sleutelkluis](https://github.com/rjmax/ArmExamples/tree/master/keyvaultexamples).
- [Sleutelkluis voorlopig verwijderen gebruiken met CLI](key-vault-soft-delete-cli.md) leidt u door het gebruik en de levenscyclus van een sleutelkluis en verschillende sleutelkluis-objecten met zachte verwijderen ingeschakeld.
- [Sleutelkluis voorlopig verwijderen gebruiken met PowerShell](key-vault-soft-delete-powershell.md) leidt u door het gebruik en de levenscyclus van een sleutelkluis en verschillende sleutelkluis-objecten met zachte verwijderen ingeschakeld.

## <a name="integrated-with-key-vault"></a>Geïntegreerd met Sleutelkluis

Deze artikelen zijn over andere scenario's en services die gebruiken of integreren met Sleutelkluis.

- [Azure Disk Encryption](../security/azure-security-disk-encryption.md) maakt gebruik van het industrie-initiatief [BitLocker](https://technet.microsoft.com/library/cc732774.aspx) functie van Windows en de [DM-Crypt](https://en.wikipedia.org/wiki/Dm-crypt) functie van Linux voor volumeversleuteling voor het besturingssysteem en de gegevensschijven. De oplossing is geïntegreerd met Azure Key Vault om te controleren en beheren van de schijf-sleutels en geheimen in uw abonnement sleutelkluis, terwijl u ervoor zorgt dat alle gegevens in de virtuele machine-schijven zijn versleuteld in rust in uw Azure-opslag.
- [Azure Data Lake Store](../data-lake-store/data-lake-store-get-started-portal.md) biedt de mogelijkheid voor het versleutelen van gegevens die zijn opgeslagen in het account. Voor sleutelbeheer biedt Data Lake Store twee modi voor het beheren van uw master versleutelingssleutels (MEKs), die nodig zijn voor het ontsleutelen van gegevens die zijn opgeslagen in de Data Lake Store. U kunt ofwel Data Lake Store beheren van de MEKs voor u of eigendom van de MEKs met uw Azure Key Vault bewaart. U kunt de modus van Sleutelbeheer opgeven tijdens het maken van een Data Lake Store-account. 
- [Azure Information Protection](/information-protection/plan-design/plan-implement-tenant-key) kunt u naar manager uw eigen tenantsleutel. Bijvoorbeeld, in plaats van Microsoft voor het beheren van uw tenantsleutel (de standaardinstelling), kunt u uw eigen tenantsleutel om te voldoen aan specifieke voorschriften die van toepassing zijn voor uw organisatie beheren. Uw eigen tenantsleutel beheren wordt ook wel aangeduid als Bring uw eigen sleutel, of BYOK.

## <a name="key-vault-overviews-and-concepts"></a>Overzichten van de Sleutelkluis en -concepten

- [Sleutelkluis voorlopig verwijderen gedrag](key-vault-ovw-soft-delete.md) beschrijft een functie waarmee het herstellen van verwijderde objecten of de verwijdering per ongeluk of opzettelijk is.
- [Sleutelkluis client beperking](key-vault-ovw-throttling.md) oriënteert u op de basisconcepten van bandbreedtebeperking en biedt een benadering voor uw app.
- [Overzicht van Sleutelkluis storage account sleutels](key-vault-ovw-storage-keys.md) beschrijft de Sleutelkluis-integratie Azure Storage-Accounts sleutels.
- [Sleutelkluis beveiligingswerelden](key-vault-ovw-security-worlds.md) beschrijft de relaties tussen regio's en beveiligingsgebieden.

## <a name="social"></a>Sociaal

- [Sleutelkluis-Blog](http://aka.ms/kvblog)
- [Sleutelkluis-Forum](http://aka.ms/kvforum)

## <a name="supporting-libraries"></a>Ondersteunende bibliotheken

- [Bibliotheek van Microsoft Azure Key Vault Core](http://www.nuget.org/packages/Microsoft.Azure.KeyVault.Core) biedt **IKey** en **IKeyResolver** interfaces voor het zoeken naar sleutels van id's en bewerkingen uitvoeren met sleutels.
- [Microsoft Azure Key Vault Extensions](http://www.nuget.org/packages/Microsoft.Azure.KeyVault.Extensions) biedt uitgebreide mogelijkheden voor Azure Sleutelkluis.


