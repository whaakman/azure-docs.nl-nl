---
title: 'Azure AD Connect-synchronisatie: een configuratiewijziging in Azure AD Connect-synchronisatie wijzigen | Microsoft Docs'
description: Leert u hoe u een wijziging aanbrengt in de configuratie in Azure AD Connect-synchronisatie.
services: active-directory
documentationcenter: 
author: andkjell
manager: femila
editor: 
ms.assetid: 7b9df836-e8a5-4228-97da-2faec9238b31
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: billmath
ms.openlocfilehash: 63a7ae9d39e1a74294637172efd607ee41b2d69b
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/03/2017
---
# <a name="azure-ad-connect-sync-how-to-make-a-change-to-the-default-configuration"></a>Azure AD Connect-synchronisatie: hoe een wijziging aanbrengt in de standaardconfiguratie
Het doel van dit onderwerp is om te zien hoe u wijzigingen aanbrengen in de standaardconfiguratie in Azure AD Connect-synchronisatie. Het bevat stappen voor enkele algemene scenario's. Met deze kennis moet u enkele eenvoudige wijzigingen aanbrengen in uw eigen configuratie op basis van uw eigen bedrijfsregels.

## <a name="synchronization-rules-editor"></a>Synchronisatie regeleditor
De regeleditor synchronisatie wordt gebruikt om te bekijken en wijzigen van de standaardconfiguratie. U kunt deze vinden in het Menu Start onder de **Azure AD Connect** groep.  
![Met de regeleditor voor synchronisatie van het Menu Start](./media/active-directory-aadconnectsync-change-the-configuration/startmenu2.png)

Als u het opent, ziet u de standaardregels voor out-of-box.

![Synchronisatie regeleditor](./media/active-directory-aadconnectsync-change-the-configuration/sre2.png)

### <a name="navigating-in-the-editor"></a>Navigeren in de editor
De vervolgkeuzelijsten aan de bovenkant van de editor kunnen u snel zoeken naar een bepaalde regel. Als u zien van de regels waarin de proxyAddresses kenmerk is opgenomen wilt, zou u de vervolgkeuzelijsten wijzigen met het volgende:  
![SRE filteren](./media/active-directory-aadconnectsync-change-the-configuration/filtering.png)  
Om te herstellen voor het filteren en een nieuwe configuratie laden, drukt u op **F5** op het toetsenbord.

De rechterbovenhoek, u hebt een knop **nieuwe regel toevoegen**. Deze knop wordt gebruikt om uw eigen aangepaste regel te maken.

Aan de onderkant hebt knoppen voor fungeert voor een geselecteerde synchronisatieregel. **Bewerken** en **verwijderen** doen wat u verwacht. **Exporteren** produceert een PowerShell-script voor het opnieuw maken van de synchronisatieregel. Deze procedure kunt u een synchronisatieregel van de ene server naar de andere te verplaatsen.

## <a name="create-your-first-custom-rule"></a>Uw eerste aangepaste regel maken
De meest voorkomende wijziging is wijzigingen in de kenmerkstromen. De gegevens in de bronmap mogelijk niet zoals in Azure AD. In het voorbeeld in deze sectie die u wilt controleren of de opgegeven naam van een gebruiker zich altijd in **juiste geval**.

### <a name="disable-the-scheduler"></a>De planner uitschakelen
De [scheduler](active-directory-aadconnectsync-feature-scheduler.md) wordt standaard elke 30 minuten uitgevoerd. U wilt controleren of dat deze niet wordt gestart wanneer u wijzigingen aanbrengen wilt en oplossen van uw nieuwe regels. Als u wilt de planner tijdelijk te deactiveren start PowerShell en voer`Set-ADSyncScheduler -SyncCycleEnabled $false`

![De planner uitschakelen](./media/active-directory-aadconnectsync-change-the-configuration/schedulerdisable.png)  

### <a name="create-the-rule"></a>De regel maken
1. Klik op **nieuwe regel toevoegen**.
2. Op de **beschrijving** pagina voert u het volgende:  
   ![Binnenkomende regel filteren](./media/active-directory-aadconnectsync-change-the-configuration/description2.png)  
   * Naam: Geeft de regel een beschrijvende naam.
   * Beschrijving: Sommige informatie zodat iemand anders wat de regel is begrijpen kan van.
   * Verbonden systeem: het systeem kan het object vinden in. In dit geval, selecteer de Active Directory-Connector.
   * Verbonden systeem/Metaverseobjecttype: Selecteer **gebruiker** en **persoon** respectievelijk.
   * Koppelingstype: Wijzig deze waarde in **Join**.
   * Prioriteit: Geef een waarde die uniek is in het systeem. Een lagere numerieke waarde geeft aan dat hogere prioriteit.
   * -Tag: Leeg laten. Alleen out-of-box regels van Microsoft, moeten dit vak gevuld met een waarde hebben.
3. Op de **Scoping filter** pagina **givenName ISNOTNULL**.  
   ![De binnenkomende regel bereikfilter](./media/active-directory-aadconnectsync-change-the-configuration/scopingfilter.png)  
   Deze sectie wordt gebruikt om te definiëren welke objecten dat de regel van toepassing moet zijn op. Als leeg wordt gelaten, zou de regel van toepassing op alle gebruikersobjecten. Maar dat zou vergaderruimten, service-accounts en andere niet-mensen gebruikersobjecten bevatten.
4. Op de **Join regels**, laat het veld leeg.
5. Op de **transformaties** pagina, wijzigt u de FlowType naar **expressie**. Selecteer het doelkenmerk **givenName**, en voer in de bron `PCase([givenName])`.
   ![Regel voor binnenkomende verbindingen transformaties](./media/active-directory-aadconnectsync-change-the-configuration/transformations.png)  
   De synchronisatie-engine is hoofdlettergevoelig zowel op naam van de functie en de naam van het kenmerk. Als u iets mis typt, ziet u een waarschuwing wanneer u de regel toevoegen. De editor kunt u opslaan en doorgaan, dus u moet de regel open en corrigeer de regel.
6. Klik op **toevoegen** de regel op te slaan.

Uw nieuwe aangepaste regel moet worden weergegeven met de andere sync-regels in het systeem.

### <a name="verify-the-change"></a>Controleer of de wijziging
Met deze nieuwe wijziging die u wilt controleren of deze werkt zoals verwacht en die niet alle fouten. Afhankelijk van het aantal objecten dat u hebt zijn er twee verschillende manieren doen in deze stap.

1. Een volledige synchronisatie worden uitgevoerd op alle objecten
2. Een voorbeeld en een volledige synchronisatie uitvoeren op een enkel object

Start **synchronisatieservice** vanuit het startmenu. De stappen in deze sectie zijn alle in dit hulpprogramma.

1. **Volledige synchronisatie van alle objecten**  
   Selecteer **Connectors** aan de bovenkant. Identificeren van de Connector die u een wijziging in de vorige sectie, in dit geval de Active Directory Domain Services aangebracht, en selecteer deze. Selecteer **uitvoeren** van acties en selecteer **volledige synchronisatie** en **OK**.
   ![Volledige synchronisatie](./media/active-directory-aadconnectsync-change-the-configuration/fullsync.png)  
   De objecten zijn nu bijgewerkt in de metaverse. Nu u om te kijken naar het object in de metaverse.
2. **Preview en een volledige synchronisatie van een enkel object**  
   Selecteer **Connectors** aan de bovenkant. Identificeren van de Connector die u een wijziging in de vorige sectie, in dit geval de Active Directory Domain Services aangebracht, en selecteer deze. Selecteer **Connectorgebied zoeken**. Gebruik scope zoeken naar een object dat u gebruiken wilt voor het testen van de wijziging. Selecteer het object en klikt u op **Preview**. Selecteer in het nieuwe scherm **doorvoeren Preview**.  
   ![Voorbeeld van doorvoeren](./media/active-directory-aadconnectsync-change-the-configuration/commitpreview.png)  
   De wijziging is nu toegewezen aan de metaverse.

**Het object in de metaverse bekijken**  
Nu u kiest een paar voorbeeld objecten om te controleren of de waarde wordt verwacht en dat de regel is toegepast. Selecteer **Metaverse zoeken** vanaf de bovenkant. Een filter dat u wilt zoeken naar de relevante objecten toevoegen. Open een object in de zoekresultaten. Bekijk de kenmerkwaarden weer en Controleer ook de **Sync regels** kolom die de regel is toegepast zoals verwacht.  
![Metaverse zoeken](./media/active-directory-aadconnectsync-change-the-configuration/mvsearch.png)  

### <a name="enable-the-scheduler"></a>De planner inschakelen
Als alles zoals verwacht, kunt u de planner opnieuw inschakelen. Voer vanuit PowerShell `Set-ADSyncScheduler -SyncCycleEnabled $true`.

## <a name="other-common-attribute-flow-changes"></a>Andere algemene wijzigingen van de kenmerk-stroom
De vorige sectie wordt beschreven hoe u wijzigingen aanbrengt aan een kenmerkstroom. In deze sectie vindt u enkele aanvullende voorbeelden. De stappen voor het maken van de synchronisatieregel wordt afgekort, maar u kunt de volledige stappen vinden in de vorige sectie.

### <a name="use-another-attribute-than-the-default"></a>Een ander kenmerk dan de standaardwaarde gebruiken
Bij Fabrikam is er een forest waarbij het lokale alfabet wordt gebruikt voor de voornaam en achternaam weergavenaam. De Latijnse tekens representatie van deze kenmerken kan worden gevonden in uitbreidingskenmerken. Tijdens het bouwen van de globale adreslijst in Azure AD en Office 365, de organisatie wil deze kenmerken worden gebruikt in plaats daarvan.

Met een standaardconfiguratie wordt een object van het lokale forest als volgt weergegeven:  
![Kenmerkstroom 1](./media/active-directory-aadconnectsync-change-the-configuration/attributeflowjp1.png)

Maakt een regel met andere kenmerkstromen, het volgende doen:

* Start **Synchronization Rule Editor** vanuit het startmenu.
* Met **inkomend** nog steeds ingeschakeld aan de linkerkant, klik op de knop **nieuwe regel toevoegen**.
* Geef de regel een naam en beschrijving. Selecteer de lokale Active Directory en de relevante objecttypen. In **koppelingstype**, selecteer **Join**. Kies voor hogere prioriteit, een getal dat niet wordt gebruikt door een andere regel. De out-of-box-regels beginnen met 100, zodat de waarde 50 kan worden gebruikt in dit voorbeeld.
  ![Kenmerkstroom 2](./media/active-directory-aadconnectsync-change-the-configuration/attributeflowjp2.png)
* Bereik leeg laten (dat wil zeggen, gelden voor alle gebruikersobjecten in het forest).
* Join regels leeg laten (dat wil zeggen, laat de out-of-box-regel joins verwerken).
* Maak de volgende stromen in transformaties:  
  ![Kenmerkstroom 3](./media/active-directory-aadconnectsync-change-the-configuration/attributeflowjp3.png)
* Klik op **toevoegen** de regel op te slaan.
* Ga naar **Synchronization Service Manager**. Op **Connectors**, selecteert u de Connector waar we de regel toegevoegd. Selecteer **uitvoeren**, en **volledige synchronisatie**. Een volledige synchronisatie wordt opnieuw berekend alle objecten van de huidige regels.

Dit is het resultaat voor hetzelfde object met deze aangepaste regel:  
![Kenmerkstroom 4](./media/active-directory-aadconnectsync-change-the-configuration/attributeflowjp4.png)

### <a name="length-of-attributes"></a>Lengte van kenmerken
Tekenreekskenmerken zijn standaard ingesteld op worden geïndexeerd en de maximale lengte is 448 tekens. Als u met tekenreekskenmerken die meer bevatten werkt mogelijk, Controleer of het volgende in de kenmerkstroom opnemen:  
`attributeName` <- `Left([attributeName],448)`

### <a name="changing-the-userprincipalsuffix"></a>De userPrincipalSuffix wijzigen
Het kenmerk userPrincipalName in Active Directory is niet altijd bekend door de gebruikers en mogelijk niet geschikt is als de aanmeldings-ID. De Azure AD Connect sync-installatiewizard kunt verzamelen van een ander kenmerk, bijvoorbeeld mail. Maar in sommige gevallen het kenmerk moet worden berekend. Het bedrijf Contoso heeft bijvoorbeeld twee Azure AD-mappen, één voor productie en één voor het testen. Ze willen dat gebruikers met hun testtenant gebruik van een andere achtervoegsel in de aanmeldings-ID.  
`userPrincipalName` <- `Word([userPrincipalName],1,"@") & "@contosotest.com"`

In deze expressie nemen alles links van de eerste @-sign (Word) en samenvoegen met een vaste tekenreeks.

### <a name="convert-a-multi-value-to-a-single-value"></a>Een met meerdere waarden converteren naar een single-waarde
Bepaalde kenmerken in Active Directory zijn meerdere waarden in het schema, zelfs als ze op zoek één waarde in Active Directory: gebruikers en Computers. Een voorbeeld is het beschrijvingskenmerk.  
`description` <- `IIF(IsNullOrEmpty([description]),NULL,Left(Trim(Item([description],1)),448))`

In deze expressie als het kenmerk een waarde heeft, krijgt het eerste item (opdracht) in het kenmerk, voorloopspaties en afsluitende spaties (Trim) verwijderen en vervolgens de eerst 448 tekens (links) blijven in de tekenreeks.

### <a name="do-not-flow-an-attribute"></a>Niet laten doorlopen voor een kenmerk
Zie voor achtergrondinformatie over het scenario voor deze sectie, [bepalen hoe het kenmerk stroom verloopt](active-directory-aadconnectsync-understanding-declarative-provisioning.md#control-the-attribute-flow-process).

Er zijn twee manieren een kenmerk niet stromen. De eerste is beschikbaar in de installatiewizard en kunt u [geselecteerde kenmerken verwijderen](active-directory-aadconnect-get-started-custom.md#azure-ad-app-and-attribute-filtering). Deze optie werkt als u nooit het kenmerk voordat hebt gesynchroniseerd. Echter, als u dit kenmerk synchroniseren en later verwijdert met deze functie hebt gestart, klikt u vervolgens de synchronisatie-engine stopt het beheren van het kenmerk en de bestaande waarden blijven in Azure AD.

Als u wilt verwijderen van de waarde van een kenmerk en zorg ervoor dat deze wordt niet in de toekomst uitgebreid, moet u een aangepaste regel maken.

Bij Fabrikam, hebben we gerealiseerde dat sommige van de kenmerken die wordt gesynchroniseerd met de cloud niet er worden moeten. We willen zeker weten dat deze kenmerken worden verwijderd uit Azure AD.  
![Ongeldige Uitbreidingskenmerken](./media/active-directory-aadconnectsync-change-the-configuration/badextensionattribute.png)

* Maak een nieuwe regel voor binnenkomende synchronisatie en de beschrijving vullen ![beschrijvingen](./media/active-directory-aadconnectsync-change-the-configuration/syncruledescription.png)
* Maken van kenmerkstromen van het type **expressie** en met de bron **AuthoritativeNull**. De letterlijke waarde **AuthoritativeNull** geeft aan dat de waarde mag niet leeg zijn in de MV zelfs als een regel voor synchronisatie van lagere prioriteit probeert te vullen van de waarde.
  ![Transformatie voor Uitbreidingskenmerken](./media/active-directory-aadconnectsync-change-the-configuration/syncruletransformations.png)
* Sla de Synchronisatieregel. Start **synchronisatieservice**, vinden de Connector, selecteert u **uitvoeren**, en **volledige synchronisatie**. Deze stap opnieuw alle kenmerkstromen berekend.
* Controleer of de gewenste wijzigingen zijn geëxporteerd door te zoeken naar het connectorgebied overgebracht.
  ![Gefaseerde verwijderen](./media/active-directory-aadconnectsync-change-the-configuration/deletetobeexported.png)

## <a name="create-rules-with-powershell"></a>Regels maken met PowerShell
Wanneer u slechts enkele wijzigingen aanbrengen, werkt goed samen met het gebruik van de regel voor synchronisatie editor. Als u wijzigingen aanbrengen veel wilt, kan een betere optie zijn door PowerShell. Sommige geavanceerde functies zijn alleen beschikbaar met PowerShell.

### <a name="get-the-powershell-script-for-an-out-of-box-rule"></a>Ophalen van het PowerShell-script voor een out-of-box-regel
Om te zien van de PowerShell-script dat een out-of-box-regel gemaakt, selecteert u de regel met de regeleditor synchronisatie en klikt u op **exporteren**. Deze actie geeft u de PowerShell-script dat de regel hebt gemaakt.

### <a name="advanced-precedence"></a>Geavanceerde prioriteit
De regels voor out-of-box synchronisatie starten met een prioriteitswaarde 100. Als u veel forests hebt en u wijzigingen wilt aanbrengen veel aangepaste, vervolgens regels 99 synchronisatie mogelijk niet voldoende.

U kunt de opdracht geven de synchronisatie-Engine die u aanvullende regels invoegen vóór de out-of-box-regels wilt. Als u dit gedrag, de volgende stappen uit:

1. Markeer de eerste out-of-box-synchronisatieregel (met deze regel wordt de **In van het AD-gebruiker toevoegen**) in de regeleditor synchronisatie en selecteer **exporteren**. Kopieer de SR-id-waarde.  
![PowerShell voor wijziging](./media/active-directory-aadconnectsync-change-the-configuration/powershell1.png)  
2. Maak de nieuwe synchronisatieregel. U kunt de regeleditor sync gebruiken om deze te maken. De regel naar een PowerShell-script exporteren.
3. In de eigenschap **PrecedenceBefore**, voeg de id-waarde van de out-of-box-regel. Stel de **voorrang** naar **0**. Zorg ervoor dat het kenmerk-id is uniek en u niet opnieuw gebruiken van een GUID van een andere regel. Controleer ook of de **ImmutableTag** eigenschap is niet ingesteld; deze eigenschap mag alleen worden ingesteld voor een out-of-box-regel. Sla het PowerShell-script en voer deze uit. Het resultaat is dat uw aangepaste regel de prioriteitswaarde van 100 is toegewezen en alle andere out-of-box-regels worden verhoogd.  
![PowerShell na wijziging](./media/active-directory-aadconnectsync-change-the-configuration/powershell2.png)  

U kunt hebben veel aangepaste synchronisatie regels met behulp van dezelfde **PrecedenceBefore** waarde indien nodig.


## <a name="enable-synchronization-of-preferreddatalocation"></a>Synchronisatie van PreferredDataLocation inschakelen
Azure AD Connect ondersteunt synchronisatie van de **PreferredDataLocation** kenmerk voor **gebruiker** objecten in versie 1.1.524.0 en na. Meer specifiek, zijn de volgende wijzigingen geïntroduceerd:

* Het schema van het objecttype **gebruiker** in de Azure AD-Connector is uitgebreid met PreferredDataLocation kenmerk is van het type tekenreeks en één waarde.

* Het schema van het objecttype **persoon** in de Metaverse is uitgebreid met PreferredDataLocation kenmerk is van het type tekenreeks en één waarde.

Standaard is het kenmerk PreferredDataLocation niet ingeschakeld voor synchronisatie omdat er geen overeenkomend PreferredDataLocation-kenmerk in de lokale Active Directory. U moet handmatig synchronisatie inschakelen.

> [!IMPORTANT]
> Azure AD kan op dit moment is dat het kenmerk PreferredDataLocation op gesynchroniseerde gebruikersobjecten en cloud gebruiker objecten rechtstreeks worden geconfigureerd met behulp van Azure AD PowerShell. Wanneer u de synchronisatie van het kenmerk PreferredDataLocation hebt ingeschakeld, moet u stoppen met Azure AD PowerShell voor het configureren van het kenmerk op **gesynchroniseerd gebruikersobjecten** als Azure AD Connect overschrijft toe op basis van de bron-kenmerkwaarden in de lokale Active Directory.

> [!IMPORTANT]
> Op 1 September 2017 Azure AD wordt niet langer het kenmerk PreferredDataLocation op toestaan **gesynchroniseerd gebruikersobjecten** rechtstreeks worden geconfigureerd met behulp van Azure AD PowerShell. Als u wilt configureren PreferredLocation-kenmerk op gesynchroniseerde gebruikersobjecten, moet u Azure AD Connect.

Voordat u de synchronisatie van het kenmerk PreferredDataLocation inschakelt, moet u het volgende doen:

 * Bepaal eerst welke lokale Active Directory-kenmerk moet worden gebruikt als het bronkenmerk. Deze moet van het type **tekenreeks** en **één waarde**.

 * Als u het kenmerk PreferredDataLocation eerder hebt geconfigureerd op bestaande gebruikersobjecten gesynchroniseerd in Azure AD dat gebruikmaakt van Azure AD PowerShell, moet u **backport** de kenmerkwaarden voor de bijbehorende gebruikersobjecten in de lokale Active Directory.
 
    > [!IMPORTANT]
    > Als u niet backport de kenmerkwaarden voor de bijbehorende gebruikersobjecten in de lokale Active Directory dit, wordt de bestaande kenmerkwaarden in Azure AD Connect verwijderd in Azure AD wanneer synchronisatie voor het kenmerk PreferredDataLocation is ingeschakeld.

 * Het wordt aanbevolen configureren van het bronkenmerk op ten minste twee lokale AD-gebruiker objecten nu, die kan worden gebruikt voor verificatie later.
 
De stappen voor het inschakelen van synchronisatie van het kenmerk PreferredDataLocation kunnen worden samengevat als:

1. Schakel de sync scheduler uit en controleer of dat er vindt geen synchronisatie uitgevoerd

2. Het bronkenmerk toevoegen aan de on-premises AD-Connector schema

3. PreferredDataLocation toevoegen aan het schema van de Azure AD-Connector

4. Maken van een synchronisatieregel voor binnenkomende om de stroom van de waarde van het kenmerk van de lokale Active Directory

5. Een uitgaande synchronisatieregel maken om te laten doorlopen van de waarde van het kenmerk naar Azure AD

6. Volledige synchronisatiecyclus uitvoeren

7. Sync scheduler inschakelen

> [!NOTE]
> De rest van deze sectie bevat informatie over deze stappen in de details. Ze worden beschreven in de context van een Azure AD-implementatie met één forest topologie en zonder aangepaste synchronisatieregels. Als u een topologie met meerdere forests hebt, aangepaste synchronisatieregels geconfigureerd of een tijdelijke server hebt, moet u de stappen overeenkomstig aanpassen.

### <a name="step-1-disable-sync-scheduler-and-verify-there-is-no-synchronization-in-progress"></a>Stap 1: Schakel de sync scheduler uit en controleer of dat er vindt geen synchronisatie uitgevoerd
Zorg ervoor dat er geen synchronisatie plaats terwijl u bent in het midden van het bijwerken van synchronisatieregels om onbedoelde wijzigingen wordt geëxporteerd naar Azure AD te voorkomen. De ingebouwde sync scheduler uitschakelen:

 1. Start PowerShell-sessie op de Azure AD Connect-server.

 2. Geplande synchronisatie uitschakelen door de cmdlet uit te voeren:`Set-ADSyncScheduler -SyncCycleEnabled $false`
 
 3. Start de **Synchronization Service Manager** gaat u naar START → Synchronization Service.
 
 4. Ga naar de **Operations** tabblad en er is geen bewerking met de status bevestigen *'wordt uitgevoerd.'*

![Controleer de Synchronization Service Manager - er zijn geen bewerkingen uitgevoerd](./media/active-directory-aadconnectsync-change-the-configuration/preferredDataLocation-step1.png)

### <a name="step-2-add-the-source-attribute-to-the-on-premises-ad-connector-schema"></a>Stap 2: Het bronkenmerk toevoegen aan de on-premises AD-Connector schema
Niet alle AD-kenmerken worden geïmporteerd in de on-premises AD Connectorgebied overgebracht. Het bronkenmerk toevoegen aan de lijst van de geïmporteerde kenmerken:

 1. Ga naar de **Connectors** tabblad Synchronization Service Manager.
 
 2. Met de rechtermuisknop op de **on-premises AD-Connector** en selecteer **eigenschappen**.
 
 3. In het pop-updialoogvenster, gaat u naar de **kenmerken selecteren** tabblad.
 
 4. Zorg ervoor dat het bronkenmerk is ingeschakeld in de lijst met kenmerken.
 
 5. Klik op **OK** om op te slaan.

![Bronkenmerk toevoegen met on-premises AD-Connector schema](./media/active-directory-aadconnectsync-change-the-configuration/preferredDataLocation-step2.png)

### <a name="step-3-add-preferreddatalocation-to-the-azure-ad-connector-schema"></a>Stap 3: PreferredDataLocation toevoegen aan het schema van de Azure AD-Connector
Standaard wordt het kenmerk PreferredDataLocation niet geïmporteerd in de Azure AD Connect-ruimte. Het kenmerk PreferredDataLocation toevoegen aan de lijst met geïmporteerde kenmerken:

 1. Ga naar de **Connectors** tabblad Synchronization Service Manager.

 2. Met de rechtermuisknop op de **Azure AD-Connector** en selecteer **eigenschappen**.

 3. In het pop-updialoogvenster, gaat u naar de **kenmerken selecteren** tabblad.

 4. Zorg ervoor dat het kenmerk PreferredDataLocation is ingeschakeld in de lijst met kenmerken.

 5. Klik op **OK** om op te slaan.

![Bronkenmerk toevoegen aan Azure AD-Connector schema](./media/active-directory-aadconnectsync-change-the-configuration/preferredDataLocation-step3.png)

### <a name="step-4-create-an-inbound-synchronization-rule-to-flow-the-attribute-value-from-on-premises-active-directory"></a>Stap 4: Een synchronisatieregel voor binnenkomende om de stroom van de waarde van het kenmerk van de lokale Active Directory maken
De synchronisatieregel voor binnenkomende wordt toegestaan de waarde van het kenmerk vanuit het bronkenmerk van de lokale Active Directory naar de Metaverse:

1. Start de **synchronisatie regeleditor** door naar START → synchronisatie regeleditor te gaan.

2. Het zoekfilter ingesteld **richting** worden **inkomend**.

3. Klik op **nieuwe regel toevoegen** om te maken van een nieuwe regel voor binnenkomende verbindingen.

4. Onder de **beschrijving** tabblad, bieden de volgende configuratie:
 
    | Kenmerk | Waarde | Details |
    | --- | --- | --- |
    | Naam | *Geef een naam* | Bijvoorbeeld: *' In uit Active Directory-gebruiker PreferredDataLocation '* |
    | Beschrijving | *Geef een beschrijving* |  |
    | Verbonden systeem | *Kies de on-premises AD-connector* |  |
    | Verbonden systeem objecttype | **Gebruiker** |  |
    | Metaverse-objecttype | **Persoon** |  |
    | Koppelingstype | **Koppelen** |  |
    | Prioriteit | *Kies een getal tussen 1-99* | 1-99 is gereserveerd voor aangepaste synchronisatie regels. Een waarde die wordt gebruikt door andere synchronisatieregels niet opgenomen. |

5. Ga naar de **Scoping filter** tabblad en voeg een **één filter bereikgroep met de volgende component**:
 
    | Kenmerk | Operator | Waarde |
    | --- | --- | --- |
    | adminDescription | NOTSTARTWITH | Gebruiker\_ | 
 
    Bereik filter bepaalt welke on-premises AD-objecten dat deze synchronisatieregel voor binnenkomende wordt toegepast op. In dit voorbeeld gebruiken we hetzelfde bereik filter gebruikt als *' In uit Active Directory-gebruiker algemene '* OOB-synchronisatieregel, waardoor de synchronisatieregel wordt toegepast op gebruikersobjecten via Azure AD-gebruiker Write-back-functie is gemaakt. Mogelijk moet u het bereik filter op basis van uw Azure AD Connect-implementatie aanpassen.

6. Ga naar de **transformatie tabblad** en implementeren van de volgende transformatieregel:
 
    | Stroomtype | Doelkenmerk | Bron | Eenmaal toepassen | Type samenvoeging |
    | --- | --- | --- | --- | --- |
    | Directe | PreferredDataLocation | Kies het bronkenmerk | Dit selectievakje is uitgeschakeld | Update |

7. Klik op **toevoegen** om de binnenkomende regel te maken.

![Synchronisatieregel voor binnenkomende maken](./media/active-directory-aadconnectsync-change-the-configuration/preferredDataLocation-step4.png)

### <a name="step-5-create-an-outbound-synchronization-rule-to-flow-the-attribute-value-to-azure-ad"></a>Stap 5: Maak een synchronisatieregel voor uitgaande om de stroom van de waarde van het kenmerk naar Azure AD
De uitgaande synchronisatieregel wordt toegestaan de waarde van het kenmerk stromen van Metaverse met het kenmerk PreferredDataLocation in Azure AD:

1. Ga naar de **synchronisatieregels** Editor.

2. Het zoekfilter ingesteld **richting** worden **uitgaand**.

3. Klik op **nieuwe regel toevoegen** knop.

4. Onder de **beschrijving** tabblad, bieden de volgende configuratie:

    | Kenmerk | Waarde | Details |
    | --- | --- | --- |
    | Naam | *Geef een naam* | Bijvoorbeeld 'uit naar AAD – gebruiker PreferredDataLocation' |
    | Beschrijving | *Geef een beschrijving* |
    | Verbonden systeem | *Selecteer de AAD-connector* |
    | Verbonden systeem objecttype | Gebruiker ||
    | Metaverse-objecttype | **Persoon** ||
    | Koppelingstype | **Koppelen** ||
    | Prioriteit | *Kies een getal tussen 1-99* | 1-99 is gereserveerd voor aangepaste synchronisatie regels. YDo niet een waarde die wordt gebruikt door andere synchronisatieregels opgenomen. |

5. Ga naar de **Scoping filter** tabblad en voeg een **één filter bereikgroep met twee componenten**:
 
    | Kenmerk | Operator | Waarde |
    | --- | --- | --- |
    | Bronobjecttype | GELIJK ZIJN AAN | Gebruiker |
    | cloudMastered | NOTEQUAL | True |

    Bereik filter bepaalt welke deze uitgaande synchronisatieregel wordt toegepast op Azure AD-objecten. In dit voorbeeld gebruiken we het filter voor hetzelfde bereik uit 'Out naar AD-gebruikers-id' OOB-synchronisatieregel. Dit voorkomt dat de synchronisatieregel wordt toegepast op gebruikers-objecten die niet zijn gesynchroniseerd vanuit de lokale Active Directory. Mogelijk moet u het bereik filter op basis van uw Azure AD Connect-implementatie aanpassen.
    
6. Ga naar de **transformatie** tabblad en de volgende transformatieregel implementeren:

    | Stroomtype | Doelkenmerk | Bron | Eenmaal toepassen | Type samenvoeging |
    | --- | --- | --- | --- | --- |
    | Directe | PreferredDataLocation | PreferredDataLocation | Dit selectievakje is uitgeschakeld | Update |

7. Sluit **toevoegen** de uitgaande regel maken.

![Uitgaande synchronisatieregel maken](./media/active-directory-aadconnectsync-change-the-configuration/preferredDataLocation-step5.png)

### <a name="step-6-run-full-synchronization-cycle"></a>Stap 6: De volledige synchronisatie uitvoeren cyclus
In het algemeen volledige synchronisatiecyclus is vereist, omdat we nieuwe kenmerken hebt toegevoegd aan zowel de advertentie en Azure AD-Connector schema en geïntroduceerd aangepaste synchronisatieregels. Het is raadzaam om de wijzigingen te controleren voordat deze naar Azure AD worden geëxporteerd. U kunt de volgende stappen gebruiken om te controleren of de wijzigingen tijdens de stappen die gezamenlijk een volledige synchronisatiecyclus handmatig uit te voeren. 

1. Voer **volledige import** stap op het **on-premises AD-Connector**:

   1. Ga naar de **Operations** tabblad Synchronization Service Manager.

   2. Met de rechtermuisknop op de **on-premises AD-Connector** en selecteer **uitvoeren...**

   3. Selecteer in het pop-updialoogvenster **volledige Import** en klik op **OK**.
    
   4. Wacht u totdat de bewerking is voltooid.

    > [!NOTE]
    > U kunt de volledige Import overslaan op de on-premises AD-Connector als het bronkenmerk al in de lijst met opgenomen is kenmerken geïmporteerd. Met andere woorden, u hebt geen wijzigt tijdens [stap 2: het bronkenmerk toevoegen aan de on-premises AD-Connector schema](#step-2-add-the-source-attribute-to-the-on-premises-ad-connector-schema).

2. Voer **volledige import** stap op het **Azure AD-Connector**:

   1. Met de rechtermuisknop op de **Azure AD-Connector** en selecteer **uitvoeren...**

   2. Selecteer in het pop-updialoogvenster **volledige Import** en klik op **OK**.
   
   3. Wacht u totdat de bewerking is voltooid.

3. Controleer of de wijzigingen in synchronisatie-regel op een bestaande gebruikersobject:

Het bronkenmerk van lokale Active Directory en PreferredDataLocation van Azure AD in de respectieve ruimte voor de connector zijn geïmporteerd. Voordat u doorgaat met de stap van de volledige synchronisatie wordt aanbevolen dat u wilt een **Preview** object op een bestaande gebruiker in de on-premises AD Connectorgebied overgebracht. Het object dat u verzameld, moet het bronkenmerk ingevuld hebben. Een geslaagde **Preview** is een goede indicatie dat u de synchronisatie hebt geconfigureerd correct regels met de PreferredDataLocation ingevuld in de Metaverse. Voor informatie over hoe u een **Preview**, Raadpleeg het gedeelte [Controleer of de wijziging](#verify-the-change).

4. Voer **volledige synchronisatie** stap op het **on-premises AD-Connector**:

   1. Met de rechtermuisknop op de **on-premises AD-Connector** en selecteer **uitvoeren...**
  
   2. Selecteer in het pop-updialoogvenster **volledige synchronisatie** en klik op **OK**.
   
   3. Wacht u totdat de bewerking is voltooid.

5. Controleer of **in behandeling zijnde uitvoer** naar Azure AD:

   1. Met de rechtermuisknop op de **Azure AD-Connector** en selecteer **Connectorgebied zoeken**.

   2. In het pop-dialoogvenster Connectorgebied zoeken:

      1. Stel **bereik** naar **in behandeling zijnde uitvoer**.
      
      2. Controleer alle drie selectievakjes, met inbegrip van **toevoegen, wijzigen en verwijderen**.
      
      3. Klik op de **Search** knop voor de lijst met objecten met wijzigingen worden geëxporteerd. Dubbelklik op het object voor het onderzoeken van de wijzigingen voor een bepaald object.
      
      4. Controleer of dat de wijzigingen worden verwacht.

6. Voer **exporteren** stap op het **Azure AD-Connector**
      
   1. Met de rechtermuisknop op de **Azure AD-Connector** en selecteer **uitvoeren...**
   
   2. Selecteer in het pop-upvenster Connector uitvoeren **exporteren** en klik op **OK**.
   
   3. Wacht u totdat de Export naar Azure AD te voltooien.

> [!NOTE]
> U merkt u wellicht dat de stappen geen de volledige synchronisatie stap en exporteren van de Azure AD-Connector bevatten. De stappen zijn niet vereist omdat de kenmerkwaarden van de lokale Active Directory naar Azure AD alleen stromen.

### <a name="step-7-re-enable-sync-scheduler"></a>Stap 7: Sync scheduler opnieuw inschakelen
De ingebouwde sync scheduler opnieuw inschakelen:

1. Start PowerShell-sessie.

2. Geplande synchronisatie opnieuw inschakelen door de cmdlet uit te voeren:`Set-ADSyncScheduler -SyncCycleEnabled $true`



## <a name="next-steps"></a>Volgende stappen
* Meer informatie over de configuratiemodel in [Understanding declaratieve inrichting](active-directory-aadconnectsync-understanding-declarative-provisioning.md).
* Meer informatie over de expressietaal in [Understanding declaratieve inrichting expressies](active-directory-aadconnectsync-understanding-declarative-provisioning-expressions.md).

**Overzichtsonderwerpen**

* [Azure AD Connect-synchronisatie: inzicht en synchronisatie aanpassen](active-directory-aadconnectsync-whatis.md)
* [Uw on-premises identiteiten integreren met Azure Active Directory](active-directory-aadconnect.md)
