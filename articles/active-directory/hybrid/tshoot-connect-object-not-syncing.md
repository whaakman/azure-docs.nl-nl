---
title: Een object dat niet kan worden gesynchroniseerd naar Azure AD oplossen | Microsoft-Docs
description: Waarom een object niet kan worden gesynchroniseerd naar Azure AD.
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
ms.openlocfilehash: 55668b8ef8019e1ee808bc0cba9d98c0db53c584
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/13/2019
ms.locfileid: "56198737"
---
# <a name="troubleshoot-an-object-that-is-not-synchronizing-to-azure-ad"></a>Een object dat niet kan worden gesynchroniseerd naar Azure AD oplossen

Als een object is niet gesynchroniseerd zoals verwacht aan Azure AD, kan het zijn de verschillende oorzaken hebben. Als u een fout e-mailbericht hebt ontvangen van Azure AD of het foutbericht wordt weergegeven in Azure AD Connect Health, lees vervolgens [exportfouten oplossen](tshoot-connect-sync-errors.md) in plaats daarvan. Maar als u een probleem opgetreden bij het waar het object niet in Azure AD is oplossen wilt, klikt u vervolgens in dit onderwerp is voor u. Dit wordt beschreven hoe u fouten gevonden in de Azure AD Connect-synchronisatie van on-premises onderdeel.

>[!IMPORTANT]
>Voor Azure Active Directory (AAD) verbinding maken met implementatie met versie 1.1.749.0 of hoger, gebruikt u de [probleemoplossing taak](tshoot-connect-objectsync.md) in de wizard voor het oplossen van problemen met synchronisatie van object. 

## <a name="synchronization-process"></a>Synchronisatieproces

Voor het onderzoeken van problemen met de synchronisatie, moeten we eerst begrijpen de **Azure AD Connect** synchronisatieproces:

  ![Azure AD Connect-synchronisatie](./media/tshoot-connect-object-not-syncing/syncingprocess.png)

### <a name="terminology"></a>**Terminologie**

* **CS:** Connectorgebied, een tabel in de database.
* **MV:** Metaverse, een tabel in de database.
* **AD:** Active Directory
* **AAD:** Azure Active Directory

### <a name="synchronization-steps"></a>**Stappen voor synchronisatie**
Het proces van synchronisatie omvat stappen te volgen:

1. **Importeren uit Active Directory:** **Active Directory** objecten worden binnengebracht **AD CS**.

2. **Importeren uit AAD:** **Azure Active Directory** objecten worden binnengebracht **AAD CS**.

3. **Synchronisatie:** **Synchronisatie van regels voor binnenkomende verbindingen** en **uitgaande synchronisatieregels** vanaf worden uitgevoerd in volgorde van prioriteit nummer lager op hoger. Als u wilt de synchronisatieregels weergeven, gaat u naar **Synchronization Rules Editor** vanuit de bureaublad-toepassingen. De **regels voor binnenkomende synchronisatie** worden in de gegevens uit de CS te MV. De **uitgaande synchronisatieregels** gegevens verplaatst van MV naar CS.

4. **Exporteren naar AD:** Nadat synchronisatie is uitgevoerd, objecten zijn geëxporteerd uit AD CS **Active Directory**.

5. **Exporteren naar AAD:** Nadat synchronisatie is uitgevoerd, objecten zijn geëxporteerd uit AAD CS **Azure Active Directory**.

## <a name="troubleshooting"></a>Problemen oplossen

Als u de fouten zoekt, gaat u om te kijken naar een aantal verschillende plaatsen in de volgende volgorde:

1. De [bewerkingslogboeken](#operations) voor het zoeken naar fouten die zijn geïdentificeerd door de synchronisatie-engine tijdens het importeren en synchroniseren.
2. De [connectorgebied](#connector-space-object-properties) voor het vinden van ontbrekende objecten en synchronisatie van fouten.
3. De [metaverse](#metaverse-object-properties) voor het vinden van problemen met betrekking tot de gegevens.

Start [Synchronization Service Manager](how-to-connect-sync-service-manager-ui.md) voordat u deze stappen begint.

## <a name="operations"></a>Bewerkingen
De operations-tabblad in de Synchronization Service Manager is waar u uw probleemoplossing moet starten. De operations-tabblad toont de resultaten van de meest recente bewerkingen.  
![Synchronisatie van Service Manager](./media/tshoot-connect-object-not-syncing/operations.png)  

Het bovenste gedeelte toont alle uitvoeringen in chronologische volgorde. Standaard Registreer de bewerkingen houdt informatie over de afgelopen zeven dagen, maar deze instelling kan worden gewijzigd met de [scheduler](how-to-connect-sync-feature-scheduler.md). U wilt zoeken naar alle uitvoeren die niet de status voltooid wordt weergegeven. U kunt het sorteren door te klikken op de headers.

De **Status** kolom zijn de belangrijkste gegevens en het meest ernstige probleem voor het uitvoeren van wordt weergegeven. Hier volgt een snel overzicht van de meest voorkomende statussen in volgorde van prioriteit voor het onderzoeken van (waarbij * verschillende mogelijke fout tekenreeksen geven).

| Status | Opmerking |
| --- | --- |
| gestopt-* |De uitvoering kan niet worden voltooid. Bijvoorbeeld, als het externe systeem niet actief is en is niet bereikbaar. |
| gestopt-fout-limiet |Er zijn meer dan 5000 fouten. De uitvoering is automatisch gestopt vanwege het grote aantal fouten. |
| voltooide -\*-fouten |De uitvoering is voltooid, maar er zijn fouten (minder dan 5000) die moeten worden onderzocht. |
| voltooide -\*-waarschuwingen |De uitvoering is voltooid, maar sommige gegevens is niet de verwachte status. Als u fouten hebt, klikt u vervolgens dit bericht is meestal alleen een symptoom zijn. Totdat u fouten hebt opgelost, moet u geen waarschuwingen onderzoeken. |
| voltooid |Geen problemen. |

Wanneer u een rij selecteert, wordt onder bijgewerkt om weer te geven van de details van die worden uitgevoerd. Aan de linkerkant van de onderste, mogelijk hebt u een lijst met de melding **stap #**. Deze lijst wordt alleen weergegeven als u meerdere domeinen in uw forest waar elk domein wordt vertegenwoordigd door een stap. Naam van het domein kunt u vinden onder de kop **partitie**. Onder **Synchronisatiestatistieken**, vindt u meer informatie over het aantal wijzigingen dat is verwerkt. U kunt klikken op de koppelingen een lijst van de gewijzigde objecten te krijgen. Als u objecten met fouten hebt, die fouten weergegeven op **synchronisatiefouten**.

### <a name="troubleshoot-errors-in-operations-tab"></a>Oplossen van fouten in de operations-tabblad
![Synchronisatie van Service Manager](./media/tshoot-connect-object-not-syncing/errorsync.png)  
Wanneer u fouten hebt, worden zowel het object in de foutmeldingen en de fout zelf koppelingen met meer informatie.

Start door te klikken op de fouttekenreeks (**sync-regel-fout-functie-geactiveerd** in de afbeelding). Eerst krijgt u een overzicht van het object. Als u wilt de echte fout ziet, klikt u op de knop **Stack-Trace**. Deze tracering bevat op gegevens voor foutopsporing voor de fout.

U kunt met de rechtermuisknop de **call stack informatie** Kies **Alles selecteren**, en **kopie**. U kunt vervolgens kopiëren van de stapel en kijken naar de fout in de gewenste editor, zoals Kladblok.

* Als de fout van is **SyncRulesEngine**, en vervolgens de call stack informatie eerst een lijst met alle kenmerken voor het object heeft. Schuif omlaag totdat u de kop ziet **InnerException = >**.  
  ![Sync Service Manager](./media/tshoot-connect-object-not-syncing/errorinnerexception.png)  
  De regel na ziet u de fout. In de bovenstaande afbeelding wordt de fout is van een aangepaste regel-Fabrikam voor synchronisatie is gemaakt.

Als de fout zich niet voldoende informatie geven heeft, is het tijd om te kijken naar de gegevens zelf. U kunt op de koppeling met de object-id en doorgaan met het oplossen van de [connector space geïmporteerde object](#cs-import).

## <a name="connector-space-object-properties"></a>Eigenschappen van het object connectorgebied
Als u nog geen fouten gevonden in de [operations](#operations) tabblad, en vervolgens de volgende stap is het connectorobject ruimte uit Active Directory naar de metaverse en naar Azure AD volgt. In dit pad, moet u zien waar het probleem zich bevindt.

### <a name="search-for-an-object-in-the-cs"></a>Zoeken naar een object in de CS

In **Synchronization Service Manager**, klikt u op **Connectors**, selecteert u de Active Directory-Connector en **Search Connector Space**.

In **bereik**, selecteer **RDN** (als u wilt zoeken op het kenmerk CN) of **DN-naam of het anker** (als u wilt zoeken op het kenmerk distinguishedName). Voer een waarde en klik op **zoeken**.  
![Connectorgebied zoeken](./media/tshoot-connect-object-not-syncing/cssearch.png)  

Als niet mogelijk is met het object u zoekt, is het mogelijk zijn gefilterd met [filteren op basis van een domein](how-to-connect-sync-configure-filtering.md#domain-based-filtering) of [OE filteren](how-to-connect-sync-configure-filtering.md#organizational-unitbased-filtering). Lees de [filtering configureren](how-to-connect-sync-configure-filtering.md) onderwerp om te controleren of het filteren is geconfigureerd zoals verwacht.

Zoeken in een ander handig is om te selecteren van de Azure AD-Connector in **bereik** Selecteer **in behandeling zijnde importeren**, en selecteer de **toevoegen** selectievakje. Deze zoekopdracht kunt u alle gesynchroniseerde objecten in Azure AD die niet kan gekoppeld aan een on-premises-object worden.  
![Connector Space zoeken zwevende](./media/tshoot-connect-object-not-syncing/cssearchorphan.png)  
Deze objecten zijn gemaakt door een andere synchronisatie-engine of een synchronisatie-engine met een andere filters gebruiken om configuratie. In deze weergave wordt een lijst met **zwevende** objecten niet meer wordt beheerd. U moet deze lijst bekijken en kunt u verwijderen van deze objecten met behulp van de [Azure AD PowerShell](https://aka.ms/aadposh) cmdlets.

### <a name="cs-import"></a>CS importeren
Wanneer u een object cs opent, zijn er meerdere tabbladen aan de bovenkant. De **importeren** tabblad bevat de gegevens die tijdelijk worden opgeslagen na importeren.  
![CS-object](./media/tshoot-connect-object-not-syncing/csobject.png)    
De **oude waarde** laat zien wat die momenteel wordt opgeslagen in Connect en de **nieuwe waarde** wat is ontvangen van het bronsysteem en is nog niet toegepast. Als er een fout op het object, worden klikt u vervolgens wijzigingen niet verwerkt.

**Fout**  
![CS-object](./media/tshoot-connect-object-not-syncing/cssyncerror.png)  
De **synchronisatiefout** tabblad is alleen zichtbaar als er een probleem met het object is. Zie voor meer informatie, [synchronisatiefouten oplossen](#troubleshoot-errors-in-operations-tab).

### <a name="cs-lineage"></a>CS afkomst
Het tabblad afkomst laat zien hoe de connector space-object is gerelateerd aan het metaverse-object. U kunt zien wanneer een wijziging in de Connector laatst worden geïmporteerd uit de verbonden systeem en welke regels toegepast voor het vullen van de gegevens in de metaverse.  
![CS afkomst](./media/tshoot-connect-object-not-syncing/cslineage.png)  
In de **actie** kolom, kunt u zien er is een **inkomend** synchronisatieregel met de actie **inrichten**. Die aangeeft, zolang deze connectorobject ruimte aanwezig is, wordt het metaverse-object blijft. Als de lijst van synchronisatieregels in plaats daarvan ziet u een synchronisatieregel met richting **uitgaand** en **inrichten**, betekent dit dat dit object wordt verwijderd wanneer de metaverse-object wordt verwijderd.  
![Sync Service Manager](./media/tshoot-connect-object-not-syncing/cslineageout.png)  
U kunt ook zien de **PasswordSync** kolom die het inkomende connectorgebied kan bijdragen tot het wachtwoord wijzigt omdat een synchronisatieregel de waarde heeft **waar**. Dit wachtwoord wordt verzonden naar Azure AD via de uitgaande regel.

Op het tabblad afkomst u toegang hebt tot de metaverse door te klikken op [eigenschappen van het Metaverseobject](#mv-attributes).

Aan de onderkant van alle tabbladen worden twee knoppen: **Preview-versie** en **Log**.

### <a name="preview"></a>Preview
De pagina voor Preview-versie wordt gebruikt voor het synchroniseren van één enkel object. Het is handig als u bepaalde aangepaste synchronisatieregels oplossen wilt en wilt zien van het effect van een wijziging op een enkel object. U kunt kiezen tussen **Full sync** en **Deltasynchronisatie**. U kunt ook selecteren tussen **Preview genereren**, die alleen de wijziging houdt in het geheugen en **doorvoeren Preview**, die de metaverse bijgewerkt en alle wijzigingen in de doel-connectorspaces fasen.  
![Sync Service Manager](./media/tshoot-connect-object-not-syncing/preview.png)  
U kunt het object en welke regel wordt toegepast voor een bepaalde kenmerkstroom inspecteren.  
![Sync Service Manager](./media/tshoot-connect-object-not-syncing/previewresult.png)

### <a name="log"></a>Logboek
De pagina wordt gebruikt om te zien van de synchronisatiestatus van wachtwoord en de geschiedenis. Zie voor meer informatie, [wachtwoord-hashsynchronisatie oplossen](tshoot-connect-password-hash-synchronization.md).

## <a name="metaverse-object-properties"></a>Eigenschappen van het Metaverseobject
Is het doorgaans beter om te beginnen met zoeken van de bron van Active Directory-connectorgebied. Maar u kunt ook beginnen met zoeken in de metaverse.

### <a name="search-for-an-object-in-the-mv"></a>Zoeken naar een object in de MV
In **Synchronization Service Manager**, klikt u op **Metaverse zoeken**. Maak een query die u weet dat de gebruiker is gevonden. U kunt zoeken naar algemene kenmerken, zoals accountName (sAMAccountName) en userPrincipalName. Zie voor meer informatie, [Metaverse zoeken](how-to-connect-sync-service-manager-ui-mvsearch.md).
![Sync Service Manager](./media/tshoot-connect-object-not-syncing/mvsearch.png)  

In de **zoekresultaten** venster, klikt u op het object.

Als u het object niet heeft gevonden, is klikt u vervolgens het niet nog bereikt de metaverse. Doorgaan met zoeken naar het object in de **Active Directory** [connectorgebied](#connector-space-object-properties). Als u het object in de **Active Directory** connectorgebied controleert, kan er een fout van synchronisatie dat wordt geblokkeerd door het object uit naar de metaverse binnenkort of er mogelijk een synchronisatieregel bereikfilter toegepast.

### <a name="object-not-found-in-the-mv"></a>Object is niet gevonden in de MV
Als het object verkeert in de **Active Directory** CS, maar niet aanwezig in de MV vervolgens bereikfilter wordt toegepast. 

* Om te kijken naar de bereikfilter gaat u naar het menu bureaubladtoepassing en klik op **Synchronization Rules Editor**. Filter de regels van toepassing het object door het onderstaande filter aan te passen.

  ![Inkomende synchronisatie regels zoeken](./media/tshoot-connect-object-not-syncing/syncrulessearch.png)

* Elke regel bekijken in de lijst hierboven en controleer de **Scoping filter**. In de onderstaande bereikfilter, als de **isCriticalSystemObject** waarde is null of onwaar of leeg zijn en het is binnen het bereik.

  ![Inkomende synchronisatie regels zoeken](./media/tshoot-connect-object-not-syncing/scopingfilter.png)

* Ga naar de [CS importeren](#cs-import) lijst en controleer welke filter wordt geblokkeerd door het verplaatsen naar de MV-object van het kenmerk. Die elkaar de **Connectorgebied** kenmerk lijst met alleen niet-null en niet-lege kenmerken wordt weergegeven. Bijvoorbeeld, als **isCriticalSystemObject** wordt niet weergegeven in de lijst en vervolgens dit betekent dat de waarde van dit kenmerk null of leeg zijn is.

### <a name="object-not-found-in-the-aad-cs"></a>Object is niet gevonden in de AAD-CS
Als het object is niet aanwezig in de **Connectorgebied** van **Azure Active Directory**. Echter aanwezig in de MV, zoek vervolgens naar het filter Scoping van de **uitgaand** regels van de bijbehorende **Connectorgebied** en controleren als het object is uitgefilterd, vanwege de [MV kenmerken](#mv-attributes) niet voldoen aan de criteria.

* Als u wilt de uitgaand bereikfilter bekijkt, selecteert u de geldende voorschriften voor het object door het onderstaande filter aan te passen. Elke regel weergeven en kijken naar de bijbehorende [MV-kenmerk](#mv-attributes) waarde.

  ![Uitgaande Synchroniztion regels zoeken](./media/tshoot-connect-object-not-syncing/outboundfilter.png)


### <a name="mv-attributes"></a>MV-kenmerken
Op het tabblad kenmerken, ziet u de waarden en welke Connector dit bijgedragen.  
![Synchronisatie van Service Manager](./media/tshoot-connect-object-not-syncing/mvobject.png)  

Als een object niet kan worden gesynchroniseerd, klikt u vervolgens bekijkt u de volgende kenmerken in de metaverse:
- Is het kenmerk **cloudFiltered** presenteren en ingesteld op **waar**? Als deze is, wordt deze is gefilterd op basis van de stappen in [kenmerk filteren op basis van](how-to-connect-sync-configure-filtering.md#attribute-based-filtering).
- Is het kenmerk **sourceAnchor** aanwezig zijn? Als dat niet het geval is, hebt u een topologie met account-resource forest? Als een object wordt geïdentificeerd als een gekoppeld postvak (het kenmerk **msExchRecipientTypeDetails** heeft de waarde 2), en vervolgens het sourceAnchor is bijgedragen door het forest met een ingeschakelde Active Directory-account. Zorg ervoor dat het hoofdaccount is geïmporteerd en correct worden gesynchroniseerd. Het hoofdaccount moet worden vermeld in de [connectors](#mv-connectors) voor het object.

### <a name="mv-connectors"></a>MV-Connectors
Het tabblad Connectors bevat alle connectorspaces waarvoor een weergave van het object.  
![Synchronisatie van Service Manager](./media/tshoot-connect-object-not-syncing/mvconnectors.png)  
U hebt een connector voor:

- In wordt elk Active Directory-forest de gebruiker aangegeven. Deze weergave kunt opnemen foreignSecurityPrincipals en neem contact op met objecten.
- Een connector in Azure AD.

Als u de Azure AD-connector ontbreekt, lees vervolgens [MV kenmerken](#mv-attributes) om te controleren of de criteria voor ingericht met Azure AD.

Op dit tabblad kunt u om te navigeren naar de [ruimte connectorobject](#connector-space-object-properties). Selecteer een rij en klikt u op **eigenschappen**.

## <a name="next-steps"></a>Volgende stappen
- [Azure AD Connect-synchronisatie](how-to-connect-sync-whatis.md).
- [Wat is hybride identiteit? ](whatis-hybrid-identity.md).
