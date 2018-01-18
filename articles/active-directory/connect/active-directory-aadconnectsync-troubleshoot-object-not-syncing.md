---
title: Problemen met een object dat niet kan worden gesynchroniseerd naar Azure AD | Microsoft-Docs
description: Waarom een object niet kan worden gesynchroniseerd naar Azure AD.
services: active-directory
documentationcenter: 
author: billmath
manager: mtillman
editor: 
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/13/2017
ms.author: billmath
ms.openlocfilehash: 7176ebd0515008147bd3797dcb760f35e2d85d45
ms.sourcegitcommit: f1c1789f2f2502d683afaf5a2f46cc548c0dea50
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/18/2018
---
# <a name="troubleshoot-an-object-that-is-not-synchronizing-to-azure-ad"></a>Problemen met een object dat niet kan worden gesynchroniseerd naar Azure AD

Als een object niet zoals verwacht naar Azure AD synchroniseren is, kan het zijn diverse redenen. Als u een e-mailbericht fout van Azure AD hebt ontvangen of u de fout in Azure AD Connect Health ziet, Lees [exportfouten oplossen](active-directory-aadconnect-troubleshoot-sync-errors.md) in plaats daarvan. Maar als u een waar het object niet in Azure AD is probleem oplossen wilt, in dit onderwerp wordt voor u. Wordt beschreven hoe u fouten gevonden in de lokale onderdeel Azure AD Connect-synchronisatie.

Als u de fouten zoekt, gaat u om te kijken naar een aantal verschillende plaatsen in de volgende volgorde:

1. De [bewerkingslogboeken](#operations) voor het zoeken naar fouten die zijn geïdentificeerd door de synchronisatie-engine tijdens het importeren en synchroniseren.
2. De [connectorruimte](#connector-space-object-properties) voor het vinden van ontbrekende objecten en synchronisatiefouten.
3. De [metaverse](#metaverse-object-properties) voor het vinden van problemen met gegevens.

Start [Synchronization Service Manager](active-directory-aadconnectsync-service-manager-ui.md) voordat u deze stappen begint.

## <a name="operations"></a>Bewerkingen
De operations-tabblad Synchronization Service Manager is waar u de probleemoplossing moet beginnen. De operations-tabblad ziet u de resultaten van de meest recente bewerkingen.  
![Sync-Service Manager](./media/active-directory-aadconnectsync-troubleshoot-object-not-syncing/operations.png)  

Het bovenste gedeelte ziet u alle wordt uitgevoerd in chronische volgorde. Standaard de bewerkingen logboekgegevens houdt van de afgelopen zeven dagen, maar deze instelling kan worden gewijzigd met de [scheduler](active-directory-aadconnectsync-feature-scheduler.md). U wilt zoeken naar eventuele uitvoeren die een status geslaagd niet weergegeven. U kunt het sorteren door te klikken op de headers.

De **Status** kolom is de belangrijkste informatie en ziet u het meest ernstige probleem voor een uitvoering. Hier volgt een korte samenvatting van de meest voorkomende statussen in volgorde van prioriteit voor het onderzoeken van (waarbij * enkele mogelijke fout tekenreeksen geven).

| Status | Opmerking |
| --- | --- |
| Stop-* |De sessie kan niet worden voltooid. Bijvoorbeeld als het externe systeem is niet actief en kan geen verbinding worden gemaakt. |
| gestopt fout limiet |Er zijn meer dan 5000 fouten. De run is automatisch gestopt vanwege het grote aantal fouten. |
| voltooide -\*-fouten |De uitvoering is voltooid, maar er zijn fouten (minder dan 5000) die moeten worden onderzocht. |
| voltooide -\*-waarschuwingen |De uitvoering is voltooid, maar sommige gegevens is niet de verwachte status. Als u fouten hebt, klikt u vervolgens dit bericht is meestal slechts een symptoom zijn. Totdat u fouten hebt opgelost, moet u de waarschuwingen niet onderzoeken. |
| voltooid |Geen problemen. |

Wanneer u een rij selecteert, wordt onder updates zodat de details van die worden uitgevoerd. Aan de linkerkant van de onderkant, moet u wellicht een lijst met de melding **stap #**. Deze lijst wordt alleen weergegeven als er meerdere domeinen in uw forest waar elk domein wordt vertegenwoordigd door een stap. Naam van het domein kan worden gevonden onder de kop **partitie**. Onder **Synchronisatiestatistieken**, vindt u meer informatie over het aantal wijzigingen dat is verwerkt. U kunt klikken op de koppelingen om een lijst van de gewijzigde objecten. Als u objecten met fouten hebt, deze fouten worden weergegeven onder **synchronisatiefouten**.

### <a name="troubleshoot-errors-in-operations-tab"></a>Oplossen van fouten in de operations-tabblad
![Sync-Service Manager](./media/active-directory-aadconnectsync-troubleshoot-object-not-syncing/errorsync.png)  
Wanneer er fouten zijn, zijn zowel het object in de fout en de fout zelf koppelingen met meer informatie.

De fouttekenreeks op te starten (**sync regel-fout-functie-geactiveerde** in de afbeelding). Eerst krijgt u een overzicht van het object. Klik op de knop overzicht van de werkelijke fout **stacktracering**. Deze tracering biedt foutopsporingsinformatie niveau voor de fout.

U kunt met de rechtermuisknop de **call stack informatie** Kies **Alles selecteren**, en **kopie**. Vervolgens kunt u de stack kopiëren en bekijk de fout in uw favoriete teksteditor, zoals Kladblok.

* Als de fout van is **SyncRulesEngine**, en vervolgens de informatie van de stack aanroep eerst een lijst met alle kenmerken voor het object heeft. Schuif omlaag totdat u de kop ziet **InnerException = >**.  
  ![Sync Service Manager](./media/active-directory-aadconnectsync-troubleshoot-object-not-syncing/errorinnerexception.png)  
  De regel na ziet u de fout. In de bovenstaande afbeelding wordt de fout is van een aangepaste regel-Fabrikam voor synchronisatie is gemaakt.

Als de fout zich niet voldoende informatie geven heeft, wordt het tijd om te kijken naar de gegevens zelf. U kunt op de koppeling met de object-id en gaat u verder de [connector ruimte van het geïmporteerde object](#cs-import).

## <a name="connector-space-object-properties"></a>Connector ruimte objecteigenschappen
Als er geen fouten gevonden in de [operations](#operations) tabblad en vervolgens de volgende stap is het connectorobject ruimte uit Active Directory, volgen de metaverse en Azure AD. In dit pad zult u waar het probleem is.

### <a name="search-for-an-object-in-the-cs"></a>Zoeken naar een object in de CS

In **Synchronization Service Manager**, klikt u op **Connectors**, selecteer de Active Directory-Connector en **Connectorgebied zoeken**.

In **bereik**, selecteer **RDN** (als u wilt zoeken op het kenmerk CN) of **DN-naam of het anker** (als u wilt zoeken op het kenmerk distinguishedName). Voer een waarde en klik op **Search**.  
![Connectorgebied zoeken](./media/active-directory-aadconnectsync-troubleshoot-object-not-syncing/cssearch.png)  

Als u het object niet kunt vinden u zoekt, wordt deze mogelijk zijn gefilterd met [filteren op basis van een domein](active-directory-aadconnectsync-configure-filtering.md#domain-based-filtering) of [filteren op basis van organisatie-eenheid](active-directory-aadconnectsync-configure-filtering.md#organizational-unitbased-filtering). Lees de [configureert filtering](active-directory-aadconnectsync-configure-filtering.md) onderwerp om te controleren of het filteren is geconfigureerd zoals verwacht.

Een andere nuttige zoekactie is het selecteren van de Azure AD-Connector in **bereik** Selecteer **in behandeling zijnde importeren**, en selecteer de **toevoegen** selectievakje. Deze zoekopdracht kunt u alle gesynchroniseerde objecten in Azure AD die niet kan gekoppeld aan een lokaal object worden.  
![Connector ruimte zoeken zwevende](./media/active-directory-aadconnectsync-troubleshoot-object-not-syncing/cssearchorphan.png)  
Deze objecten zijn gemaakt door een andere synchronisatie-engine of een synchronisatie-engine met een andere configuratie filteren. Deze weergave is een lijst met **zwevende** objecten niet meer wordt beheerd. Controleer deze lijst en overwegen te verwijderen van deze objecten met behulp van de [Azure AD PowerShell](http://aka.ms/aadposh) cmdlets.

### <a name="cs-import"></a>CS importeren
Wanneer u een object cs opent, zijn er meerdere tabbladen boven. De **importeren** tabblad ziet u de gegevens die tijdelijk worden opgeslagen na importeren.  
![CS-object](./media/active-directory-aadconnectsync-troubleshoot-object-not-syncing/csobject.png)    
De **oude waarde** ziet u wat momenteel wordt opgeslagen in het Connect en de **nieuwe waarde** wat is ontvangen van het bronsysteem en is nog niet toegepast. Als er een fout op het object, worden wijzigingen niet verwerkt.

**Error**  
![CS-object](./media/active-directory-aadconnectsync-troubleshoot-object-not-syncing/cssyncerror.png)  
De **synchronisatiefout** tabblad is alleen zichtbaar als er een probleem met het object. Zie voor meer informatie [synchronisatiefouten oplossen](#troubleshoot-errors-in-operations-tab).

### <a name="cs-lineage"></a>CS-afkomst
Het tabblad afkomst ziet u hoe het connectorobject ruimte is gerelateerd aan het metaverse-object. U kunt zien wanneer een wijziging in de Connector laatste worden geïmporteerd vanuit het verbonden systeem en welke regels toegepast op het vullen van de gegevens in de metaverse.  
![CS-afkomst](./media/active-directory-aadconnectsync-troubleshoot-object-not-syncing/cslineage.png)  
In de **actie** kolom, kunt u zien er is een **inkomend** synchronisatieregel met de actie **inrichten**. Die aangeeft, zolang deze connectorobject ruimte aanwezig is, wordt het metaverse-object blijft. Als de lijst met regels voor synchronisatie in plaats daarvan ziet u een synchronisatieregel met richting **uitgaand** en **inrichten**, betekent dit dat dit object wordt verwijderd wanneer de metaverse-object is verwijderd.  
![Sync Service Manager](./media/active-directory-aadconnectsync-troubleshoot-object-not-syncing/cslineageout.png)  
U kunt ook zien in de **PasswordSync** kolom die het inkomende connectorgebied overgebracht kan bijdragen tot het wachtwoord wordt gewijzigd omdat een synchronisatieregel de waarde heeft **True**. Dit wachtwoord wordt dan gezonden naar Azure AD via de uitgaande regel.

Op het tabblad afkomst hebt u de metaverse door te klikken op [eigenschappen Metaverse-Object](#mv-attributes).

Aan de onderkant van alle tabs zijn twee knoppen: **Preview** en **logboek**.

### <a name="preview"></a>Preview
De voorbeeldpagina wordt gebruikt voor het synchroniseren van een enkel object. Dit is handig als u bepaalde regels aangepaste synchronisatie oplossen wilt en wilt zien van het effect van een wijziging in een enkel object. U kunt kiezen tussen **volledige synchronisatie** en **Deltasynchronisatie**. U kunt ook selecteren tussen **Preview genereren**, die alleen de wijziging blijft in het geheugen en **doorvoeren Preview**, die de metaverse bijgewerkt en alle wijzigingen naar de doel-connectorspaces fasen.  
![Sync Service Manager](./media/active-directory-aadconnectsync-troubleshoot-object-not-syncing/preview.png)  
U kunt het object en welke regel wordt toegepast voor een bepaalde kenmerkstroom inspecteren.  
![Sync Service Manager](./media/active-directory-aadconnectsync-troubleshoot-object-not-syncing/previewresult.png)

### <a name="log"></a>Logboek
De pagina wordt gebruikt om te zien van de synchronisatiestatus van wachtwoord en de geschiedenis. Zie voor meer informatie [Wachtwoordsynchronisatie oplossen](active-directory-aadconnectsync-troubleshoot-password-synchronization.md).

## <a name="metaverse-object-properties"></a>Eigenschappen van Metaverse-object
Meestal is het beter om te beginnen met het zoeken van de Active Directory-bron [connectorruimte](#connector-space). Maar u kunt ook starten zoeken vanaf de metaverse.

### <a name="search-for-an-object-in-the-mv"></a>Zoeken naar een object in de MV
In **Synchronization Service Manager**, klikt u op **Metaverse zoeken**. Maak een query die u weet dat de gebruiker is gevonden. U kunt zoeken naar gangbare kenmerken, zoals accountName (sAMAccountName) en userPrincipalName. Zie voor meer informatie [Metaverse zoeken](active-directory-aadconnectsync-service-manager-ui-mvsearch.md).
![Sync Service Manager](./media/active-directory-aadconnectsync-troubleshoot-object-not-syncing/mvsearch.png)  

In de **zoekresultaten** venster, klikt u op het object.

Als u het object niet gevonden, is klikt u vervolgens het niet nog bereikt de metaverse. Doorgaan met zoeken naar het object in de Active Directory [connectorruimte](#connector-space-object-properties). Kan er een fout opgetreden in synchronisatie dat door het object naar de metaverse niet wordt geblokkeerd of er mogelijk een filter toegepast.

### <a name="mv-attributes"></a>MV-kenmerken
Op het tabblad kenmerken ziet u de waarden en welke Connector het bijgedragen.  
![Sync-Service Manager](./media/active-directory-aadconnectsync-troubleshoot-object-not-syncing/mvobject.png)  

Als een object niet wordt gesynchroniseerd, klikt u vervolgens bekijkt u de volgende kenmerken in de metaverse:
- Het kenmerk **cloudFiltered** aanwezig is en ingesteld op **true**? Als het is, dan is gefilterd volgens de stappen in [kenmerk gebaseerd filteren](active-directory-aadconnectsync-configure-filtering.md#attribute-based-filtering).
- Het kenmerk **sourceAnchor** aanwezig? Als dat niet het geval is, hebt u een topologie met account-resource forest? Als een object wordt geïdentificeerd als een gekoppeld postvak (het kenmerk **msExchRecipientTypeDetails** heeft de waarde 2), en vervolgens de sourceAnchor is die is bijgedragen door het forest met een ingeschakelde Active Directory-account. Zorg ervoor dat de hoofdaccount is geïmporteerd en juist is gesynchroniseerd. De hoofdaccount moet worden vermeld in de [connectors](#mv-connectors) voor het object.

### <a name="mv-connectors"></a>MV-Connectors
Het tabblad Connectors bevat alle connectorspaces waarvoor een representatie van het object.  
![Sync-Service Manager](./media/active-directory-aadconnectsync-troubleshoot-object-not-syncing/mvconnectors.png)  
U hebt een connector voor:

- Elk Active Directory-forest de gebruiker wordt weergegeven. Deze weergave kan bestaan foreignSecurityPrincipals en neem contact op met objecten.
- Een connector in Azure AD.

Als u de connector naar Azure AD ontbreekt, Lees [MV-kenmerken](#MV-attributes) om te controleren of de criteria voor naar Azure AD wordt ingericht.

Op dit tabblad kunt u om te navigeren naar de [ruimte connectorobject](#connector-space-object-properties). Selecteer een rij en klikt u op **eigenschappen**.

## <a name="next-steps"></a>Volgende stappen
Meer informatie over de [Azure AD Connect-synchronisatie](active-directory-aadconnectsync-whatis.md) configuratie.

Lees meer over het [integreren van uw on-premises identiteiten met Azure Active Directory ](active-directory-aadconnect.md).
