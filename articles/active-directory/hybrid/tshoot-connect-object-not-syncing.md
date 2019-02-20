---
title: Een object dat niet wordt gesynchroniseerd met Azure Active Directory oplossen | Microsoft-Docs
description: Problemen met een object dat niet wordt gesynchroniseerd met Azure Active Directory.
services: active-directory
documentationcenter: ''
author: billmath
manager: daveba
editor: ''
ms.assetid: ''
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/10/2018
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 931865803328189d89c0fbae15caa801c3f7f7c6
ms.sourcegitcommit: 79038221c1d2172c0677e25a1e479e04f470c567
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/19/2019
ms.locfileid: "56416918"
---
# <a name="troubleshoot-an-object-that-is-not-synchronizing-with-azure-active-directory"></a>Een object dat niet kan worden gesynchroniseerd met Azure Active Directory oplossen

Als een object wordt niet gesynchroniseerd zoals verwacht met Microsoft Azure Active Directory (Azure AD), kan het zijn verschillende oorzaken hebben. Als u een fout e-mailbericht hebt ontvangen van Azure AD of het foutbericht wordt weergegeven in Azure AD Connect Health, lezen [synchronisatiefouten oplossen](tshoot-connect-sync-errors.md) in plaats daarvan. Maar als u een probleem opgetreden bij het waar het object niet in Azure AD is oplossen wilt, in dit artikel is voor u. Dit wordt beschreven hoe u fouten gevonden in de on-premises onderdeel Azure AD Connect-synchronisatie.

>[!IMPORTANT]
>Voor Azure AD verbinding maken met implementatie met versie 1.1.749.0 of hoger, gebruikt u de [probleemoplossing taak](tshoot-connect-objectsync.md) in de wizard voor het oplossen van problemen met synchronisatie object. 

## <a name="synchronization-process"></a>Synchronisatieproces

Voordat we synchroniseren problemen onderzoeken, moeten we eerst de Azure AD Connect synchroniseren proces te begrijpen:

  ![Diagram van Azure AD Connect sync-proces](./media/tshoot-connect-object-not-syncing/syncingprocess.png)

### <a name="terminology"></a>**Terminologie**

* **CS:** Connectorgebied controleert, een tabel in een database
* **MV:** Metaverse, een tabel in een database

### <a name="synchronization-steps"></a>**Stappen voor synchronisatie**
Het synchroniseren proces omvat stappen te volgen:

1. **Importeren uit Active Directory:** Active Directory-objecten worden in de Active Directory-CS gebracht.

2. **Importeren uit Azure AD:** Azure AD-objecten worden in de Azure AD CS gebracht.

3. **Synchronisatie:** Synchronisatieregels voor binnenkomende en regels voor uitgaande synchronisatie worden uitgevoerd in de volgorde van prioriteit getal, van lagere hoger. Als u wilt de synchronisatieregels weergeven, gaat u naar de Synchronization Rules Editor vanuit de bureaublad-toepassingen. De synchronisatieregels voor binnenkomende importeren gegevens in uit CS MV. De regels voor uitgaande synchronisatie worden gegevens van MV verplaatsen naar CS.

4. **Exporteren naar AD:** Objecten zijn na het synchroniseren wordt geëxporteerd uit de Active Directory-CS aan Active Directory.

5. **Exporteren naar Azure AD:** Na het synchroniseren, worden objecten vanuit de Azure AD CS geëxporteerd naar Azure AD.

## <a name="troubleshooting"></a>Problemen oplossen

Voor de fouten, zoekt u op een aantal verschillende plaatsen in de volgende volgorde:

1. De [bewerkingslogboeken](#operations) te vinden fouten die zijn geïdentificeerd door de synchronisatie-engine tijdens het importeren en synchroniseren.
2. De [connectorgebied](#connector-space-object-properties) om ontbrekende objecten en synchronisatiefouten te vinden.
3. De [metaverse](#metaverse-object-properties) te vinden van problemen met betrekking tot de gegevens.

Start [Synchronization Service Manager](how-to-connect-sync-service-manager-ui.md) voordat u deze stappen begint.

## <a name="operations"></a>Bewerkingen
De **Operations** tabblad in Synchronization Service Manager is waar u uw probleemoplossing moet starten. Op dit tabblad toont de resultaten van de meest recente bewerkingen. 

![Schermafbeelding van de Synchronization Service Manager, van bewerkingen tabblad geselecteerd](./media/tshoot-connect-object-not-syncing/operations.png)  

Het bovenste gedeelte van de **Operations** tabblad toont alle uitvoeringen in chronologische volgorde. Standaard Registreer de bewerkingen houdt informatie over de afgelopen zeven dagen, maar deze instelling kan worden gewijzigd met de [scheduler](how-to-connect-sync-feature-scheduler.md). Weergave voor alle die uitvoeren niet wordt weergegeven een **succes** status. U kunt het sorteren door te klikken op de headers.

De **Status** kolom bevat de meest belangrijke informatie en het meest ernstige probleem voor het uitvoeren van wordt weergegeven. Hier volgt een snel overzicht van de meest voorkomende statussen in volgorde van prioriteit onderzoek (waarbij * tekenreeksen met verschillende mogelijke fout aangeeft).

| Status | Opmerking |
| --- | --- |
| gestopt-* |De uitvoering kan niet worden voltooid. Dit kan bijvoorbeeld gebeuren als het externe systeem niet actief is en is niet bereikbaar. |
| gestopt-fout-limiet |Er zijn meer dan 5000 fouten. De uitvoering is automatisch gestopt vanwege het grote aantal fouten. |
| voltooide -\*-fouten |De uitvoering is voltooid, maar er zijn fouten (minder dan 5000) die moeten worden onderzocht. |
| voltooide -\*-waarschuwingen |De uitvoering is voltooid, maar sommige gegevens zijn niet de verwachte status. Als u fouten hebt, wordt dit bericht is meestal alleen een symptoom zijn. Geen waarschuwingen onderzoeken, totdat u fouten hebt opgelost. |
| voltooid |Geen problemen. |

Wanneer u selecteert u een rij, onder aan de **Operations** tabblad wordt bijgewerkt om de details van deze uitvoering weer te geven. Klik aan de uiterst links van dit gebied mogelijk hebt u een lijst met de titel **stap #**. Deze lijst wordt alleen weergegeven als er meerdere domeinen in uw forest en elk domein wordt vertegenwoordigd door een stap. Naam van het domein kunt u vinden onder de kop **partitie**. Onder de **Synchronisatiestatistieken** kop, u vindt meer informatie over het aantal wijzigingen dat is verwerkt. Selecteer de koppelingen voor een lijst van de gewijzigde objecten. Als u objecten met fouten hebt, die fouten weergegeven op de **synchronisatiefouten** kop.

### <a name="errors-on-the-operations-tab"></a>Fouten op het tabblad bewerkingen
Wanneer u fouten hebt, ziet u Synchronization Service Manager zowel het object in de fout als de fout zelf als koppelingen met meer informatie.

![Schermafbeelding van fouten in Synchronization Service Manager](./media/tshoot-connect-object-not-syncing/errorsync.png)  
Begin met het selecteren van de fouttekenreeks. (In de afbeelding hierboven de fouttekenreeks is **sync-regel-fout-functie-geactiveerd**.) Eerst krijgt u een overzicht van het object. De echte fout Selecteer **Stack-Trace**. Deze tracering bevat foutopsporingsniveau informatie voor de fout.

Met de rechtermuisknop op de **Call Stack informatie** Klik **Alles selecteren**, en selecteer vervolgens **kopie**. Kopieer de stack en bekijk de fout in de gewenste editor, zoals Kladblok.

Als de fout van is **SyncRulesEngine**, de call stack informatie eerst een lijst met alle kenmerken van het object. Schuif omlaag totdat u de kop ziet **InnerException = >**.  

  ![Schermafbeelding van de Synchronization Service Manager of met gegevens over de fout onder de kop InnerException = >](./media/tshoot-connect-object-not-syncing/errorinnerexception.png)
  
De regel na de kop ziet u de fout. In de afbeelding hierboven, wordt de fout is van een aangepaste synchronisatieregel die Fabrikam gemaakt.

Als de fout niet voldoende informatie geven heeft, is het tijd om te kijken naar de gegevens zelf. Selecteer de koppeling met de object-id en doorgaan met het oplossen van de [connector space geïmporteerde object](#cs-import).

## <a name="connector-space-object-properties"></a>Eigenschappen van het object connectorgebied
Als de [ **Operations** ](#operations) tabblad bevat geen fouten zijn, volgt u de connector space-object in Active Directory naar de metaverse naar Azure AD. In dit pad, moet u zien waar het probleem zich bevindt.

### <a name="searching-for-an-object-in-the-cs"></a>Zoeken naar een object in de CS

Selecteer in Synchronization Service Manager **Connectors**, selecteert u de Active Directory-Connector en selecteer **Search Connector Space**.

In de **bereik** Schakel **RDN** als u wilt zoeken op het kenmerk CN, of selecteer **DN-naam of het anker** als u wilt zoeken op de **distinguishedName**  kenmerk. Voer een waarde in en selecteer **zoeken**. 
 
![Schermafbeelding van een connectorgebied zoeken](./media/tshoot-connect-object-not-syncing/cssearch.png)  

Als u het object niet vindt u op zoek bent voor het mogelijk zijn gefilterd met [filteren op basis van een domein](how-to-connect-sync-configure-filtering.md#domain-based-filtering) of [OE filteren](how-to-connect-sync-configure-filtering.md#organizational-unitbased-filtering). Als u wilt controleren of het filteren is geconfigureerd zoals verwacht, lezen [Azure AD Connect-synchronisatie: Filtering configureren](how-to-connect-sync-configure-filtering.md).

U kunt een andere nuttige zoekopdracht uitvoeren door te selecteren van de Azure AD-Connector. In de **bereik** Schakel **in behandeling zijnde importeren**, en selecteer vervolgens de **toevoegen** selectievakje. Deze zoekopdracht kunt u alle gesynchroniseerde objecten in Azure AD die niet kan gekoppeld aan een on-premises-object worden.  

![Schermafbeelding van wezen in een connectorgebied zoeken](./media/tshoot-connect-object-not-syncing/cssearchorphan.png) 
 
Deze objecten zijn gemaakt door een andere synchronisatie-engine of een synchronisatie-engine met een andere filters gebruiken om configuratie. Deze zwevende objecten zijn niet meer wordt beheerd. Deze lijst bekijken en kunt u deze objecten verwijderen met behulp van de [Azure AD PowerShell](https://aka.ms/aadposh) cmdlets.

### <a name="cs-import"></a>CS importeren
Wanneer u een object CS opent, zijn er meerdere tabbladen aan de bovenkant. De **importeren** tabblad bevat de gegevens die tijdelijk worden opgeslagen na importeren.  

![Schermafbeelding van het venster Eigenschappen van het Object Connectorgebied met de Import-tabblad geselecteerd](./media/tshoot-connect-object-not-syncing/csobject.png)    

De **oude waarde** kolom ziet u wat momenteel wordt opgeslagen in Connect, en de **nieuwe waarde** kolom ziet u wat is ontvangen van het bronsysteem en is nog niet toegepast. Als er een fout op het object, worden de wijzigingen niet verwerkt.

De **synchronisatiefout** tabblad wordt weergegeven in de **eigenschappen van het Object Connectorgebied** venster alleen als er een probleem met het object. Raadpleeg voor meer informatie over het [synchronisatiefouten oplossen op de **Operations** tabblad](#errors-on-the-operations-tab).

![Schermopname van het tabblad synchronisatiefout in het venster Eigenschappen van het Object Connectorgebied](./media/tshoot-connect-object-not-syncing/cssyncerror.png)  

### <a name="cs-lineage"></a>CS afkomst
De **afkomst** tabblad de **eigenschappen van het Object Connectorgebied** venster ziet u hoe de connector space-object is gerelateerd aan het metaverse-object. U kunt zien wanneer de connector laatst geïmporteerd een wijziging van de verbonden systeem en welke regels toegepast voor het vullen van de gegevens in de metaverse.  

![Schermopname van het tabblad afkomst in het venster Eigenschappen van het Object Connectorgebied](./media/tshoot-connect-object-not-syncing/cslineage.png)  

In de afbeelding hierboven de **actie** kolom ziet u een regel voor inkomende synchronisatie met de actie **inrichten**. Die aangeeft, zolang deze connectorobject ruimte aanwezig is, wordt het metaverse-object blijft. Als de lijst van synchronisatieregels in plaats daarvan ziet u een regel voor uitgaande synchronisatie met een **inrichten** actie, dit object wordt verwijderd wanneer de metaverse-object wordt verwijderd.  

![Schermafbeelding van een venster afkomst op het tabblad afkomst in het venster Eigenschappen van het Object Connectorgebied](./media/tshoot-connect-object-not-syncing/cslineageout.png)  

In de voorgaande afbeelding ziet u ook de **PasswordSync** kolom die het inkomende connectorgebied kan bijdragen tot het wachtwoord wijzigt omdat een regel voor synchronisatie de waarde heeft **waar**. Dit wachtwoord wordt verzonden naar Azure AD via de uitgaande regel.

Uit de **afkomst** tabblad u toegang hebt tot de metaverse hiervoor [ **eigenschappen van het Metaverseobject**](#mv-attributes).

### <a name="preview"></a>Preview
In de linkerbenedenhoek van de **eigenschappen van het Object Connectorgebied** venster is de **Preview** knop. Deze knop opent u de **Preview** pagina, waar u een enkel object kunt synchroniseren. Deze pagina is handig als u een aangepaste synchronisatieregels oplossen wilt en wilt zien van het effect van een wijziging in een enkel object. U kunt een **Full sync** of een **Deltasynchronisatie**. U kunt ook selecteren **genereren Preview**, die alleen de wijziging houdt in het geheugen. Of selecteer **doorvoeren Preview**, die de metaverse-updates en alle wijzigingen in de doel-connectorspaces fasen.  

![Schermafbeelding van de voorbeeldpagina met voorbeeldweergave starten geselecteerd](./media/tshoot-connect-object-not-syncing/preview.png)  

U kunt inspecteren van het object en welke regel toegepast voor een bepaald kenmerkstroom ziet in de Preview-versie.  

![Schermafbeelding van de voorbeeldpagina importeren Kenmerkstroom](./media/tshoot-connect-object-not-syncing/previewresult.png)

### <a name="log"></a>Logboek
Naast de **Preview** knop, selecteer de **Log** te openen de **Log** pagina. Hier kunt u zien dat de synchronisatiestatus van wachtwoord en de geschiedenis. Zie voor meer informatie, [wachtwoord-hashsynchronisatie met Azure AD Connect-synchronisatie oplossen](tshoot-connect-password-hash-synchronization.md).

## <a name="metaverse-object-properties"></a>Eigenschappen van het Metaverseobject
Is het doorgaans beter om te beginnen met zoeken van de bron van Active Directory-connectorgebied. Maar u kunt ook beginnen met zoeken in de metaverse.

### <a name="searching-for-an-object-in-the-mv"></a>Zoeken naar een object in de MV
Selecteer in Synchronization Service Manager **Metaverse zoeken**, zoals in de volgende afbeelding. Maak een query waarvan u weet dat de gebruiker is gevonden. Zoeken naar algemene kenmerken, zoals **accountName** (**sAMAccountName**) en **userPrincipalName**. Zie voor meer informatie, [Sync Service Manager Metaverse zoeken](how-to-connect-sync-service-manager-ui-mvsearch.md).

![Schermafbeelding van de Synchronization Service Manager of met het tabblad Metaverse zoeken](./media/tshoot-connect-object-not-syncing/mvsearch.png)  

In de **zoekresultaten** venster, klikt u op het object.

Als u het object niet vinden hebt, is het niet nog bereikt de metaverse. Doorgaan met zoeken naar het object in Active Directory [connectorgebied](#connector-space-object-properties). Als u het object in de Active Directory-connectorgebied vinden, kan er een synchronisatiefout die door het object uit naar de metaverse binnenkort wordt geblokkeerd of een synchronisatie bereikfilter voor de regel kan worden toegepast.

### <a name="object-not-found-in-the-mv"></a>Object is niet gevonden in de MV
Als het object in de Active Directory-CS verkeert, maar niet aanwezig is in de MV, een bereikfilter wordt toegepast. Als u wilt kijken naar de bereikfilter, gaat u naar het menu bureaubladtoepassing en selecteer **Synchronization Rules Editor**. Filter de regels van toepassing op het object door het onderstaande filter aan te passen.

  ![Schermafbeelding van de Synchronization Rules Editor, met een inkomende synchronisatie regels zoeken](./media/tshoot-connect-object-not-syncing/syncrulessearch.png)

Elke regel bekijken in de lijst hierboven en controleer de **Scoping filter**. In de volgende bereikfilter, als de **isCriticalSystemObject** waarde is null of onwaar of leeg zijn, het is binnen het bereik.

  ![Schermafbeelding van een bereikfilter in een zoekopdracht van de regel voor inkomende synchronisatie](./media/tshoot-connect-object-not-syncing/scopingfilter.png)

Ga naar de [CS importeren](#cs-import) van het kenmerk lijst en controleer welke filter wordt geblokkeerd door het object niet naar de MV worden verplaatst. De **Connectorgebied** kenmerk lijst met alleen niet-null en niet-lege kenmerken wordt weergegeven. Bijvoorbeeld, als **isCriticalSystemObject** niet weergegeven in de lijst met de waarde van dit kenmerk is null of leeg zijn.

### <a name="object-not-found-in-the-azure-ad-cs"></a>Object is niet gevonden in de Azure AD CS
Als het object niet aanwezig in het connectorgebied van Azure AD is, maar aanwezig in de MV is, kijken naar de bereikfilter van de regels voor uitgaand verkeer van het betreffende connectorgebied en ontdek als het object is uitgefilterd, omdat de [MV kenmerken](#mv-attributes)niet voldoen aan de criteria.

Als u wilt de uitgaand bereikfilter bekijkt, selecteert u de geldende voorschriften voor het object door het onderstaande filter aan te passen. Elke regel weergeven en kijken naar de bijbehorende [MV-kenmerk](#mv-attributes) waarde.

  ![Schermafbeelding van een zoekopdracht van de regels voor uitgaande synchronisatie in Synchronization Rules Editor](./media/tshoot-connect-object-not-syncing/outboundfilter.png)


### <a name="mv-attributes"></a>MV-kenmerken
Op de **kenmerken** tabblad ziet u de waarden en welke connectors deze bijgedragen.  

![Schermafbeelding van het venster Eigenschappen Metaverse-Object met het tabblad kenmerken geselecteerd](./media/tshoot-connect-object-not-syncing/mvobject.png)  

Als een object wordt niet gesynchroniseerd, vraagt u de volgende vragen over de statussen kenmerk in de metaverse:
- Is het kenmerk **cloudFiltered** presenteren en ingesteld op **waar**? Als dit het geval is, deze is gefilterd op basis van de stappen in [filteren op basis van het kenmerk](how-to-connect-sync-configure-filtering.md#attribute-based-filtering).
- Is het kenmerk **sourceAnchor** aanwezig zijn? Als dat niet het geval is, hebt u een topologie met account-resource forest? Als een object wordt geïdentificeerd als een gekoppeld postvak (het kenmerk **msExchRecipientTypeDetails** heeft de waarde **2**), wordt de **sourceAnchor** is bijgedragen door het forest met een Active Directory-account ingeschakeld. Zorg ervoor dat het hoofdaccount is geïmporteerd en juist gesynchroniseerd. Het hoofdaccount moet worden vermeld onder de [connectors](#mv-connectors) voor het object.

### <a name="mv-connectors"></a>MV-connectors
De **Connectors** tabblad toont alle connectorspaces waarvoor een weergave van het object. 
 
![Schermafbeelding van het venster Eigenschappen Metaverse-Object met het tabblad Connectors is geselecteerd](./media/tshoot-connect-object-not-syncing/mvconnectors.png)  

U hebt een connector voor:

- In wordt elk Active Directory-forest de gebruiker aangegeven. Deze weergave kunt opnemen **foreignSecurityPrincipals** en **Neem contact op met** objecten.
- Een connector in Azure AD.

Als u de Azure AD-connector ontbreekt, raadpleegt u de sectie op [MV kenmerken](#mv-attributes) om te controleren of de criteria voor het inrichten van Azure AD.

Uit de **Connectors** tabblad kunt u ook naar de [ruimte connectorobject](#connector-space-object-properties). Selecteer een rij en klikt u op **eigenschappen**.

## <a name="next-steps"></a>Volgende stappen
- Meer informatie over [Azure AD Connect-synchronisatie](how-to-connect-sync-whatis.md).
- Meer informatie over [hybride identiteit](whatis-hybrid-identity.md).
