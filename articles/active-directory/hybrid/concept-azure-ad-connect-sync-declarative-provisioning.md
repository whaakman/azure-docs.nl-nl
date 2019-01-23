---
title: 'Azure AD Connect: Informatie over declaratieve inrichting | Microsoft Docs'
description: Verklaart de declaratieve inrichting configuratiemodel in Azure AD Connect.
services: active-directory
documentationcenter: ''
author: billmath
manager: daveba
editor: ''
ms.assetid: cfbb870d-be7d-47b3-ba01-9e78121f0067
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/13/2017
ms.component: hybrid
ms.author: billmath
ms.openlocfilehash: 45b145d9a8922bc3da50cef7d9fa7aacf260417d
ms.sourcegitcommit: cf88cf2cbe94293b0542714a98833be001471c08
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/23/2019
ms.locfileid: "54471769"
---
# <a name="azure-ad-connect-sync-understanding-declarative-provisioning"></a>Azure AD Connect-synchronisatie: Inzicht in declaratieve inrichting
In dit onderwerp wordt uitgelegd dat het configuratiemodel in Azure AD Connect. Het model wordt genoemd declaratieve inrichting en deze kunt u een configuratiewijziging met gemak. Groot aantal dingen die worden beschreven in dit onderwerp zijn geavanceerde en niet vereist voor de meeste klantscenario's.

## <a name="overview"></a>Overzicht
Declaratieve inrichting objecten afkomstig is van een verbonden bronmap wordt verwerkt en bepaalt hoe het object en de kenmerken moeten worden omgezet vanuit een bron naar een doel. Een object in een pijplijn synchronisatie wordt verwerkt en de pijplijn is hetzelfde voor inkomende en uitgaande regels. Een inkomende regel is van een connectorgebied overgebracht naar de metaverse en een uitgaande regel is van de metaverse naar een connectorgebied.

![Synchronisatie-pijplijn](./media/concept-azure-ad-connect-sync-declarative-provisioning/sync1.png)  

De pijplijn heeft verschillende andere modules. Elke gebeurtenis wordt die verantwoordelijk is voor een concept in objectsynchronisatie.

![Synchronisatie-pijplijn](./media/concept-azure-ad-connect-sync-declarative-provisioning/pipeline.png)  

* Bron, het bronobject
* [Bereik](#scope), vindt u alle synchronisatieregels die binnen het bereik vallen
* [Deelnemen aan](#join), bepaalt de relatie tussen de connector space en metaverse
* [Transformeren](#transform), berekent hoe kenmerken moeten worden omgezet en flow
* [Prioriteit](#precedence), lost conflicterende kenmerk bijdragen
* Doel, het beoogde doelobject

## <a name="scope"></a>Bereik
De scope-module is een object evalueren en bepaalt de regels die in het bereik en moeten worden opgenomen in de verwerking. Afhankelijk van de waarden voor kenmerken op het object, worden andere synchronisatieregels geëvalueerd als binnen het bereik. Een uitgeschakelde gebruiker geen Exchange-postvak heeft bijvoorbeeld andere regels dan een ingeschakelde gebruiker met een postvak.  
![Bereik](./media/concept-azure-ad-connect-sync-declarative-provisioning/scope1.png)  

Het bereik wordt gedefinieerd als groepen en -componenten. De componenten zijn binnen een groep. Een logische en wordt tussen alle componenten in een groep gebruikt. Bijvoorbeeld: (afdeling IT en land = = Denemarken). Een logische OR wordt gebruikt tussen groepen.

![Bereik](./media/concept-azure-ad-connect-sync-declarative-provisioning/scope2.png)  
Het bereik in deze afbeelding moet worden gelezen als (afdeling IT en land = = Denemarken) of (land = Zweden). Als de groep 1 of 2-groep wordt geëvalueerd op waar, wordt de regel is binnen het bereik.

De scope-module biedt ondersteuning voor de volgende bewerkingen.

| Bewerking | Description |
| --- | --- |
| GELIJK, NOTEQUAL |Het vergelijken van een tekenreeks die wordt geëvalueerd als de waarde is gelijk aan de waarde in het kenmerk. Zie ISIN en ISNOTIN voor meerwaardige kenmerken. |
| LESSTHAN, LESSTHAN_OR_EQUAL |De vergelijken van een tekenreeks die wordt geëvalueerd als de waarde is dan de waarde in het kenmerk. |
| BEVAT, NOTCONTAINS |Het vergelijken van een tekenreeks die wordt geëvalueerd als waarde kan worden gevonden ergens in de waarde in het kenmerk. |
| STARTSWITH, NOTSTARTSWITH |Het vergelijken van een tekenreeks die wordt geëvalueerd als de waarde in het begin van de waarde in het kenmerk is. |
| ENDSWITH, NOTENDSWITH |Het vergelijken van een tekenreeks die wordt geëvalueerd als waarde aan het einde van de waarde in het kenmerk. |
| GROTER DAN, GREATERTHAN_OR_EQUAL |Het vergelijken van een tekenreeks die wordt geëvalueerd als waarde groter is dan de waarde in het kenmerk. |
| ISNULL, ISNOTNULL |Als het kenmerk is niet aanwezig in het object. Als het kenmerk niet aanwezig zijn en dus null is, klikt u vervolgens is de regel binnen het bereik. |
| ISIN, ISNOTIN |Als de waarde in het opgegeven kenmerk evalueert. Deze bewerking is de variatie op meerdere waarden gelijk zijn en NOTEQUAL. Het kenmerk moet zijn van een kenmerk met meerdere waarden en als de waarde kan worden gevonden in een van de kenmerkwaarden weer, klikt u vervolgens de regel is binnen het bereik. |
| ISBITSET, ISNOTBITSET |Evalueert als een bepaalde bit is ingesteld. Bijvoorbeeld, kan worden gebruikt om te evalueren van de bits in userAccountControl om te zien als een gebruiker is ingeschakeld of uitgeschakeld. |
| ISMEMBEROF, ISNOTMEMBEROF |De waarde moet een DN-naam aan een groep in het connectorgebied bevatten. Als het object lid van de groep die is opgegeven is, wordt de regel is binnen het bereik. |

## <a name="join"></a>Koppelen
De join-module in de pijplijn synchronisatie is verantwoordelijk voor het vinden van de relatie tussen het object in de bron- en een object in het doel. Deze relatie is op een binnenkomende regel een object in een connectorgebied zoeken van een relatie met een object in de metaverse.  
![Deelnemen aan tussen cs en mv](./media/concept-azure-ad-connect-sync-declarative-provisioning/join1.png)  
Het doel is om te zien dat als er een object is al in de metaverse, die zijn gemaakt door een andere Connector dit moet worden gekoppeld aan. In een account-bron-forest moet de gebruiker uit het accountforest bijvoorbeeld worden toegevoegd aan de gebruiker van de bron-forest.

Samenvoegingen worden voornamelijk op regels voor binnenkomende verbindingen gebruikt om ruimte connectorobjecten samen toevoegen aan de dezelfde metaverse-object.

De joins worden gedefinieerd als een of meer groepen. Binnen een groep hebt van de EU. Een logische en wordt tussen alle componenten in een groep gebruikt. Een logische OR wordt gebruikt tussen groepen. De groepen worden verwerkt in volgorde van boven naar beneden. Wanneer een groep exact één overeenkomst met een object dat in het doel gevonden is, worden geen andere regels voor lid worden geëvalueerd. Als nul of meer dan één object is gevonden, blijft verwerking van de volgende groep van regels. Om deze reden moet de regels is gemaakt in de volgorde van de meeste expliciete eerst en meer zoeken bij benadering aan het einde.  
![Definitie toevoegen](./media/concept-azure-ad-connect-sync-declarative-provisioning/join2.png)  
De koppelingen in deze afbeelding worden verwerkt van boven naar beneden. Eerst ziet de pijplijn sync als er een overeenkomst op werknemer-id is. Als dat niet het geval is, de tweede regel ziet als de naam van het kan worden gebruikt om de objecten samenvoegen. Als dat niet overeenkomt met een, is de derde en laatste regel een meer fuzzy overeenkomst met behulp van de naam van gebruiker.

Als alle join-regels zijn geëvalueerd en er niet precies één overeenkomst is, de **koppelingstype** op de **beschrijving** pagina wordt gebruikt. Als deze optie is ingesteld op **inrichten**, en vervolgens een nieuw object in het doel is gemaakt.  
![Inrichten of join](./media/concept-azure-ad-connect-sync-declarative-provisioning/join3.png)  

Een object mag slechts één enkele synchronisatieregel met join regels binnen het bereik hebben. Als er meerdere synchronisatieregels waarbij de join is gedefinieerd, wordt er een fout optreedt. Prioriteit wordt niet gebruikt om conflicten tussen join. Een object moet een join-regel in het bereik voor kenmerken met dezelfde inkomende/uitgaande richting stromen hebben. Als u kenmerken van gebruikersstroom zowel inkomend als uitgaand naar hetzelfde object wilt, moet u een inkomende en een synchronisatieregel voor uitgaande met join hebben.

Uitgaande join is een speciaal gedrag als er wordt geprobeerd om in te richten van een object naar een doel-connectorgebied. Het kenmerk DN-naam wordt gebruikt voor het eerst proberen een reverse-join. Als er al een object in de doel-connectorgebied met de dezelfde DN-naam, zijn de objecten gekoppeld.

De module join wordt alleen beoordeeld wanneer wanneer een nieuwe synchronisatieregel voor wordt geleverd in het bereik. Wanneer een object is gekoppeld, is het niet verwijderen, zelfs als de samenvoegcriteria niet meer wordt voldaan. Als u afmelden van een object wilt, moet de synchronisatieregel die lid van de objecten niet binnen het bereik gaan.

### <a name="metaverse-delete"></a>Metaverse-verwijderen
Een metaverse-object blijft zolang er is een synchronisatieregel binnen het bereik met **koppelingstype** ingesteld op **inrichten** of **StickyJoin**. Een StickyJoin wordt gebruikt wanneer een Connector is niet toegestaan voor het inrichten van een nieuw object naar de metaverse, maar wanneer deze is gekoppeld, deze in de bron moet worden verwijderd voordat de metaverse-object wordt verwijderd.

Wanneer een metaverse-object wordt verwijderd, alle objecten die zijn gekoppeld aan een synchronisatieregel voor uitgaande gemarkeerd voor **inrichten** zijn gemarkeerd voor verwijderen.

## <a name="transformations"></a>Transformaties
De transformaties worden gebruikt om te bepalen hoe kenmerken moeten stromen van de bron naar het doel. De stromen kunnen hebben een van de volgende **flow typen**: Directe,-constante of -expressie. Een directe stroom een kenmerkwaarde als stromen-is geen aanvullende transformaties. Een constante waarde wordt ingesteld voor de opgegeven waarde. Een expressie maakt gebruik van de declaratieve inrichting expressietaal Express hoe de transformatie moet worden. De details voor de expressietaal kunnen worden gevonden de [begrijpen declaratieve inrichting expressietaal](concept-azure-ad-connect-sync-declarative-provisioning-expressions.md) onderwerp.

![Inrichten of join](./media/concept-azure-ad-connect-sync-declarative-provisioning/transformations1.png)  

De **eenmaal toepassen** selectievakje definieert dat het kenmerk moet alleen worden ingesteld als het object in eerste instantie wordt gemaakt. Deze configuratie kan bijvoorbeeld worden gebruikt om in te stellen van een eerste wachtwoord voor een nieuwe gebruikersobject.

### <a name="merging-attribute-values"></a>Kenmerkwaarden samenvoegen
Er is een instelling om te bepalen als meerwaardige kenmerken uit diverse verschillende Connectors moeten worden samengevoegd in de kenmerkstromen. De standaardwaarde is **Update**, wat aangeeft dat de synchronisatieregel met hoogste prioriteit te winnen.

![Typen samenvoegen](./media/concept-azure-ad-connect-sync-declarative-provisioning/mergetype.png)  

Er is ook **samenvoegen** en **MergeCaseInsensitive**. Deze opties kunnen u waarden uit verschillende bronnen samen te voegen. Het kan bijvoorbeeld worden gebruikt voor het samenvoegen van het lid of proxyAddresses-kenmerk uit diverse verschillende forests. Wanneer u deze optie gebruikt, moeten alle synchronisatieregels binnen het bereik van een object hetzelfde type samenvoeging gebruiken. U kunt geen definiëren **Update** van één Connector en **samenvoegen** van een andere. Als u probeert, ontvangt u een foutbericht.

Het verschil tussen **samenvoegen** en **MergeCaseInsensitive** wordt beschreven hoe u voor het verwerken van dubbele kenmerkwaarden. De synchronisatie-engine zorgt ervoor dat dubbele waarden zijn niet ingevoegd in het doelkenmerk. Met **MergeCaseInsensitive**, dubbele waarden met alleen een verschil in het geval niet meer aanwezig zijn. Bijvoorbeeld, niet ziet u beide "SMTP:bob@contoso.com'en'smtp:bob@contoso.com' in het doelkenmerk. **Samenvoegen** wordt alleen gekeken naar de exacte waarden en meerdere waarden waarbij er alleen een verschil in kan de aanvraag aanwezig zijn.

De optie **vervangen** is hetzelfde als **Update**, maar wordt niet gebruikt.

### <a name="control-the-attribute-flow-process"></a>Het proces voor kenmerk stroom beheren
Als meerdere inkomende synchronisatieregels zijn geconfigureerd om bij te dragen aan het dezelfde metaverse-kenmerk, wordt prioriteit gebruikt om te bepalen van de winnaar. De synchronisatieregel met hoogste prioriteit (laagste numerieke waarde) gaat om bij te dragen van de waarde. Hetzelfde gebeurt voor regels voor uitgaand verkeer. De synchronisatie met de hoogste prioriteit wins-regel en de waarde in de verbonden adreslijst bijdragen.

In sommige gevallen, in plaats van een waarde, met bijdragen moet de synchronisatieregel bepalen hoe de andere regels zich moeten gedragen. Er zijn enkele speciale letterlijke waarden gebruikt voor deze aanvraag.

Voor binnenkomende synchronisatieregels, de letterlijke waarde **NULL** kan worden gebruikt om aan te geven dat de stroom heeft geen waarde om bij te dragen. Een andere regel met een lagere prioriteit kan een waarde bijdragen. Als er geen regel een waarde, bijgedragen wordt het metaverse-kenmerk is verwijderd. Voor een uitgaande regel als **NULL** is de laatste waarde nadat alle synchronisatieregels zijn verwerkt, wordt de waarde in de verbonden adreslijst is verwijderd.

De letterlijke waarde **AuthoritativeNull** is vergelijkbaar met **NULL** , maar met het verschil is dat er geen prioriteitsregels met lagere hun bijdrage een waarde leveren.

Een kenmerkstroom kunt **IgnoreThisFlow**. Het is vergelijkbaar met NULL in die zin dat deze geeft aan dat er is niets om bij te dragen. Het verschil is dat het niet een al bestaande waarde in het doel verwijdert. Het is net als de kenmerkstroom nooit er is.

Hier volgt een voorbeeld:

In *Out met AD - gebruiker Exchange hybride* kunt u de volgende stroom vinden:  
`IIF([cloudSOAExchMailbox] = True,[cloudMSExchSafeSendersHash],IgnoreThisFlow)`  
Deze expressie moet worden gelezen als: als het gebruikerspostvak in Azure AD bevindt zich, klikt u vervolgens het kenmerk van Azure AD-AD flow. Als dat niet het geval is, iets terug naar Active Directory niet flow. Dit houdt in dat geval wordt de bestaande waarde in AD.

### <a name="importedvalue"></a>ImportedValue
De functie ImportedValue is anders dan alle andere functies, omdat de naam aanhalingstekens in plaats van tussen vierkante haken tussen moet:  
`ImportedValue("proxyAddresses")`.

Een kenmerk gebruikt meestal tijdens de synchronisatie de verwachte waarde, zelfs als deze nog niet is zijn geëxporteerd of er is een fout ontvangen tijdens het exporteren ('boven aan de tower"). Een inkomende synchronisatie wordt ervan uitgegaan dat een kenmerk dat nog niet heeft bereikt een verbonden adreslijst uiteindelijk wordt bereikt. In sommige gevallen is het belangrijk dat u alleen een waarde die is goedgekeurd door de verbonden adreslijst ('hologram en delta-tower importeren') worden gesynchroniseerd.

Een voorbeeld van deze functie kunt u vinden in de Synchronisatieregel out-of-box *In uit Active Directory-gebruiker algemene in Exchange*. In hybride Exchange, moet de waarde die is toegevoegd door Exchange online alleen worden gesynchroniseerd wanneer deze is bevestigd dat de waarde is geëxporteerd:  
`proxyAddresses` <- `RemoveDuplicates(Trim(ImportedValue("proxyAddresses")))`

## <a name="precedence"></a>Prioriteit
Wanneer verschillende synchronisatieregels probeert om bij te dragen van de dezelfde kenmerkwaarde naar het doel, wordt de prioriteitswaarde wordt gebruikt om te bepalen van de winnaar. De regel met de hoogste prioriteit, de laagste numerieke waarde, is om bij te dragen van het kenmerk in een conflict gaan.

![Typen samenvoegen](./media/concept-azure-ad-connect-sync-declarative-provisioning/precedence1.png)  

Deze volgorde kan worden gebruikt voor het definiëren van nauwkeuriger kenmerkstromen voor een kleine subset van objecten. Bijvoorbeeld, de out-van-vak-regels moeten u ervoor dat die kenmerken van een ingeschakeld account (**gebruiker AccountEnabled**) hebben voorrang van andere accounts.

Prioriteit kan worden gedefinieerd tussen Connectors. Die kunt Connectors met betere gegevens om bij te dragen waarden eerst.

### <a name="multiple-objects-from-the-same-connector-space"></a>Meerdere objecten vanuit de dezelfde connector ruimte
Hebt u verschillende objecten in de dezelfde connector ruimte toegevoegd aan de dezelfde metaverse-object, moet prioriteit worden aangepast. De synchronisatie-engine is niet kunnen bepalen prioriteit als meerdere objecten binnen het bereik van de synchronisatieregel met dezelfde. Het is niet eenduidig welke bronobject moet de waarde in de metaverse bijdragen. Deze configuratie wordt gerapporteerd als niet-eenduidige, zelfs als de kenmerken in de bron van dezelfde waarde hebben.  
![Meerdere objecten samengevoegd tot hetzelfde mv-object](./media/concept-azure-ad-connect-sync-declarative-provisioning/multiple1.png)  

Voor dit scenario moet u het bereik van de synchronisatieregels wijzigen, zodat de bronobjecten andere synchronisatieregels binnen het bereik hebben. Hierdoor kunt u verschillende prioriteit definiëren.  
![Meerdere objecten samengevoegd tot hetzelfde mv-object](./media/concept-azure-ad-connect-sync-declarative-provisioning/multiple2.png)  

## <a name="next-steps"></a>Volgende stappen
* Meer informatie over de expressietaal in [Understanding declaratieve inrichting expressies](concept-azure-ad-connect-sync-declarative-provisioning-expressions.md).
* Zie hoe declaratieve inrichting is gebruikte out-of-box in [inzicht in de standaardconfiguratie](concept-azure-ad-connect-sync-default-configuration.md).
* Zien hoe u een praktische wijzigen met behulp van declaratieve inrichting [hoe u een wijziging in de standaardconfiguratie](how-to-connect-sync-change-the-configuration.md).
* Blijven lezen hoe gebruikers en contactpersonen samenwerken [inzicht krijgen in gebruikers en contactpersonen](concept-azure-ad-connect-sync-user-and-contacts.md).

**Overzichtsonderwerpen**

* [Azure AD Connect-synchronisatie: Begrijpen en aanpassen van synchronisatie](how-to-connect-sync-whatis.md)
* [Uw on-premises identiteiten integreren met Azure Active Directory](whatis-hybrid-identity.md)

**Onderwerpen met naslaginformatie**

* [Azure AD Connect-synchronisatie: Functieverwijzing](reference-connect-sync-functions-reference.md)
