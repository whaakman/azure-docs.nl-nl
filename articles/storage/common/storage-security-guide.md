---
title: Azure Storage-beveiligingshandleiding | Microsoft Docs
description: Details van de vele methoden voor het beveiligen van Azure Storage, waaronder maar niet beperkt tot RBAC, versleuteling voor opslagservice, Client-side-versleuteling, SMB 3.0 en Azure Disk Encryption.
services: storage
author: craigshoemaker
ms.service: storage
ms.topic: article
ms.date: 05/31/2018
ms.author: cshoe
ms.component: common
ms.openlocfilehash: 584db1400ddd2b4f447b7ca397f2c98b701c7848
ms.sourcegitcommit: 1aacea6bf8e31128c6d489fa6e614856cf89af19
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/16/2018
ms.locfileid: "49341868"
---
# <a name="azure-storage-security-guide"></a>Azure Storage-beveiligingshandleiding

Azure Storage biedt een uitgebreide set met mogelijkheden voor beveiliging die samen kunnen ontwikkelaars veilige toepassingen te bouwen:

- Alle gegevens die zijn geschreven naar Azure Storage automatisch versleuteld met behulp van [Storage Service Encryption (SSE)](storage-service-encryption.md). Zie voor meer informatie, [standaard codering aankondiging voor Azure Blobs, bestanden, Table en Queue Storage](https://azure.microsoft.com/blog/announcing-default-encryption-for-azure-blobs-files-table-and-queue-storage/).
- Azure Active Directory (Azure AD) en op rollen gebaseerd toegangsbeheer (RBAC) worden ondersteund voor Azure Storage voor zowel resource management-bewerkingen en bewerkingen voor gegevens, als volgt:   
    - U kunt RBAC-rollen binnen het bereik van het opslagaccount dat aan beveiligings-principals en het gebruik van Azure AD te autoriseren resource management-bewerkingen zoals Sleutelbeheer toewijzen.
    - Azure AD-integratie wordt ondersteund in de Preview-versie van bewerkingen op de services Blob en wachtrij. U kunt RBAC-rollen binnen het bereik van een abonnement, resourcegroep, opslagaccount, of een afzonderlijke container of wachtrij wilt een beveiligings-principal of een beheerde identiteit voor Azure-resources kunt toewijzen. Zie voor meer informatie, [verifiëren van toegang tot Azure Storage met behulp van Azure Active Directory (Preview)](storage-auth-aad.md).   
- Gegevens in transit tussen een toepassing en Azure kunnen worden beveiligd met behulp van [Clientversleuteling](../storage-client-side-encryption.md), HTTPS en SMB 3.0.  
- Besturingssysteem en gegevensschijven die worden gebruikt door virtuele machines van Azure kunnen worden versleuteld met [Azure Disk Encryption](../../security/azure-security-disk-encryption.md). 
- Gedelegeerde toegang tot de gegevensobjecten in Azure Storage kan worden verleend met behulp van [Shared Access Signatures](../storage-dotnet-shared-access-signature-part-1.md).

Dit artikel bevat een overzicht van elk van deze beveiligingsfuncties die kunnen worden gebruikt met Azure Storage. Koppelingen zijn opgegeven voor artikelen die details van elke functie, kunt u eenvoudig doen nader onderzoek op elk onderwerp.

Hier volgen de onderwerpen in dit artikel worden besproken:

* [Beveiliging gegevenslaag](#management-plane-security) – uw Storage-Account beveiligen

  De beheerlaag bestaat uit de resources die worden gebruikt voor het beheren van uw storage-account. In deze sectie bevat informatie over het Azure Resource Manager-implementatiemodel en het gebruik van Role-Based Access Control (RBAC) voor het beheren van toegang tot uw storage-accounts. Deze ook een oplossing voor beheer van uw storage-accountsleutels en hoe u ze opnieuw genereren.
* [Gegevens vlak Security](#data-plane-security) : toegang tot uw gegevens beveiligen

  In deze sectie zullen we op het toestaan van toegang tot de objecten van de werkelijke gegevens in uw Storage-account, zoals blobs, bestanden, wachtrijen en tabellen, met behulp van handtekeningen voor gedeelde toegang en opgeslagen toegangsbeleid. We komen zowel serviceniveau SAS en SAS op accountniveau. We ziet ook hoe u wilt beperken van toegang tot een specifiek IP-adres (of een bereik van IP-adressen), het beperken van het protocol dat wordt gebruikt voor het HTTPS en hoe u een handtekening voor gedeelde toegang intrekken zonder te wachten tot het verlopen.
* [Versleuteling 'in transit'](#encryption-in-transit)

  In deze sectie wordt beschreven hoe u gegevens kunt beveiligen wanneer u deze naar of uit Azure Storage overdraagt. Bespreken we het aanbevolen gebruik van HTTPS en de versleuteling van SMB 3.0 voor Azure-bestandsshares. We zullen ook een overzicht van de Client-side-versleuteling, waarmee u de gegevens worden versleuteld voordat deze wordt overgedragen naar de opslag in een clienttoepassing en de gegevens ontsleutelen nadat deze zijn overgebracht geen opslagruimte meer.
* [Versleuteling 'at rest'](#encryption-at-rest)

  We zullen over SSE Storage Service Encryption (), die wordt nu automatisch ingeschakeld voor nieuwe en bestaande storage-accounts hebben. Er wordt ook het gebruik van Azure Disk Encryption en Verken de belangrijkste verschillen en aanvragen van schijfversleuteling versus SSE ten opzichte van de Client-Side Encryption kijken. Geef een korte kijken we FIPS-compatibiliteit voor VS Computers van de overheid.
* Met behulp van [Opslaganalyse](#storage-analytics) toegang van Azure Storage controleren

  In deze sectie wordt beschreven hoe u informatie vinden in de storage analytics-logboeken voor een aanvraag. We eens echte opslaganalyse logboekgegevens en zien hoe u te onderscheiden zijn of een aanvraag wordt gedaan met de opslagaccountsleutel, met een handtekening voor gedeelde toegang, of anoniem en of deze is geslaagd of mislukt.
* [Browser-gebaseerde Clients met behulp van CORS inschakelen](#Cross-Origin-Resource-Sharing-CORS)

  In deze sectie wordt besproken hoe u kunt cross-origin resource sharing (CORS) toestaan. Praten we over cross-domein toegang te verlenen, en hoe u verwerkt; het met de CORS-mogelijkheden die zijn ingebouwd in Azure Storage.

## <a name="management-plane-security"></a>Beveiliging gegevenslaag
De beheerlaag bestaat uit bewerkingen die invloed hebben op het opslagaccount zelf. Bijvoorbeeld, kunt u maken of verwijderen van een storage-account, een lijst met opslagaccounts in een abonnement ophalen, de storage-accountsleutels ophalen of de storage-accountsleutels opnieuw genereren.

Als u een nieuw opslagaccount maakt, selecteert u een van de klassieke of Resource Manager-implementatiemodel. Het klassieke model voor het maken van resources in Azure kan alleen niks toegang tot het abonnement en op zijn beurt de storage-account.

Deze handleiding is gericht op het Resource Manager-model dat is de aanbevolen methode voor het maken van de storage-accounts. Met het Resource Manager-opslagaccounts, in plaats van die toegang tot het hele abonnement, kunt u toegangsbeheer op het niveau van een meer beperkt tot de beheerlaag met behulp van toegangsbeheer op basis van rollen (RBAC).

### <a name="how-to-secure-your-storage-account-with-role-based-access-control-rbac"></a>Over het beveiligen van uw opslagaccount met Role-Based Access Control (RBAC)
Laten we eens wat RBAC is en hoe u deze kunt gebruiken. Elk Azure-abonnement heeft een Azure Active Directory. Gebruikers, groepen en toepassingen uit die map kunnen toegang tot het beheer van resources in het Azure-abonnement met het Resource Manager-implementatiemodel worden verleend. Dit type beveiliging wordt aangeduid als toegangsbeheer op basis van rollen (RBAC). Voor het beheren van deze toegang, kunt u de [Azure-portal](https://portal.azure.com/), wordt de [Azure CLI-hulpprogramma's](../../cli-install-nodejs.md), [PowerShell](/powershell/azureps-cmdlets-docs), of de [Azure Storage Resource Provider REST API's](https://msdn.microsoft.com/library/azure/mt163683.aspx).

Met het Resource Manager-model plaatst u het opslagaccount in een resource group en beheren van toegang tot de beheerlaag van dat specifieke opslagaccount met behulp van Azure Active Directory. Bijvoorbeeld, u kunt specifieke gebruikers geven de mogelijkheid voor toegang tot de opslagaccountsleutels, terwijl andere gebruikers informatie over de storage-account bekijken kunnen, maar geen toegang de sleutels voor het opslagaccount tot.

#### <a name="granting-access"></a>Het verlenen van toegang
Toegang wordt verleend door de juiste RBAC-rol toewijzen aan gebruikers, groepen en toepassingen, op het juiste bereik. Om toegang tot het hele abonnement, moet u een rol op het abonnementsniveau van het toewijzen. U kunt toegang verlenen tot alle resources in een resourcegroep door machtigingen te verlenen aan de resourcegroep zelf. U kunt ook specifieke rollen toewijzen aan specifieke resources, zoals storage-accounts.

Hier volgen de belangrijkste punten die u weten wilt over het gebruik van RBAC voor toegang tot de bewerkingen van een Azure Storage-account:

* Wanneer u toegang hebt toegewezen, toewijzen u in feite een rol aan het account dat u toegang wilt. U kunt toegang tot de bewerkingen die worden gebruikt voor het beheren van dat opslagaccount, maar niet aan de objecten in het account kunt beheren. Bijvoorbeeld, u kunt machtigen om op te halen van de eigenschappen van het storage-account (zoals redundantie), maar niet aan een container of de gegevens in een container in Blob Storage.
* Voor iemand om machtiging voor toegang tot de objecten in de storage-account, hebt u ze kunt geven, kunt u machtiging voor het lezen van de storage-accountsleutels en dat de gebruiker kan vervolgens die sleutels gebruiken voor toegang tot de blobs, wachtrijen, tabellen en bestanden.
* Rollen kunnen worden toegewezen aan een specifieke gebruikersaccount, een groep gebruikers, of naar een specifieke toepassing.
* Elke rol heeft een lijst van acties en niet-acties. De rol Inzender voor virtuele machines heeft bijvoorbeeld een actie van het 'listKeys' die ondersteuning biedt voor de storage-accountsleutels moet worden gelezen. De inzender heeft 'Niet acties' zoals het bijwerken van de toegang voor gebruikers in de Active Directory.
* Rollen voor opslag bevatten (maar niet beperkt tot) de volgende rollen:

  * Eigenaar: ze kunnen alles beheren, met inbegrip van toegang.
  * Inzender: ze kunnen doen de eigenaar kunt behalve toegang. Iemand met deze rol kunt weergeven en opnieuw genereren van sleutels voor de opslagaccount. Met de storage-accountsleutels ze toegang tot de gegevensobjecten.
  * Lezer: ze kunnen informatie over de storage-account, met uitzondering van geheimen weergeven. Bijvoorbeeld, als u een rol met leesmachtigingen voor het opslagaccount aan iemand toewijst, kunnen ze de eigenschappen van het storage-account bekijken, maar ze niet wijzigingen aanbrengen in de eigenschappen of weergeven van de sleutels voor het opslagaccount.
  * Inzender voor Opslagaccounts: ze kunnen de storage-account beheren: ze kunnen lezen van het abonnement resourcegroepen en resources, en maken en beheren van brongroepimplementaties abonnement. Ze hebben ook toegang tot de opslagaccountsleutels, wat op zijn beurt betekent dat ze toegang krijgen tot de gegevenslaag.
  * Beheerder van gebruikerstoegang – kunnen ze toegang tot het opslagaccount voor gebruikers beheren. Ze kunnen bijvoorbeeld lezerstoegang verlenen aan een specifieke gebruiker.
  * Inzender voor virtuele machines, kunnen ze virtuele machines, maar niet het opslagaccount waarmee ze zijn verbonden beheren. Deze rol kan de opslagaccountsleutels lijst, wat betekent dat de gebruiker aan wie u deze rol toewijzen op het vlak van gegevens kunt bijwerken.

    Ze hebben in de order voor een gebruiker een virtuele machine maken, kunnen de bijbehorende VHD-bestand in een opslagaccount maken. Om dit te doen, moeten ze kunnen de opslagaccountsleutel ophalen en deze doorgeven aan de API die het maken van de virtuele machine. Daarom moeten ze deze machtiging hebben zodat ze kunnen de storage-accountsleutels worden vermeld.
* De mogelijkheid voor het definiëren van aangepaste rollen is een functie waarmee u voor het opstellen van een reeks acties op basis van een lijst met beschikbare acties die kunnen worden uitgevoerd op Azure-resources.
* De gebruiker moet worden ingesteld in uw Azure Active Directory voordat u een rol aan deze toewijzen kunt.
* U kunt een rapport van die verleend/ingetrokken wat voor soort toegang naar/van wie en op welke bereik met behulp van PowerShell of Azure CLI maken.

#### <a name="resources"></a>Resources
* [Toegangsbeheer op basis van rollen in Azure Active Directory](../../role-based-access-control/role-assignments-portal.md)

  In dit artikel wordt het toegangsbeheer op basis van rollen in Azure Active Directory uitgelegd.
* [RBAC: ingebouwde rollen](../../role-based-access-control/built-in-roles.md)

  Dit artikel worden alle van de beschikbare ingebouwde rollen in RBAC.
* [Resource Manager-implementatie en klassieke implementatie begrijpen](../../azure-resource-manager/resource-manager-deployment-model.md)

  Dit artikel worden de Resource Manager-implementatie en het klassieke implementatiemodel, en wordt uitgelegd van de voordelen van het gebruik van de Resource Manager en resource-groepen. Hierin wordt uitgelegd hoe de Azure Compute, Network en Storage Providers onder het Resource Manager-model werken.
* [Toegangsbeheer op basis van rollen beheren met de REST-API](../../role-based-access-control/role-assignments-rest.md)

  Dit artikel beschrijft hoe u de REST-API gebruikt om RBAC te beheren.
* [Azure Storage Resource Provider REST API-verwijzing](https://msdn.microsoft.com/library/azure/mt163683.aspx)

  Deze API-verwijzing beschrijving van de API's kunt u uw storage-account op programmatische wijze beheren.
* [Gebruik Resource Manager verificatie-API voor toegang tot abonnementen](../../azure-resource-manager/resource-manager-api-authentication.md)

  Dit artikel leest hoe u verifieert met behulp van de Resource Manager-API's.
* [Toegangsbeheer op basis van rollen voor Microsoft Azure vanuit Ignite](https://channel9.msdn.com/events/Ignite/2015/BRK2707)

  Dit is een koppeling naar een video op Channel 9 van de vergadering over MS Ignite in 2015. In deze sessie wordt ingegaan op de opties voor toegangsbeheer en rapportage in Azure en worden aanbevolen procedures behandeld voor de beveiliging van de toegang tot Azure-abonnementen met Azure Active Directory.

### <a name="managing-your-storage-account-keys"></a>De sleutels van uw Storage-Account beheren
De opslagaccountsleutels zijn 512-bits-tekenreeksen die zijn gemaakt door Azure die samen met de naam van het opslagaccount, kan worden gebruikt voor toegang tot de objecten die zijn opgeslagen in de storage-account, bijvoorbeeld, blobs, entiteiten in een tabel, berichten in wachtrij plaatsen en bestanden op een Azure-bestandsshare. Beheren van toegang tot de storage-account sleutels controleert de toegang tot de gegevenslaag voor dat opslagaccount.

Elk opslagaccount heeft twee sleutels die worden aangeduid als "Sleutel 1" en "Sleutel 2" in de [Azure-portal](http://portal.azure.com/) en in de PowerShell-cmdlets. Deze kunnen worden hersteld handmatig met behulp van een van de verschillende methoden, met inbegrip van, maar niet beperkt tot met behulp van de [Azure-portal](https://portal.azure.com/), PowerShell, de Azure CLI of programmatisch met behulp van de Storage-clientbibliotheek voor .NET of de Azure Storage-Services REST-API.

Er zijn een aantal redenen uw storage-accountsleutels opnieuw genereren.

* U kunt ze opnieuw genereren op gezette tijden voor opmaaktalen wordt om beveiligingsredenen.
* Als iemand beheerd inbreken in een toepassing en ophalen van de sleutel die is vastgelegd of opgeslagen in een configuratiebestand, zodat ze volledige toegang tot uw storage-account, zou u uw storage-accountsleutels opnieuw genereren.
* Een andere aanvraag voor de sleutel opnieuw genereren is als uw team met behulp van een Storage Explorer-toepassing die de opslagaccountsleutel behoudt, en een van de teamleden verlaat. De toepassing nu doorgaan met werken, zodat ze toegang hebben tot uw storage-account nadat ze verdwenen zijn. Dit is eigenlijk de primaire reden die ze accountniveau Shared Access Signatures gemaakt – u kunt een SAS op accountniveau gebruiken in plaats van de toegangssleutel in een configuratiebestand.

#### <a name="key-regeneration-plan"></a>Plan voor sleutel opnieuw genereren
U wilt niet dat alleen opnieuw genereren van de sleutel die u zonder een planning gebruikt. Als u dat doet, kan u alle toegang naar dit opslagaccount, wat leiden onderbreking van de belangrijkste tot kan afgekapt. Dit is de reden waarom er twee sleutels zijn. U moet één sleutel opnieuw genereren op een tijdstip.

Voordat u uw sleutels opnieuw genereren, moet u er een lijst met al uw toepassingen die afhankelijk zijn van het storage-account, evenals andere services die u gebruikt in Azure. Bijvoorbeeld, als u van Azure Media Services die afhankelijk van uw opslagaccount gebruikmaakt zijn, u moet toegangssleutels opnieuw synchroniseren de met uw mediaservice nadat u de sleutel opnieuw genereren. Als u alle toepassingen, zoals storage explorer gebruikt, moet u voor de nieuwe sleutels voor deze toepassingen. Als u virtuele machines waarvan VHD-bestanden worden opgeslagen in de storage-account hebt, worden ze niet beïnvloed door de storage-accountsleutels opnieuw te genereren.

U kunt uw sleutels in Azure portal opnieuw genereren. Wanneer sleutels worden opnieuw gegenereerd, kunnen ze tot 10 minuten worden gesynchroniseerd binnen de Storage-Services.

Wanneer u klaar bent, wordt hier het algemene proces met gedetailleerde informatie over hoe u uw sleutel te wijzigen. In dit geval wordt ervan uitgegaan dat u momenteel van sleutel 1 gebruikmaakt en u alle instellingen gaat voor het gebruik van sleutel 2 in plaats daarvan wijzigen.

1. Genereer een nieuwe sleutel 2 om ervoor te zorgen dat deze beveiligd worden. U kunt dit doen in Azure portal.
2. In alle toepassingen waarin de opslagsleutel is opgeslagen, wijzigen de toegangssleutel voor het gebruik van de nieuwe waarde van de sleutel 2. Testen en publiceren van de toepassing.
3. Nadat alle van de toepassingen en services actief zijn en dat is uitgevoerd, opnieuw met het genereren van sleutel 1. Dit zorgt ervoor dat iedereen aan wie u niet expliciet de nieuwe sleutel hebben niet langer toegang tot het opslagaccount hebben wordt.

Als u momenteel sleutel 2 gebruikt, kunt u hetzelfde proces gebruiken, maar de sleutelnamen omkeren.

U kunt migreren via een paar dagen, wijzigen van elke toepassing om de nieuwe sleutel te gebruiken en deze publiceren. Nadat ze allemaal zijn voltooid, moet u vervolgens gaat u terug en de oude sleutel opnieuw genereren, zodat deze niet meer werkt.

Een andere optie is om de opslagaccountsleutel een [Azure Key Vault](https://azure.microsoft.com/services/key-vault/) als een geheim en zijn uw toepassingen ophalen van de toegangssleutel van daaruit. Klik wanneer u de sleutel opnieuw genereren en bijwerken van de Azure Key Vault, hoeft de toepassingen niet te worden geïmplementeerd omdat ze gewoon door de nieuwe sleutel van de Azure Key Vault automatisch. Houd er rekening mee dat u kunt de toepassing telkens wanneer die u ze nodig hebt voor de sleutel lezen hebben, of u kunt opslaan in cache deze in het geheugen en als dit mislukt wanneer u de sleutel opnieuw ophalen uit de Azure Key Vault.

Een ander niveau van beveiliging voor uw storage-sleutels met behulp van Azure Key Vault ook worden toegevoegd. Als u deze methode gebruikt, hebt u nooit de opslag sleutel vastgelegd in een configuratiebestand, waardoor deze doorverwezen iemand de toegang tot de sleutels zonder de uitdrukkelijke toestemming worden verwijderd.

Een ander voordeel van het gebruik van Azure Key Vault is dat u kunt ook toegang tot uw sleutels met behulp van Azure Active Directory beheren. Dit betekent dat u kunt toegang verlenen tot het aantal toepassingen waarvoor de sleutels ophalen uit Azure Key Vault en weten dat andere toepassingen pas weer toegang krijgen tot de sleutels zonder deze machtiging verlenen specifiek.

> [!NOTE]
> Microsoft adviseert om slechts één van de sleutels in al uw toepassingen op hetzelfde moment. Als u met sleutel 1 op bepaalde plaatsen en sleutel 2 in andere, wordt het niet mogelijk uw om sleutels te rouleren zonder dat een toepassing toegang verliezen.

#### <a name="resources"></a>Resources

* [Instellingen van opslagaccount in Azure portal beheren](storage-account-manage.md)
* [Azure Storage Resource Provider REST API-verwijzing](https://msdn.microsoft.com/library/mt163683.aspx)

## <a name="data-plane-security"></a>Beveiliging van gegevens vlak
Beveiliging van gegevens vlak verwijst naar de methoden voor het beveiligen van de gegevensobjecten die zijn opgeslagen in Azure Storage: blobs, wachtrijen, tabellen en bestanden. We hebben gezien methoden voor het versleutelen van de gegevens en de beveiliging tijdens de overdracht van de gegevens, maar hoe u doen over het beheren van toegang tot de objecten?

Hebt u drie opties voor het verlenen van toegang tot objecten in Azure Storage, met inbegrip van:

- Met behulp van Azure AD om toegang tot containers en wachtrijen (Preview) te verlenen. Azure AD biedt voordelen ten opzichte van andere methoden voor autorisatie, waaronder de noodzaak voor het opslaan van geheimen in uw code verwijderen. Zie voor meer informatie, [verifiëren van toegang tot Azure Storage met behulp van Azure Active Directory (Preview)](storage-auth-aad.md). 
- Met behulp van de sleutels van uw storage-account om toegang via gedeelde sleutel te verlenen. Autoriseren via gedeelde sleutel moet opslaan van de sleutels van uw storage-account in uw toepassing, zodat Microsoft raadt het Azure AD in plaats daarvan gebruik waar mogelijk. Voor productie-Apps, of voor het verlenen van toegang tot Azure-tabellen en bestanden, voert u de gedeelde sleutel gebruiken terwijl Azure AD-integratie beschikbaar als preview is.
- Met behulp van handtekeningen voor gedeelde toegang te verlenen omschreven machtigingen beschikt voor specifieke objecten gedurende een bepaalde periode.

Bovendien voor Blob Storage kunt u openbare toegang tot uw BLOB door in te stellen het toegangsniveau voor de container met de blobs dienovereenkomstig. Als u toegang hebt ingesteld voor een container voor Blob of Container, kunnen er openbare leestoegang voor de blobs in deze container worden. Dit betekent dat iedereen met een URL die verwijst naar een blob in die container kunt openen in een browser zonder met behulp van een Shared Access Signature of met de storage-accountsleutels.

Naast het beperken van toegang tot en met autorisatie, u kunt ook gebruiken [Firewalls en virtuele netwerken](storage-network-security.md) om te beperken van toegang tot het opslagaccount op basis van netwerkregels.  Met deze benadering kan weigeren van toegang tot openbare internetverkeer en verlenen toegang tot alleen specifieke Azure Virtual Networks of openbare internet IP-adresbereiken.

### <a name="storage-account-keys"></a>Opslagaccountsleutels
Opslagaccountsleutels zijn 512-bits-tekenreeksen die zijn gemaakt door Azure, die samen met de naam van het opslagaccount, kan worden gebruikt voor toegang tot de gegevensobjecten die zijn opgeslagen in de storage-account.

U kunt bijvoorbeeld BLOB's lezen, schrijven naar wachtrijen, tabellen maken en wijzigen van bestanden. Veel van deze acties kunnen worden uitgevoerd via de Azure-portal of met behulp van een van de vele Storage Explorer-toepassingen. U kunt ook schrijven van code voor het gebruik van de REST-API of een van de Opslagclientbibliotheken deze bewerkingen uit te voeren.

Zoals beschreven in de sectie op de [Management vlak Security](#management-plane-security), toegang tot de opslagsleutels voor een klassiek opslagaccount kan worden verleend door middel van een volledige toegang tot de Azure-abonnement. Toegang tot de opslagsleutels voor een opslagaccount met behulp van de Azure Resource Manager-model kan worden beheerd via op rollen gebaseerd toegangsbeheer (RBAC).

### <a name="how-to-delegate-access-to-objects-in-your-account-using-shared-access-signatures-and-stored-access-policies"></a>Het overdragen van toegang tot objecten in uw account met behulp van handtekeningen voor gedeelde toegang en opgeslagen toegangsbeleid
Een Shared Access Signature is een tekenreeks met een beveiligingstoken dat kan worden gekoppeld aan een URI waarmee u toegang tot opslagobjecten delegeren en beperkingen, zoals de machtigingen en de datum-/ tijdbereik van toegang opgeven.

U kunt toegang verlenen tot blobs, containers, berichten in wachtrij plaatsen, bestanden en tabellen. Met tabellen, kunt u daadwerkelijk machtiging voor toegang tot een bereik van entiteiten in de tabel door op te geven van de partitie- en recordsleutels sleutelbereiken waarnaar u de gebruiker toegang wilt verlenen. Bijvoorbeeld, als u gegevens die zijn opgeslagen met een partitiesleutel van geografische status hebben, kan geeft u iemand toegang tot alleen de gegevens voor Californië.

Een ander voorbeeld: mogelijk u geven een web-App een SAS-token waarmee deze gegevens schrijven naar een wachtrij en geef een werknemer rol toepassing een SAS-token aan berichten ophalen uit de wachtrij en ze verwerken. Of u kunt één klant geven een SAS-token mogen gebruiken om afbeeldingen te uploaden naar een container in Blob-opslag en een web toepassing machtigt om te lezen die foto's. In beide gevallen wordt er een scheiding van taken – elke toepassing kan alleen de toegang die ze nodig hebben om uit te voeren hun taak worden verleend. Dit is mogelijk door het gebruik van handtekeningen voor gedeelde toegang.

#### <a name="why-you-want-to-use-shared-access-signatures"></a>Waarom u wilt gebruiken van handtekeningen voor gedeelde toegang
Waarom wilt u een SAS in plaats van alleen geven van de sleutel van uw opslagaccount, dit dus veel eenvoudiger is te gebruiken? Doorgeven van de sleutel van uw opslagaccount is vergelijkbaar met de sleutels van uw opslag Koninkrijk delen. Deze verleent volledige toegang. Iemand kan uw sleutels te gebruiken en hun volledige muziekbibliotheek uploaden naar uw opslagaccount. Ze kunnen ook uw bestanden vervangen door versies geïnfecteerde of uw gegevens te stelen. Direct onbeperkte toegang geven tot uw storage-account is iets dat geen licht moeten worden genomen.

Met handtekeningen voor gedeelde toegang, kunt u een client alleen de machtigingen die vereist zijn voor een beperkte hoeveelheid van de tijd geven. Bijvoorbeeld, als iemand anders een blob naar uw account uploaden wordt, verleent u hen toegang voor schrijven voor net voldoende tijd voor het uploaden van de blob (afhankelijk van de grootte van de blob natuurlijk). En als u van gedachten verandert, kunt u dat de toegang intrekken.

Daarnaast kunt u opgeven dat aanvragen met behulp van een SAS beperkt tot een bepaalde IP-adres of IP-adresbereik externe naar Azure zijn. U kunt ook vereisen dat aanvragen worden gedaan met behulp van een specifieke protocol (HTTPS of HTTP/HTTPS). Dit betekent dat als u wilt dat alleen HTTPS-verkeer toe te staan, u het vereiste protocol alleen op HTTPS instellen kunt en HTTP-verkeer wordt geblokkeerd.

#### <a name="definition-of-a-shared-access-signature"></a>Definitie van een Shared Access Signature
Een Shared Access Signature is een set van query-parameters die zijn toegevoegd aan de URL die verwijst naar de resource

dat biedt informatie over de toegang is toegestaan en de tijdsduur waarvoor de toegang is toegestaan. Hier volgt een voorbeeld. Deze URI biedt leestoegang tot een blob gedurende vijf minuten. Houd er rekening mee dat SAS queryparameters moet URL-gecodeerde, zoals % 3A voor dubbele punt (:) of 20% van een spatie.

```
http://mystorage.blob.core.windows.net/mycontainer/myblob.txt (URL to the blob)
?sv=2015-04-05 (storage service version)
&st=2015-12-10T22%3A18%3A26Z (start time, in UTC time and URL encoded)
&se=2015-12-10T22%3A23%3A26Z (end time, in UTC time and URL encoded)
&sr=b (resource is a blob)
&sp=r (read access)
&sip=168.1.5.60-168.1.5.70 (requests can only come from this range of IP addresses)
&spr=https (only allow HTTPS requests)
&sig=Z%2FRHIX5Xcg0Mq2rqI3OlWTjEg2tYkboXr1P9ZUXDtkk%3D (signature used for the authentication of the SAS)
```

#### <a name="how-the-shared-access-signature-is-authorized-by-the-azure-storage-service"></a>Hoe de Shared Access Signature is geautoriseerd door de Azure Storage-Service
Wanneer de aanvraag wordt ontvangen door de storage-service, de invoer voor de query-parameters en een handtekening met behulp van dezelfde methode als het aanroepende programma gemaakt. Vervolgens worden de twee handtekeningen vergeleken. Als ze akkoord gaat, kunt de versie van het storage-service om te controleren of dat de parameter is geldig, Controleer of de huidige datum en tijd binnen het opgegeven venster zijn, zorg ervoor dat de toegang tot de aangevraagde komt overeen met de aanvraag is ingediend, enzovoort controleren door de storage-service.

Bijvoorbeeld, met onze URL mislukken als de URL is verwijst naar een bestand in plaats van een blob, dit verzoek omdat deze geeft aan dat de handtekening voor gedeelde toegang voor een blob. Als de REST-opdracht die wordt aangeroepen om bij te werken van een blob is, zou deze mislukken omdat de handtekening voor gedeelde toegang geeft aan dat alleen lezen-toegang is toegestaan.

#### <a name="types-of-shared-access-signatures"></a>Typen van handtekeningen voor gedeelde toegang
* Een service level SAS kan worden gebruikt voor toegang tot bepaalde resources in een storage-account. Enkele voorbeelden hiervan zijn bij het ophalen van een lijst met blobs in een container, een blob te downloaden, bijwerken van een entiteit in een tabel, berichten toe te voegen aan een wachtrij of een bestand uploadt naar een bestandsshare.
* Een SAS op accountniveau kan worden gebruikt voor toegang tot alles dat een SAS serviceniveau kan worden gebruikt voor. Bovendien geeft de opties in de bronnen die niet zijn toegestaan als een service level SAS, zoals de mogelijkheid om te maken van containers, tabellen, wachtrijen en bestandsshares. U kunt ook toegang tot meerdere services in één keer opgeven. Bijvoorbeeld, u iemand mogelijk geven toegang tot blobs en bestanden in uw storage-account.

#### <a name="creating-a-sas-uri"></a>Het maken van een SAS-URI
1. U kunt een URI maken op aanvraag, alle van de queryparameters telkens definiëren.

   Deze aanpak is flexibel, maar hebt u een logische set parameters die vergelijkbaar telkens zijn, met behulp van een opgeslagen toegangsbeleid is een beter idee.
2. U kunt een opgeslagen toegangsbeleid voor een volledige container, een bestandsshare, een tabel of een wachtrij maken. Vervolgens kunt u dit als basis voor de SAS-URI's die u maakt. Machtigingen op basis van opgeslagen toegangsbeleid kunnen eenvoudig worden ingetrokken. U kunt maximaal vijf beleid gedefinieerd voor elke container, wachtrij, tabel of bestandsshare hebben.

   Als u meer zijn veel mensen lezen van de blobs in een specifieke container hebt, kunt u bijvoorbeeld een opgeslagen toegangsbeleid met de tekst 'leestoegang geven' en alle andere instellingen die hetzelfde telkens wanneer zijn maken. Vervolgens maakt u een SAS-URI met behulp van de instellingen van het toegangsbeleid opgeslagen en de vervaldatum van de datum en tijd op te geven. Het voordeel hiervan is dat u alle op te geven van de queryparameters telkens geen hebt.

#### <a name="revocation"></a>Intrekken
Stel dat de SAS is aangetast, of u wilt wijzigen vanwege zakelijke beveiligings- of wettelijke vereisten. Hoe u toegang tot een resource met behulp van deze SAS intrekken? Dat hangt ervan af hoe u de SAS-URI hebt gemaakt.

Als u ad-hoc URI's gebruikt, hebt u drie opties. U kunt SAS-tokens uitgeven met korte verloopbeleid en wacht tot de SAS is verlopen. U kunt wijzigen of verwijderen van de resource (ervan uitgaande dat het token is afgestemd op een enkel object). U kunt de opslagaccountsleutels wijzigen. Deze laatste optie kan een aanzienlijke impact, afhankelijk van hoeveel services met behulp van dit account, en is waarschijnlijk niet iets dat die u doen wilt zonder een planning.

Als u van een SAS die is afgeleid van een toegangsbeleid die zijn opgeslagen gebruikmaakt, kunt u toegang verwijderen door het intrekken van het toegangsbeleid die zijn opgeslagen – u kunt deze alleen wijzigen zodat deze al is verlopen, of kunt u deze helemaal verwijderen. Dit wordt direct van kracht en elke SAS gemaakt met behulp van deze opgeslagen toegangsbeleid wordt ongeldig. Het bijwerken of verwijderen van de opgeslagen toegangsbeleid kunnen invloed personen met toegang tot die specifieke container, bestandsshare, tabel of wachtrij via SAS, maar als de clients zijn geschreven, zodat ze een nieuwe SAS aanvragen wanneer de oude heeft ongeldige, wordt dit werkt prima.

Omdat met behulp van een SAS die is afgeleid van een opgeslagen toegangsbeleid u de mogelijkheid om in te trekken die SAS onmiddellijk biedt, is de aanbevolen toegangsbeleid opgeslagen indien mogelijk altijd wilt gebruiken.

#### <a name="resources"></a>Resources
Raadpleeg de volgende artikelen voor meer gedetailleerde informatie over het gebruik van handtekeningen voor gedeelde toegang en opgeslagen toegangsbeleid, inclusief voorbeelden:

* Dit zijn de referentie-artikelen.

  * [Service-SAS](https://msdn.microsoft.com/library/dn140256.aspx)

    In dit artikel bevat voorbeelden van het gebruik van een SAS serviceniveau met blobs, berichten in de wachtrij, tabel-bereiken en bestanden.
  * [Maken van een service-SAS](https://msdn.microsoft.com/library/dn140255.aspx)
  * [Maken van een account-SAS](https://msdn.microsoft.com/library/mt584140.aspx)
* Dit zijn de zelfstudies voor het gebruik van de .NET-clientbibliotheek om handtekeningen voor gedeelde toegang en opgeslagen toegangsbeleid te maken.

  * [Shared Access Signatures (SAS) gebruiken](../storage-dotnet-shared-access-signature-part-1.md)
  * [Shared Access Signatures, deel 2: Maken en gebruiken van een SAS met de Blob-Service](../blobs/storage-dotnet-shared-access-signature-part-2.md)

    Dit artikel bevat een uitleg van de SAS-model, voorbeelden van handtekeningen voor gedeelde toegang en aanbevelingen voor het beste gebruik van SAS. Ook wordt besproken, is het intrekken van de machtiging is verleend.

* Verificatie

  * [Verificatie voor de Azure Storage-Services](https://msdn.microsoft.com/library/azure/dd179428.aspx)
* Shared Access Signatures aan de slag zelfstudie

  * [Aan de slag zelfstudie SAS](https://github.com/Azure-Samples/storage-dotnet-sas-getting-started)

## <a name="encryption-in-transit"></a>Versleuteling tijdens overdracht
### <a name="transport-level-encryption--using-https"></a>Versleuteling op transportniveau – via HTTPS
Er is een andere stap die u nemen moet om te controleren of de beveiliging van uw Azure Storage-gegevens voor het versleutelen van gegevens tussen de client en de Azure Storage. De eerste aanbeveling is altijd gebruik van de [HTTPS](https://en.wikipedia.org/wiki/HTTPS) -protocol, dat zorgt ervoor veilige communicatie via het openbare Internet dat.

Als u een beveiligd communicatiekanaal wilt weergeven, moet u altijd HTTPS gebruiken tijdens de REST API's aanroepen of toegang tot in de opslag objecten. Bovendien **Shared Access Signatures**, die kan worden gebruikt voor toegang tot Azure Storage-objecten overdragen, een optie om op te geven dat alleen het HTTPS-protocol kan worden gebruikt bij het gebruik van handtekeningen voor gedeelde toegang, ervoor zorgen dat iedereen verzenden koppelingen met SAS tokens het juiste protocol gebruikt.

U kunt het gebruik van HTTPS afdwingen bij het aanroepen van de REST-API's voor toegang tot objecten in de storage-accounts door in te schakelen [veilige overdracht vereist](../storage-require-secure-transfer.md) voor het opslagaccount. Verbindingen met behulp van HTTP zal worden geweigerd als deze optie is ingeschakeld.

### <a name="using-encryption-during-transit-with-azure-file-shares"></a>Met behulp van versleuteling tijdens de overdracht met Azure-bestandsshares
[Azure Files](../files/storage-files-introduction.md) ondersteunt versleuteling via SMB 3.0 en HTTPS bij het gebruik van de File REST-API. Wanneer koppelen buiten de Azure-regio van de Azure-bestandsshare zich bevindt, bijvoorbeeld on-premises of in een andere Azure-regio, is SMB 3.0 met-versleuteling altijd vereist. SMB 2.1 biedt geen ondersteuning voor versleuteling, zodat standaard alleen verbindingen zijn toegestaan binnen dezelfde regio in Azure, maar SMB 3.0 met-versleuteling kan worden afgedwongen door [veilige overdracht vereisen](../storage-require-secure-transfer.md) voor het opslagaccount.

SMB 3.0 met-codering is beschikbaar in [alle ondersteunde besturingssystemen voor Windows en Windows Server](../files/storage-how-to-use-files-windows.md) met uitzondering van Windows 7 en Windows Server 2008 R2, die alleen ondersteuning voor SMB 2.1. SMB 3.0 wordt ook ondersteund op [macOS](../files/storage-how-to-use-files-mac.md) en in-distributie van [Linux](../files/storage-how-to-use-files-linux.md) met behulp van Linux-kernel 4.11 en hoger. Ondersteuning voor SMB 3.0 is ook backported tot oudere versies van de Linux-kernel door verschillende Linux-distributies, raadpleegt u [clientvereisten voor informatie over SMB](../files/storage-how-to-use-files-linux.md#smb-client-reqs).

### <a name="using-client-side-encryption-to-secure-data-that-you-send-to-storage"></a>Met behulp van Client-side-versleuteling voor het beveiligen van gegevens die u naar opslag verzendt
Een andere optie waarmee u ervoor te zorgen dat uw gegevens goed beveiligd worden terwijl ze worden overgebracht tussen een clienttoepassing en -opslag is de Client-side-versleuteling. De gegevens worden versleuteld voordat ze worden overgedragen naar Azure Storage. Bij het ophalen van de gegevens uit Azure Storage, worden de gegevens worden ontsleuteld nadat deze zijn ontvangen op de client. Zelfs als de gegevens worden versleuteld gaan via de kabel, raden wij aan dat u ook HTTPS, omdat er gegevens integriteitscontroles ingebouwde die zodat netwerkfouten die betrekking hebben op de integriteit van de gegevens worden verminderd.

Clientversleuteling is ook een methode voor het versleutelen van uw gegevens in rust, als de gegevens worden opgeslagen in versleutelde vorm. Bespreken we dit in de sectie in meer detail op [versleuteling-at-Rest](#encryption-at-rest).

## <a name="encryption-at-rest"></a>Versleuteling-at-Rest
Er zijn drie Azure-functies die bieden van versleuteling-at-rest. Azure Disk Encryption wordt gebruikt voor het versleutelen van het besturingssysteem en gegevensschijven in IaaS virtuele Machines. Client-side Encryption en SSE beide worden gebruikt voor het versleutelen van gegevens in Azure Storage. 

U kunt de Client-side-versleuteling gebruiken voor het versleutelen van de gegevens die onderweg zijn (dit wordt ook opgeslagen in versleutelde vorm in de opslag), wellicht kunt u beter gebruik van HTTPS tijdens de overdracht en hebben enkele manier waarop de gegevens worden automatisch versleuteld wanneer deze is opgeslagen. Er zijn twee manieren hiervoor--Azure Disk Encryption en SSE. Een wordt gebruikt voor het versleutelen van de gegevens op het besturingssysteem en gegevensschijven die wordt gebruikt door virtuele machines rechtstreeks en de andere wordt gebruikt voor het versleutelen van gegevens die naar Azure Blob-opslag geschreven.

### <a name="storage-service-encryption-sse"></a>Storage Service Encryption (SSE)

SSE is ingeschakeld voor alle opslagaccounts en kan niet worden uitgeschakeld. SSE versleutelt automatisch uw gegevens bij het schrijven van deze naar Azure Storage. Wanneer u gegevens uit Azure Storage leest, wordt er door Azure Storage ontsleuteld voordat het wordt geretourneerd. Met SSE kunt u uw gegevens te beveiligen zonder code te wijzigen of voeg code toe aan alle toepassingen.

U kunt Microsoft beheerde sleutels of uw eigen aangepaste sleutels gebruiken. Microsoft beheerde sleutels genereert en hun beveiligde opslag, evenals hun reguliere roteren, zoals gedefinieerd door het interne Microsoft-beleid worden verwerkt. Zie voor meer informatie over het gebruik van aangepaste sleutels [Storage Service Encryption door de klant beheerde sleutels in Azure Key Vault](storage-service-encryption-customer-managed-keys.md).

SSE versleutelt automatisch gegevens in alle prestatielagen (Standaard en Premium), alle implementatiemodellen (Azure Resource Manager en het klassieke model) en alle services van Azure Storage (Blob, Queue, Table en File). 

### <a name="client-side-encryption"></a>Clientversleuteling
We vermeld clientversleuteling wanneer u de versleuteling van de gegevens die onderweg zijn. Deze functie kunt u uw gegevens in een clienttoepassing voordat deze via de kabel wordt verzonden naar Azure Storage worden geschreven, en voor het ontsleutelen van uw gegevens via een programma bij het ophalen van uit de Azure-opslag programmatisch te versleutelen.

Dit biedt versleuteling tijdens overdracht, maar biedt ook de functie van versleuteling-at-Rest. Hoewel de gegevens in transit zijn versleuteld, raden wij nog steeds gebruik van HTTPS om te profiteren van de ingebouwde integriteitscontroles die zodat netwerkfouten die betrekking hebben op de integriteit van de gegevens worden verminderd.

Een voorbeeld van waar u deze kunt gebruiken is als u een webtoepassing die wordt opgeslagen blobs en blobs opgehaald hebt en u wilt dat de toepassing en gegevens net zo veilig mogelijk. In dat geval gebruikt u client-side-versleuteling. Het verkeer tussen de client en de Azure Blob-Service bevat de versleutelde resource en niemand kan interpreteren van de gegevens die onderweg zijn en deze naar uw persoonlijke blobs reconstrueren.

Clientversleuteling is ingebouwd in de Java en .NET storage-clientbibliotheken, die op zijn beurt de Azure Key Vault API's gebruiken, zodat u eenvoudig voor u om te implementeren. Het proces van het versleutelen en ontsleutelen van de gegevens gebruikt de envelop techniek en slaat metagegevens op die worden gebruikt door de versleuteling in elke opslagobject. Bijvoorbeeld, voor blobs, wordt opgeslagen in de metagegevens van de blob, terwijl voor wachtrijen, deze toegevoegd aan het bericht voor elke wachtrij.

U kunt genereren en beheren van uw eigen versleutelingssleutels voor de versleuteling zelf. U kunt ook sleutels die worden gegenereerd door de Azure Storage-clientbibliotheek gebruiken of u kunt de Azure Key Vault genereren van de sleutels hebben. U kunt uw versleutelingssleutels opslaan in uw on-premises sleutel opslag of u ze kunt opslaan in een Azure Key Vault. Azure Key Vault kunt u voor het verlenen van toegang tot de geheimen in Azure Key Vault aan specifieke gebruikers met Azure Active Directory. Dit betekent dat niet alleen iedereen kan lezen van de Azure Key Vault en ophalen van de sleutels die u voor versleuteling van de client-side gebruikt.

#### <a name="resources"></a>Resources
* [Blobs in Microsoft Azure Storage met behulp van Azure Key Vault versleutelen en ontsleutelen](../blobs/storage-encrypt-decrypt-blobs-key-vault.md)

  In dit artikel laat zien hoe clientversleuteling gebruiken met Azure Key Vault, inclusief het maken van de KEK-sleutel en op te slaan in de kluis met behulp van PowerShell.
* [Client-Side-versleuteling en Azure Key Vault voor Microsoft Azure Storage](../storage-client-side-encryption.md)

  Dit artikel geeft een uitleg van de client-side-versleuteling en bevat voorbeelden van het gebruik van de storage-clientbibliotheek voor het versleutelen en ontsleutelen van resources van de vier storage-services. Het is ook het vertelt over Azure Key Vault.

### <a name="using-azure-disk-encryption-to-encrypt-disks-used-by-your-virtual-machines"></a>Met behulp van Azure Disk Encryption voor het versleutelen van schijven die worden gebruikt door uw virtuele machines
Azure Disk Encryption is een nieuwe functie. Deze functie kunt u voor het versleutelen van de besturingssysteemschijven en gegevensschijven die zijn gebruikt door een virtuele IaaS-Machine. De stations zijn versleuteld met versleutelingstechnologie van industriestandaard BitLocker voor Windows. De schijven worden versleuteld met behulp van de technologie DM-Crypt voor Linux. Dit is geïntegreerd met Azure Key Vault kunt u controleren en beheren van de versleutelingssleutels voor de schijf.

De oplossing ondersteunt de volgende scenario's voor IaaS-VM's als ze zijn ingeschakeld in Microsoft Azure:

* Integratie met Azure Key Vault
* Standard-laag virtuele machines: [A, D, DS, G, GS en enzovoort reeks IaaS-VM's](https://azure.microsoft.com/pricing/details/virtual-machines/)
* Inschakelen van versleuteling op Windows en Linux IaaS-VM 's
* Uitschakelen van versleuteling op besturingssysteem en schijven voor Windows IaaS-VM 's
* Codering op gegevensstations voor Linux IaaS-VM's uitschakelen
* Inschakelen van versleuteling op IaaS-VM's met Windows-clientbesturingssysteem
* Versleuteling voor volumes met koppelpunt paden is ingeschakeld
* Inschakelen van versleuteling op Linux-VM's die zijn geconfigureerd met de schijf (RAID) met behulp van mdadm striping
* Inschakelen van versleuteling op Linux-VM's met behulp van LVM voor gegevensschijven
* Inschakelen van versleuteling op Windows-VM's die zijn geconfigureerd met behulp van opslagruimten
* Alle Azure openbare regio's worden ondersteund

De oplossing ondersteunt niet de volgende scenario's, functies en -technologie in de release:

* Basic-laag IaaS-VM 's
* Uitschakelen van versleuteling op een station van het besturingssysteem voor Linux IaaS-VM 's
* IaaS-VM's die zijn gemaakt met behulp van de klassieke methode voor het maken van de virtuele machine
* Integratie met uw on-premises Key Management Service
* Azure Files (gedeelde bestandssysteem), Network File System (NFS), dynamische volumes en virtuele machines van Windows die zijn geconfigureerd met software gebaseerde RAID-systemen


> [!NOTE]
> Linux-besturingssysteem schijfversleuteling wordt momenteel ondersteund in de volgende Linux-distributies: RHEL 7.2 CentOS 7.2n en Ubuntu 16.04.
>
>

Deze functie zorgt ervoor dat alle gegevens op schijven van uw virtuele machine is versleuteld in rust in Azure Storage.

#### <a name="resources"></a>Resources
* [Azure Disk Encryption voor Windows en Linux IaaS-VM 's](https://docs.microsoft.com/azure/security/azure-security-disk-encryption)

### <a name="comparison-of-azure-disk-encryption-sse-and-client-side-encryption"></a>Vergelijking van Azure Disk Encryption, SSE en Client-Side-versleuteling

#### <a name="iaas-vms-and-their-vhd-files"></a>IaaS-VM's en hun VHD-bestanden

Azure Disk Encryption wordt aanbevolen voor data-schijven die worden gebruikt door virtuele IaaS-machines. Als u een virtuele machine met niet-beheerde schijven met een installatiekopie uit de Azure Marketplace maken, Azure voert een [recente kopie](https://en.wikipedia.org/wiki/Object_copying) van de installatiekopie naar uw storage-account in Azure Storage en het is niet versleuteld, zelfs als u SSE ingeschakeld. Nadat de virtuele machine wordt gemaakt en gestart met het bijwerken van de installatiekopie, start SSE versleutelen van gegevens. Daarom is het raadzaam te gebruiken van Azure Disk Encryption op VM's met niet-beheerde schijven gemaakt op basis van installatiekopieën in de Azure Marketplace als u wilt dat ze volledig versleuteld. Als u een virtuele machine met Managed Disks maken, SSE-versleutelt de gegevens standaard gebruik van platform beheerde sleutels. 

Als u een vooraf versleutelde VM naar Azure vanaf on-premises overbrengen, kunt u zich de versleutelingssleutels uploaden naar Azure Key Vault en blijven de versleuteling gebruiken voor die VM die u on-premises zijn gebruikt. Azure Disk Encryption is ingeschakeld voor het afhandelen van dit scenario.

Als u niet-versleutelde VHD van on-premises hebt, kunt u in de galerie, zoals een aangepaste installatiekopie uploaden en een virtuele machine van het inrichten. Als u dit met behulp van de Resource Manager-sjablonen doen, kunt u vragen deze Azure Disk Encryption inschakelen wanneer deze de virtuele machine wordt opgestart.

Wanneer u een gegevensschijf toevoegen en deze op de virtuele machine koppelen, kunt u de Azure Disk Encryption inschakelen op de gegevensschijf. Eerst op de gegevensschijf van die lokaal zal worden versleuteld en vervolgens de implementatie van het klassieke modellaag doet een langzame schrijfbewerkingen voor opslag, zodat de inhoud van de opslag is versleuteld.

#### <a name="client-side-encryption"></a>Clientversleuteling
Clientversleuteling is de veiligste methode van het versleutelen van uw gegevens, omdat deze gegevens voorafgaand aan doorvoer versleutelt.  Het is echter vereist dat u code toevoegen aan uw toepassingen met behulp van opslag, waarmee u niet wilt doen. In deze gevallen kunt u HTTPS gebruiken voor het beveiligen van uw gegevens die onderweg zijn. Wanneer gegevens Azure Storage bereikt, is het versleuteld met SSE.

U kunt met client-side encryption tabelitems, Wachtrijberichten en blobs versleutelen. 

Clientversleuteling wordt volledig beheerd door de toepassing. Dit is de veiligste methode, maar vereist dat u programmatische wijzigingen aanbrengen in uw toepassing en sleutelbeheer processen opstellen. U zou gebruik deze instelling als u wilt dat de extra beveiliging tijdens de overdracht en u wilt dat uw opgeslagen gegevens moeten worden versleuteld.

Clientversleuteling meer belasting van de client is en u moet voor dit account in uw plannen voor schaalbaarheid, met name als u worden gecodeerd en een grote hoeveelheid gegevens.

#### <a name="storage-service-encryption-sse"></a>Storage Service Encryption (SSE)

SSE wordt beheerd door Azure Storage. SSE biedt geen voor de beveiliging van de gegevens die onderweg zijn, maar deze wordt de gegevens versleuteld wanneer ze worden geschreven naar Azure Storage. SSE heeft geen invloed op de prestaties van Azure Storage.

U kunt elk soort gegevens van het opslagaccount met behulp van SSE versleutelt (blok-blobs, toevoeg-blobs, pagina-blobs, gegevens in een tabel, wachtrijgegevens en bestanden).

Als u een archief of een bibliotheek met VHD-bestanden die u als basis gebruiken voor het maken van nieuwe virtuele machines hebt, kunt u een nieuw opslagaccount maken en vervolgens de VHD-bestanden uploaden naar dat account. Deze VHD-bestanden wordt versleuteld door Azure Storage.

Als u Azure Disk Encryption is ingeschakeld voor de schijven in een virtuele machine hebt, klikt u vervolgens geen nieuw geschreven gegevens versleuteld met SSE of Azure Disk Encryption.

## <a name="storage-analytics"></a>Storage Analytics
### <a name="using-storage-analytics-to-monitor-authorization-type"></a>Met behulp van Storage Analytics voor het bewaken van autorisatietype
U kunt Azure Storage Analytics uitvoeren van logboekregistratie en metrische gegevens opslaan inschakelen voor elke storage-account. Dit is een uitstekend hulpprogramma om te gebruiken wanneer u wilt controleren van de maatstaven voor prestaties van een storage-account, of een opslagaccount oplossen moet omdat u prestatieproblemen ondervindt.

Een andere hoeveelheid gegevens die u in de logboeken van storage analytics zien kunt wordt de verificatiemethode die wordt gebruikt door iemand anders wanneer ze toegang hebben tot opslag. Bijvoorbeeld, met Blob-opslag, kunt u zien als ze een Shared Access Signature of de storage-accountsleutels gebruikt, of als de toegang tot blob openbaar is.

Dit kan nuttig zijn als u toegang tot opslag nauw zijn beveiligen. Bijvoorbeeld, in Blob-opslag kunt u alle van de containers ingesteld op privé en het gebruik van een SAS-service implementeren in uw toepassingen. Vervolgens kunt u de logboeken regelmatig om te zien of uw blobs worden geopend met behulp van de storage-accountsleutels, wat op een schending van beveiliging duiden kunnen, of als de blobs openbaar zijn, maar ze mag niet controleren.

#### <a name="what-do-the-logs-look-like"></a>Hoe zien de logboeken eruit?
Nadat u de metrische gegevens voor opslagaccount inschakelen en registreren via de Azure-portal, analytics-gegevens begint met het snel oplopen. De logboekregistratie en metrische gegevens voor elke service is gescheiden; de logboekregistratie wordt pas geschreven wanneer er activiteit in dit account, is terwijl de metrische gegevens worden geregistreerd per minuut, elk uur of elke dag, afhankelijk van hoe u deze configureren.

De logboeken worden opgeslagen in blok-blobs in een container met de naam $logs in de storage-account. Deze container wordt automatisch gemaakt wanneer Opslaganalyse is ingeschakeld. Zodra deze container is gemaakt, u kunt het niet verwijderen, maar u kunt de inhoud ervan verwijderen.

Er is een map voor elke service onder de container met $logs en submappen er voor het jaar/maand/dag/uur te zijn. Minder dan uur, worden de logboeken genummerd. Dit is hoe de mapstructuur eruit:

![Weergave van logboekbestanden](./media/storage-security-guide/image1.png)

Elke aanvraag naar Azure Storage wordt geregistreerd. Hier volgt een momentopname van een logboekbestand, waarin de eerste paar velden.

![Momentopname van een logboekbestand](./media/storage-security-guide/image2.png)

U kunt zien dat u de Logboeken gebruiken kunt voor het bijhouden van welke aard dan ook van aanroepen naar een opslagaccount.

#### <a name="what-are-all-of-those-fields-for"></a>Wat zijn al deze velden voor?
Er is een artikel die worden vermeld in de onderstaande die bronnen bevat de lijst de veel velden in de logboeken en waarvoor ze worden gebruikt. Hier volgt de lijst met velden in volgorde:

![Momentopname van de velden in een logboekbestand](./media/storage-security-guide/image3.png)

We zijn geïnteresseerd in de vermeldingen voor GetBlob, en hoe ze gemachtigd zijn, dus we moeten zoeken naar vermeldingen met bewerkingstype 'Get-Blob' en controleer de status van de aanvraag (vierde</sup> kolom) en de autorisatie-type (achtste</sup> kolom).

Bijvoorbeeld, de status van de aanvraag wordt "Geslaagd" in de eerste paar rijen in de bovenstaande aanbieding, en het autorisatie-type 'geverifieerd'. Dit betekent dat de aanvraag is geautoriseerd met behulp van de opslagaccountsleutel.

#### <a name="how-is-access-to-my-blobs-being-authorized"></a>Hoe wordt toegang tot mijn blobs is geautoriseerd?
We hebben drie gevallen waarin we geïnteresseerd zijn.

1. De blob openbaar is en deze is toegankelijk via een URL zonder een handtekening voor gedeelde toegang. In dit geval de status van de aanvraag is 'AnonymousSuccess' en de autorisatie-type is 'anoniem'.

   1.0; 2015-11-17T02:01:29.0488963Z; GetBlob; **AnonymousSuccess**; 200; 124; 37; **anonieme**; mijnopslag...
2. De blob is privé; werd gebruikt met een handtekening voor gedeelde toegang. In dit geval de status van de aanvraag is 'SASSuccess' en de autorisatie-type is 'sas'.

   1.0;2015-11-16T18:30:05.6556115Z;GetBlob;**SASSuccess**;200;416;64;**sas**;;mystorage…
3. De blob persoonlijk is en de opslagsleutel is gebruikt om deze te openen. In dit geval de status van de aanvraag is '**succes**'en het autorisatie-type is'**geverifieerde**'.

   1.0; 2015-11-16T18:32:24.3174537Z; GetBlob; **Succes**; 206; 59; 22; **geverifieerde**; mijnopslag...

U kunt de Microsoft Message Analyzer gebruiken om te bekijken en analyseren van deze logboeken. Dit omvat mogelijkheden voor zoeken en filteren. Bijvoorbeeld, kunt u om te zoeken naar exemplaren van GetBlob om te zien als het gebruik wat u verwacht, dat wil zeggen, om te controleren of iemand anders is niet verkrijgen van toegang tot uw storage-account ongepast.

#### <a name="resources"></a>Resources
* [Storage Analytics](../storage-analytics.md)

  In dit artikel wordt een overzicht van storage analytics en hoe u ze wilt inschakelen.
* [Storage Analytics logboekindeling](https://msdn.microsoft.com/library/azure/hh343259.aspx)

  In dit artikel ziet u de indeling van de Storage Analytics-logboeken en informatie over de beschikbare velden opslagmedia, met inbegrip van verificatie-type, waarmee wordt aangegeven welk type verificatie gebruikt voor de aanvraag.
* [Een Opslagaccount in Azure portal controleren](../storage-monitor-storage-account.md)

  In dit artikel laat zien hoe het configureren van bewaking van metrische gegevens en logboekregistratie voor een opslagaccount.
* [End-to-End-probleemoplossing op basis van metrische gegevens van Azure Storage en logboekregistratie, AzCopy en Message Analyzer](../storage-e2e-troubleshooting.md)

  In dit artikel praat over het oplossen van problemen met behulp van de Storage Analytics en laat zien hoe de Microsoft Message Analyzer gebruiken.
* [Microsoft Message Analyzer besturingssysteem-handleiding](https://technet.microsoft.com/library/jj649776.aspx)

  In dit artikel is van de referentie voor de Microsoft Message Analyzer en bevat koppelingen naar een zelfstudie, quickstart en overzicht van functies.

## <a name="cross-origin-resource-sharing-cors"></a>CORS (Cross-Origin Resource Sharing)
### <a name="cross-domain-access-of-resources"></a>Interdomein-toegang krijgen tot resources
Wanneer een webbrowser die worden uitgevoerd in één domein maakt een HTTP-aanvraag voor een resource vanaf een ander domein, is dit een HTTP-aanvraag voor cross-origin genoemd. Een HTML-pagina worden aangeleverd vanuit contoso.com maakt bijvoorbeeld een aanvraag voor een JPEG-indeling die wordt gehost op fabrikam.blob.core.windows.net. Uit veiligheidsoverwegingen beperken browsers cross-origin HTTP-aanvragen is gestart vanaf in scripts, zoals JavaScript. Dit betekent dat wanneer een JavaScript-code op een webpagina op contoso.com die jpeg op fabrikam.blob.core.windows.net aanvraagt, de browser wordt niet toestaan dat de aanvraag.

Wat is dit er moet gebeuren met Azure Storage? Goed, als u statische assets zoals JSON of XML-bestanden worden opgeslagen in Blob-opslag met behulp van een storage-account genoemd Fabrikam, het domein voor de activa is fabrikam.blob.core.windows.net en de webtoepassing contoso.com pas weer toegang tot deze met behulp van JavaScript omdat de domeinen verschillend zijn. Dit geldt ook zijn als u probeert om aan te roepen een van de Azure Storage-Services, zoals Table Storage – die JSON-gegevens moeten worden verwerkt door de JavaScript-client geretourneerd.

#### <a name="possible-solutions"></a>Mogelijke oplossingen
Eén manier om op te lossen dit is een aangepast domein, zoals 'storage.contoso.com' toewijzen aan fabrikam.blob.core.windows.net. Het probleem is dat u kunt alleen aangepaste domein toewijzen aan een storage-account. Wat gebeurt er als de assets worden opgeslagen in meerdere opslagaccounts?

Er is een andere manier om dit probleem oplossen dat de webtoepassing die fungeren als proxy voor het aanroepen van de opslag. Dit betekent dat als u een bestand naar Blob-opslag uploaden wilt, de web-App zou lokaal schrijven en vervolgens naar Blob Storage te kopiëren of heeft deze zouden alle ervan worden gelezen in het geheugen en vervolgens schrijven naar Blob-opslag. U kunt ook een toegewezen web-App (zoals een Web-API) die lokaal deze bestanden worden geüpload en schrijft deze naar de Blob-opslag schrijven. In beide gevallen moet u rekening voor die functie als nodig is voor het bepalen van de schaalbaarheid.

#### <a name="how-can-cors-help"></a>Hoe u kunt CORS?
Azure Storage kunt u CORS: Cross-Origin-resources delen inschakelen. U kunt domeinen die toegang hebben tot de resources in het storage-account opgeven voor elke storage-account. Bijvoorbeeld, in ons geval die hierboven worden beschreven, kunnen we CORS inschakelen in het opslagaccount fabrikam.blob.core.windows.net en deze zodanig configureren dat toegang tot contoso.com. De web application contoso.com kan vervolgens rechtstreeks toegang tot de resources in fabrikam.blob.core.windows.net.

Eén ding te weten is dat CORS toegang toestaat, maar biedt geen verificatie vereist voor alle niet-openbare toegang tot opslagresources is. Dit betekent dat u kunt alleen toegang tot blobs als ze openbaar zijn of als u een Shared Access Signature zodat u de benodigde machtiging opnemen. Tabellen, wachtrijen en bestanden hebben geen openbare toegang en vereisen een SAS.

CORS is standaard uitgeschakeld op alle services. U kunt CORS inschakelen met behulp van de REST-API of de storage-clientbibliotheek om aan te roepen een van de methoden voor het instellen van de service-beleid. Als u dat doet, kunt u een CORS-regel, dat zich in de XML opnemen. Hier volgt een voorbeeld van een CORS-regel die is ingesteld met behulp van de bewerking van de Service-eigenschappen instellen voor de Blob-Service voor een opslagaccount. U kunt met behulp van de storage-clientbibliotheek of de REST-API's voor Azure Storage bewerking uitvoeren.

```xml
<Cors>    
    <CorsRule>
        <AllowedOrigins>http://www.contoso.com, http://www.fabrikam.com</AllowedOrigins>
        <AllowedMethods>PUT,GET</AllowedMethods>
        <AllowedHeaders>x-ms-meta-data*,x-ms-meta-target*,x-ms-meta-abc</AllowedHeaders>
        <ExposedHeaders>x-ms-meta-*</ExposedHeaders>
        <MaxAgeInSeconds>200</MaxAgeInSeconds>
    </CorsRule>
<Cors>
```

Dit is wat elke rij betekent:

* **AllowedOrigins** Hiermee wordt aangegeven welke domeinen niet-overeenkomend kunnen aanvragen en ontvangen van gegevens van de storage-service. Dit vertelt dat zowel contoso.com en fabrikam.com gegevens uit de Blob-opslag voor een specifiek opslagaccount opvragen kunnen. U kunt dit ook instellen op een jokerteken (\*) om toe te staan alle domeinen toegang tot aanvragen.
* **AllowedMethods** dit is de lijst met methoden (HTTP-aanvraagbewerkingen) die kunnen worden gebruikt bij het maken van de aanvraag. In dit voorbeeld worden alleen plaatsen en GET toegestaan. U kunt dit instellen op een jokerteken (\*) om toe te staan alle methoden moet worden gebruikt.
* **AllowedHeaders** dit is de aanvraagheaders die het oorspronkelijke domein opgeven kunt wanneer de aanvraag. In dit voorbeeld zijn alle headers van Werkstroommetagegevens beginnen met x-ms-meta-data, x-ms-meta-doel en de x-ms-meta-abc toegestaan. Het jokerteken (\*) geeft aan dat header die begint met het opgegeven voorvoegsel is toegestaan.
* **ExposedHeaders** Hiermee wordt aangegeven welke antwoordheaders moeten worden weergegeven door de browser voor de aanvraagverlener. In dit voorbeeld wordt een header die beginnen met ' x-ms - meta-' weergegeven.
* **MaxAgeInSeconds** dit is de maximale hoeveelheid tijd dat een browser de voorbereidende aanvraag OPTIONS cache worden opgeslagen. (Controleer het onderstaande eerste artikel voor meer informatie over de voorbereidende aanvraag).

#### <a name="resources"></a>Resources
Bekijk deze bronnen voor meer informatie over CORS en het inschakelen ervan.

* [Cross-Origin Resource Sharing (CORS) ondersteuning voor de Azure Storage-Services op Azure.com](../storage-cors-support.md)

  Dit artikel bevat een overzicht van CORS en over het instellen van de regels voor de verschillende storage-services.
* [Cross-Origin Resource Sharing (CORS) ondersteuning voor de Azure Storage-Services op MSDN](https://msdn.microsoft.com/library/azure/dn535601.aspx)

  Dit is de referentiedocumentatie voor de CORS-ondersteuning voor de Azure Storage-Services. Dit bevat koppelingen naar artikelen toe te passen op elke storage-service en toont een voorbeeld en wordt uitgelegd van elk element in het bestand CORS.
* [Microsoft Azure Storage: Maak kennis met CORS](http://blogs.msdn.com/b/windowsazurestorage/archive/2014/02/03/windows-azure-storage-introducing-cors.aspx)

  Dit is een koppeling naar het blogartikel initiële aankondiging van CORS en het gebruik ervan weergegeven.

## <a name="frequently-asked-questions-about-azure-storage-security"></a>Veelgestelde vragen over Azure Storage-beveiliging
1. **Hoe kan ik controleren of de integriteit van de blobs die ik overbrengen ben naar of uit Azure Storage als ik het HTTPS-protocol niet kunt gebruiken?**

   Als voor een bepaalde reden u moet voor gebruik van HTTP in plaats van HTTPS en u werkt met blok-blobs, kunt u MD5 controleren om te controleren of de integriteit van de blobs die worden overgebracht. Hiermee kunt u met beveiliging tegen fouten van netwerk/transport layer, maar niet per se met tussenliggende aanvallen.

   Als u HTTPS, dit transport level security biedt, gebruik van MD5 controleren is overbodig en onnodige gebruiken kunt.

   Voor meer informatie, bekijk de [overzicht van Azure Blob MD5](http://blogs.msdn.com/b/windowsazurestorage/archive/2011/02/18/windows-azure-blob-md5-overview.aspx).
2. **Hoe zit het met FIPS-naleving voor de Verenigde Staten Overheid?**

   De Verenigde Staten Federal Information Processing Standard (FIPS) definieert cryptografische algoritmen die zijn goedgekeurd voor gebruik door Amerikaanse Federale overheid computersystemen voor de beveiliging van gevoelige gegevens. Het inschakelen van FIPS-modus op een Windows server of het bureaublad wordt aan het besturingssysteem dat cryptografische algoritmen alleen FIPS-gevalideerde moeten worden gebruikt. Als een toepassing maakt gebruik van niet-compatibele algoritmen, worden de toepassingen wordt verbroken. With.NET Framework versie 4.5.2 of hoger gebruikt, verandert de toepassing automatisch de cryptografiealgoritmen voor het gebruik compatibele FIPS-algoritmen wanneer de computer zich in de FIPS-modus.

   Microsoft blijft deze maximaal elke klant om te bepalen of inschakelen van FIPS-modus. Wij vinden dat er is geen dwingende redenen voor klanten die niet onderworpen aan wettelijke voorschriften zijn voor het inschakelen van FIPS-modus standaard.

### <a name="resources"></a>Resources
* [Waarom We nu niet aanbevelen 'FIPS-modus' meer](https://blogs.technet.microsoft.com/secguide/2014/04/07/why-were-not-recommending-fips-mode-anymore/)

  In dit blogartikel geeft een overzicht van FIPS en wordt uitgelegd waarom ze niet standaard ingeschakeld in de FIPS-modus.
* [FIPS 140 validatie](https://technet.microsoft.com/library/cc750357.aspx)

  In dit artikel bevat informatie over hoe Microsoft-producten en cryptografische modules aan de FIPS-standaard voor de Verenigde Staten voldoet Federale overheid.
* ["Systeemcryptografie: gebruik FIPS-compatibele algoritmen voor versleuteling, hashing en ondertekening" beveiligingsrisico van instellingen in Windows XP en nieuwere versies van Windows](https://support.microsoft.com/kb/811833)

  In dit artikel vertelt over het gebruik van FIPS-modus in oudere Windows-computers.