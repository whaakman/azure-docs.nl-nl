---
title: Azure Storage-beveiligingshandleiding | Microsoft Docs
description: De vele methoden voor het beveiligen van Azure Storage, inclusief maar niet beperkt tot RBAC, versleuteling van opslag-, Client-side-versleuteling, SMB 3.0 en Azure Disk Encryption van gegevens.
services: storage
documentationcenter: .net
author: tamram
manager: timlt
editor: tysonn
ms.assetid: 6f931d94-ef5a-44c6-b1d9-8a3c9c327fb2
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 12/08/2016
ms.author: tamram
ms.openlocfilehash: 9cb109dd9ce5a14bb80be61577c10d7191ec5ce6
ms.sourcegitcommit: 3fca41d1c978d4b9165666bb2a9a1fe2a13aabb6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/15/2017
---
# <a name="azure-storage-security-guide"></a>Azure Storage-beveiligingshandleiding
## <a name="overview"></a>Overzicht
Azure Storage biedt een uitgebreide reeks beveiligingsmogelijkheden die samen kunnen ontwikkelaars om beveiligde toepassingen te bouwen. Het opslagaccount zelf kan worden beveiligd met op rollen gebaseerde toegangsbeheer en Azure Active Directory. Gegevens onderweg tussen een toepassing en Azure kunnen worden beveiligd met behulp van [versleuteling van de Client-Side](../storage-client-side-encryption.md), HTTPS- of SMB 3.0. Gegevens kunnen worden ingesteld op automatisch worden versleuteld wanneer geschreven naar het gebruik van Azure Storage [opslag Service versleuteling (SSE)](storage-service-encryption.md). Besturingssysteem en gegevensschijven die worden gebruikt door virtuele machines kunnen worden ingesteld om te worden versleuteld met [Azure Disk Encryption](../../security/azure-security-disk-encryption.md). Gedelegeerde toegang tot de gegevensbron-objecten in Azure Storage kan worden verleend met behulp van [Shared Access Signatures](../storage-dotnet-shared-access-signature-part-1.md).

In dit artikel biedt een overzicht van elk van deze beveiligingsfuncties die kunnen worden gebruikt met Azure Storage. Koppelingen worden verstrekt aan artikelen die u de details van elke functie zodat krijgt u eenvoudig kunt doen nader onderzoek op elk onderwerp.

Hier volgen de onderwerpen in dit artikel aan bod:

* [Beveiliging van Management vlak](#management-plane-security) – voor het beveiligen van uw Storage-Account

  Het management vlak bestaat uit de bronnen die worden gebruikt voor het beheren van uw opslagaccount. In deze sectie bespreken we het implementatiemodel van Azure Resource Manager en het gebruik van op rollen gebaseerde toegangsbeheer (RBAC) toegang tot uw storage-accounts. Er wordt ook contact opnemen over het beheren van uw toegangscodes voor opslag en hoe u ze opnieuw genereren.
* [Gegevens vlak beveiliging](#data-plane-security) – toegang tot uw gegevens beveiligen

  In deze sectie zullen we het toestaan van toegang tot de werkelijke gegevensbron objecten in uw opslagaccount, zoals blobs, bestanden, wachtrijen en tabellen, met behulp van handtekeningen voor gedeelde toegang en toegangsbeleid opgeslagen. Zowel serviceniveau-SAS en account niveau SAS wordt uitgelegd. We ziet ook hoe u wilt beperken van toegang tot een specifiek IP-adres (of een bereik van IP-adressen), het beperken van het protocol dat wordt gebruikt voor HTTPS en hoe voor het intrekken van een Shared Access Signature zonder te wachten deze verlopen.
* [Versleuteling 'in transit'](#encryption-in-transit)

  Deze sectie wordt beschreven hoe gegevens worden beveiligd wanneer u het overbrengen van of naar Azure Storage. Bespreken we het aanbevolen gebruik van HTTPS en de versleuteling van SMB 3.0 voor Azure-bestandsshares. Er wordt ook rekening houden met kijken clientzijde versleuteling, waarmee u de gegevens te versleutelen voordat deze worden overgedragen naar de opslag in een clienttoepassing en de gegevens te decoderen nadat deze zijn overgebracht buiten de opslag.
* [Versleuteling 'at rest'](#encryption-at-rest)

  We zullen hebben over de Storage-Service-versleuteling (SSE) en hoe u kunt inschakelen voor een opslagaccount, wat resulteert in uw blok-blobs, pagina-blobs en toevoeg-blobs worden automatisch versleuteld wanneer geschreven naar Azure Storage. Ook kijken we hoe u Azure Disk Encryption gebruiken en de belangrijkste verschillen en aanvragen van schijfversleuteling versus SSE ten opzichte van de Client-Side versleuteling verkennen. Kort kijken we FIPS-naleving voor VS Computers van de overheid.
* Met behulp van [Opslaganalyse](#storage-analytics) toegang van Azure Storage controleren

  Deze sectie wordt beschreven hoe om informatie te vinden in de storage analytics-logboeken voor een aanvraag. We eens kijken echte opslaganalyse logboekgegevens en informatie over het achterhalen of een aanvraag wordt gedaan door de sleutel van de opslag, met een handtekening van de toegang tot gedeelde of anoniem en of deze is geslaagd of mislukt.
* [Browser gebaseerde Clients met behulp van CORS inschakelen](#Cross-Origin-Resource-Sharing-CORS)

  Deze sectie wordt gesproken over het toestaan van cross-origin-resource delen (CORS). Bespreken we domeintoegang en hoe u verwerkt met de CORS-mogelijkheden is ingebouwd in Azure Storage.

## <a name="management-plane-security"></a>Beveiliging van de vlak Management
Het management vlak bestaat uit bewerkingen die invloed hebben op het opslagaccount zelf. Bijvoorbeeld, kunt u maken of een opslagaccount verwijderen, een lijst met storage-accounts in een abonnement ophalen, ophalen van de opslagaccountsleutels of de sleutels van opslagaccount opnieuw genereren.

Als u een nieuw opslagaccount maakt, selecteert u een implementatiemodel van klassieke of Resource Manager. Het klassieke model van het maken van resources in Azure kunt alleen ofwel volledig, ofwel toegang tot het abonnement, en daarmee ook de storage-account.

Deze handleiding is gericht op het Resource Manager-model dat de aanbevolen manier is voor het maken van opslagaccounts. Met de storage-accounts voor Resource Manager, in plaats van die toegang tot het hele abonnement, kunt u de toegang op een meer beperkte niveau aan het beheer met behulp van op rollen gebaseerde toegangsbeheer (RBAC) beheren.

### <a name="how-to-secure-your-storage-account-with-role-based-access-control-rbac"></a>Het beveiligen van uw opslagaccount met op rollen gebaseerde toegangsbeheer (RBAC)
We wat RBAC is en hoe u deze kunt gebruiken. Elk Azure-abonnement heeft een Azure Active Directory. Toegang tot het beheer van bronnen in het Azure-abonnement met het implementatiemodel van Resource Manager kunnen verleend aan gebruikers, groepen en toepassingen van die map. Dit is op rollen gebaseerde toegangsbeheer (RBAC) genoemd. Als u wilt deze toegang beheren, kunt u de [Azure-portal](https://portal.azure.com/), wordt de [Azure CLI-hulpprogramma's](../../cli-install-nodejs.md), [PowerShell](/powershell/azureps-cmdlets-docs), of de [Azure Storage Resource Provider REST-API's](https://msdn.microsoft.com/library/azure/mt163683.aspx).

Met het Resource Manager-model plaatst u het opslagaccount in een resource group en beheren van toegang op het beheervlak van het specifieke storage-account met Azure Active Directory. Bijvoorbeeld, kunt u specifieke gebruikers de mogelijkheid bieden toegang tot de opslagaccountsleutels, terwijl andere gebruikers informatie over het opslagaccount kunnen bekijken, maar heeft geen toegang de opslagaccountsleutels tot.

#### <a name="granting-access"></a>Het verlenen van toegang
Toegang wordt verleend door de juiste RBAC-rol toewijzen aan gebruikers, groepen en toepassingen op het juiste bereik. Om toegang tot het hele abonnement, moet u een rol op het abonnementsniveau toewijzen. U kunt toegang verlenen tot alle resources in een resourcegroep door machtigingen te verlenen aan de resourcegroep zelf. U kunt ook specifieke rollen toewijzen aan specifieke bronnen, zoals de storage-accounts.

Hier volgen de belangrijkste punten die u weten wilt over het gebruik van RBAC voor toegang tot de beheerbewerkingen die van een Azure Storage-account:

* Wanneer u toegang toewijst, toewijzen u in feite een rol aan het account dat u wilt toegang hebben. U kunt toegang tot de bewerkingen die wordt gebruikt voor het beheren van dit opslagaccount, maar niet aan de gegevensobjecten in het account beheren. U kunt bijvoorbeeld toestemming voor het ophalen van de eigenschappen van het storage-account (zoals redundantie) geven, maar niet naar een container of de gegevens binnen een container in Blob Storage.
* U krijgt deze machtiging voor het lezen van de opslagaccountsleutels voor iemand om gemachtigd voor toegang tot de gegevensbron-objecten in het opslagaccount en kan die gebruiker vervolgens die sleutels gebruiken voor toegang tot de blobs, wachtrijen, tabellen en bestanden.
* Rollen kunnen worden toegewezen aan een specifieke gebruikersaccount in een groep gebruikers, of op een specifieke toepassing.
* Elke functie heeft een lijst van acties en geen acties. De rol Inzender van de virtuele Machine heeft bijvoorbeeld een actie van het 'listKeys' waarmee de opslagaccountsleutels om te lezen. De inzender heeft 'Geen acties' zoals het bijwerken van de toegang voor gebruikers in de Active Directory.
* Rollen voor opslag bevatten (maar niet beperkt tot) het volgende:

  * Eigenaar: ze kunnen alles beheren, inclusief toegang.
  * Inzender – ze kunnen alles doen de eigenaar kunt behalve toegang toewijzen. Iemand met deze functie kunt weergeven en opnieuw genereren van sleutels voor de opslagaccount. Met de opslagaccountsleutels toegang te krijgen tot de gegevensobjecten.
  * Lezer – kunnen ze informatie over het opslagaccount, met uitzondering van geheimen bekijken. Bijvoorbeeld als u een rol met de Lezersmachtigingen op het opslagaccount naar iemand toewijst, kunnen ze de eigenschappen van het storage-account bekijken, maar mag niet het aanbrengen van wijzigingen in de eigenschappen of de opslagaccountsleutels weergeven.
  * Storage-Account Inzender – kunnen ze het storage-account beheren: de resourcegroepen en resources, het abonnement kan worden gelezen en maken en beheren van resourcegroepimplementaties abonnement. Ze kunnen ook toegang tot de opslagaccountsleutels, wat betekent dat op zijn beurt toegang te krijgen tot het vlak van gegevens.
  * Beheerder voor gebruikerstoegang – kunnen ze toegang tot het storage-account beheren. Ze kunnen bijvoorbeeld lezerstoegang verlenen aan een specifieke gebruiker.
  * Virtuele Machine Inzender – kunnen ze virtuele machines, maar niet het opslagaccount waarvoor ze zijn verbonden beheren. Deze rol kan de opslagaccountsleutels weergeven, wat betekent dat de gebruiker aan wie u deze rol toewijst op het vlak van de gegevens kunt bijwerken.

    Ze hebben om een gebruiker een virtuele machine maken, kunnen de bijbehorende VHD-bestand in een opslagaccount maken. Hiervoor moeten ze kunnen de opslagaccountsleutel opgehaald en doorgegeven aan de API voor het maken van de virtuele machine. Daarom moeten ze deze machtiging hebben, zodat ze de opslagaccountsleutels kunnen weergeven.
* De mogelijkheid voor het definiëren van aangepaste rollen is een functie waarmee u voor het opstellen van een reeks acties in een lijst met beschikbare acties die kunnen worden uitgevoerd op Azure-resources.
* De gebruiker moet worden ingesteld in uw Azure Active Directory voordat u kunt een rol aan ze toewijzen.
* U kunt een rapport van die verleend/ingetrokken wat voor soort toegang naar/van wie en op welke bereik met PowerShell of Azure CLI kunt maken.

#### <a name="resources"></a>Resources
* [Toegangsbeheer op basis van rollen in Azure Active Directory](../../active-directory/role-based-access-control-configure.md)

  In dit artikel wordt het toegangsbeheer op basis van rollen in Azure Active Directory uitgelegd.
* [RBAC: ingebouwde rollen](../../active-directory/role-based-access-built-in-roles.md)

  In dit artikel beschrijft alle van de ingebouwde rollen die beschikbaar zijn in RBAC.
* [Resource Manager-implementatie en klassieke implementatie begrijpen](../../azure-resource-manager/resource-manager-deployment-model.md)

  Dit artikel wordt uitgelegd van de Resource Manager-implementatie en klassieke implementatiemodellen en wordt uitgelegd van de voordelen van het gebruik van de Resource Manager en resource-groepen. Hierin wordt uitgelegd hoe de Azure Compute, Network en Storage Providers onder het Resource Manager-model werken.
* [Toegangsbeheer op basis van rollen beheren met de REST-API](../../active-directory/role-based-access-control-manage-access-rest.md)

  Dit artikel beschrijft hoe u de REST-API gebruikt om RBAC te beheren.
* [Azure Storage Resource Provider REST API-verwijzing](https://msdn.microsoft.com/library/azure/mt163683.aspx)

  Dit is de verwijzing voor de API's die u gebruiken kunt voor het beheren van uw storage-account via een programma.
* [Ontwikkelaarshandleiding voor verificatie met Azure Resource Manager-API](http://www.dushyantgill.com/blog/2015/05/23/developers-guide-to-auth-with-azure-resource-manager-api/)

  Dit artikel laat zien hoe u verifieert met behulp van de Resource Manager-API's.
* [Toegangsbeheer op basis van rollen voor Microsoft Azure vanuit Ignite](https://channel9.msdn.com/events/Ignite/2015/BRK2707)

  Dit is een koppeling naar een video op Channel 9 van de vergadering over MS Ignite in 2015. In deze sessie wordt ingegaan op de opties voor toegangsbeheer en rapportage in Azure en worden aanbevolen procedures behandeld voor de beveiliging van de toegang tot Azure-abonnementen met Azure Active Directory.

### <a name="managing-your-storage-account-keys"></a>Het beheren van uw toegangscodes voor opslag
Toegangscodes voor opslag zijn 512-bits-tekenreeksen die zijn gemaakt door Azure die samen met de naam van het opslagaccount, kan worden gebruikt voor toegang tot de gegevensobjecten die zijn opgeslagen in de storage-account, bijvoorbeeld blobs, entiteiten in een tabel, Wachtrijberichten en bestanden op een bestandsshare in Azure. Beheren van toegang tot het account sleutels besturingselementen toegang tot opslag op het vlak van gegevens voor dat opslagaccount.

Elk opslagaccount heeft twee sleutels die worden aangeduid als 'Sleutel 1' en "Sleutel 2" in de [Azure-portal](http://portal.azure.com/) en in de PowerShell-cmdlets. Deze kunnen worden hersteld handmatig met behulp van een van de verschillende methoden, waaronder, maar niet beperkt tot het gebruik van de [Azure-portal](https://portal.azure.com/), PowerShell, de Azure CLI of programmatisch met behulp van de Storage-clientbibliotheek voor .NET of de REST-API van Azure Storage-Services.

Er zijn een aantal redenen opnieuw genereren van sleutels voor uw opslagaccount.

* U kunt ze opnieuw genereren uit veiligheidsoverwegingen regelmatig.
* Als iemand beheerd inbreken in een toepassing en het ophalen van de sleutel die is vastgelegd of opgeslagen in een configuratiebestand deze volledige toegang geven tot uw storage-account, zou u uw sleutels van opslagaccount opnieuw genereren.
* Een andere wordt voor sessiesleutels als uw team met behulp van een toepassing Opslagverkenner die de sleutel van het opslagaccount behoudt en een van de teamleden verlaat. De toepassing wilt blijven werken, zodat ze toegang krijgt tot uw storage-account nadat ze verwijderd zijn. Dit is feitelijk de primaire reden die ze account niveau Shared Access Signatures gemaakt: u kunt een SAS niveau gebruiken in plaats van de toegangssleutels opslaan in een configuratiebestand.

#### <a name="key-regeneration-plan"></a>Plan toegangssleutel wordt opnieuw gegenereerd
U wilt niet dat alleen de sleutel die u zonder een planning gebruikt te genereren. Als u dat doet, kan u alle toegang voor dit opslagaccount, wat leiden onderbreking van de primaire tot kan afgekapt. Daarom zijn er twee sleutels. U moet één sleutel opnieuw genereren tegelijk.

Voordat u uw sleutels genereren, moet u er een lijst met alle toepassingen die afhankelijk zijn van het opslagaccount, evenals andere services die u gebruikt in Azure. Bijvoorbeeld, als u Azure Media Services die afhankelijk van uw storage-account zijn gebruikt, moet u opnieuw synchroniseren de sneltoetsen met uw mediaservice nadat u de sleutel opnieuw genereren. Als u alle toepassingen, zoals een Opslagverkenner gebruikt, moet u de nieuwe sleutels voor deze toepassingen en bieden. Houd er rekening mee dat als u virtuele machines waarvan VHD-bestanden worden opgeslagen in de storage-account hebt, ze niet worden beïnvloed door de sleutels van opslagaccount opnieuw genereren.

U kunt uw sleutels in de Azure portal opnieuw genereren. Maximaal 10 minuten worden gesynchroniseerd via Storage-Services kunnen nemen wanneer sleutels opnieuw worden gegenereerd.

Wanneer u klaar bent, wordt hier het algemene proces met gedetailleerde informatie over hoe u uw sleutel moet wijzigen. In dit geval is de veronderstelling dat u momenteel Key 1 gebruikt en gaat u alles om te gebruiken sleutel 2 in plaats daarvan wijzigen.

1. Opnieuw genereren sleutel 2 om ervoor te zorgen dat deze beveiligd worden. U kunt dit doen in de Azure portal.
2. In alle van de toepassingen waar de opslagsleutel is opgeslagen, moet u de opslagsleutel voor het gebruik van de nieuwe waarde van de sleutel 2 aanpassen. Testen en publiceren van de toepassing.
3. Nadat alle van de toepassingen en services actief zijn en dat met succes is uitgevoerd, opnieuw genereren van sleutel 1. Dit zorgt ervoor dat iemand aan wie u niet expliciet de nieuwe sleutel hebben niet langer toegang tot het opslagaccount hebben wordt.

Als u momenteel sleutel 2 gebruikt, kunt u hetzelfde proces gebruiken, maar de sleutelnamen omkeren.

U kunt migreren via een paar dagen, het wijzigen van elke toepassing voor het gebruik van de nieuwe sleutel en publiceren. Nadat ze allemaal zijn voltooid, moet u vervolgens gaat u terug en de oude sleutel opnieuw genereren, zodat deze niet meer werkt.

Er is een andere optie om de sleutel van het opslagaccount voor een [Azure Key Vault](https://azure.microsoft.com/services/key-vault/) als een geheim en hebben uw toepassingen op te halen de sleutel van daaruit. Wanneer u de sleutel opnieuw genereren en bijwerken van de Azure Sleutelkluis, moet de toepassingen niet worden geïmplementeerd omdat ze de nieuwe sleutel opgehaald uit de Azure Sleutelkluis automatisch. Houd er rekening mee dat u kunt de toepassing de sleutel lezen telkens wanneer die u deze nodig hebben, of u kunt deze in de cache in het geheugen en als het mislukt bij het gebruik, de sleutel ophalen opnieuw vanuit de Azure Sleutelkluis.

Een ander niveau van beveiliging voor de opslagsleutels met Azure Key Vault ook worden toegevoegd. Als u deze methode gebruikt, hebt u nooit de belangrijkste hardcoded opslag in een configuratiebestand, waardoor deze doorverwezen iemand wordt de toegang tot de sleutels zonder specifieke machtiging wordt verwijderd.

Een ander voordeel van het gebruik van Azure Sleutelkluis is dat kunt u ook toegang tot uw sleutels met Azure Active Directory beheren. Dit betekent dat u kunt toegang verlenen tot de handvol toepassingen die moeten de sleutels ophalen uit Azure Key Vault en weten dat andere toepassingen kan niet worden toegang heeft tot de sleutels zonder deze machtiging verlenen specifiek.

Opmerking: het verdient aanbeveling gebruik slechts één van de sleutels in al uw toepassingen op hetzelfde moment. Als u sleutel 1 op bepaalde plaatsen en sleutel 2 in andere gevallen gebruikt, kunt u zich niet uw sleutels draaien zonder een toepassing toegang verliezen.

#### <a name="resources"></a>Resources
* [Over Azure Storage-Accounts](storage-create-storage-account.md#regenerate-storage-access-keys)

  Dit artikel biedt een overzicht van de storage-accounts en weergeven, kopiëren en opnieuw genereren van de toegangssleutels voor opslag.
* [Azure Storage Resource Provider REST API-verwijzing](https://msdn.microsoft.com/library/mt163683.aspx)

  Dit artikel bevat koppelingen naar specifieke artikelen over het ophalen van de opslagaccountsleutels en opnieuw genereren van sleutels voor de opslagaccount voor een Azure-Account met de REST API. Opmerking: Dit is voor Resource Manager-opslagaccounts.
* [Bewerkingen voor opslagaccounts](https://msdn.microsoft.com/library/ee460790.aspx)

  Dit artikel in de Storage-Service Manager REST API Reference bevat koppelingen naar specifieke artikelen op te halen en opnieuw genereren van sleutels voor de opslagaccount met de REST API. Opmerking: Dit is voor de klassieke opslagaccounts.
* [Zeg goodbye voor sleutelbeheer: toegang tot Azure Storage-gegevens met behulp van Azure AD beheren](http://www.dushyantgill.com/blog/2015/04/26/say-goodbye-to-key-management-manage-access-to-azure-storage-data-using-azure-ad/)

  In dit artikel laat zien hoe Active Directory gebruiken om toegang tot uw Azure Storage-sleutels in Azure Sleutelkluis te beheren. U ziet ook het gebruik van een Azure Automation-taak voor het genereren van de sleutels op uurbasis.

## <a name="data-plane-security"></a>Beveiliging van gegevens vlak
Beveiliging van gegevens vlak verwijst naar de methoden voor het beveiligen van de gegevensobjecten die is opgeslagen in Azure Storage-de blobs, wachtrijen, tabellen en bestanden. We methoden voor het versleutelen van de gegevens en beveiliging tijdens de overdracht van de gegevens hebt gezien, maar hoe moet u doen over het beheren van toegang tot de objecten?

Er zijn twee methoden voor het verlenen van toegang tot de gegevensobjecten zelf. Het gaat hierbij om toegang tot de opslagaccountsleutels beheren en het gebruik van handtekeningen voor gedeelde toegang om toegang te verlenen aan specifieke gegevensobjecten voor een specifieke hoeveelheid tijd.

Bovendien voor Blob Storage kunt u openbare toegang om uw blobs te door het instellen van het toegangsniveau voor de container met de blobs dienovereenkomstig. Als u toegang hebt ingesteld voor een container voor Blob of Container, is het toegestaan openbare leestoegang voor de blobs in de container. Dit betekent dat iedereen met een URL die verwijst naar een blob in de container kunt openen in een browser zonder met behulp van een Shared Access Signature of het hebben van de opslagaccountsleutels.

Naast het beperken van toegang tot en met de autorisatie, kunt u ook gebruiken [Firewalls en virtuele netwerken](storage-network-security.md) om toegang tot het opslagaccount op basis van regels netwerk te beperken.  Deze aanpak kunt weigeren van toegang tot openbare internetverkeer, en Verleen toegang tot alleen op specifieke Azure Virtual Networks of openbare internet IP-adresbereiken.


### <a name="storage-account-keys"></a>Opslagaccountsleutels
Toegangscodes voor opslag zijn 512-bits-tekenreeksen die zijn gemaakt door Azure die samen met de naam van het opslagaccount, kan worden gebruikt voor toegang tot de gegevensobjecten die zijn opgeslagen in het opslagaccount.

U kunt bijvoorbeeld BLOB's lezen, schrijven naar wachtrijen, tabellen maken en wijzigen van bestanden. Veel van deze acties kunnen worden uitgevoerd via de Azure-portal of met behulp van een van de vele Opslagverkenner toepassingen. U kunt ook schrijven van code voor het gebruik van de REST-API of een van de Opslagclientbibliotheken deze bewerkingen uit te voeren.

Zoals beschreven in de sectie op de [Management vlak beveiliging](#management-plane-security), toegang tot de opslagsleutels voor een opslagaccount van klassiek kan worden verleend door te geven van volledige toegang tot het Azure-abonnement. Toegang tot de opslagsleutels voor een opslagaccount met het Azure Resource Manager-model kan worden beheerd via op rollen gebaseerde toegangsbeheer (RBAC).

### <a name="how-to-delegate-access-to-objects-in-your-account-using-shared-access-signatures-and-stored-access-policies"></a>Het overdragen van toegang tot objecten in uw account met handtekeningen voor gedeelde toegang en toegangsbeleid opgeslagen
Een Shared Access Signature is een tekenreeks met een beveiligingstoken dat kan worden gekoppeld aan een URI die u kunt toegang tot opslagobjecten delegeren en beperkingen, zoals de machtigingen en de datum/tijd reeks toegang opgeven.

U kunt toegang verlenen tot blobs, containers, Wachtrijberichten, bestanden en tabellen. Met tabellen, kunt u daadwerkelijk toestemming voor toegang tot een bereik van entiteiten in de tabel door te geven van de partitie- en rijkoppen sleutelbereiken waarnaar u wilt dat de gebruiker toegang heeft. Bijvoorbeeld, als u gegevens opgeslagen met een partitiesleutel van geografische staat hebt, kan geeft u iemand toegang tot alleen de gegevens voor Californië.

In een ander voorbeeld kan u een webtoepassing geven een SAS-token dat deze vermeldingen schrijven naar een wachtrij kan en geef een werknemer rol toepassing een SAS-token voor het ophalen van berichten uit de wachtrij en ze te verwerken. Of u een klant een SAS-token ze kunnen gebruiken voor het uploaden van afbeeldingen naar een Blob Storage-container en een web application machtigen om te lezen van deze afbeeldingen kan geven. In beide gevallen is er een scheiding van zorgen – elke toepassing kan alleen de toegang die ze nodig hebben om hun taak uitvoeren worden verleend. Dit is mogelijk door het gebruik van handtekeningen voor gedeelde toegang.

#### <a name="why-you-want-to-use-shared-access-signatures"></a>Waarom u wilt gebruiken van handtekeningen voor gedeelde toegang
Waarom zou u wilt gebruiken van een SAS in plaats van alleen de opslagaccountsleutel veel eenvoudiger is verlenen? Geven van de sleutel van uw opslagaccount is vergelijkbaar met de sleutels van uw opslag Koninkrijk delen. Deze verleent volledige toegang. Iemand anders uw sleutels te gebruiken en hun volledige muziekbibliotheek uploaden naar uw opslagaccount. Ze kunnen ook uw bestanden vervangen door versies geïnfecteerde of uw gegevens worden gestolen. Opslag onbeperkte toegang geven tot uw storage-account is iets dat geen licht moet worden gehouden.

Met handtekeningen voor gedeelde toegang kunt u een client alleen de benodigde machtigingen voor een beperkte hoeveelheid tijd geven. Bijvoorbeeld als iemand anders een blob naar uw account uploaden wordt, verleent u hen schrijftoegang voor net voldoende tijd voor het uploaden van de blob is (afhankelijk van de grootte van de blob natuurlijk). En als u van gedachten verandert, kunt u die toegang intrekken.

Daarnaast kunt u opgeven dat aanvragen met een SAS beperkt tot een bepaalde IP-adres of IP-adresbereik externe naar Azure zijn. U kunt ook vereisen dat aanvragen worden gedaan met een bepaald protocol (HTTPS of HTTP/HTTPS). Dit betekent dat als u alleen wilt toestaan dat HTTPS-verkeer, u het vereiste protocol alleen op HTTPS instellen kunt en HTTP-verkeer wordt geblokkeerd.

#### <a name="definition-of-a-shared-access-signature"></a>Definitie van een Shared Access Signature
Een Shared Access Signature is een reeks queryparameters toegevoegd aan de URL die verwijst naar de resource

die bevat informatie over de toegang is toegestaan en de hoeveelheid tijd op waarvoor de toegang is toegestaan. Hier volgt een voorbeeld; Deze URI bevat leestoegang tot een blob gedurende vijf minuten. Opmerking SAS queryparameters moet gecodeerde URL, zoals % 3A voor dubbele punt (:) of 20% van een spatie.

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

#### <a name="how-the-shared-access-signature-is-authenticated-by-the-azure-storage-service"></a>Hoe de Shared Access Signature is geverifieerd door de Azure Storage-Service
Als de storage-service heeft de aanvraag ontvangt, bevat de parameters voor invoer voor de query en een handtekening met dezelfde methode als het aanroepende programma gemaakt. De twee handtekeningen worden vervolgens vergeleken. Als de gebruiker akkoord gaat, vervolgens controleren de storage-service de versie van de storage-service om te controleren of dat deze geldig is, Controleer of de huidige datum en tijd binnen het opgegeven venster zijn, zorg ervoor dat de toegang aangevraagd komt overeen met de aanvraag is ingediend, enzovoort.

Bijvoorbeeld, met onze URL, zou als de URL die naar een bestand in plaats van een blob verwijst is deze aanvraag mislukt omdat deze geeft aan dat de Shared Access Signature voor een blob. Als de REST-opdracht wordt aangeroepen om bij te werken van een blob, zou deze mislukken omdat de Shared Access Signature geeft aan dat alleen lezen-toegang is toegestaan.

#### <a name="types-of-shared-access-signatures"></a>Typen handtekeningen voor gedeelde toegang
* Een SAS serviceniveau kan worden gebruikt voor toegang tot specifieke bronnen in een opslagaccount. Enkele voorbeelden hiervan zijn bij het ophalen van een lijst met blobs in een container, downloaden van een blob, het bijwerken van een entiteit in een tabel, berichten toe te voegen aan een wachtrij of een bestand uploadt naar een bestandsshare.
* Een SAS niveau kan worden gebruikt voor toegang tot alles die een SAS serviceniveau voor kan worden gebruikt. Bovendien kan deze opties naar bronnen die niet zijn toegestaan als een service level SAS, zoals de mogelijkheid voor het maken van containers, tabellen, wachtrijen en bestandsshares geven. U kunt ook toegang tot meerdere services in één keer opgeven. Bijvoorbeeld, u iemand mogelijk geeft toegang tot blobs en -bestanden in uw opslagaccount.

#### <a name="creating-an-sas-uri"></a>Maken van een SAS-URI
1. U kunt een ad-hoc URI maken op aanvraag alle van de queryparameters telkens definiëren.

   Dit echt flexibel is, maar als er een logische set parameters die vergelijkbaar telkens zijn, een beter idee krijgen met behulp van een toegangsbeleid opgeslagen is.
2. U kunt een toegangsbeleid opgeslagen voor een volledige container, een bestandsshare, een tabel of een wachtrij maken. Vervolgens kunt u dit als basis voor de SAS URI's die u maakt. Machtigingen op basis van opgeslagen-beleidsregels kunnen gemakkelijk worden ingetrokken. U kunt maximaal 5 beleid gedefinieerd voor elke container, queue, table of bestandsshare hebben.

   Als u zijn te veel mensen lezen van de blobs in een specifieke container hebben, kan u bijvoorbeeld een toegangsbeleid opgeslagen met de tekst 'leestoegang geven' en eventuele andere instellingen die dezelfde telkens worden maken. Vervolgens kunt u een SAS-URI met behulp van de instellingen van het toegangsbeleid opgeslagen en de vervaldatum van de datum en tijd op te geven. Het voordeel hiervan is dat er geen alle op te geven van de queryparameters elke keer.

#### <a name="revocation"></a>Intrekken
Stel dat uw SAS is aangetast, of u wilt deze vanwege het beveiligingsbeleid van het bedrijf of naleving van regelgeving vereisten wijzigen. Hoe u toegang tot een bron met die SAS intrekken? Dit is afhankelijk van hoe u de SAS-URI hebt gemaakt.

Als u van ad-hoc URI gebruikmaakt, hebt u drie opties. U kunt de SAS-tokens uitgeven met korte verloopbeleid en wacht gewoon de SAS verloopt. U kunt wijzigen of verwijderen van de resource (uitgaande van het token is gericht op een enkel object). U kunt de opslagaccountsleutels wijzigen. Deze laatste optie kan een grote invloed, afhankelijk van hoeveel services met behulp van dit opslagaccount en is waarschijnlijk niet iets dat die u doen wilt zonder een planning.

Als u van een SAS afgeleid van een toegangsbeleid opgeslagen gebruikmaakt, kunt u toegang verwijderen door het beleid voor opgeslagen toegang intrekken – u kunt deze alleen wijzigen zodat deze al is verlopen, of kunt u deze helemaal verwijderen. Dit wordt direct van kracht en elke SAS gemaakt met behulp van dat beleid opgeslagen toegang wordt ongeldig gemaakt. Bijwerken of verwijderen van het toegangsbeleid opgeslagen mogelijk gevolgen personen met toegang tot die specifieke container, bestandsshare, tabel of wachtrij via SAS, maar als de clients worden geschreven, zodat ze een nieuw SAS aanvragen wanneer de oude heeft ongeldige, wordt dit werkt prima.

Omdat met behulp van een SAS afgeleid van een opgeslagen-beleid u de mogelijkheid om in te trekken die SAS onmiddellijk biedt, is het aanbevolen gebruik altijd toegangsbeleid opgeslagen indien mogelijk.

#### <a name="resources"></a>Resources
Raadpleeg de volgende artikelen voor meer gedetailleerde informatie over het gebruik van handtekeningen voor gedeelde toegang en opgeslagen toegangsbeleid, met voorbeelden voltooid:

* Dit zijn de verwijzing naar artikelen.

  * [Service-SAS](https://msdn.microsoft.com/library/dn140256.aspx)

    Dit artikel vindt voorbeelden van het gebruik van een SAS serviceniveau met blobs, Wachtrijberichten, tabel bereiken en bestanden.
  * [Maken van een service-SAS](https://msdn.microsoft.com/library/dn140255.aspx)
  * [Een SAS-account maken](https://msdn.microsoft.com/library/mt584140.aspx)
* Deze zijn zelfstudies voor de .NET-clientbibliotheek gebruiken voor het maken van handtekeningen voor gedeelde toegang en toegangsbeleid opgeslagen.

  * [Met behulp van Shared Access Signatures (SAS)](../storage-dotnet-shared-access-signature-part-1.md)
  * [Shared Access Signatures, deel 2: Maken en gebruiken van een SAS met de Blob-Service](../blobs/storage-dotnet-shared-access-signature-part-2.md)

    Dit artikel bevat een uitleg van de SAS-model, voorbeelden van handtekeningen voor gedeelde toegang en aanbevelingen voor de beste gebruik van SAS. Ook besproken, is de intrekking van de machtiging verleend.

* Authentication

  * [Verificatie voor de Azure Storage-Services](https://msdn.microsoft.com/library/azure/dd179428.aspx)
* Shared Access Signatures aan de slag zelfstudie

  * [Aan de slag zelfstudie SAS](https://github.com/Azure-Samples/storage-dotnet-sas-getting-started)

## <a name="encryption-in-transit"></a>Codering in Transit
### <a name="transport-level-encryption--using-https"></a>Transportniveau codering – via HTTPS
Er is nog een stap die u nemen moet om te controleren of de beveiliging van uw Azure Storage-gegevens voor het versleutelen van de gegevens tussen de client en de Azure Storage. De eerste aanbeveling is gebruik altijd de [HTTPS](https://en.wikipedia.org/wiki/HTTPS) protocol, waardoor beveiligde communicatie via het openbare Internet.

Als u een kanaal voor veilige communicatie, moet u altijd HTTPS gebruiken tijdens het aanroepen van de REST API's of toegang tot in de opslag objecten. Bovendien **Shared Access Signatures**, die kan worden gebruikt voor toegang tot Azure Storage-objecten delegeren, een optie om op te geven dat alleen het HTTPS-protocol kan worden gebruikt bij het gebruik van handtekeningen voor gedeelde toegang, ervoor te zorgen dat iedereen koppelingen met SAS-tokens verstuurt het juiste protocol wordt gebruikt.

U kunt het gebruik van HTTPS afdwingen bij het aanroepen van de REST-API's voor toegang tot objecten in de storage-accounts door in te schakelen [vereiste gegevensoverdracht Secure](../storage-require-secure-transfer.md) voor het opslagaccount. Verbindingen met HTTP geweigerd als deze optie is ingeschakeld.

### <a name="using-encryption-during-transit-with-azure-file-shares"></a>Met behulp van versleuteling tijdens de overdracht met Azure-bestandsshares
Azure Files ondersteunt HTTPS wanneer met de REST API, maar meer vaak gebruikt als een SMB-bestandsshare op een virtuele machine is gekoppeld. SMB 2.1 biedt geen ondersteuning voor versleuteling, zodat de verbindingen zijn alleen toegestaan binnen dezelfde regio in Azure. Echter, SMB 3.0 ondersteunt versleuteling, en is beschikbaar in Windows Server 2012 R2, Windows 8, Windows 8.1 en Windows 10, zodat de regio-overschrijdende toegang en zelfs toegang op het bureaublad.

Houd er rekening mee dat hoewel Azure-bestandsshares kunnen worden gebruikt voor Unix, de Linux SMB-client nog geen versleuteling, ondersteunt zodat toegang is alleen toegestaan binnen een Azure-regio. Ondersteuning voor Linux codering is op het overzicht van Linux-ontwikkelaars die verantwoordelijk is voor SMB-functionaliteit. Bij het toevoegen van versleuteling, hebt u dezelfde mogelijkheid voor het openen van een Azure-bestandsshare op Linux, zoals u zou voor Windows doen.

U kunt het gebruik van versleuteling met de service Azure Files afdwingen door in te schakelen [vereiste gegevensoverdracht Secure](../storage-require-secure-transfer.md) voor het opslagaccount. Als u de REST API's gebruikt, wordt HTTPs is vereist. Voor SMB, wordt SMB alleen verbindingen die ondersteuning bieden voor versleuteling verbinding gemaakt.

#### <a name="resources"></a>Resources
* [Inleiding Azure Files](../files/storage-files-introduction.md)
* [Aan de slag met Azure-bestanden in Windows](../files/storage-how-to-use-files-windows.md)

  In dit artikel biedt een overzicht van Azure-bestandsshares en het koppelen en gebruiken ze in Windows.

* [Azure Files gebruiken met Linux](../files/storage-how-to-use-files-linux.md)

  In dit artikel laat zien hoe een Azure-bestandsshare op een Linux-systeem en uploaden/downloaden van bestanden te koppelen.

### <a name="using-client-side-encryption-to-secure-data-that-you-send-to-storage"></a>Met behulp van Client-side-versleuteling voor het beveiligen van gegevens die u naar de opslag verzendt
Een andere optie waarmee u ervoor te zorgen dat uw gegevens beveiligd is terwijl ze worden overgebracht tussen een clienttoepassing en -opslag is versleuteling aan de clientzijde. De gegevens worden versleuteld voordat ze worden overgedragen naar de Azure-opslag. Als de gegevens worden opgehaald uit Azure Storage, worden de gegevens worden ontsleuteld nadat het is ontvangen op de client. Hoewel de gegevens worden versleuteld via de kabel gaat, wordt aangeraden dat u ook gebruik van HTTPS, omdat deze gegevens integriteitscontroles ingebouwd die toegangsbeheermodel netwerkfouten die invloed hebben op de integriteit van de gegevens.

Client-side '-versleuteling is ook een methode voor het versleutelen van uw gegevens in rust, zoals de gegevens worden opgeslagen in de versleutelde vorm. Bespreken we dit in de sectie uitvoeriger op [versleuteling in rust](#encryption-at-rest).

## <a name="encryption-at-rest"></a>Codering in rust
Er zijn drie Azure-functies die at-rest versleuteling wilt gebruiken een. Azure Disk Encryption wordt gebruikt voor het versleutelen van het besturingssysteem en gegevensschijven in IaaS virtuele Machines. De twee andere – clientzijde versleuteling en SSE – zijn beide gebruikt voor het versleutelen van gegevens in Azure Storage. Laten we kijken naar elk van deze, en vervolgens Doe een vergelijking en Zie wanneer elk ervan kan worden gebruikt.

U kunt aan de clientzijde versleuteling gebruiken voor het versleutelen van de gegevens onderweg (dit wordt ook opgeslagen in de versleutelde vorm in de opslag), kunt u desgewenst gewoon HTTPS gebruiken tijdens de overdracht en hebben enkele manier voor de gegevens worden automatisch versleuteld wanneer deze is opgeslagen. Er zijn twee manieren hiervoor--Azure Disk Encryption en SSE. Een wordt gebruikt voor het versleutelen van de gegevens op het besturingssysteem en gegevensschijven die wordt gebruikt door virtuele machines rechtstreeks en de andere wordt gebruikt voor het versleutelen van gegevens naar Azure Blob-opslag geschreven.

### <a name="storage-service-encryption-sse"></a>Versleuteling van opslag-Service (SSE)
SSE kunt u aanvragen dat de storage-service de gegevens automatisch coderen wanneer het schrijven naar Azure Storage. Als u de gegevens uit Azure Storage leest, wordt deze door de storage-service worden ontsleuteld voordat het wordt geretourneerd. Hiermee kunt u uw gegevens te beveiligen zonder code wijzigen of voeg code toe aan alle toepassingen.

Dit is een instelling die van toepassing op het hele opslagaccount is. U kunt inschakelen en uitschakelen van deze functie door de waarde van de instelling te wijzigen. Om dit te doen, kunt u de Azure-portal, PowerShell, de Azure CLI, de REST-API van Storage Resource Provider of de Storage-clientbibliotheek voor .NET. SSE is standaard uitgeschakeld.

Op dit moment worden de sleutels die worden gebruikt voor het versleutelingsniveau dat wordt beheerd door Microsoft. We oorspronkelijk de sleutels genereren en beheren van de veilige opslag van de sleutels, evenals de reguliere draaiing zoals gedefinieerd door intern beleid van Microsoft. U wordt in de toekomst krijgen de mogelijkheid voor het beheren van uw eigen versleutelingssleutels en bieden een migratiepad van door Microsoft beheerde sleutels naar de klant beheerde sleutels.

Deze functie is beschikbaar voor Standard en Premium-opslag-accounts die zijn gemaakt met het implementatiemodel van Resource Manager. SSE is van toepassing op elk soort gegevens: blok-blobs, pagina-blobs, toevoeg-blobs, tabellen, wachtrijen en -bestanden.

Gegevens worden alleen versleuteld als SSE is ingeschakeld en de gegevens worden geschreven naar Blob Storage. In- of uitschakelen van SSE heeft geen gevolgen voor bestaande gegevens. Met andere woorden, wanneer u deze versleuteling inschakelt, wordt deze niet teruggaan en gegevens versleutelen die al bestaat; ook worden de gegevens die al aanwezig is wanneer het uitschakelen van SSE gedecodeerd.

Als u deze functie gebruiken met een opslagaccount van klassiek wilt, kunt u een nieuwe Resource Manager-opslagaccount maken en AzCopy gebruiken om de gegevens kopiëren naar de nieuwe account.

### <a name="client-side-encryption"></a>Client-side-versleuteling
Versleuteling aan clientzijde wordt gezegd wanneer u de versleuteling van de gegevens onderweg. Deze functie kunt u uw gegevens in een clienttoepassing voordat het wordt verzonden via de kabel worden geschreven naar Azure Storage en programmatisch uw om gegevens te ontsleutelen deze bij het ophalen van Azure Storage programmatisch te versleutelen.

Dit biedt versleuteling onderweg, maar biedt ook de functie van versleuteling in rust. Houd er rekening mee dat hoewel de gegevens onderweg zijn versleuteld, nog steeds HTTPS aangeraden om te profiteren van de ingebouwde gegevens integriteitscontroles die toegangsbeheermodel netwerkfouten die invloed hebben op de integriteit van de gegevens.

Een voorbeeld van waar u dit kunt gebruiken is als u een webtoepassing die blobs opgeslagen en opgehaald van BLOB's hebt en u wilt dat de toepassing en gegevens worden zo veilig mogelijk. In dat geval gebruikt u versleuteling aan clientzijde. Het verkeer tussen de client en de Azure Blob-Service bevat de versleutelde resource en niemand kan interpreteren van de gegevens onderweg en het opnieuw samenstellen in uw persoonlijke blobs.

Versleuteling aan clientzijde is ingebouwd in de Java en de clientbibliotheken .NET storage, dat op zijn beurt de Azure Key Vault API's, zodat u heel eenvoudig bij de implementatie te gebruiken. Het proces van het versleutelen en ontsleutelen van de gegevens de envelop techniek gebruikt en die wordt gebruikt door de versleuteling in elke opslagobject metagegevens worden opgeslagen. Bijvoorbeeld: voor blobs, het opgeslagen in de blobmetagegevens, terwijl voor wachtrijen, het voegt het toe aan het bericht voor elke wachtrij.

U kunt voor de versleuteling zelf, genereren en beheren van uw eigen versleutelingssleutels. U kunt ook sleutels die zijn gegenereerd door de Azure Storage-clientbibliotheek gebruiken of u kunt de Azure Sleutelkluis genereren van de sleutels hebben. U kunt uw versleutelingssleutels opslaan in de opslag van uw lokale sleutels of kunt u ze opslaan in een Azure Sleutelkluis. Azure Sleutelkluis kunt u toegang verlenen tot de geheimen in Azure Key Vault voor specifieke gebruikers met Azure Active Directory. Dit betekent dat niet alleen iedereen kan lezen van de Azure Sleutelkluis en de sleutels die u voor versleuteling aan clientzijde gebruikt ophalen.

#### <a name="resources"></a>Resources
* [Blobs in Microsoft Azure Storage met Azure Key Vault versleutelen en ontsleutelen](../blobs/storage-encrypt-decrypt-blobs-key-vault.md)

  In dit artikel laat zien hoe client-side '-versleuteling gebruiken met Azure Sleutelkluis, inclusief het maken van de KEK en op te slaan in de kluis met behulp van PowerShell.
* [Client-Side-versleuteling en Azure Sleutelkluis voor Microsoft Azure Storage](../storage-client-side-encryption.md)

  Dit artikel geeft een uitleg van client-side '-versleuteling en vindt u voorbeelden van het gebruik van de storage-clientbibliotheek voor het versleutelen en ontsleutelen van bronnen van de vier storage-services. Het is ook wordt gesproken over Azure Sleutelkluis.

### <a name="using-azure-disk-encryption-to-encrypt-disks-used-by-your-virtual-machines"></a>Met behulp van Azure Disk Encryption schijven die worden gebruikt door uw virtuele machines versleutelen
Azure Disk Encryption is een nieuwe functie. Deze functie kunt u de OS-schijven en gegevensschijven die wordt gebruikt door een virtuele Machine voor IaaS versleutelen. De stations zijn versleuteld met behulp van standaard-BitLocker-versleuteling technologie voor Windows. De schijven zijn versleuteld met behulp van de technologie DM-Crypt voor Linux. Dit is geïntegreerd met Azure Sleutelkluis kunt u te controleren en beheren van de versleutelingssleutels voor de schijf.

De oplossing ondersteunt de volgende scenario's voor IaaS VM's wanneer ze zijn ingeschakeld in Microsoft Azure:

* Integratie met Azure Sleutelkluis
* Standard-laag VMs: [A, D, DS, G, GS en enzovoort reeks IaaS VM's](https://azure.microsoft.com/pricing/details/virtual-machines/)
* Codering voor Windows en Linux IaaS VM's inschakelen
* Het uitschakelen van versleuteling op besturingssysteem en schijven voor Windows IaaS VM 's
* Codering op gegevensstations voor Linux IaaS VM's uitschakelen
* IaaS VM's waarop Windows client-OS-codering inschakelen
* Versleuteling voor volumes met koppelpunten paden inschakelen
* Op Linux-virtuele machines die zijn geconfigureerd met de schijf (RAID) te verwijderen met behulp van mdadm-codering inschakelen
* Inschakelen van versleuteling op Linux VM's met behulp van LVM voor gegevensschijven
* Op Windows-virtuele machines die zijn geconfigureerd met behulp van opslagruimten-codering inschakelen
* Alle openbare Azure-regio worden ondersteund

De oplossing ondersteunt niet de volgende scenario's, functies en -technologie in de release:

* Basisstaffel IaaS VM 's
* Het uitschakelen van Linux IaaS VM's op een station OS versleuteling
* IaaS VM's die zijn gemaakt met behulp van de methode voor het maken van klassieke VM
* Integratie met uw on-premises Key Management Service
* Azure Files (gedeelde bestandssysteem), Network File System (NFS), dynamische volumes en virtuele machines van Windows die zijn geconfigureerd met software gebaseerde RAID-systemen


> [!NOTE]
> Schijfversleuteling Linux-besturingssysteem wordt momenteel ondersteund in de volgende Linux-distributies: RHEL 7.2, CentOS 7.2n en Ubuntu 16.04.
>
>

Deze functie zorgt ervoor dat alle gegevens voor de virtuele machine-schijven is versleuteld in rust in Azure Storage.

#### <a name="resources"></a>Resources
* [Azure Disk Encryption for Windows and Linux IaaS VM 's](https://docs.microsoft.com/azure/security/azure-security-disk-encryption)

### <a name="comparison-of-azure-disk-encryption-sse-and-client-side-encryption"></a>Vergelijking van Azure Disk Encryption, SSE en Client-Side-versleuteling
#### <a name="iaas-vms-and-their-vhd-files"></a>IaaS VM's en hun VHD-bestanden
Voor schijven die door IaaS VM's gebruikt, wordt u aangeraden Azure Disk Encryption. U kunt SSE voor het versleutelen van de VHD-bestanden die worden gebruikt voor back-deze schijven in Azure Storage inschakelen, maar alleen nieuw geschreven gegevens worden versleuteld. Dit betekent dat als u een virtuele machine maken en schakel vervolgens SSE op het opslagaccount waarin het VHD-bestand, alleen de wijzigingen worden versleuteld, niet de oorspronkelijke VHD-bestand.

Als u een VM die gebruikmaakt van een afbeelding uit Azure Marketplace maakt, Azure voert een [recente kopie](https://en.wikipedia.org/wiki/Object_copying) van de installatiekopie naar uw storage-account in Azure Storage en is niet versleuteld zelfs als er SSE ingeschakeld. Nadat de virtuele machine wordt gemaakt en begint met het bijwerken van de installatiekopie, start SSE de gegevens te coderen. Daarom is het beste Azure Disk Encryption op virtuele machines die zijn gemaakt op basis van installatiekopieën in Azure Marketplace als u deze volledig versleuteld wilt gebruiken.

Als u een vooraf zijn versleuteld virtuele machine in Azure van on-premises brengt, kunt u zich de versleutelingssleutels uploaden naar Azure Sleutelkluis en doorgaan met de codering voor die VM dat u waren met on-premises. Azure Disk Encryption is ingeschakeld voor het afhandelen van dit scenario.

Als u niet-versleutelde VHD on-premises hebt, kunt u deze in de galerie als een aangepaste installatiekopie uploaden en inrichten van een virtuele machine uit. Als u dit met behulp van de Resource Manager-sjablonen doen, kunt u deze om in te schakelen wanneer deze wordt opgestart van de virtuele machine Azure Disk Encryption vragen.

Wanneer u een gegevensschijf toevoegen en op de virtuele machine te koppelen, kunt u Azure Disk Encryption inschakelen op de gegevensschijf. De gegevensschijf van die lokaal eerst zal worden versleuteld en de service management-laag wordt vervolgens een vertraagde schrijven tegen opslag doen, zodat de inhoud van de opslag is versleuteld.

#### <a name="client-side-encryption"></a>Clientversleuteling
Client-side '-versleuteling is de veiligste methode van het versleutelen van uw gegevens, omdat het versleutelt voordat de doorvoer en de gegevens in rust versleutelt. Het is echter vereist dat u code toevoegen aan uw toepassingen met behulp van opslag, waarmee u niet wilt doen. U kunt in deze gevallen HTTPs gebruiken voor uw gegevens in de doorvoer en SSE om de gegevens in rust te versleutelen.

U kunt tabelentiteiten, Wachtrijberichten en blobs versleutelen met client-side '-versleuteling. U kunt alleen blobs versleutelen met SSE. Als u table en queue gegevens moet moeten worden versleuteld, moet u versleuteling aan clientzijde.

Versleuteling aan clientzijde wordt volledig beheerd door de toepassing. Dit is de veiligste benadering, maar vereist dat u programmatische wijzigingen aanbrengen in uw toepassing en processen voor sleutelbeheer plaatsen in plaats. U kunt dit zou gebruiken wanneer u de extra beveiliging tijdens de overdracht wilt en u wilt dat uw opgeslagen gegevens worden versleuteld.

Versleuteling aan clientzijde meer belasting van de client is en u moet voor dit account in uw plan schaalbaarheid, vooral als u versleutelen en overdracht van grote hoeveelheden gegevens.

#### <a name="storage-service-encryption-sse"></a>Versleuteling van opslag-Service (SSE)
SSE wordt beheerd door Azure Storage. Met behulp van SSE biedt geen voor de beveiliging van de gegevens onderweg, maar deze de gegevens Hiermee versleutelen zoals naar Azure Storage geschreven. Er zijn geen gevolgen op de prestaties bij gebruik van deze functie.

U kunt elk soort gegevens van het opslagaccount met behulp van SSE versleutelen (blok-blobs, toevoeg-blobs, pagina-blobs, tabelgegevens, wachtrijgegevens en bestanden).

Als u een archiveren of bibliotheek van VHD-bestanden die u als basis gebruikt voor het maken van nieuwe virtuele machines hebt, kunt u een nieuw opslagaccount maken, SSE inschakelen en vervolgens de VHD-bestanden uploaden naar dat account. De VHD-bestanden wordt versleuteld door Azure Storage.

Als u Azure Disk Encryption is ingeschakeld voor de schijven in een virtuele machine en de SSE ingeschakeld op het opslagaccount dat u de VHD-bestanden bevat hebt, werkt deze goed; Dit leidt ertoe dat geen nieuw geschreven gegevens tweemaal wordt versleuteld.

## <a name="storage-analytics"></a>Storage Analytics
### <a name="using-storage-analytics-to-monitor-authorization-type"></a>Met behulp van Storage Analytics autorisatie type bewaken
U kunt Azure Storage Analytics logboekregistratie uitvoeren en metrische gegevens opslaan inschakelen voor elke storage-account. Dit is een uitstekend hulpprogramma moet worden gebruikt wanneer u wilt controleren van de maatstaven voor prestaties van een opslagaccount of problemen met een opslagaccount omdat u problemen met de prestaties ondervindt.

Een andere gegevenseenheid u in de logboeken van storage analytics ziet is de verificatiemethode die door iemand wordt gebruikt wanneer ze toegang hebben tot opslag. Bijvoorbeeld, met Blob Storage ziet u als ze een Shared Access Signature of de opslagaccountsleutels gebruikt, of als de toegang tot blob openbaar is.

Dit is heel nuttig als u toegang tot opslag nauw zijn beveiligen. Bijvoorbeeld in Blob Storage kunt u alle van de containers ingesteld op persoonlijke en het gebruik van een SAS-service implementeren in uw toepassingen. Vervolgens kunt u de logboeken regelmatig om te zien als uw blobs worden geopend met behulp van de opslagaccountsleutels, dit kunnen duiden op een schending van beveiliging, of als de blobs openbaar zijn, maar ze mag niet controleren.

#### <a name="what-do-the-logs-look-like"></a>Wat de logboeken eruit?
Nadat u de metrische gegevens storage account inschakelen en aan te melden via de Azure portal, analytische gegevens snel worden gestart. De logboekregistratie en metrische gegevens voor elke service is gescheiden; de logboekregistratie is alleen geschreven wanneer er activiteiten in die storage-account, terwijl de metrische gegevens worden geregistreerd elke minuut of elk uur per dag uit, afhankelijk van hoe u configureert.

De logboeken worden opgeslagen in een blok-blobs in een container met de naam $logs in het opslagaccount. Deze container wordt automatisch gemaakt wanneer Opslaganalyse is ingeschakeld. Zodra deze container is gemaakt, verwijderen u niet, maar u kunt de inhoud ervan verwijderen.

Er is een map voor elke service onder de container $logs en submappen er voor het jaar/maand/dag/uur te zijn. De logboeken zijn gewoon genummerd onder uur. Dit is de mapstructuur zoeken:

![Weergave van logboekbestanden](./media/storage-security-guide/image1.png)

Elke aanvraag naar Azure Storage wordt vastgelegd. Hier volgt een momentopname van een logboekbestand, met de eerste paar velden.

![Momentopname van een logboekbestand](./media/storage-security-guide/image2.png)

U kunt zien dat u de Logboeken gebruiken kunt voor het bijhouden van elk soort aanroepen naar een opslagaccount.

#### <a name="what-are-all-of-those-fields-for"></a>Wat zijn alle die velden voor?
Er is een artikel dat wordt vermeld in de onderstaande die bronnen bevat de lijst met de veel velden in de logboeken en wat ze worden gebruikt. Hier volgt de lijst met velden in volgorde:

![Momentopname van de velden in een logboekbestand](./media/storage-security-guide/image3.png)

We geïnteresseerd bent in de vermeldingen voor GetBlob en hoe ze zijn geverifieerd, dus moeten we vermeldingen met type bewerking 'Get-Blob' zoekt, en controleer de status van de aanvraag (4<sup>th</sup> kolom) en de autorisatie-type (8<sup>th</sup> kolom).

Bijvoorbeeld, in de eerste paar rijen in de bovenstaande aanbieding, de status van de aanvraag is 'Geslaagd' en de autorisatie-type 'geverifieerd'. Dit betekent dat de aanvraag is gevalideerd met behulp van de sleutel van het opslagaccount.

#### <a name="how-are-my-blobs-being-authenticated"></a>Hoe worden mijn blobs geverifieerd?
We hebben drie gevallen waarin we geïnteresseerd bent.

1. De blob openbaar is en toegankelijk is via een URL zonder een Shared Access Signature. In dit geval wordt de status van de aanvraag is 'AnonymousSuccess' en het autorisatie-type is 'anonymous'.

   1.0; 2015-11-17T02:01:29.0488963Z; GetBlob; **AnonymousSuccess**; 200; 124; 37; **anonieme**; mystorage...
2. De blob is persoonlijk en is gebruikt met een Shared Access Signature. In dit geval wordt de status van de aanvraag is 'SASSuccess' en het autorisatie-type is 'sas'.

   1.0; 2015-11-16T18:30:05.6556115Z; GetBlob; **SASSuccess**; 200; 416; 64; **SAS**; mystorage...
3. De blob is persoonlijk en de opslagsleutel gebruikt om deze te openen. In dit geval wordt de status van de aanvraag is '**geslaagd**'en het autorisatie-type is'**geverifieerde**'.

   1.0; 2015-11-16T18:32:24.3174537Z; GetBlob; **Geslaagd**; 206 59; 22; **geverifieerde**; mystorage...

U kunt de Microsoft Message Analyzer weergeven en analyseren van deze logboeken. Het bevat de mogelijkheden van zoeken en filteren. Bijvoorbeeld, u mogelijk wilt zoeken voor exemplaren van GetBlob om te zien of het gebruik van wat u verwacht, dat wil zeggen om ervoor te zorgen dat iemand anders wordt uw storage-account niet verkeerd opent.

#### <a name="resources"></a>Resources
* [Storage Analytics](../storage-analytics.md)

  Dit artikel is een overzicht van storage analytics en hoe u ze inschakelen.
* [Storage Analytics logboekindeling](https://msdn.microsoft.com/library/azure/hh343259.aspx)

  Dit artikel ziet u de indeling voor logboekbestanden Storage Analytics en details op de beschikbare velden, zoals verificatie van het type, die welk type verificatie dat wordt gebruikt voor de aanvraag aangeeft.
* [Een Opslagaccount in de Azure portal controleren](../storage-monitor-storage-account.md)

  In dit artikel laat zien hoe maatstaven voor controle en logboekregistratie voor een opslagaccount te configureren.
* [End-to-End-problemen oplossen met metrische gegevens van Azure Storage en logboekregistratie, AzCopy en Message Analyzer](../storage-e2e-troubleshooting.md)

  Dit artikel wordt gesproken over het oplossen van problemen met de Storage Analytics en laat zien hoe u de Microsoft Message Analyzer.
* [Microsoft Message Analyzer besturingssysteem handleiding](https://technet.microsoft.com/library/jj649776.aspx)

  In dit artikel is van de referentie voor de Microsoft Message Analyzer en bevat koppelingen naar een zelfstudie: snel starten en samenvatting-functie.

## <a name="cross-origin-resource-sharing-cors"></a>CORS (Cross-Origin Resource Sharing)
### <a name="cross-domain-access-of-resources"></a>Domeintoegang van resources
Wanneer een webbrowser in één domein met een HTTP-aanvraag voor een resource van een ander domein, is dit een cross-origin-HTTP-aanvraag genoemd. Een HTML-pagina aangeboden via contoso.com maakt bijvoorbeeld een aanvraag voor een jpeg gehost op fabrikam.blob.core.windows.net. Uit veiligheidsoverwegingen beperken browsers cross-origin HTTP-aanvragen is gestart vanaf in scripts, zoals JavaScript. Dit betekent dat wanneer wat JavaScript-code op een webpagina op contoso.com die jpeg op fabrikam.blob.core.windows.net aanvraagt, de browser wordt niet toestaan dat de aanvraag.

Wat heeft dit doen met Azure Storage? Ook als u statische activa zoals JSON of XML-gegevensbestanden zijn opgeslagen in Blob Storage met behulp van een opslagaccount Fabrikam genoemd, het domein voor de activa zijn fabrikam.blob.core.windows.net en de webtoepassing contoso.com niet mogelijk toegang toe hebben met JavaScript omdat de domeinen verschillend zijn. Dit geldt ook zijn als u probeert aan te roepen op een van de Azure Storage-Services – zoals Table Storage – die JSON-gegevens worden verwerkt door de JavaScript-client geretourneerd.

#### <a name="possible-solutions"></a>Mogelijke oplossingen
Er is een manier om dit probleem oplossen voor het toewijzen van een aangepast domein, zoals 'storage.contoso.com' aan fabrikam.blob.core.windows.net. Het probleem is dat u kunt alleen aangepaste domein toewijzen aan één opslagaccount opgeven. Wat gebeurt er als de activa zijn opgeslagen in meerdere opslagaccounts?

Dit probleem oplossen op een andere manier is dat de webtoepassing die fungeren als proxy voor het aanroepen van de opslag. Dit betekent dat als u een bestand naar Blob Storage uploaden wilt, de webtoepassing zou lokaal schrijven en vervolgens te kopiëren naar Blob-opslag of het zou alles in het geheugen worden gelezen en vervolgens te schrijven naar Blob Storage. U kunt ook een specifieke webtoepassing (zoals een Web-API) die de bestanden lokaal uploadt en schrijft deze naar Blob Storage schrijven. In beide gevallen moet u bij het bepalen van de schaalbaarheid moet voor die functie-account.

#### <a name="how-can-cors-help"></a>Hoe kunt CORS
Azure-opslag kunt u inschakelen van CORS – Cross-Origin-resources delen. U kunt domeinen die toegang heeft tot de resources in dit opslagaccount opgeven voor elke storage-account. We kunnen in ons geval die hierboven worden beschreven, bijvoorbeeld CORS inschakelen in de fabrikam.blob.core.windows.net storage-account en configureer deze voor toegang tot contoso.com. De web application contoso.com kan vervolgens rechtstreeks toegang tot de resources in fabrikam.blob.core.windows.net.

Er is alleen Houd er rekening mee dat CORS toegang toestaat, maar biedt geen verificatie is vereist voor alle niet-openbare toegang tot storage-resources. Dit betekent dat u hebt alleen toegang tot blobs als ze openbaar zijn of u een Shared Access Signature zodat u de benodigde machtiging omvat. Tabellen, wachtrijen en -bestanden hebben geen openbare toegang en een SAS vereisen.

CORS is standaard uitgeschakeld op alle services. U kunt CORS inschakelen met behulp van de REST-API of de storage-clientbibliotheek aan te roepen op een van de methoden om de beleidsregels van de service. Wanneer u dat doet, kunt u een regel CORS, dat zich in XML bevindt opnemen. Hier volgt een voorbeeld van een CORS-regel die is ingesteld met behulp van de bewerking van de Service-eigenschappen instellen voor de Blob-Service voor een opslagaccount. U kunt uitvoeren die voor deze bewerking met de storage-clientbibliotheek of de REST API's voor Azure Storage.

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

* **AllowedOrigins** dit geeft aan welke domeinen niet-overeenkomend kunnen aanvragen en gegevens ontvangen van de storage-service. Dit staat dat contoso.com en fabrikam.com gegevens uit Blob-opslag voor een specifieke storage-account aanvragen kunnen. U kunt dit ook instellen op een jokerteken (\*) zodat alle domeinen voor toegang tot aanvragen.
* **AllowedMethods** dit is de lijst met methoden (HTTP-aanvraag verbs) die kunnen worden gebruikt wanneer de aanvraag. In dit voorbeeld worden alleen PUT en GET toegestaan. U kunt dit instellen op een jokerteken (\*) om toe te staan alle methoden moet worden gebruikt.
* **AllowedHeaders** dit is de aanvraagheaders die het brondomein opgeven kunt wanneer de aanvraag. In dit voorbeeld zijn alle metagegevens headers beginnen met x-ms-meta-data x-ms-meta-doel en de x-ms-meta-abc toegestaan. Het jokerteken (\*) geeft aan dat een koptekst die begint met het opgegeven voorvoegsel is toegestaan.
* **ExposedHeaders** Zo weet welke antwoordheaders moeten worden weergegeven door de browser van de uitgever van de aanvraag. In dit voorbeeld wordt een koptekst die beginnen met ' x-ms - meta-' zichtbaar.
* **MaxAgeInSeconds** dit is de maximale tijdsduur dat een browser cache worden opgeslagen door de voorbereidende opties-aanvraag. (Raadpleeg de onderstaande eerste artikel voor meer informatie over de voorbereidende aanvraag).

#### <a name="resources"></a>Resources
Controleer deze bronnen voor meer informatie over CORS en hoe deze in te schakelen.

* [Cross-Origin-Resource delen (CORS) ondersteuning voor de Azure Storage-Services op Azure.com](../storage-cors-support.md)

  Dit artikel bevat een overzicht van CORS en het instellen van de regels voor de verschillende storage-services.
* [Cross-Origin-Resource delen (CORS) ondersteuning voor de Azure Storage-Services op MSDN](https://msdn.microsoft.com/library/azure/dn535601.aspx)

  Dit is de documentatie bij de CORS-ondersteuning voor de Azure Storage-Services. Dit bevat koppelingen naar artikelen toepassen op elke storage-service en toont een voorbeeld en wordt uitgelegd voor elk element in de CORS-bestand.
* [Microsoft Azure Storage: Introducing CORS](http://blogs.msdn.com/b/windowsazurestorage/archive/2014/02/03/windows-azure-storage-introducing-cors.aspx)

  Dit is een koppeling naar het artikel initiële blog aangekondigd CORS en het gebruik ervan wordt weergegeven.

## <a name="frequently-asked-questions-about-azure-storage-security"></a>Veelgestelde vragen over Azure Storage-beveiliging
1. **Hoe kan ik controleren van de integriteit van de blobs die ik van of naar Azure Storage brengen ben als ik het HTTPS-protocol niet gebruiken?**

   Als voor een bepaalde reden moet u HTTP gebruiken in plaats van HTTPS en u werkt met blok-blobs, kunt u MD5 controleren om te controleren of de integriteit van de blobs worden overgebracht. Dit helpt met bescherming tegen netwerk/transport layer fouten, maar niet noodzakelijkerwijs met tussenliggende aanvallen.

   Als u HTTPS, waardoor transport level security, dan is het gebruik van MD5 controleren redundante en onnodige gebruiken kunt.

   Voor meer informatie raadpleegt de [overzicht van Azure Blob-MD5](http://blogs.msdn.com/b/windowsazurestorage/archive/2011/02/18/windows-azure-blob-md5-overview.aspx).
2. **Hoe zit het met FIPS-naleving voor de Verenigde Staten Government?**

   De Verenigde Staten Federal Information Processing Standard (FIPS) definieert cryptografische algoritmen die zijn goedgekeurd voor gebruik door het Amerikaanse Computersystemen overheid ter bescherming van gevoelige gegevens. Inschakelen FIPS-modus op een WindowsServer of het bureaublad Hiermee geeft u het besturingssysteem dat alleen FIPS-gevalideerde cryptografische algoritmen moeten worden gebruikt. Als een toepassing gebruikt een niet-compatibele algoritmen, worden de toepassingen wordt verbroken. Versies van With.NET Framework 4.5.2 of hoger, verandert de toepassing automatisch de cryptografiealgoritmen voor het gebruik compatibele FIPS-algoritmen als de computer zich in de FIPS-modus.

   Microsoft blijft deze tot elke klant om te bepalen of FIPS-modus in te schakelen. We zijn ervan overtuigd dat er geen dwingende redenen voor klanten die niet zijn onderworpen aan wettelijke voorschriften FIPS-modus standaard inschakelen.

   **Bronnen**

* [Waarom We je niet aanbevelen 'FIPS-modus' meer](https://blogs.technet.microsoft.com/secguide/2014/04/07/why-were-not-recommending-fips-mode-anymore/)

  In dit artikel blog geeft een overzicht van FIPS en wordt uitgelegd waarom ze niet standaard ingeschakeld in de FIPS-modus.
* [FIPS 140 validatie](https://technet.microsoft.com/library/cc750357.aspx)

  In dit artikel bevat informatie over hoe Microsoft-producten en cryptografiemodules aan de FIPS-standaard voor de Verenigde Staten voldoet Federale overheid.
* [' Systeemcryptografie: gebruik FIPS-compatibele algoritmen voor versleuteling, hashing en ondertekening ' beveiligingsrisico van instellingen in Windows XP en latere versies van Windows](https://support.microsoft.com/kb/811833)

  In dit artikel wordt gesproken over het gebruik van FIPS-modus in oudere Windows-computers.
