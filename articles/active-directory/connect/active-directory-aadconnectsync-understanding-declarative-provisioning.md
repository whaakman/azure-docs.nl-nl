---
title: 'Azure AD Connect: Inzicht in declaratieve inrichting | Microsoft Docs'
description: Verklaart de declaratieve inrichting configuratiemodel in Azure AD Connect.
services: active-directory
documentationcenter: 
author: billmath
manager: mtillman
editor: 
ms.assetid: cfbb870d-be7d-47b3-ba01-9e78121f0067
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/13/2017
ms.author: billmath
ms.openlocfilehash: 50fce526d667fa829551425edff4bd3863429ef2
ms.sourcegitcommit: f1c1789f2f2502d683afaf5a2f46cc548c0dea50
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/18/2018
---
# <a name="azure-ad-connect-sync-understanding-declarative-provisioning"></a>Azure AD Connect-synchronisatie: inzicht declaratieve inrichting
Dit onderwerp worden de configuratiemodel in Azure AD Connect. Het model heet declaratieve inrichting en Hiermee kunt u een configuratiewijziging met gemak wijzigen. Groot aantal dingen beschreven in dit onderwerp zijn geavanceerde en niet vereist voor de meeste scenario's van klanten.

## <a name="overview"></a>Overzicht
Declaratieve inrichting is het verwerken van objecten die binnenkomen vanaf een verbonden bronmap en bepaalt hoe het object en de kenmerken moeten worden omgezet vanuit een bron aan een doel. Een object in een pijplijn synchronisatie wordt verwerkt en de pijplijn is hetzelfde voor regels voor binnenkomend en uitgaand. Een inkomende regel is van een connectorgebied overgebracht naar de metaverse en een uitgaande regel is van metaverse met een connectorruimte.

![Synchronisatie-pipeline](./media/active-directory-aadconnectsync-understanding-declarative-provisioning/sync1.png)  

De pijplijn heeft verschillende andere modules. Elk criterium is verantwoordelijk voor één concept in object-synchronisatie.

![Synchronisatie-pipeline](./media/active-directory-aadconnectsync-understanding-declarative-provisioning/pipeline.png)  

* Bron, het bronobject
* [Bereik](#scope), vindt u alle synchronisatie-regels die in het bereik
* [Deelnemen aan](#join), bepaalt de relatie tussen connectorruimte en metaverse
* [Transformeren](#transform), berekent hoe kenmerken moeten worden omgezet en stroom
* [Prioriteit](#precedence), conflicterende kenmerk bijdragen wordt omgezet
* Doel voor het doelobject

## <a name="scope"></a>Bereik
De module bereik evalueert een object en bepaalt de regels die in het bereik en moeten worden opgenomen in de verwerking. Afhankelijk van de waarden van de kenmerken van het object, worden de regels voor verschillende synchronisatie geëvalueerd als binnen het bereik. Een uitgeschakelde gebruiker geen Exchange-postvak heeft bijvoorbeeld andere regels dan een ingeschakelde gebruiker met een postvak.  
![Bereik](./media/active-directory-aadconnectsync-understanding-declarative-provisioning/scope1.png)  

Het bereik wordt gedefinieerd als groepen en componenten. De componenten zijn in een groep. Een logische en wordt gebruikt tussen alle componenten in een groep. Bijvoorbeeld: (afdeling = IT en land = Denemarken). Een logische OR wordt gebruikt tussen groepen.

![Bereik](./media/active-directory-aadconnectsync-understanding-declarative-provisioning/scope2.png)  
Het bereik in deze afbeelding moet worden gelezen als (afdeling = IT en land = Denemarken) of (land = Zweden). Als groep 1 of groep 2 wordt vastgesteld op true, wordt de regel is binnen het bereik.

De scope-module biedt ondersteuning voor de volgende bewerkingen.

| Bewerking | Beschrijving |
| --- | --- |
| GELIJK, NOTEQUAL |Het vergelijken van een tekenreeks die wordt geëvalueerd als de waarde is gelijk aan de waarde in het kenmerk. Zie ISIN en ISNOTIN voor kenmerken met meerdere waarden. |
| LESSTHAN, LESSTHAN_OR_EQUAL |Een tekenreeks vergelijken die wordt geëvalueerd als de waarde is minder dan de waarde in het kenmerk. |
| BEVAT, NOTCONTAINS |Het vergelijken van een tekenreeks die wordt geëvalueerd als waarde kan worden gevonden ergens in de waarde in het kenmerk. |
| STARTSWITH, NOTSTARTSWITH |Het vergelijken van een tekenreeks die wordt geëvalueerd als de waarde in het begin van de waarde in het kenmerk is. |
| ENDSWITH, NOTENDSWITH |Het vergelijken van een tekenreeks die wordt geëvalueerd als de waarde in het einde van de waarde in het kenmerk is. |
| GROTER DAN, GREATERTHAN_OR_EQUAL |Het vergelijken van een tekenreeks die wordt geëvalueerd als waarde groter dan de waarde in het kenmerk is. |
| ISNULL, ISNOTNULL |Als het kenmerk afwezig is evalueert uit het object. Als het kenmerk niet aanwezig is en daarom null is, klikt u vervolgens is de regel binnen het bereik. |
| ISIN, ISNOTIN |Als de waarde aanwezig in het opgegeven kenmerk is evalueert. Deze bewerking is de meerdere waarden variatie gelijk en NOTEQUAL. Het kenmerk moet een kenmerk met meerdere waarden zijn en als de waarde kan worden gevonden in een van de kenmerkwaarden, klikt u vervolgens de regel is binnen het bereik. |
| ISBITSET, ISNOTBITSET |Evalueert als een bepaalde bit is ingesteld. Bijvoorbeeld, kan worden gebruikt om te evalueren bits in userAccountControl om te zien als een gebruiker is ingeschakeld of uitgeschakeld. |
| ISMEMBEROF, ISNOTMEMBEROF |De waarde moet een DN-naam aan een groep in het connectorgebied overgebracht bevatten. Als het object lid van de groep die is opgegeven is, is de regel in het bereik. |

## <a name="join"></a>Koppelen
De join-module in de pijplijn voor synchronisatie is verantwoordelijk voor het zoeken van de relatie tussen het object in de bron- en een object in het doel. Deze relatie zou een object in een connectorruimte om een relatie met een object in de metaverse te vinden zijn in een inkomende regel.  
![Deelnemen aan tussen cs en mv](./media/active-directory-aadconnectsync-understanding-declarative-provisioning/join1.png)  
Het doel is om te zien dat als er een object in de metaverse, gemaakt door een andere Connector al deze moet worden gekoppeld. In een account-bron-forest moet de gebruiker uit het accountforest bijvoorbeeld worden toegevoegd aan de gebruiker van de bron-forest.

Joins worden voornamelijk in binnenkomende regels gebruikt voor de connector ruimteobjecten samenvoegen met hetzelfde metaverse-object.

De joins worden gedefinieerd als een of meer groepen. U hebt componenten in een groep. Een logische en wordt gebruikt tussen alle componenten in een groep. Een logische OR wordt gebruikt tussen groepen. De groepen worden in volgorde van boven naar beneden verwerkt. Wanneer een groep precies één overeenkomst met een object in het doel gevonden is, worden geen andere regels voor lid worden geëvalueerd. Als nul of meer dan één object is gevonden, blijft verwerking van de volgende groep regels. Daarom moet de regels in de volgorde van meest expliciete eerste gemaakt en meer fuzzy aan het einde.  
![Lid worden van definitie](./media/active-directory-aadconnectsync-understanding-declarative-provisioning/join2.png)  
De joins in deze afbeelding worden verwerkt van boven naar beneden. De synchronisatie-pijplijn ziet eerst of er een overeenkomst op werknemer-id is. Als dat niet het geval is, de tweede regel ziet als de accountnaam kan worden gebruikt voor de objecten samenvoegen. Als dit niet overeen met ofwel, is de derde regel een meer fuzzy overeenkomst met behulp van de naam van gebruiker.

Als alle join-regels zijn geëvalueerd en er niet precies één overeenkomst, de **koppelingstype** op de **beschrijving** pagina wordt gebruikt. Als deze optie is ingesteld op **inrichten**, en vervolgens een nieuw object in de doel-wordt gemaakt.  
![Inrichten of join](./media/active-directory-aadconnectsync-understanding-declarative-provisioning/join3.png)  

Een object mag alleen één enkele synchronisatieregel met join regels binnen het bereik hebben. Als er meerdere synchronisatie-regels waarvoor join is gedefinieerd, wordt er een fout optreedt. Prioriteit wordt niet gebruikt join conflicten op te lossen. Een object moet een join-regel binnen het bereik van kenmerken stromen met dezelfde binnenkomend en uitgaand richting hebben. Als u op kenmerken van de stroom zowel binnenkomende als uitgaande met hetzelfde object moet, moet u een inkomende en een synchronisatieregel voor uitgaande met join hebben.

Uitgaande join is een speciaal gedrag bij het inrichten van een object met een doel-connector-ruimte. De DN-kenmerk wordt gebruikt om te proberen eerst een reverse-join. Als er al een object in de ruimte van de doel-connector met de dezelfde DN, worden de objecten zijn gekoppeld.

De module join wordt alleen geëvalueerd zodra wanneer een nieuwe synchronisatieregel in het bereik komt. Wanneer een object is gekoppeld, is het niet verwijderen zelfs als de join-criteria niet meer wordt voldaan. Als u een object Meld wilt, moet de synchronisatieregel die lid van de objecten buiten het bereik vallen.

### <a name="metaverse-delete"></a>Metaverse verwijderen
Metaverse-object blijft zolang er is een synchronisatieregel binnen het bereik met **koppelingstype** ingesteld op **inrichten** of **StickyJoin**. Een StickyJoin wordt gebruikt wanneer een Connector is niet toegestaan voor het inrichten van een nieuw object aan de metaverse, maar wanneer deze is gekoppeld, het in de bron moet worden verwijderd voordat de metaverse-object is verwijderd.

Wanneer een metaverse-object is verwijderd, alle objecten gekoppeld met een synchronisatieregel voor uitgaande gemarkeerd voor **inrichten** zijn gemarkeerd voor verwijderen.

## <a name="transformations"></a>Transformaties
De transformaties worden gebruikt om te definiëren hoe kenmerken moeten stromen van de bron naar het doel. De stroom kunnen hebben een van de volgende **stromen typen**: directe, constante of -expressie. Een directe stroom loopt een kenmerkwaarde als-is met geen extra transformaties. Een constante waarde wordt ingesteld voor de opgegeven waarde. Een expressie maakt gebruik van de declaratieve inrichting expressietaal kunt definiëren hoe de transformatie moet worden. De details voor de expressietaal kunnen worden gevonden in de [begrijpen declaratieve inrichting expressietaal](active-directory-aadconnectsync-understanding-declarative-provisioning-expressions.md) onderwerp.

![Inrichten of join](./media/active-directory-aadconnectsync-understanding-declarative-provisioning/transformations1.png)  

De **eenmaal toepassen** selectievakje definieert dat het kenmerk moet alleen worden ingesteld wanneer het object in eerste instantie is gemaakt. Deze configuratie kan bijvoorbeeld worden gebruikt om in te stellen van een eerste wachtwoord voor een nieuwe gebruikersobject.

### <a name="merging-attribute-values"></a>Kenmerkwaarden samenvoegen
Er is een instelling om te bepalen als kenmerken met meerdere waarden moeten worden samengevoegd met verschillende andere connectoren in de kenmerkstromen. De standaardwaarde is **Update**, wat aangeeft dat de synchronisatieregel met de hoogste prioriteit moet win.

![Typen samenvoegen](./media/active-directory-aadconnectsync-understanding-declarative-provisioning/mergetype.png)  

Er is ook **samenvoegen** en **MergeCaseInsensitive**. Deze opties kunt u de waarden van verschillende gegevensbronnen samenvoegen. Bijvoorbeeld, kan het samenvoegen van het lid of proxyAddresses kenmerk uit diverse verschillende forests worden gebruikt. Wanneer u deze optie gebruikt, moeten alle synchronisatie regels binnen het bereik van een object hetzelfde type samenvoeging gebruiken. U kunt geen definiëren **Update** van één Connector en **samenvoegen** van een andere. Als u probeert, ontvangt u een fout opgetreden.

Het verschil tussen **samenvoegen** en **MergeCaseInsensitive** is het verwerken van dubbele kenmerkwaarden. De synchronisatie-engine zorgt u er dubbele waarden zijn niet ingevoegd in het kenmerk target. Met **MergeCaseInsensitive**, dubbele waarden met alleen een verschil in geval gaan niet aanwezig zijn. Bijvoorbeeld, niet ziet u beide 'SMTP:bob@contoso.com'en'smtp:bob@contoso.com' in de doelkenmerk. **Samenvoegen** is alleen kijken naar de exacte waarden en meerdere waarden waarbij er alleen een verschil in kan de aanvraag aanwezig zijn.

De optie **vervangen** is hetzelfde als **Update**, maar wordt niet gebruikt.

### <a name="control-the-attribute-flow-process"></a>Het kenmerk stroom proces beheren
Wanneer meerdere regels voor binnenkomende synchronisatie om bij te dragen naar de metaverse-kenmerk hetzelfde zijn geconfigureerd, wordt prioriteit gebruikt om te bepalen gewonnen. De synchronisatieregel met de hoogste prioriteit (laagste numerieke waarde) gaat bijdragen van de waarde. Hetzelfde gebeurt voor uitgaande regels. De synchronisatie met de hoogste prioriteit wins-regel en bijdragen van de waarde voor de gekoppelde adreslijst.

In sommige gevallen, in plaats van een waarde bijdragen moet de synchronisatieregel bepalen hoe de andere regels zich moeten gedragen. Er zijn enkele speciale letterlijke waarden voor deze aanvraag.

Voor binnenkomende synchronisatieregels de letterlijke waarde **NULL** kan worden gebruikt om aan te geven dat de stroom heeft geen waarde om bij te dragen. Een andere regel met een lagere prioriteit kan een waarde bijdragen. Als er geen regel een waarde bijgedragen, wordt het metaverse-kenmerk verwijderd. Voor een uitgaande regel als **NULL** is de uiteindelijke waarde nadat alle regels voor synchronisatie is verwerkt, wordt de waarde in de gekoppelde map is verwijderd.

De letterlijke waarde **AuthoritativeNull** is vergelijkbaar met **NULL** maar met het verschil dat geen prioriteitsregels met lagere een waarde kunnen bijdragen.

Een kenmerkstroom kunt **IgnoreThisFlow**. Dit is vergelijkbaar met NULL in de zin dat deze aangeeft dat er is niets om bij te dragen. Het verschil is dat wordt niet verwijderd een al bestaande waarde in het doel. Het is net als de kenmerkstroom nooit er is.

Hier volgt een voorbeeld:

In *Out naar AD - gebruiker Exchange hybride* kunt u de volgende stroom vinden:  
`IIF([cloudSOAExchMailbox] = True,[cloudMSExchSafeSendersHash],IgnoreThisFlow)`  
Deze expressie moet worden gelezen als: als het gebruikerspostvak zich bevindt in Azure AD, klikt u vervolgens het kenmerk van Azure AD-AD stromen. Als dat niet het geval is, moet u iets terug naar Active Directory niet stromen. Dit houdt in dat geval de bestaande waarde in AD.

### <a name="importedvalue"></a>ImportedValue
De functie ImportedValue is anders dan alle andere functies, omdat de naam van het kenmerk moet tussen aanhalingstekens in plaats van vierkante haken:  
`ImportedValue("proxyAddresses")`.

Meestal tijdens synchronisatie gebruikt een kenmerk de verwachte waarde, zelfs als deze nog nog niet zijn uitgevoerd of is een fout ontvangen tijdens het exporteren ('boven aan de tower'). Een inkomende synchronisatie wordt ervan uitgegaan dat een kenmerk dat nog niet is bereikt een gekoppelde adreslijst uiteindelijk wordt bereikt. In sommige gevallen is het belangrijk dat u alleen een waarde die is bevestigd door de gekoppelde adreslijst ('hologram en delta-tower importeren') te synchroniseren.

Een voorbeeld van deze functie vindt u in de out-of-box-Synchronisatieregel *In uit Active Directory-gebruiker algemene vanuit Exchange*. In hybride Exchange moet de waarde die is toegevoegd door de Exchange online alleen worden gesynchroniseerd wanneer deze is bevestigd dat de waarde met succes is uitgevoerd:  
`proxyAddresses` <- `RemoveDuplicates(Trim(ImportedValue("proxyAddresses")))`

## <a name="precedence"></a>Prioriteit
Wanneer verschillende synchronisatie regels bijdragen van de waarde van het hetzelfde kenmerk met de doelserver wilt, wordt de prioriteitswaarde gebruikt om te bepalen gewonnen. De regel met de hoogste prioriteit, de laagste numerieke waarde wilt bijdragen van het kenmerk in een conflict.

![Typen samenvoegen](./media/active-directory-aadconnectsync-understanding-declarative-provisioning/precedence1.png)  

Deze volgorde kan worden gebruikt voor het definiëren van nauwkeurigere kenmerkstromen voor een kleine subset van objecten. Bijvoorbeeld, de out-van-box-regels controleren die kenmerken van een account ingeschakeld (**gebruiker AccountEnabled**) hebben voorrang van andere accounts.

Prioriteit kan worden gedefinieerd tussen Connectors. Waarmee Connectors met betere gegevens bij te dragen waarden eerst.

### <a name="multiple-objects-from-the-same-connector-space"></a>Meerdere objecten van dezelfde connectorruimte
Als u meerdere objecten in dezelfde connectorruimte die lid zijn van hetzelfde metaverse-object hebt, moet prioriteit worden aangepast. De synchronisatie-engine is niet kunnen bepalen voorrang als meerdere objecten binnen het bereik van de synchronisatieregel met dezelfde. Het is niet eenduidig welke bronobject moet de waarde voor de metaverse bijdragen. Deze configuratie wordt gerapporteerd als niet-eenduidige, zelfs als de kenmerken in de bron dezelfde waarde hebben.  
![Meerdere objecten samengevoegd tot hetzelfde mv-object](./media/active-directory-aadconnectsync-understanding-declarative-provisioning/multiple1.png)  

In dit scenario moet u het bereik van de synchronisatie-regels te wijzigen zodat de bronobjecten verschillende synchronisatie regels binnen het bereik hebben. Waarmee u verschillende voorrang definiëren.  
![Meerdere objecten samengevoegd tot hetzelfde mv-object](./media/active-directory-aadconnectsync-understanding-declarative-provisioning/multiple2.png)  

## <a name="next-steps"></a>Volgende stappen
* Meer informatie over de expressietaal in [Understanding declaratieve inrichting expressies](active-directory-aadconnectsync-understanding-declarative-provisioning-expressions.md).
* Zie hoe declaratieve inrichting is gebruikte out of box in [inzicht in de standaardconfiguratie](active-directory-aadconnectsync-understanding-default-configuration.md).
* Informatie over het maken van een praktische wijzigen met behulp van declaratieve inrichting [hoe een wijziging aanbrengt in de standaardconfiguratie](active-directory-aadconnectsync-change-the-configuration.md).
* Ze blijven lezen hoe gebruikers en contactpersonen samenwerken [inzicht krijgen in gebruikers en contactpersonen](active-directory-aadconnectsync-understanding-users-and-contacts.md).

**Overzichtsonderwerpen**

* [Azure AD Connect-synchronisatie: inzicht en synchronisatie aanpassen](active-directory-aadconnectsync-whatis.md)
* [Uw on-premises identiteiten integreren met Azure Active Directory](active-directory-aadconnect.md)

**Onderwerpen met naslaginformatie**

* [Azure AD Connect-synchronisatie: functieverwijzing](active-directory-aadconnectsync-functions-reference.md)
