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
ms.openlocfilehash: d2f0956b44d6df64fb73e5eee7844574237d8755
ms.sourcegitcommit: 0ae3139c7e2f9d27e8200ae02e6eed6f52aca476
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/06/2019
ms.locfileid: "65067637"
---
# <a name="fix-modified-default-rules-in-azure-ad-connect"></a>Gewijzigde standaardregels in Azure AD Connect oplossen

Azure Active Directory (Azure AD) Connect maakt gebruik van standaardregels voor synchronisatie.  Deze regels toepassing geen helaas universeel op alle organisaties. Op basis van uw behoeften, mogelijk moet u deze wijzigen. In dit artikel worden besproken twee voorbeelden van de meest voorkomende aanpassingen, en wordt uitgelegd van de juiste manier om deze aanpassingen uitvoeren.

>[!NOTE] 
> Wijzigen van bestaande standaardregels voor het bereiken van een benodigde aanpassingen wordt niet ondersteund. Als u dit doet, voorkomt u dat deze regels wordt bijgewerkt naar de nieuwste versie in toekomstige releases. U wordt niet moet u de oplossingen voor problemen of nieuwe functies ophalen. Dit document wordt uitgelegd hoe u hetzelfde resultaat bereiken zonder de bestaande standaardregels wijzigen. 

## <a name="how-to-identify-modified-default-rules"></a>Gewijzigde standaardregels identificeren
Vanaf versie 1.3.7.0 van Azure AD Connect, is het eenvoudig om te identificeren van de standaardregel voor de gewijzigde. Ga naar **Apps op bureaublad**, en selecteer **Synchronization Rules Editor**.

![Azure AD Connect, met de Synchronization Rules Editor gemarkeerd](media/how-to-connect-fix-default-rules/default1.png)

In de Editor, worden alle gewijzigde standaardregels met een waarschuwingspictogram weergegeven in het zicht van de naam weergegeven.

![Waarschuwingspictogram](media/how-to-connect-fix-default-rules/default2.png)

 Een uitgeschakelde regel met dezelfde naam naast het verschijnt ook (dit is de regel standaard).

![Synchronization Rules Editor, met standaard-regel en de standaardregel voor de gewijzigde](media/how-to-connect-fix-default-rules/default2a.png)

## <a name="common-customizations"></a>Algemene aanpassingen
Hier volgen algemene aanpassingen aan de standaardregels:

- Wijzigen van de kenmerkstroom
- Bereikfilter wijzigen
- Join-voorwaarde wijzigen

Voordat u een van de regels wijzigen:

- De synchronisatieplanning uitschakelen. De scheduler wordt standaard elke 30 minuten uitgevoerd. Zorg ervoor dat deze wordt niet gestart tijdens het aanbrengen van wijzigingen en het oplossen van uw nieuwe regels. Als u wilt de scheduler tijdelijk uitschakelen, start PowerShell en voer `Set-ADSyncScheduler -SyncCycleEnabled $false`.
 ![PowerShell-opdrachten om uit te schakelen van de synchronisatieplanning](media/how-to-connect-fix-default-rules/default3.png)

- De wijziging in bereikfilter kan leiden tot het verwijderen van objecten in de doelmap. Wees voorzichtig voordat u wijzigingen aanbrengt in het bereik van objecten. Het is raadzaam dat u wijzigingen in een staging-server aanbrengt voordat u wijzigingen op de actieve server.
- Een preview worden uitgevoerd op een enkel object, zoals vermeld in de [Synchronisatieregel valideren](#validate-sync-rule) sectie na het toevoegen van een nieuwe regel.
- Een volledige synchronisatie uitvoeren nadat een nieuwe regel toevoegen of wijzigen van een aangepaste synchronisatieregel. Deze synchronisatie is nieuwe regels van toepassing op alle objecten.

## <a name="change-attribute-flow"></a>Wijzigen van de kenmerkstroom
Er zijn drie verschillende scenario's voor het wijzigen van de kenmerkstroom:
- Een nieuw kenmerk toe te voegen.
- De waarde van een bestaand kenmerk te overschrijven.
- Als u niet een bestaand kenmerk synchroniseren.

U kunt doen dit zonder te wijzigen van standaard-regels.

### <a name="add-a-new-attribute"></a>Een nieuw kenmerk toevoegen
Als u vindt dat een kenmerk is stromen niet vanuit de bronmap naar de doelmap, gebruikt u de [Azure AD Connect-synchronisatie: Mapextensies](how-to-connect-sync-feature-directory-extensions.md) dit probleem oplossen.

Als de extensies niet werkt, kunt u twee nieuwe synchronisatieregels, die worden beschreven in de volgende secties toe te voegen.


#### <a name="add-an-inbound-sync-rule"></a>Toevoegen van een regel voor inkomende synchronisatie
Een regel voor inkomende synchronisatie betekent dat de bron voor het kenmerk is een connectorgebied en het doel is de metaverse. Bijvoorbeeld, om een nieuw kenmerk stromen van on-premises Active Directory aan Azure Active Directory, maakt u een nieuwe regel voor inkomende synchronisatie. Start de **Synchronization Rules Editor**, selecteer **inkomend** als de richting en selecteer **nieuwe regel toevoegen**. 

 ! Synchronisatie regels Editor](media/how-to-connect-fix-default-rules/default3a.png)

Uw eigen naamconventie om een naam van de regel te volgen. We gebruiken hier, **aangepaste In uit Active Directory - gebruiker**. Dit betekent dat de regel een aangepaste regel is en een inkomende regel van het Active Directory-connectorgebied overgebracht naar de metaverse is.   

 ![Synchronisatieregel voor binnenkomende gegevens maken](media/how-to-connect-fix-default-rules/default3b.png)

Geef uw eigen beschrijving van de regel zodat toekomstige onderhoud van de regel eenvoudig is. De beschrijving kan bijvoorbeeld worden gebaseerd op wat het doel van de regel is, en waarom dat nodig.

Uw selecties hebt gemaakt voor de **verbonden systeem**, **verbonden systeem objecttype**, en **Metaverse-objecttype** velden.

Geef de prioriteit van 0 t/m 99 (des te lager het nummer, hoe hoger de prioriteit). Voor de **Tag**, **wachtwoordsynchronisatie inschakelen**, en **uitgeschakelde** velden, gebruikt u de standaardselecties.

Houd **Scoping filter** leeg zijn. Dit betekent dat de regel voor alle objecten geldt gekoppeld tussen het Active Directory verbonden systeem en de metaverse.

Houd **Join regels** leeg zijn. Dit betekent dat deze regel maakt gebruik van de join-voorwaarde die is gedefinieerd in de regel standaard. Dit is een andere reden niet uit te schakelen of te verwijderen van de regel standaard. Als er geen join-voorwaarde is, wordt niet het kenmerk stromen. 

Juiste transformaties voor het kenmerk toevoegen. U kunt een constante, om te maken van een constante waarde van de stroom naar het doelkenmerk. U kunt direct toewijzing tussen de bron- of doelabonnements-kenmerk gebruiken. Of u kunt een expressie voor het kenmerk. Hier volgen verschillende [functies](https://docs.microsoft.com/azure/active-directory/hybrid/reference-connect-sync-functions-reference) u kunt gebruiken.

#### <a name="add-an-outbound-sync-rule"></a>Toevoegen van een synchronisatieregel voor uitgaande
Als u wilt het kenmerk een koppeling naar de doelmap, moet u een uitgaande regel maken. Dit betekent dat de bron de metaverse is en het doel het verbonden systeem is. Een uitgaande regel maken, start de **Synchronization Rules Editor**, wijzigen de **richting** naar **uitgaand**, en selecteer **nieuwe regel toevoegen**. 

![Synchronization Rules Editor](media/how-to-connect-fix-default-rules/default3c.png)

Als kunt de binnenkomende regel, u uw eigen naamconventie gebruiken om de naam van de regel. Selecteer de **verbonden systeem** als de Azure AD-tenant, en selecteer het verbonden systeem-object waarnaar u wilt de kenmerkwaarde ingesteld. Stel de prioriteit van 0 t/m 99. 

![Regel voor uitgaande synchronisatie maken](media/how-to-connect-fix-default-rules/default3d.png)

Houd **Scoping filter** en **Join regels** leeg zijn. Vul in de transformatie als constante, direct of expressie. 

U weet nu hoe u een nieuw kenmerk voor een gebruiker Objectstroom uit Active Directory in Azure Active Directory. U kunt deze stappen gebruiken om toe te wijzen een kenmerk van een object op bron en doel. Zie voor meer informatie, [het maken van aangepaste synchronisatieregels](how-to-connect-create-custom-sync-rule.md) en [voorbereiding voor gebruikers inrichten](https://docs.microsoft.com/office365/enterprise/prepare-for-directory-synchronization).

### <a name="override-the-value-of-an-existing-attribute"></a>De waarde van een bestaand kenmerk overschrijven
Het is raadzaam voor de onderdrukking van de waarde van een kenmerk dat is al toegewezen. Maak bijvoorbeeld een inkomende regel alleen op als u altijd een null-waarde met een kenmerk is ingesteld in Azure AD wilt, gewoon. Geef de constante waarde `AuthoritativeNull`, stroom naar het doelkenmerk. 

>[!NOTE] 
> Gebruik `AuthoritativeNull` in plaats van `Null` in dit geval. Dit is omdat de niet-null-waarde wordt vervangen door de null-waarde, zelfs als er een lagere prioriteit (een hogere numerieke waarde in de regel). `AuthoritativeNull`, aan de andere kant de is niet wordt vervangen door een niet-null-waarde door andere regels. 

### <a name="dont-sync-existing-attribute"></a>Bestaand kenmerk worden niet gesynchroniseerd
Als u uitsluiten van een kenmerk wilt van het synchroniseren, gebruikt u het kenmerk filteren-functie in Azure AD Connect. Start **Azure AD Connect** van het pictogram op het bureaublad en selecteer vervolgens **aanpassen Synchronisatieopties**.

![Opties voor Azure AD Connect extra taken](media/how-to-connect-fix-default-rules/default4.png)

 Zorg ervoor dat **Azure AD-app en kenmerkfilters** is geselecteerd en selecteer **volgende**.

![Optionele functies van Azure AD Connect](media/how-to-connect-fix-default-rules/default5.png)

Schakel de kenmerken die u uitsluiten wilt van synchronisatie.

![Azure AD Connect-kenmerken](media/how-to-connect-fix-default-rules/default6a.png)

## <a name="change-scoping-filter"></a>Bereikfilter wijzigen
Azure AD Sync zorgt dat de meeste objecten. U kunt Verklein het bereik van objecten en verminder het aantal objecten om te worden geëxporteerd, zonder dat u wijzigt de standaard synchronisatie-regels. 

Gebruik een van de volgende methoden voor het verminderen van het bereik van de objecten die u synchroniseert:

- cloudFiltered kenmerk
- Organisatie-eenheid filteren

Als u het bereik van de gebruikers gesynchroniseerd wordt verminderen, de wachtwoord-hash-synchronisatie ook geblokkeerd voor de gebruikers gefilterd-out. Als de objecten zijn al gesynchroniseerd nadat u het bereik beperken, worden de objecten gefilterd-out verwijderd uit de doelmap. Zorg ervoor dat u het bereik van zeer zorgvuldig om deze reden.

>[!IMPORTANT] 
> Vergroot het bereik van objecten die zijn geconfigureerd door Azure AD Connect wordt niet aanbevolen. In dat geval, waardoor het lastig is voor het ondersteuningsteam van Microsoft om te begrijpen van de aanpassingen. Als u het bereik van objecten verhogen moet, bewerk de bestaande regel, deze klonen en de oorspronkelijke regel uitschakelen. 

### <a name="cloudfiltered-attribute"></a>cloudFiltered kenmerk
U kunt dit kenmerk in Active Directory niet instellen. Stel de waarde van dit kenmerk door een nieuwe regel voor binnenkomende verbindingen toe te voegen. Vervolgens kunt u **transformatie** en **expressie** om in te stellen van dit kenmerk in de metaverse. Het volgende voorbeeld ziet u dat u niet synchroniseren van alle gebruikers waarvan de naam van afdeling begint wilt met **HRD** (niet hoofdlettergevoelig):

`cloudFiltered <= IIF(Left(LCase([department]), 3) = "hrd", True, NULL)`

We eerst de afdeling van bron (Active Directory) geconverteerd naar kleine letters. Klik vervolgens met behulp van de `Left` functie, we hebben alleen de eerste drie tekens en een vergeleken met `hrd`. Als deze overeenkomt, de waarde is ingesteld op `True`, anders `NULL`. In de waarde wordt ingesteld op null, kunt een andere regel met een lagere prioriteit (een hoger getal) erin schrijven met een andere voorwaarde. Voer Preview-versie van een object voor het valideren van synchronisatieregel, zoals vermeld in de [valideren synchronisatieregel](#validate-sync-rule) sectie.

![Opties voor de regel voor inkomende synchronisatie maken](media/how-to-connect-fix-default-rules/default7a.png)

### <a name="organizational-unit-filtering"></a>Organisatie-eenheid filteren
U kunt een of meer organisatie-eenheden (OU's) maken en verplaatsen van de objecten die u niet wilt synchroniseren met deze organisatie-eenheden. Configureer vervolgens de organisatie-eenheid filteren in Azure AD Connect. Start **Azure AD Connect** van het pictogram op het bureaublad en selecteer de volgende opties. U kunt ook de organisatie-eenheid op het moment van de installatie van Azure AD Connect-filtering configureren. 

![Azure AD Connect extra taken](media/how-to-connect-fix-default-rules/default8.png)

Volg de wizard en schakelt u de organisatie-eenheden u niet wilt synchroniseren.

![Azure AD Connect-domein en organisatie-eenheid filteropties](media/how-to-connect-fix-default-rules/default9.png)

## <a name="change-join-condition"></a>Join-voorwaarde wijzigen
Gebruik de join situaties die standaard door Azure AD Connect geconfigureerd. Wijzigen van standaard join-voorwaarden, waardoor het lastig voor ondersteuning van Microsoft om te begrijpen van de aanpassingen en ondersteuning voor het product.

## <a name="validate-sync-rule"></a>Valideren van synchronisatieregel
U kunt de zojuist toegevoegde synchronisatieregel valideren met behulp van de preview-functie, zonder dat de cyclus volledige synchronisatie worden uitgevoerd. Selecteer in Azure AD Connect, **Synchronization Service**.

![Azure AD Connect, met de synchronisatieservice is gemarkeerd](media/how-to-connect-fix-default-rules/default10.png)

Selecteer **Metaverse zoeken**. Selecteer het Bereikobject als **persoon**, selecteer **component toevoegen**, en vermeld uw zoekcriteria. Selecteer vervolgens **zoeken**, en dubbelklik op het object in de lijst met zoekresultaten. Zorg ervoor dat uw gegevens in Azure AD Connect bijgewerkt voor dat object is door te voeren importeren en synchroniseren op het forest voordat u deze stap uitvoert.

![Synchronization Service Manager](media/how-to-connect-fix-default-rules/default11.png)

Op **eigenschappen van het Metaverseobject**, selecteer **Connectors**, selecteert u het object in de bijbehorende connector (forest) en selecteer **eigenschappen...** .

![Eigenschappen van het Metaverse-Object](media/how-to-connect-fix-default-rules/default12.png)

Selecteer **Preview...**

![Eigenschappen van het Object Connectorgebied](media/how-to-connect-fix-default-rules/default13a.png)

Selecteer in het voorbeeldvenster **genereren Preview** en **importeren Kenmerkstroom** in het linkerdeelvenster.

![Preview](media/how-to-connect-fix-default-rules/default14.png)
 
U ziet hier dat de zojuist toegevoegde regel wordt uitgevoerd op het object en heeft de `cloudFiltered` kenmerk in op true.

![Preview](media/how-to-connect-fix-default-rules/default15a.png)
 
Als u wilt vergelijken van de gewijzigde regel met de standaardregel, exporteren met de regels afzonderlijk als tekstbestanden. Deze regels worden geëxporteerd als een PowerShell-scriptbestand. U kunt vergelijken met behulp van elk bestand vergelijking hulpprogramma (bijvoorbeeld windiff) om de wijzigingen te bekijken. 
 
U ziet dat in de gewijzigde regel, de `msExchMailboxGuid` kenmerk is gewijzigd in de **expressie** type, in plaats van **Direct**. Ook de waarde is gewijzigd in **NULL** en **ExecuteOnce** optie. Identificatie en prioriteit verschillen, kunt u negeren. 

![de uitvoer van Windiff hulpprogramma](media/how-to-connect-fix-default-rules/default17.png)
 
De gewijzigde regel verwijderen om op te lossen uw regels om ze te wijzigen op de standaardinstellingen, en de standaardregel inschakelen. Zorg ervoor dat u niet verloren gaan de aanpassingen die u probeert te bereiken. Wanneer u klaar bent, voert **volledige synchronisatie**.

## <a name="next-steps"></a>Volgende stappen
- [Hardware en vereisten](how-to-connect-install-prerequisites.md) 
- [Snelle instellingen](how-to-connect-install-express.md)
- [Aangepaste instellingen](how-to-connect-install-custom.md)



