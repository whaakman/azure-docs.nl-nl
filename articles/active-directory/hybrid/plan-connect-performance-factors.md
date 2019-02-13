---
title: Factoren die invloed hebben op de prestaties van Azure AD Connect
description: Dit document wordt uitgelegd hoe verschillende factoren van invloed zijn op de Azure AD Connect engine wordt ingericht. Deze factoren kunnen organisaties hun Azure AD Connect-implementatie om ervoor te zorgen dat deze voldoet aan de vereisten van de synchronisatie plannen.
services: active-directory
author: billmath
manager: daveba
tags: azuread
ms.service: active-directory
ms.topic: conceptual
ms.workload: identity
ms.date: 10/06/2018
ms.reviewer: martincoetzer
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 3a3a57fbe5df690e4dbdba8cbab85e62648bb298
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/13/2019
ms.locfileid: "56192571"
---
# <a name="factors-influencing-the-performance-of-azure-ad-connect"></a>Factoren die invloed hebben op de prestaties van Azure AD Connect

Azure AD Connect synchroniseert uw Active Directory naar Azure AD. Deze server is een essentieel onderdeel van het verplaatsen van uw gebruikers-id's naar de cloud. De primaire factoren die invloed hebben op de prestaties van een Azure AD Connect zijn:

| **Ontwerpfactor**| **Definitie** |
|:-|-|
| Topologie| De distributie van de eindpunten en de onderdelen van Azure AD Connect moet op het netwerk beheren. |
| Schalen| Het aantal objecten, zoals de gebruikers, groepen en organisatie-eenheden, worden beheerd door Azure AD Connect. |
| Hardware| De hardware (fysiek of virtueel) voor de Azure AD Connect en de prestaties van afhankelijke capaciteit van elke hardware-onderdeel zoals CPU, geheugen, netwerk en de configuratie van de harde schijf. |
| Configuratie| Hoe Azure AD Connect-processen de mappen en gegevens. |
| Belasting| De frequentie van object wordt gewijzigd. De belasting kunnen variëren tijdens een uur, dag of week. Mogelijk moet u afhankelijk van het onderdeel ontwerp voor piekbelasting of gemiddelde belasting. |

Het doel van dit document is om te beschrijven van de factoren die invloed op de prestaties van de Azure AD Connect engine wordt ingericht. Groot of complex organisaties (organisaties meer dan 100.000 objecten inrichting) kunnen de aanbevelingen gebruiken voor het optimaliseren van hun Azure AD Connect-implementatie, als ze een prestatieproblemen die hier wordt beschreven. De andere onderdelen van Azure AD Connect, zoals [Azure AD Connect health](how-to-connect-health-agent-install.md) en agents worden niet op de hier beschreven.

> [!IMPORTANT]
> Microsoft biedt geen ondersteuning voor wijzigen of uitvoeren van Azure AD Connect buiten de acties die officieel is gedocumenteerd. Alle andere acties kunnen resulteren in een inconsistente of niet-ondersteunde status van de Azure AD Connect-synchronisatie. Daarom biedt Microsoft geen technische ondersteuning voor dergelijke implementaties.

## <a name="azure-ad-connect-component-factors"></a>Factoren van Azure AD Connect-onderdeel

Het volgende diagram toont een architectuur op hoog niveau van het inrichten van engine verbinding te maken met één forest, hoewel meerdere forests worden ondersteund. Deze architectuur laat zien hoe de verschillende onderdelen met elkaar communiceren.

![AzureADConnentInternal](media/plan-connect-performance-factors/AzureADConnentInternal.png)

De engine voor het inrichtingsproces verbindt voor elk Active Directory-forest en naar Azure AD. Het proces voor het lezen van gegevens van elke map heet importeren. Export verwijst naar het bijwerken van de mappen van de inrichting-engine. Synchronisatie evalueert de regels van hoe de objecten in de inrichting-engine worden overgebracht. Voor meer informatie over kunt u verwijzen naar [Azure AD Connect-synchronisatie: Inzicht in de architectuur](https://docs.microsoft.com/azure/active-directory/hybrid/concept-azure-ad-connect-sync-architecture).

Azure AD Connect maakt gebruik van de volgende ruimten met tijdelijke bestanden, regels en processen om de synchronisatie van Active Directory naar Azure AD:

* **Connector-ruimte (CS)** -objecten uit elke gekoppelde adreslijst (CD), de werkelijke mappen worden tijdelijk opgeslagen hier eerst voordat ze kunnen worden verwerkt door de engine voor het inrichten. Azure AD heeft een eigen CS en elk forest dat u verbinding met maakt een eigen CS.
* **Metaverse (MV)** -objecten die moeten worden gesynchroniseerd zijn hier maken op basis van de synchronisatieregels. Objecten moeten zich in de MV voordat ze de objecten en kenmerken aan de andere verbonden mappen kunnen vullen. Er is slechts één MV.
* **Synchroniseren van regels** -ze bepalen welke objecten worden gemaakt (verwachte) of die zijn verbonden (gekoppelde) op in de MV-objecten. De synchronisatieregels kunt u ook bepalen welke kenmerkwaarden worden gekopieerd of getransformeerd naar en van de mappen.
* **Uitvoeringsprofielen** -worden de stappen van het kopiëren van objecten en hun kenmerkwaarden volgens de synchronisatieregels tussen de ruimten met tijdelijke bestanden en mappen verbonden.

Verschillende uitvoeringsprofielen bestaan voor het optimaliseren van de prestaties van de inrichting-engine. De meeste organisaties wordt gebruikt van de standaardschema's en -profielen voor normale bewerkingen uitvoeren, maar sommige organisaties hebben mogelijk naar [het schema wijzigen](https://docs.microsoft.com/azure/active-directory/hybrid/how-to-connect-sync-feature-scheduler) of andere uitvoeringsprofielen die geschikt zijn voor de zeldzame gevallen activeren. De volgende uitvoerprofielen zijn beschikbaar:

### <a name="initial-sync-profile"></a>Profiel van de initiële synchronisatie

Het profiel voor initiële synchronisatie is het proces voor het lezen van de gekoppelde mappen, zoals een Active Directory-forest voor de eerste keer. Vervolgens wordt een analyse op alle vermeldingen in de database van de synchronisatie-engine. De eerste cyclus is wordt nieuwe objecten maken in Azure AD en extra tijd in beslag als uw Active Directory-forests groot zijn. De eerste synchronisatie bevat de volgende stappen uit:

1. Volledige import op alle connectors
2. Volledige synchronisatie van alle connectors
3. Op alle connectors exporteren

### <a name="delta-sync-profile"></a>Delta-sync-profiel

Het optimaliseren van het synchronisatieproces dit profiel uitvoert alleen verwerken de wijzigingen (maakt, worden verwijderd en updates) van objecten in uw verbonden mappen, sinds de laatste synchronisatieproces. Het profiel van delta-synchronisatie wordt standaard elke 30 minuten uitgevoerd. Organisaties moeten streven naar het behouden van de tijd die nodig op onder de 30 minuten om te controleren of dat de Azure AD is bijgewerkt. U kunt de status van Azure AD Connect controleren met de [voor health monitoring agent](how-to-connect-health-sync.md) om te zien of er problemen met het proces. De delta-sync-profiel bevat de volgende stappen uit:

1. Delta-import op alle connectors
2. Deltasynchronisatie op alle connectors
3. Op alle connectors exporteren

Er is een typische enterprise organisatie delta-sync-scenario:

- ~ 1% van de objecten zijn verwijderd
- ~ 1% van de objecten worden gemaakt
- ongeveer 5% van de objecten worden gewijzigd

De wijzigingssnelheid kan variëren, afhankelijk van hoe vaak de gebruikers in uw Active Directory voor het bijwerken van uw organisatie. Hogere snelheid van de wijziging kunnen bijvoorbeeld optreden met de periodieke variatie van verhuur en medewerkers minder.

### <a name="full-sync-profile"></a>Volledige synchronisatie-profiel

De cyclus van een volledige synchronisatie is vereist als u een van de volgende configuratiewijzigingen hebt aangebracht:



- Vergroot het bereik van de objecten of de kenmerken van de gekoppelde mappen worden geïmporteerd. Bijvoorbeeld, als u toevoegt een domein of organisatie-eenheid aan het bereik van uw invoer.
- Wijzigingen aangebracht in de synchronisatieregels. Bijvoorbeeld, wanneer u een nieuwe regel voor het vullen van de functie van een gebruiker in Azure AD van extension_attribute3 in Active Directory maakt. Deze update is vereist dat de provisioning-engine opnieuw alle bestaande gebruikers bijwerken hun titels om toe te passen van de wijziging voortaan onderzoeken.

De volgende bewerkingen zijn opgenomen in een cyclus van een volledige synchronisatie:

1. Volledige import op alle connectors
2. Volledige/Deltasynchronisatie op alle connectors
3. Op alle connectors exporteren

> [!NOTE]
> Zorgvuldige planning is vereist bij het uitvoeren van bulksgewijze updates te veel objecten in uw Active Directory of Azure AD. Bulksgewijze updates zorgt ervoor dat het synchronisatieproces delta langer duren bij het importeren, aangezien veel objecten zijn gewijzigd. Lange invoer kunnen gebeuren, zelfs als de bulk-update niet van invloed zijn op het synchronisatieproces. Bijvoorbeeld, licenties toewijzen aan een groot aantal gebruikers in Azure AD zorgt ervoor dat een cyclus lang importeren uit Azure AD, maar resulteert niet in de kenmerkwijzigingen in Active Directory.

### <a name="synchronization"></a>Synchronisatie

Het proces synchroniseren runtime heeft de volgende prestatiekenmerken:

* Synchronisatie is één thread, wat betekent dat de provisioning-engine geen parallelle verwerking van de uitvoeringsprofielen van verbonden mappen, objecten of kenmerken.
* Importeren neemt lineair toe met het aantal objecten wordt gesynchroniseerd. Bijvoorbeeld, als 10.000 objecten 10 minuten om te importeren, duurt klikt u vervolgens 20.000 objecten ongeveer 20 minuten op dezelfde server.
* Exporteren is ook lineaire.
* De synchronisatie, zullen groeien exponentieel op basis van het aantal objecten met verwijzingen naar andere objecten. Lidmaatschappen van groepen en geneste groepen hebt de belangrijkste prestatie-impact omdat hun leden naar objecten of andere groepen verwijzen. Deze verwijzingen moeten worden gevonden en waarnaar wordt verwezen naar werkelijke objecten in de MV om uit te voeren van de synchronisatiecyclus.

### <a name="filtering"></a>Filteren

De grootte van de Active Directory-topologie die u wilt importeren, is de nummer één factor invloed op de prestaties en de totale tijd die de interne onderdelen van de engine voor het inrichten duurt.

[Filteren](https://docs.microsoft.com/azure/active-directory/hybrid/how-to-connect-sync-configure-filtering) moet worden gebruikt om de objecten op de gesynchroniseerde verminderen. Er wordt voorkomen dat onnodige objecten worden verwerkt en geëxporteerd naar Azure AD. In de volgorde van voorkeur, zijn de volgende methoden voor het filteren zijn beschikbaar:



- **Filteren op basis van een domein** – Gebruik deze optie om specifieke domeinen om te synchroniseren naar Azure AD selecteren. U moet toevoegen en domeinen verwijderen uit de synchronisatie-engine-configuratie wanneer u wijzigingen in uw on-premises infrastructuur aanbrengen nadat u Azure AD Connect-synchronisatie installeren.
- **Filteren van de organisatie-eenheid (OE)** -OE's gebruikt om u te richten op specifieke objecten in Active Directory-domeinen voor levering aan Azure AD. OE filteren is het tweede aanbevolen filteren mechanisme, omdat deze eenvoudige LDAP bereik-query's gebruikt voor een kleinere subset van objecten importeren uit Active Directory.
- **Kenmerk filteren per object** -maakt gebruik van de kenmerkwaarden voor objecten om te bepalen of bepaald object in Active Directory is ingericht in Azure AD. Kenmerkfilters is erg handig voor het optimaliseren van de filters bij het domein en OE-filters niet voldoet aan de specifieke vereisten voor het filteren. Kenmerkfilters niet importeren op hoelang maar vermindert de synchronisatie en tijden exporteren.
- **Filteren op basis van een groep** -groepslidmaatschap van gebruikt om te beslissen of objecten moeten worden ingericht in Azure AD. Filteren op basis van een groep is alleen geschikt zijn voor het testen van situaties en niet aanbevolen voor productie, vanwege de extra overhead vereist om te controleren van groepslidmaatschap tijdens de synchronisatiecyclus.

Veel permanente [disconnectorobjecten](concept-azure-ad-connect-sync-architecture.md#relationships-between-staging-objects-and-metaverse-objects) in uw Active Directory-CS kan leiden tot langere synchronisatietijden, omdat de engine voor het leveren van gegevens moet elk object disconnector voor mogelijke verbinding in de synchronisatiecyclus Evalueer. Overweeg om dit probleem verhelpen door een van de volgende aanbevelingen:



- Plaats de disconnectorobjecten buiten het bereik van importeren met behulp van domein of OE filteren.
- Project/join de objecten naar het MV en stel de [cloudFiltered](how-to-connect-sync-configure-filtering.md#negative-filtering-do-not-sync-these) kenmerk gelijk is aan True, om te voorkomen dat de inrichting van deze objecten in de Azure AD CS.

> [!NOTE]
> Gebruikers kunnen verwarring of toepassing machtigingen kunnen er problemen optreden, bij te veel objecten worden gefilterd. Bijvoorbeeld in een hybride Exchange online-implementatie ziet gebruikers met de postvakken van de on-premises meer gebruikers in de globale adreslijst dan gebruikers met de postvakken in Exchange online. In andere gevallen wordt kan een gebruiker toegang wilt verlenen in een cloud-app naar een andere gebruiker die geen deel uitmaakt van het bereik van de gefilterde set van objecten.

### <a name="attribute-flows"></a>Kenmerkstromen

Kenmerkstromen is het proces voor het kopiëren of transformeren de kenmerkwaarden van objecten van een verbonden adreslijst naar een andere verbonden adreslijst. Ze zijn gedefinieerd als onderdeel van de synchronisatieregels. Bijvoorbeeld, als het telefoonnummer van een gebruiker wordt gewijzigd in uw Active Directory, wordt het telefoonnummer in Azure AD bijgewerkt. Organisaties kunnen [wijzigen van de kenmerkstromen](https://docs.microsoft.com/azure/active-directory/hybrid/how-to-connect-sync-change-the-configuration) met suite verschillende vereisten. Het verdient aanbeveling kopieert u de bestaande kenmerkstromen voordat u deze wijzigt.

Eenvoudige omleidingen, zoals stromen een kenmerkwaarde kopiëren naar een ander kenmerk heeft geen invloed op de prestaties van materiaal. Een voorbeeld van een omleiding is een mobiele nummer in Active Directory het zakelijke telefoonnummer doorgestuurd in Azure AD.

Kenmerkwaarden transformeren, kan een prestatie-impact hebben op het synchronisatieproces. Kenmerkwaarden transformeren bevat wijzigen, formatteren, samenvoegen of af te trekken van waarden van kenmerken.

Organisaties kunnen voorkomen dat bepaalde kenmerken overgebracht naar Azure AD, maar deze wordt niet van invloed zijn op de prestaties van de inrichting-engine.

> [!NOTE]
> Geen ongewenste kenmerkstromen in uw synchronisatieregels verwijderen. Het wordt aanbevolen, in plaats daarvan uitschakelen omdat verwijderde regels opnieuw tijdens upgrades van de Azure AD Connect gemaakt worden.

## <a name="azure-ad-connect-dependency-factors"></a>Factoren van Azure AD Connect-afhankelijkheid

De prestaties van Azure AD Connect is afhankelijk van de prestaties van de gekoppelde mappen deze invoer en uitvoer naar. Bijvoorbeeld, de grootte van de Active Directory nodig is om te importeren of de netwerklatentie tot de Azure AD-service. De SQL-database die maakt gebruik van de engine voor het leveren van gegevens zijn ook van invloed op de algehele prestaties van de synchronisatiecyclus.

### <a name="active-directory-factors"></a>Active Directory-factoren

Zoals eerder vermeld, het aantal objecten moeten worden geïmporteerd van invloed op de prestaties aanzienlijk. De [hardware- en vereisten voor Azure AD Connect](how-to-connect-install-prerequisites.md) overzicht van specifieke hardware-lagen op basis van de grootte van uw implementatie. Azure AD Connect ondersteunen alleen specifieke topologieën zoals wordt beschreven in [topologieën voor Azure AD Connect](plan-connect-topologies.md). Er zijn geen prestatieoptimalisatie en aanbevelingen voor niet-ondersteunde topologieën.

Zorg ervoor dat uw Azure AD Connect-server voldoet aan de hardwarevereisten voldoet op basis van de grootte van uw Active Directory te importeren. Ongeldige of trage netwerkverbinding tussen de Azure AD Connect-server en uw Active Directory-domeincontrollers vertragen de importeren.

### <a name="azure-ad-factors"></a>Azure AD-factoren

Azure AD maakt gebruik van beperking van de service in de cloud beschermen tegen denial-of-service (DoS) aanvallen. Azure AD heeft momenteel een beperkingslimiet van 7000 schrijfbewerkingen per vijf minuten (84,000 per uur). De volgende bewerkingen kunnen bijvoorbeeld worden beperkt:



- Azure AD Connect-export naar Azure AD.
- PowerShell-scripts of toepassingen bijwerken van de Azure AD rechtstreeks zelfs op de achtergrond, zoals lidmaatschap van dynamische groepen.
- Gebruikers hun eigen identiteit records, zoals registreren voor MFA of SSPR (Self-service voor wachtwoord opnieuw instellen) bijwerken.
- Bewerkingen binnen de grafische gebruikersinterface.

Plan voor implementatie en het onderhoud taken, om ervoor te zorgen dat een upgradecyclus voor uw Azure AD Connect-synchronisatie wordt niet beïnvloed door het beperken van limieten. Bijvoorbeeld, hebt u een grote aannemen van personeel golf waar u duizenden gebruikers-id's maakt, kan dit leiden tot updates dynamische groepslidmaatschappen, toewijzingen, licentieverlening en Self-service voor wachtwoord opnieuw instellen van registraties. Is het beter deze schrijfbewerkingen gedurende enkele uren of dagen kwijt bent een paar verspreiden.

### <a name="sql-database-factors"></a>SQL database factoren

De grootte van uw Active Directory-topologie van bron van invloed zijn op de prestaties van uw SQL-database. Ga als volgt de [hardwarevereisten](how-to-connect-install-prerequisites.md) voor de SQL server-database en de onderstaande aanbevelingen:



- Organisaties met meer dan 100.000 gebruikers kunnen netwerkvertragingen door het plaatsen van de SQL-database en de inrichting-engine op dezelfde server verminderen.
- Vanwege de hoge schijf invoer en uitvoer gebruiken (i/o)-vereisten van het synchronisatieproces Solid State Drives (SSD) voor de SQL-database van de inrichting-engine voor optimale resultaten als niet mogelijk is, kunt u overwegen RAID 0 of RAID 1-configuraties.
- Een volledige synchronisatie niet preventief; doet Dit zorgt ervoor dat onnodige verloop en langzamer reactietijden.

## <a name="conclusion"></a>Conclusie

Houd rekening met de volgende aanbevelingen voor het optimaliseren van de prestaties van uw Azure AD Connect-implementatie:



- Gebruik de [aanbevolen hardwareconfiguratie](how-to-connect-install-prerequisites.md) op basis van de grootte van uw implementatie voor de Azure AD Connect-server.
- Bij het upgraden van Azure AD Connect in grootschalige implementaties, kunt u overwegen [swing migratiemethode](https://docs.microsoft.com/azure/active-directory/hybrid/how-to-upgrade-previous-version#swing-migration), om ervoor te zorgen dat u hebt de minimale downtime en de beste betrouwbaarheid. 
- Gebruik SSD voor de SQL-database voor de beste prestaties te schrijven.
- Filteren van het Active Directory-bereik zodanig dat alleen objecten die worden ingericht in Azure AD moeten, met behulp van het domein, organisatie-eenheid of kenmerk filteren.
- Als u nodig hebt om te wijzigen van de standaardregels voor de stroom van kenmerk, eerst kopieert u de regel, wijzig vervolgens het exemplaar en de oorspronkelijke regel uitschakelen. Vergeet niet een volledige synchronisatie opnieuw uit te voeren.
- Plan voldoende tijd voor de eerste volledige synchronisatie uitvoeren profiel.
- Streven ernaar om uit te voeren van de delta-synchronisatiecyclus in 30 minuten. Als de delta-sync-profiel niet in 30 minuten voltooid, wijzigt u de standaardfrequentie synchroniseren om op te nemen van een delta-volledige synchronisatiecyclus.
- Monitor uw [Azure AD Connect sync-status](how-to-connect-health-agent-install.md) in Azure AD.

## <a name="next-steps"></a>Volgende stappen
Lees meer over het [integreren van uw on-premises identiteiten met Azure Active Directory](whatis-hybrid-identity.md).
