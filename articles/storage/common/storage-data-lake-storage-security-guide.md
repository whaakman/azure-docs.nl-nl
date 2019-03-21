---
title: Azure Data Lake Storage Gen2 Storage-beveiligingshandleiding | Microsoft Docs
description: Details van de vele methoden van het beveiligen van Azure Storage, waaronder maar niet beperkt tot RBAC en Storage-Serviceversleuteling
services: storage
author: roygara
ms.service: storage
ms.topic: article
ms.date: 02/07/2019
ms.author: rogarana
ms.subservice: common
ms.openlocfilehash: 22b070e6d70208057c85ad6a2322cc440d12a0fa
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "58008205"
---
# <a name="azure-data-lake-storage-gen2-security-guide"></a>Azure Data Lake Storage Gen2-beveiligingshandleiding

Azure Data Lake Storage Gen2, is een verscheidenheid aan functies die is gebouwd op Azure Storage-accounts. Daarom is alle verwijzingen in dit artikel bedoeld voor een Azure Storage-account met hiërarchische naamruimte ingeschakeld (Data Lake Storage Gen2 mogelijkheden).

- Alle gegevens die zijn geschreven naar Azure Storage automatisch versleuteld met behulp van [Storage Service Encryption (SSE)](storage-service-encryption.md). Zie voor meer informatie, [standaard codering aankondiging voor Azure-Blobs, bestanden, tabellen en Queue Storage](https://azure.microsoft.com/blog/announcing-default-encryption-for-azure-blobs-files-table-and-queue-storage/).
- Azure Active Directory (Azure AD) en op rollen gebaseerd toegangsbeheer (RBAC) worden ondersteund voor Azure Storage voor zowel resource management-bewerkingen en bewerkingen voor gegevens, als volgt:
    - U kunt RBAC-rollen binnen het bereik van het opslagaccount dat aan beveiligings-principals en het gebruik van Azure AD te autoriseren resource management-bewerkingen zoals Sleutelbeheer toewijzen.
    - Azure AD-integratie wordt ondersteund voor bewerkingen voor gegevens in Azure Storage. U kunt RBAC-rollen binnen het bereik van een abonnement, resourcegroep, opslagaccount of een afzonderlijke bestandssysteem een beveiligings-principal of een beheerde identiteit voor Azure-resources kunt toewijzen. Zie voor meer informatie, [verifiëren van toegang tot Azure Storage met behulp van Azure Active Directory](storage-auth-aad.md).
- Gedelegeerde toegang tot de gegevensobjecten in Azure Storage kan worden verleend met behulp van [Shared Access Signatures](../storage-dotnet-shared-access-signature-part-1.md).

Dit artikel bevat een overzicht van elk van deze beveiligingsfuncties die kunnen worden gebruikt met Azure Storage. Koppelingen zijn opgegeven voor artikelen die details van elke functie, kunt u eenvoudig doen nader onderzoek op elk onderwerp.

Hier volgen de onderwerpen in dit artikel worden besproken:

* [Beveiliging gegevenslaag](#management-plane-security) – uw Storage-Account beveiligen

  De beheerlaag bestaat uit de resources die worden gebruikt voor het beheren van uw storage-account. In deze sectie bevat informatie over het Azure Resource Manager-implementatiemodel en het gebruik van Role-Based Access Control (RBAC) voor het beheren van toegang tot uw storage-accounts. Deze ook een oplossing voor beheer van uw storage-accountsleutels en hoe u ze opnieuw genereren.
* [Gegevens vlak Security](#data-plane-security) : toegang tot uw gegevens beveiligen

  In deze sectie zullen we toegang tot de objecten van de werkelijke gegevens in uw Storage-account, dat wil zeggen bestanden en mappen, zodat met handtekeningen voor gedeelde toegang en opgeslagen toegangsbeleid. We komen zowel serviceniveau SAS en SAS op accountniveau. We ziet ook hoe u wilt beperken van toegang tot een specifiek IP-adres (of een bereik van IP-adressen), het beperken van het protocol dat wordt gebruikt voor het HTTPS en hoe u een handtekening voor gedeelde toegang intrekken zonder te wachten tot het verlopen.
* [Versleuteling 'in transit'](#encryption-in-transit)

In deze sectie wordt beschreven hoe u gegevens kunt beveiligen wanneer u overbrengen naar of uit een opslagaccount met Data Lake Storage Gen2 is ingeschakeld. Bespreken we het aanbevolen gebruik van HTTPS.

## <a name="management-plane-security"></a>Beveiliging gegevenslaag

De beheerlaag bestaat uit bewerkingen die invloed hebben op het opslagaccount zelf. Bijvoorbeeld, kunt u maken of verwijderen van een storage-account, een lijst met opslagaccounts in een abonnement ophalen, de storage-accountsleutels ophalen of de storage-accountsleutels opnieuw genereren.

Deze handleiding is gericht op het Resource Manager-model van de implementatie, dit de methode is voor het storage-accounts maken met Data Lake Storage Gen2 mogelijkheden. Met het Resource Manager-opslagaccounts, in plaats van die toegang tot het hele abonnement, kunt u toegangsbeheer op het niveau van een meer beperkt tot de beheerlaag met behulp van toegangsbeheer op basis van rollen (RBAC).

### <a name="how-to-secure-your-storage-account-with-role-based-access-control-rbac"></a>Over het beveiligen van uw opslagaccount met Role-Based Access Control (RBAC)

Laten we eens wat RBAC is en hoe u deze kunt gebruiken. Elk Azure-abonnement heeft een Azure Active Directory. Gebruikers, groepen en toepassingen uit die map kunnen toegang tot het beheer van resources in het Azure-abonnement met het Resource Manager-implementatiemodel worden verleend. Dit type beveiliging wordt aangeduid als toegangsbeheer op basis van rollen (RBAC). Voor het beheren van deze toegang, gebruik **toegangsbeheer (IAM)** in Azure portal.

Met het Resource Manager-model plaatst u het opslagaccount in een resource group en beheren van toegang tot de beheerlaag van dat specifieke opslagaccount met behulp van Azure Active Directory. Bijvoorbeeld, u kunt specifieke gebruikers geven de mogelijkheid voor toegang tot de opslagaccountsleutels, terwijl andere gebruikers informatie over de storage-account bekijken kunnen, maar geen toegang de sleutels voor het opslagaccount tot.

#### <a name="granting-access"></a>Het verlenen van toegang

Toegang wordt verleend door de juiste RBAC-rol toewijzen aan gebruikers, groepen en toepassingen, op het juiste bereik. Om toegang tot het hele abonnement, moet u een rol op het abonnementsniveau van het toewijzen. U kunt toegang verlenen tot alle resources in een resourcegroep door machtigingen te verlenen aan de resourcegroep zelf. U kunt ook specifieke rollen toewijzen aan specifieke resources, zoals storage-accounts.

Hier volgen de belangrijkste punten die u weten wilt over het gebruik van RBAC voor toegang tot de bewerkingen van een Azure Storage-account:

* Voor iemand om machtiging voor toegang tot de objecten in de storage-account, hebt u ze kunt geven, kunt u machtiging voor het lezen van de storage-accountsleutels en dat de gebruiker kan vervolgens die sleutels gebruiken voor toegang tot de bestanden en mappen.
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
* [RBAC: Ingebouwde rollen](../../role-based-access-control/built-in-roles.md)

  Dit artikel worden alle van de beschikbare ingebouwde rollen in RBAC.
* [Resource Manager-implementatie en klassieke implementatie begrijpen](../../azure-resource-manager/resource-manager-deployment-model.md)

### <a name="managing-your-storage-account-keys"></a>De sleutels van uw Storage-Account beheren

De opslagaccountsleutels zijn 512-bits-tekenreeksen die zijn gemaakt door Azure die samen met de naam van het opslagaccount, kan worden gebruikt voor toegang tot de objecten die zijn opgeslagen in de storage-account, bijvoorbeeld, blobs, entiteiten in een tabel, berichten in wachtrij plaatsen en bestanden op een Azure-bestandsshare. Beheren van toegang tot de storage-account sleutels controleert de toegang tot de gegevenslaag voor dat opslagaccount.

Elk opslagaccount heeft twee sleutels die worden aangeduid als "Sleutel 1" en "Sleutel 2" in de [Azure-portal](https://portal.azure.com/) en in de PowerShell-cmdlets. Deze kunnen worden hersteld handmatig met behulp van een van de verschillende methoden, met inbegrip van, maar niet beperkt tot met behulp van de [Azure-portal](https://portal.azure.com/), PowerShell, de Azure CLI of programmatisch met behulp van de Storage-clientbibliotheek voor .NET of de Azure Storage-Services REST-API.

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

Een andere optie is om de opslagaccountsleutel een [Azure Key Vault](https://azure.microsoft.com/services/key-vault/) als een geheim en zijn uw toepassingen ophalen van de toegangssleutel van daaruit. Klik wanneer u de sleutel opnieuw genereren en bijwerken van de Azure Key Vault, hoeft de toepassingen niet te worden geïmplementeerd omdat ze gewoon door de nieuwe sleutel van de Azure Key Vault automatisch. U kunt de toepassing telkens wanneer die u ze nodig hebt voor de sleutel lezen hebben, of u kunt opslaan in cache deze in het geheugen en als dit mislukt wanneer u de sleutel opnieuw ophalen uit de Azure Key Vault.

Een ander niveau van beveiliging voor uw storage-sleutels met behulp van Azure Key Vault ook worden toegevoegd. Als u deze methode gebruikt, hebt u nooit de opslag sleutel vastgelegd in een configuratiebestand, waardoor deze doorverwezen iemand de toegang tot de sleutels zonder de uitdrukkelijke toestemming worden verwijderd.

Een ander voordeel van het gebruik van Azure Key Vault is dat u kunt ook toegang tot uw sleutels met behulp van Azure Active Directory beheren. Dit betekent dat u kunt toegang verlenen tot het aantal toepassingen waarvoor de sleutels ophalen uit Azure Key Vault en weten dat andere toepassingen pas weer toegang krijgen tot de sleutels zonder deze machtiging verlenen specifiek.

> [!NOTE]
> Microsoft adviseert om slechts één van de sleutels in al uw toepassingen op hetzelfde moment. Als u met sleutel 1 op bepaalde plaatsen en sleutel 2 in andere, wordt het niet mogelijk uw om sleutels te rouleren zonder dat een toepassing toegang verliezen.

#### <a name="resources"></a>Resources

* [Instellingen van opslagaccount in Azure portal beheren](storage-account-manage.md)
* [Naslaginformatie over de REST-API voor de Azure Storage-resourceprovider](https://msdn.microsoft.com/library/mt163683.aspx)

## <a name="data-plane-security"></a>Beveiliging van gegevens vlak
Beveiliging van gegevens vlak verwijst naar de methoden voor het beveiligen van de gegevensobjecten die zijn opgeslagen in Azure Storage. We hebben gezien methoden voor het versleutelen van de gegevens en de beveiliging tijdens de overdracht van de gegevens, maar hoe u doen over het beheren van toegang tot de objecten?

Hebt u drie opties voor het verlenen van toegang tot objecten in Azure Storage, met inbegrip van:

- Met behulp van Azure AD toegang verlenen aan de bestandssystemen en wachtrijen. Azure AD biedt voordelen ten opzichte van andere methoden voor autorisatie, waaronder de noodzaak voor het opslaan van geheimen in uw code verwijderen. Zie voor meer informatie, [verifiëren van toegang tot Azure Storage met behulp van Azure Active Directory](storage-auth-aad.md). 
- Met behulp van de sleutels van uw storage-account om toegang via gedeelde sleutel te verlenen. Autoriseren via gedeelde sleutel moet opslaan van de sleutels van uw storage-account in uw toepassing, zodat Microsoft raadt het Azure AD in plaats daarvan gebruik waar mogelijk. Voor productie-Apps, of voor het verlenen van toegang tot Azure-tabellen en bestanden, voert u de gedeelde sleutel gebruiken terwijl Azure AD-integratie beschikbaar als preview is.
- Met behulp van handtekeningen voor gedeelde toegang te verlenen omschreven machtigingen beschikt voor specifieke objecten gedurende een bepaalde periode.

Naast het beperken van toegang tot en met autorisatie, u kunt ook gebruiken [Firewalls en virtuele netwerken](storage-network-security.md) om te beperken van toegang tot het opslagaccount op basis van netwerkregels.  Met deze benadering kan weigeren van toegang tot openbare internetverkeer en verlenen toegang tot alleen specifieke Azure Virtual Networks of openbare internet IP-adresbereiken.

### <a name="storage-account-keys"></a>Opslagaccountsleutels

Opslagaccountsleutels zijn 512-bits-tekenreeksen die zijn gemaakt door Azure, die samen met de naam van het opslagaccount, kan worden gebruikt voor toegang tot de gegevensobjecten die zijn opgeslagen in de storage-account.

U kunt bijvoorbeeld bestanden lezen. Veel van deze acties kunnen worden uitgevoerd via de Azure-portal of met behulp van een van de vele Storage Explorer-toepassingen. U kunt ook schrijven van code voor het gebruik van de REST-API om uit te voeren van deze bewerkingen.

Zoals beschreven in de sectie op de [Management vlak Security](#management-plane-security), toegang tot de opslagsleutels voor een opslagaccount met behulp van de Azure Resource Manager-model kan worden beheerd via op rollen gebaseerd toegangsbeheer (RBAC).

### <a name="how-to-delegate-access-to-objects-in-your-account-using-shared-access-signatures-and-stored-access-policies"></a>Het overdragen van toegang tot objecten in uw account met behulp van handtekeningen voor gedeelde toegang en opgeslagen toegangsbeleid

Een Shared Access Signature is een tekenreeks met een beveiligingstoken dat kan worden gekoppeld aan een URI waarmee u toegang tot opslagobjecten delegeren en beperkingen, zoals de machtigingen en de datum-/ tijdbereik van toegang opgeven.

U kunt toegang verlenen tot bestanden of mappen.

U kunt een SAS-token mogen gebruiken om afbeeldingen te uploaden naar een bestandssysteem in Blob-opslag en een web toepassing machtigt om te lezen die foto's één klant. In beide gevallen wordt er een scheiding van taken – elke toepassing kan alleen de toegang die ze nodig hebben om uit te voeren hun taak worden verleend. Dit is mogelijk door het gebruik van handtekeningen voor gedeelde toegang.

#### <a name="why-you-want-to-use-shared-access-signatures"></a>Waarom u wilt gebruiken van handtekeningen voor gedeelde toegang

Waarom wilt u een SAS in plaats van alleen geven van de sleutel van uw opslagaccount, dit dus veel eenvoudiger is te gebruiken? Doorgeven van de sleutel van uw opslagaccount is vergelijkbaar met de sleutels van uw opslag Koninkrijk delen. Deze verleent volledige toegang. Iemand kan uw sleutels te gebruiken en hun volledige muziekbibliotheek uploaden naar uw opslagaccount. Ze kunnen ook uw bestanden vervangen door versies geïnfecteerde of uw gegevens te stelen. Direct onbeperkte toegang geven tot uw storage-account is iets dat geen licht moeten worden genomen.

Met handtekeningen voor gedeelde toegang, kunt u een client alleen de machtigingen die vereist zijn voor een beperkte hoeveelheid van de tijd geven. Bijvoorbeeld, als iemand anders een bestand naar uw account uploaden wordt, verleent u hen toegang voor schrijven voor voldoende tijd voor het uploaden van het bestand (afhankelijk van de grootte van het bestand natuurlijk). En als u van gedachten verandert, kunt u dat de toegang intrekken.

Daarnaast kunt u opgeven dat aanvragen met behulp van een SAS beperkt tot een bepaalde IP-adres of IP-adresbereik externe naar Azure zijn. U kunt ook vereisen dat aanvragen worden gedaan met behulp van een specifieke protocol (HTTPS of HTTP/HTTPS). Dit betekent dat als u wilt dat alleen HTTPS-verkeer toe te staan, u het vereiste protocol alleen op HTTPS instellen kunt en HTTP-verkeer wordt geblokkeerd.

#### <a name="definition-of-a-shared-access-signature"></a>Definitie van een Shared Access Signature

Een Shared Access Signature is een set van query-parameters die zijn toegevoegd aan de URL die verwijst naar de resource

dat biedt informatie over de toegang is toegestaan en de tijdsduur waarvoor de toegang is toegestaan. Hier volgt een voorbeeld. Deze URI biedt leestoegang tot een blob gedurende vijf minuten. SAS-queryparameters moet URL-gecodeerde, zoals % 3A voor dubbele punt (:) of % 20 voor een spatie.

```
http://mystorage.dfs.core.windows.net/myfilesystem/myfile.txt (URL to the file)
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

* Een service level SAS kan worden gebruikt voor toegang tot bepaalde resources in een storage-account. Enkele voorbeelden hiervan zijn bij het ophalen van een lijst met bestanden in een bestandssysteem of een bestand wordt gedownload.
* Een SAS op accountniveau kan worden gebruikt voor toegang tot alles dat een SAS serviceniveau kan worden gebruikt voor. Bovendien geeft de opties in de bronnen die niet zijn toegestaan als een service level SAS, zoals de mogelijkheid om te maken van bestandssystemen.

#### <a name="creating-a-sas-uri"></a>Het maken van een SAS-URI

1. U kunt een URI maken op aanvraag, alle van de queryparameters telkens definiëren.

   Deze aanpak is flexibel, maar hebt u een logische set parameters die vergelijkbaar telkens zijn, met behulp van een opgeslagen toegangsbeleid is een beter idee.
2. U kunt een opgeslagen toegangsbeleid voor een volledige bestandssysteem, bestandsshare, tabel of wachtrij maken. Vervolgens kunt u dit als basis voor de SAS-URI's die u maakt. Machtigingen op basis van opgeslagen toegangsbeleid kunnen eenvoudig worden ingetrokken. U kunt maximaal vijf beleid dat is gedefinieerd op elk bestandssysteem, wachtrij, tabel of bestandsshare hebben.

   Als u van plan zijn dat veel mensen lezen van de blobs in een specifieke bestandssysteem, kan u bijvoorbeeld een opgeslagen toegangsbeleid met de tekst 'leestoegang geven' en alle andere instellingen die gelijk elke keer zijn maken. Vervolgens maakt u een SAS-URI met behulp van de instellingen van het toegangsbeleid opgeslagen en de vervaldatum van de datum en tijd op te geven. Het voordeel hiervan is dat u alle op te geven van de queryparameters telkens geen hebt.

#### <a name="revocation"></a>Intrekken

Stel dat de SAS is aangetast, of u wilt wijzigen vanwege zakelijke beveiligings- of wettelijke vereisten. Hoe u toegang tot een resource met behulp van deze SAS intrekken? Dat hangt ervan af hoe u de SAS-URI hebt gemaakt.

Als u ad-hoc URI's gebruikt, hebt u drie opties. U kunt SAS-tokens uitgeven met korte verloopbeleid en wacht tot de SAS is verlopen. U kunt wijzigen of verwijderen van de resource (ervan uitgaande dat het token is afgestemd op een enkel object). U kunt de opslagaccountsleutels wijzigen. Deze laatste optie kan een aanzienlijke impact, afhankelijk van hoeveel services met behulp van dit account, en is waarschijnlijk niet iets dat die u doen wilt zonder een planning.

Als u van een SAS die is afgeleid van een toegangsbeleid die zijn opgeslagen gebruikmaakt, kunt u toegang verwijderen door het intrekken van het toegangsbeleid die zijn opgeslagen – u kunt deze alleen wijzigen zodat deze al is verlopen, of kunt u deze helemaal verwijderen. Dit wordt direct van kracht en elke SAS gemaakt met behulp van deze opgeslagen toegangsbeleid wordt ongeldig. Het bijwerken of verwijderen van de opgeslagen toegangsbeleid kunnen invloed personen met toegang tot die specifieke bestandssysteem, bestandsshare, tabel of wachtrij via SAS, maar als de clients zijn geschreven, zodat ze een nieuwe SAS aanvragen wanneer de oude heeft ongeldige, wordt dit werkt prima.

Omdat met behulp van een SAS die is afgeleid van een opgeslagen toegangsbeleid u de mogelijkheid om in te trekken die SAS onmiddellijk biedt, is de aanbevolen toegangsbeleid opgeslagen indien mogelijk altijd wilt gebruiken.

#### <a name="resources"></a>Resources

Raadpleeg de volgende artikelen voor meer gedetailleerde informatie over het gebruik van handtekeningen voor gedeelde toegang en opgeslagen toegangsbeleid, inclusief voorbeelden:

* Dit zijn de referentie-artikelen.

  * [Service SAS](https://msdn.microsoft.com/library/dn140256.aspx)

    In dit artikel bevat voorbeelden van het gebruik van een SAS serviceniveau met blobs, berichten in de wachtrij, tabel-bereiken en bestanden.
  * [Maken van een service-SAS](https://msdn.microsoft.com/library/dn140255.aspx)
  * [Maken van een account-SAS](https://msdn.microsoft.com/library/mt584140.aspx)
* Dit zijn de zelfstudies voor het gebruik van de .NET-clientbibliotheek om handtekeningen voor gedeelde toegang en opgeslagen toegangsbeleid te maken.

  * [Shared Access Signatures (SAS) gebruiken](../storage-dotnet-shared-access-signature-part-1.md)
  * [Shared Access Signatures, deel 2: Maken en gebruiken van een SAS met de Blob-Service](../blobs/storage-dotnet-shared-access-signature-part-2.md)

    Dit artikel bevat een uitleg van de SAS-model, voorbeelden van handtekeningen voor gedeelde toegang en aanbevelingen voor het beste gebruik van SAS. Ook wordt besproken, is het intrekken van de machtiging is verleend.

* Authentication

  * [Verificatie voor de Azure Storage-Services](https://msdn.microsoft.com/library/azure/dd179428.aspx)
* Shared Access Signatures aan de slag zelfstudie

  * [Aan de slag zelfstudie SAS](https://github.com/Azure-Samples/storage-dotnet-sas-getting-started)

## <a name="encryption-in-transit"></a>Versleuteling tijdens overdracht

### <a name="transport-level-encryption--using-https"></a>Versleuteling op transportniveau – via HTTPS

Er is een andere stap die u nemen moet om te controleren of de beveiliging van uw Azure Storage-gegevens voor het versleutelen van gegevens tussen de client en de Azure Storage. De eerste aanbeveling is altijd gebruik van de [HTTPS](https://en.wikipedia.org/wiki/HTTPS) -protocol, dat zorgt ervoor veilige communicatie via het openbare Internet dat.

Als u een beveiligd communicatiekanaal wilt weergeven, moet u altijd HTTPS gebruiken tijdens de REST API's aanroepen of toegang tot in de opslag objecten. Bovendien **Shared Access Signatures**, die kan worden gebruikt voor toegang tot Azure Storage-objecten overdragen, een optie om op te geven dat alleen het HTTPS-protocol kan worden gebruikt bij het gebruik van handtekeningen voor gedeelde toegang, ervoor zorgen dat iedereen verzenden koppelingen met SAS tokens het juiste protocol gebruikt.

U kunt het gebruik van HTTPS afdwingen bij het aanroepen van de REST-API's voor toegang tot objecten in de storage-accounts door in te schakelen [veilige overdracht vereist](../storage-require-secure-transfer.md) voor het opslagaccount. Verbindingen met behulp van HTTP zal worden geweigerd als deze optie is ingeschakeld.

## <a name="encryption-at-rest"></a>Versleuteling-at-Rest

### <a name="storage-service-encryption-sse"></a>Storage Service Encryption (SSE)

SSE is ingeschakeld voor alle opslagaccounts en kan niet worden uitgeschakeld. SSE versleutelt automatisch uw gegevens bij het schrijven van deze naar Azure Storage. Wanneer u gegevens uit Azure Storage leest, wordt er door Azure Storage ontsleuteld voordat het wordt geretourneerd. Met SSE kunt u uw gegevens te beveiligen zonder code te wijzigen of voeg code toe aan alle toepassingen.

U kunt Microsoft beheerde sleutels of uw eigen aangepaste sleutels gebruiken. Microsoft beheerde sleutels genereert en hun beveiligde opslag, evenals hun reguliere roteren, zoals gedefinieerd door het interne Microsoft-beleid worden verwerkt. Zie voor meer informatie over het gebruik van aangepaste sleutels [Storage Service Encryption door de klant beheerde sleutels in Azure Key Vault](storage-service-encryption-customer-managed-keys.md).
