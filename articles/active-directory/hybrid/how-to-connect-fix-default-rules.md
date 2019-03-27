---
title: Voor het oplossen van gewijzigde standaardregels - Azure AD Connect | Microsoft Docs
description: Informatie over het oplossen van gewijzigde standaardregels die worden geleverd met Azure AD Connect.
services: active-directory
author: billmath
manager: daveba
editor: curtand
ms.reviewer: darora10
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 03/21/2019
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: f524e8cef3878816cec53575217bdb6d0fd9be7b
ms.sourcegitcommit: f24fdd1ab23927c73595c960d8a26a74e1d12f5d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/27/2019
ms.locfileid: "58501254"
---
# <a name="fix-modified-default-rules-in-azure-ad-connect"></a>Gewijzigde standaardregels in Azure AD Connect oplossen

Azure AD Connect wordt geleverd met de standaardregels voor synchronisatie.  Helaas deze regels niet algemeen van toepassing op alle organisaties en is er mogelijk tijdstippen, op basis van uw vereisten, als u wilt wijzigen.

 Als u de standaardregels hebt gewijzigd of van plan bent te wijzigen, klikt u vervolgens Neem even de tijd om dit document te lezen.

Dit document bevat weliswaar 2 voorbeelden van de meest voorkomende aanpassingen die zijn gemaakt door gebruikers en de juiste manier om deze aanpassingen uitgelegd.

>[!NOTE] 
> Wijzigen van bestaande standaard (s) voor het bereiken van een benodigde aanpassingen wordt niet ondersteund - zodoende wordt voorkomen dat deze regels bijwerken naar de nieuwste versie in toekomstige releases. Hiermee wordt voorkomen dat u aan de benodigde oplossingen voor problemen en nieuwe functies.  Dit document wordt uitgelegd hoe u hetzelfde resultaat bereiken zonder de bestaande standaardregels wijzigen. 

## <a name="how-to-identify-modified-default-rules"></a>Het identificeren van gewijzigde standaardregels?
Vanaf versie 1.3.7.0 van **Azure AD Connect**, het is nu eenvoudig om te identificeren van de standaardregel voor de gewijzigde. U kunt gaat u naar Apps op het bureaublad en klik op **Synchronization Rules Editor**.

![editor](media/how-to-connect-fix-default-rules/default1.png)

In de Editor wordt een gewijzigde standaardregels weergegeven met een pictogram voor de naam, zoals hieronder weergegeven:

![Pictogram](media/how-to-connect-fix-default-rules/default2.png)

 Ook ziet u een uitgeschakelde regel met dezelfde naam naast het dit de standaard-regel is:

![Standaardregels](media/how-to-connect-fix-default-rules/default2a.png)

## <a name="common-customizations"></a>Algemene aanpassingen
Hier volgen algemene aanpassingen aan de standaardregels:

- [Wijzigen van de kenmerkstroom](#changing-attribute-flow)
- [Bereikfilter wijzigen](#changing-scoping-filter)
- [Join-voorwaarde wijzigen](#changing-join-condition)

## <a name="before-changing-any-rules"></a>Voordat u een van de regels wijzigen
- De synchronisatieplanning uitschakelen.  De scheduler wordt standaard elke 30 minuten uitgevoerd. Zorg ervoor dat deze wordt niet gestart tijdens het aanbrengen van wijzigingen en het oplossen van uw nieuwe regels. Als u wilt de scheduler tijdelijk uitschakelen, start PowerShell en voer `Set-ADSyncScheduler -SyncCycleEnabled $false`.
 ![Standaardregels](media/how-to-connect-fix-default-rules/default3.png)

- De wijziging in bereikfilter kan leiden tot het verwijderen van objecten in de doelmap. Houd er rekening zorgvuldig voordat u wijzigingen aanbrengt in het bereik van objecten. Het is raadzaam dat u wijzigingen aanbrengen in een staging-server voordat u wijzigingen op de actieve server.
- Voer een Preview-versie op een enkel object zoals vermeld in [Synchronisatieregel valideren](#validate-sync-rule) sectie na het toevoegen van een nieuwe regel.
- Voer een volledige synchronisatie na een nieuwe regel toevoegen of wijzigen van een aangepaste synchronisatieregel. Deze synchronisatie wordt nieuwe regels toepassen op alle objecten.

## <a name="changing-attribute-flow"></a>Wijzigen van de kenmerkstroom
Er zijn 3 verschillende scenario's voor de kenmerkstroom, laten we kijken hoe u om dit te bereiken zonder te wijzigen van standaard-regels.
- Nieuw kenmerk toe te voegen
- Waarde van bestaande kenmerk overschrijven
- Bestaand kenmerk worden niet gesynchroniseerd

### <a name="adding-new-attribute"></a>Nieuw kenmerk toe te voegen:
Als u vindt dat een kenmerk niet vanaf de bron-directory worden doorgestuurd naar de doelmap en kunt u de [Azure AD Connect-synchronisatie: Mapextensies](how-to-connect-sync-feature-directory-extensions.md) stromen van de nieuwe kenmerken.

Houd er rekening mee dat de eerste keuze te gebruiken moet [Azure AD Connect-synchronisatie: Mapextensies](how-to-connect-sync-feature-directory-extensions.md), een buiten-vak-functie geleverd door Azure AD Connect. Echter, als dit niet voor u werkt vervolgens via de volgende stappen voor het doorlopen van een kenmerk zonder te wijzigen van bestaande standaard-synchronisatieregel, u kunt dit doen door twee nieuwe synchronisatieregels toe te voegen.


#### <a name="add-an-inbound-sync-rule"></a>Voeg een regel voor inkomende synchronisatie:
Regel voor inkomende synchronisatie betekent dat de bron voor het kenmerk is een connectorgebied en het doel is metaverse. Bijvoorbeeld als u wilt laten doorlopen een nieuw kenmerk uit on-premises Active Directory naar Azure Active Directory, een nieuwe regel voor inkomende synchronisatie maken door het starten van de **Synchronization Rule Editor**, selecteer vervolgens richting als **voorbinnenkomendverkeer** en klikt u op **nieuwe regel toevoegen**. 

 ![Standaardregels](media/how-to-connect-fix-default-rules/default3a.png)

Uw eigen naamconventie om een naam van de regel te volgen, wordt hier gebruikt we **aangepaste In uit Active Directory - gebruiker**, dit betekent dat de regel een aangepaste regel is en een inkomende regel van de AD-connectorgebied naar de Metaverse is. 

 ![Standaardregels](media/how-to-connect-fix-default-rules/default3b.png)

Geef uw eigen beschrijving van de regel zodat het toekomstig onderhoud van de regel is eenvoudig, zoals wat het doel van deze regel is en waarom het nodig is.
Verbonden systeem (het forest) hebt geselecteerd de bron van het kenmerk. Vervolgens objecttype verbonden systeem en het Type Metaverse-Object.

Geef de prioriteitswaarde tussen 0 en 99 (lagere waarde, hoger de prioriteit). Houd andere velden, zoals 'Tag', 'Wachtwoordsynchronisatie inschakelen' en 'Uitgeschakeld' als standaard.

Houd Scoping 'filter' leeg, dit betekent dat de regel van toepassing op alle objecten gekoppeld tussen AD verbonden systeem- en Metaverse.

Houd de lege 'Regels Join', wat betekent deze regel dat wordt gemaakt op de join-voorwaarde die is gedefinieerd in de regel standaard. Dit is een andere reden niet aan de uitschakelen/verwijderen. de regel standaard omdat als er geen join-voorwaarde ontvangstbevestigingen vervolgens het kenmerk niet overgebracht. 

Juiste transformatie toevoegen voor het kenmerk, kunt u constante stromen een constante waarde naar het doelkenmerk toewijzen of directe toewijzing tussen de bron- of doelentiteit kenmerk of een expressie voor het kenmerk. Hier volgen verschillende [functies](https://docs.microsoft.com/azure/active-directory/hybrid/reference-connect-sync-functions-reference) u kunt gebruiken.

#### <a name="add-an-outbound-sync-rule"></a>Voeg een synchronisatieregel voor uitgaande:
Tot nu toe door toe te voegen alleen een regel voor inkomende synchronisatie hebben we de helft het werk, omdat het kenmerk is nog niet gekoppeld aan de doelmap. Om te koppelen van het kenmerk aan de doel-directeur die u wilt maken van een uitgaande regel, wat betekent dat de bron is metaverse en het doel is verbonden systeem. Een uitgaande regel maken, starten **Synchronization Rule Editor**, wijzigen de **richting** naar **uitgaand** en klikt u op **nieuwe regel toevoegen**. 

![Standaardregels](media/how-to-connect-fix-default-rules/default3c.png)

Zoals in de binnenkomende regel, kunt u uw eigen naamgevingsconventie op toe **naam** de regel. Selecteer de **verbonden systeem** als Azure AD-tenant, selecteert u het verbonden systeem-object waarnaar u wilt ingesteld de kenmerkwaarde. Stel de prioriteit in tussen 0 - 99. 

![Standaardregels](media/how-to-connect-fix-default-rules/default3d.png)

Houd **Scoping filter** leeg is, behouden **deelnemen aan regels** leeg, vult u de transformatie als constante, Direct of expressie. 

In dit voorbeeld is het nieuwe kenmerk voor een gebruikersobject in Active Directory naar Azure Active Directory flow gedemonstreerd. U kunt deze stappen gebruiken om toe te wijzen een kenmerk van een object op bron en doel.  Zie voor meer informatie [het maken van aangepaste synchronisatieregels](how-to-connect-create-custom-sync-rule.md) en [voorbereiding voor gebruikers inrichten](https://docs.microsoft.com/office365/enterprise/prepare-for-directory-synchronization).

### <a name="overriding-value-of-existing-attribute"></a>Waarde van bestaande kenmerk overschrijven
Het is mogelijk dat u wilt overschrijven van de waarde van kenmerk al zijn toegewezen, zoals u wilt altijd Null-waarde met een kenmerk is ingesteld in Azure AD, kunt u dit doen door alleen een inkomende regel te maken zoals vermeld in de vorige stap en flow  **AuthoritativeNull** constante waarde voor het doelkenmerk. Houd er rekening mee dat we AuthoritativeNulll in dit geval in plaats van Null gebruikt hebben. Dit is omdat het niet-null-waarde wordt vervangen door een Null-waarde, zelfs als er een lagere prioriteit (hogere numerieke waarde in de regel). Echter, de AuthoritativeNull zullen worden behandeld als Null en worden niet vervangen met niet-null-waarde door andere regels. 

### <a name="dont-sync-existing-attribute"></a>Bestaand kenmerk worden niet gesynchroniseerd
Als u uitsluiten van een kenmerk wilt kunnen worden gesynchroniseerd, kunt u het kenmerk filteren in Azure AD Connect-functie gebruiken. Start **Azure AD Connect** van pictogram op het bureaublad en selecteer vervolgens **aanpassen Synchronisatieopties**.

![Standaardregels](media/how-to-connect-fix-default-rules/default4.png)

 Zorg ervoor dat **Azure AD-app en kenmerkfilters** is geselecteerd en klik op **volgende**.

![Standaardregels](media/how-to-connect-fix-default-rules/default5.png)

Schakel de kenmerken die u uitsluiten wilt van synchronisatie.

![Standaardregels](media/how-to-connect-fix-default-rules/default6a.png)

## <a name="changing-scoping-filter"></a>Bereikfilter wijzigen
Azure AD Sync zorgt voor de meeste objecten, kunt u Verklein het bereik van objecten en minder objecten worden uitgevoerd in een ondersteunde manier zonder dat u wijzigt de standaard-synchronisatieregels verkleinen. Als u verhogen van het bereik van objecten wilt, kunt u **bewerken** de bestaande regel het klonen en de oorspronkelijke regel uitschakelen. Microsoft raadt u niet te verhogen van de scope is geconfigureerd door Azure AD Connect. Toename in het bereik van objecten maakt het moeilijk ondersteuningsteam om te begrijpen van de aanpassingen en ondersteuning voor het product.

Hier ziet u hoe u het bereik van objecten die zijn gesynchroniseerd met Azure AD kunt verminderen. Houd er rekening mee dat als u Verklein het bereik van de **gebruikers** wordt gesynchroniseerd en vervolgens de wachtwoord-hash-synchronisatie ook voor de gebruikers gefilterd-out stoppen wordt. Als de objecten zijn al synchroniseert, klik na het verminderen van bereik, worden de objecten gefilterd-out wordt verwijderd uit de doelmap, werkt u samen op scoping zorgvuldig.
Hier volgen de ondersteunde manieren om Verklein het bereik van de objecten die u synchroniseert.

- [cloudFiltered kenmerk](#cloudfiltered-attribute)
- [OE filteren](#ou-filtering)

### <a name="cloudfiltered-attribute"></a>cloudFiltered kenmerk
Houd er rekening mee dat dit is niet een kenmerk die kan worden ingesteld in Active Directory. U moet de waarde van dit kenmerk ingesteld door het toevoegen van een nieuwe regel voor binnenkomende verbindingen, zoals vermeld in **waarde van bestaande kenmerk overschrijven** sectie. Vervolgens kunt u de **transformatie** en gebruik **expressie** om in te stellen van dit kenmerk in de Metaverse. Hier volgt een voorbeeld dat u niet wilt synchroniseren alle van de gebruiker waarvan de naam van afdeling begint met hoofdlettergevoelig **HRD**:

`cloudFiltered <= IIF(Left(LCase([department]), 3) = "hrd", True, NULL)`

We hebben eerst de afdeling van bron (Active Directory) geconverteerd naar kleine letters. Maakt gebruik van de functie Left, we hebben alleen de eerste 3 tekens en tegenover hrd. Als het vergeleken, en vervolgens de waarde ingesteld op True null zijn. Houd er rekening mee dat we de waarde NULL, instelt zodat een andere regel met een lagere prioriteit (hoger getal) naar het met andere voorwaarde schrijven kunt. Voer de Preview-versie op een object voor het valideren van synchronisatieregel, zoals vermeld in [Synchronisatieregel valideren](#validate-sync-rule) sectie.

![Standaardregels](media/how-to-connect-fix-default-rules/default7a.png)



### <a name="ou-filtering"></a>OE filteren
U kunt een of meer organisatie-eenheden maken en verplaatsen van de objecten die u niet wilt synchroniseren met deze organisatie-eenheden. Configureer vervolgens de organisatie-eenheid in Azure AD Connect te filteren op starten **Azure AD Connect** uit het pictogram op het bureaublad en selecteer de opties zoals hieronder wordt weergegeven. U kunt ook de organisatie-eenheid op het moment van de installatie van Azure AD Connect-filtering configureren. 

![Standaardregels](media/how-to-connect-fix-default-rules/default8.png)

Volg de wizard en klikt u vervolgens Hef de selectie van de organisatie-eenheden u niet wilt synchroniseren.

![Standaardregels](media/how-to-connect-fix-default-rules/default9.png)

## <a name="changing-join-condition"></a>Join-voorwaarde wijzigen
Microsoft raadt u dat u de standaard deelnemen aan bepalingen die zijn geconfigureerd door Azure AD Connect. Wijzigen van standaard join-voorwaarden maakt het moeilijk ondersteuningsteam om te begrijpen van de aanpassingen en ondersteuning voor het product.

## <a name="validate-sync-rule"></a>Valideren van synchronisatieregel
U kunt de zojuist toegevoegde synchronisatieregel valideren met behulp van de preview-functie zonder dat volledige synchronisatiecyclus worden uitgevoerd. Start **Synchronization Service** gebruikersinterface.

![Standaardregels](media/how-to-connect-fix-default-rules/default10.png)

Klik op de **Metaverse zoeken**, selecteer Bereikobject als **persoon**, **component toevoegen** en vermeld uw zoekcriteria. Klik op **zoeken** knop en dubbelklik op het object in de **zoekresultaten** Houd er rekening mee dat u het importeren en synchroniseren op het forest uitvoert voordat u deze stap uitvoert, dit is om te controleren of de gegevens in Azure AD Connect is bijgewerkt voor dat object.

![Standaardregels](media/how-to-connect-fix-default-rules/default11.png)



Selecteer **Connectors**, selecteert u het object in de bijbehorende connector(forest), klikt u op **eigenschappen...** .

![Standaardregels](media/how-to-connect-fix-default-rules/default12.png)

Klik op de **Preview...**

![Standaardregels](media/how-to-connect-fix-default-rules/default13a.png)

Klik op **genereren Preview** en **importeren Kenmerkstroom** in het linkerdeelvenster.

![Standaardregels](media/how-to-connect-fix-default-rules/default14.png)
 
Hier ziet u dat de zojuist toegevoegde regel wordt uitgevoerd op het object en het kenmerk cloudFiltered is ingesteld op True.

![Standaardregels](media/how-to-connect-fix-default-rules/default15a.png)
 
Hoe kunt u het gewijzigde regel met de standaardregel vergelijken?
U kunt zowel de regels afzonderlijk exporteren als tekstbestanden. Deze regels worden geëxporteerd als powershell-scriptbestand. U kunt vergelijken met behulp van een bestand vergelijken-hulpmiddel om te zien wat voor soort wijzigingen worden uitgevoerd. Hier in dit voorbeeld gebruikt ik windiff om te vergelijken twee bestanden.
 
Ziet u dat in het dialoogvenster gebruiker gewijzigd regel, kenmerk msExchMailboxGuid wordt gewijzigd naar **expressie** typt u in plaats van **Direct** met als **NULL** en  **ExecuteOnce** optie. Identificatie en prioriteit verschillen, kunt u negeren. 

![Standaardregels](media/how-to-connect-fix-default-rules/default17.png)
 
Over het oplossen van een gewijzigde standaardregel?
U kunt met het oplossen van uw regels op de standaardinstellingen, u kunt de gewijzigde regel verwijderen en inschakelen van de standaardregel voor zoals hieronder wordt weergegeven en voer een **volledige synchronisatie**. Eerst die Neem even de corrigerende acties zoals hierboven vermeld, zodat u niet de aanpassing verliest u probeert te bereiken ## Vervolgstappen

## <a name="next-steps"></a>Volgende stappen
- [Hardware en vereisten](how-to-connect-install-prerequisites.md) 
- [Snelle instellingen](how-to-connect-install-express.md)
- [Aangepaste instellingen](how-to-connect-install-custom.md)

