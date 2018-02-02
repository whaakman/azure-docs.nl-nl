---
title: 'Azure AD Connect-synchronisatie: een configuratiewijziging in Azure AD Connect-synchronisatie wijzigen | Microsoft Docs'
description: Leert u hoe u een wijziging aanbrengt in de configuratie in Azure AD Connect-synchronisatie.
services: active-directory
documentationcenter: 
author: billmath
manager: mtillman
editor: 
ms.assetid: 7b9df836-e8a5-4228-97da-2faec9238b31
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/13/2018
ms.author: billmath
ms.openlocfilehash: e97d3e3e35ee87864c5d38e75e08e62088e25fdb
ms.sourcegitcommit: 9d317dabf4a5cca13308c50a10349af0e72e1b7e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/01/2018
---
# <a name="azure-ad-connect-sync-make-a-change-to-the-default-configuration"></a>Azure AD Connect-synchronisatie: een wijziging aanbrengt in de standaardconfiguratie
Het doel van dit artikel is om te zien hoe u wijzigingen aanbrengen in de standaardconfiguratie in Azure Active Directory (Azure AD) Connect-synchronisatie. Het bevat stappen voor enkele algemene scenario's. Met deze kennis moet u het volgende kunnen eenvoudige wijzigingen aanbrengen in uw eigen configuratie op basis van uw eigen bedrijfsregels.

## <a name="synchronization-rules-editor"></a>Synchronisatie regeleditor
De regeleditor synchronisatie wordt gebruikt om te bekijken en wijzigen van de standaardconfiguratie. U kunt vinden op de **Start** menu onder de **Azure AD Connect** groep.  
![Met de regeleditor voor synchronisatie van het menu Start](./media/active-directory-aadconnectsync-change-the-configuration/startmenu2.png)

Als u de editor opent, ziet u de standaardregels voor out-of-box.

![Synchronisatie regeleditor](./media/active-directory-aadconnectsync-change-the-configuration/sre2.png)

### <a name="navigating-in-the-editor"></a>Navigeren in de editor
Met de vervolgkeuzelijsten aan de bovenkant van de editor, kunt u snel vinden een specifieke regel. Als u zien van de regels waarin de proxyAddresses kenmerk is opgenomen wilt, kunt u bijvoorbeeld de vervolgkeuzelijsten wijzigen met het volgende:  
![SRE filteren](./media/active-directory-aadconnectsync-change-the-configuration/filtering.png)  
Om te herstellen voor het filteren en een nieuwe configuratie laden, druk op F5 op het toetsenbord.

Op de rechterbovenhoek is de **nieuwe regel toevoegen** knop. Deze knop kunt u uw eigen aangepaste regel maken.

Aan de onderkant zijn knoppen voor fungeert voor een geselecteerde synchronisatieregel. **Bewerken** en **verwijderen** doen wat u verwacht. **Exporteren** produceert een PowerShell-script voor het opnieuw maken van de synchronisatieregel. Met deze procedure kunt u een synchronisatieregel van de ene server verplaatsen naar een andere.

## <a name="create-your-first-custom-rule"></a>Uw eerste aangepaste regel maken
De meest voorkomende wijzigingen worden naar de kenmerkstromen. De gegevens in de bronmap mogelijk niet hetzelfde als in Azure AD. Controleer of de opgegeven naam van een gebruiker zich altijd in in het voorbeeld in deze sectie *juiste geval*.

### <a name="disable-the-scheduler"></a>De planner uitschakelen
De [scheduler](active-directory-aadconnectsync-feature-scheduler.md) wordt standaard elke 30 minuten uitgevoerd. Zorg ervoor dat het niet starten tijdens het aanbrengen van wijzigingen en het oplossen van uw nieuwe regels. Als u wilt de planner tijdelijk te deactiveren start PowerShell en voer `Set-ADSyncScheduler -SyncCycleEnabled $false`.

![De planner uitschakelen](./media/active-directory-aadconnectsync-change-the-configuration/schedulerdisable.png)  

### <a name="create-the-rule"></a>De regel maken
1. Klik op **nieuwe regel toevoegen**.
2. Op de **beschrijving** pagina, voert u het volgende:  
   ![Binnenkomende regel filteren](./media/active-directory-aadconnectsync-change-the-configuration/description2.png)  
   * **Naam**: een beschrijvende naam opgeven voor de regel.
   * **Beschrijving**: Sommige informatie geven, zodat iemand anders wat de regel is begrijpen kan van.
   * **Verbonden systeem**: dit is het systeem waarin het object kan worden gevonden. In dit geval selecteren **Active Directory-Connector**.
   * **Verbonden systeem/Metaverseobjecttype**: Selecteer **gebruiker** en **persoon**respectievelijk.
   * **Type koppeling**: Wijzig deze waarde in **Join**.
   * **Prioriteit**: Geef een waarde die uniek is in het systeem. Een lagere numerieke waarde geeft aan dat hogere prioriteit.
   * **Tag**: leeg laten. Alleen out-of-box regels van Microsoft, moeten dit vak gevuld met een waarde hebben.
3. Op de **Scoping filter** pagina **givenName ISNOTNULL**.  
   ![De binnenkomende regel bereikfilter](./media/active-directory-aadconnectsync-change-the-configuration/scopingfilter.png)  
   Deze sectie wordt gebruikt om te definiëren op welke objecten de regel moet worden toegepast. Als deze leeg blijft, zou de regel van toepassing op alle gebruikersobjecten. Die zou echter vergaderruimten, service-accounts en andere niet-mensen gebruikersobjecten bevatten.
4. Op de **Join regels** pagina, laat het veld leeg.
5. Op de **transformaties** pagina, wijzigt u **FlowType** naar **expressie**. Voor **doelkenmerk**, selecteer **givenName**. En voor **bron**, voer **PCase([givenName])**.
   ![Regel voor binnenkomende verbindingen transformaties](./media/active-directory-aadconnectsync-change-the-configuration/transformations.png)  
   De synchronisatie-engine is hoofdlettergevoelig voor zowel de naam van de functie en de naam van het kenmerk. Als u iets mis typt, ziet u een waarschuwing wanneer u de regel toevoegen. U kunt opslaan en doorgaan, maar u wilt openen en de regel te corrigeren.
6. Klik op **toevoegen** de regel op te slaan.

Uw nieuwe aangepaste regel moet worden weergegeven met de andere sync-regels in het systeem.

### <a name="verify-the-change"></a>Controleer of de wijziging
Met deze nieuwe wijziging die u wilt controleren of deze werkt zoals verwacht en die niet alle fouten. Afhankelijk van het aantal objecten dat u hebt, zijn er twee manieren Voer deze stap:

- Een volledige synchronisatie worden uitgevoerd op alle objecten.
- Een voorbeeld en de volledige synchronisatie uitvoeren op een enkel object.

Open de **synchronisatieservice** van de **Start** menu. De stappen in deze sectie zijn alle in dit hulpprogramma.

**Volledige synchronisatie van alle objecten**  

   1. Selecteer **Connectors** aan de bovenkant. Identificeren van de connector die u hebt gewijzigd in de vorige sectie (in dit geval Active Directory Domain Services) en selecteer deze. 
   2. Voor **acties**, selecteer **uitvoeren**.
   3. Selecteer **volledige synchronisatie**, en selecteer vervolgens **OK**.
   ![Volledige synchronisatie](./media/active-directory-aadconnectsync-change-the-configuration/fullsync.png)  
   De objecten zijn nu bijgewerkt in de metaverse. Controleer uw wijzigingen door te kijken naar het object in de metaverse.

**Preview en een volledige synchronisatie van een enkel object**  

   1. Selecteer **Connectors** aan de bovenkant. Identificeren van de connector die u hebt gewijzigd in de vorige sectie (in dit geval Active Directory Domain Services) en selecteer deze.
   2. Selecteer **Connectorgebied zoeken**. 
   3. Gebruik **bereik** zoeken naar een object dat u gebruiken wilt voor het testen van de wijziging. Selecteer het object en klikt u op **Preview**. 
   4. Selecteer op het nieuwe scherm **doorvoeren Preview**.  
   ![Voorbeeld van doorvoeren](./media/active-directory-aadconnectsync-change-the-configuration/commitpreview.png)  
   De wijziging is nu toegewezen aan de metaverse.

**Het object te bekijken in de metaverse**  

1. Kies een paar voorbeeld objecten om ervoor te zorgen dat de waarde wordt verwacht en dat de regel is toegepast. 
2. Selecteer **Metaverse zoeken** vanaf de bovenkant. Voeg alle filters die u nodig hebt om de relevante objecten te vinden. 
3. Open een object in de zoekresultaten. Bekijk op de kenmerkwaarden weer en ook controleren in de **Sync regels** kolom die de regel is toegepast zoals verwacht.  
![Metaverse zoeken](./media/active-directory-aadconnectsync-change-the-configuration/mvsearch.png)  

### <a name="enable-the-scheduler"></a>De planner inschakelen
Als alles zoals verwacht, kunt u de planner opnieuw inschakelen. Voer vanuit PowerShell `Set-ADSyncScheduler -SyncCycleEnabled $true`.

## <a name="other-common-attribute-flow-changes"></a>Andere algemene wijzigingen van de kenmerk-stroom
De vorige sectie wordt beschreven hoe u wijzigingen aanbrengt aan een kenmerkstroom. In deze sectie vindt u enkele aanvullende voorbeelden. De stappen voor het maken van de synchronisatieregel wordt afgekort, maar u kunt de volledige stappen vinden in de vorige sectie.

### <a name="use-an-attribute-other-than-the-default"></a>Een kenmerk dan de standaard gebruiken
In dit scenario Fabrikam is er een forest waarbij het lokale alfabet wordt gebruikt voor de voornaam en achternaam weergavenaam. De Latijnse tekens representatie van deze kenmerken kan worden gevonden in uitbreidingskenmerken. Een lijst voor het bouwen van een globaal adres in Azure AD en Office 365, de organisatie wil deze kenmerken in plaats daarvan gebruiken.

Met een standaardconfiguratie wordt een object van het lokale forest als volgt weergegeven:  
![Kenmerkstroom 1](./media/active-directory-aadconnectsync-change-the-configuration/attributeflowjp1.png)

Maakt een regel met andere kenmerkstromen, het volgende doen:

1. Open de **synchronisatie regeleditor** van de **Start** menu.
2. Met **inkomend** nog steeds geselecteerd links, klikt u op de **nieuwe regel toevoegen** knop.
3. Geef de regel een naam en beschrijving. Selecteer de lokale Active Directory-exemplaar en de relevante objecttypen. In **koppelingstype**, selecteer **Join**. Voor **voorrang**, kies een getal dat niet wordt gebruikt door een andere regel. De out-of-box-regels beginnen met 100, zodat de waarde 50 kan worden gebruikt in dit voorbeeld.
  ![Kenmerkstroom 2](./media/active-directory-aadconnectsync-change-the-configuration/attributeflowjp2.png)
4. Laat **Scoping filter** leeg. (Dat wil zeggen, het gelden voor alle gebruikersobjecten in het forest.)
5. Laat **Join regels** leeg. (Dat wil zeggen, laat de out-of-box-regel joins afhandelen.)
6. In **transformaties**, maken de volgende stromen:  
  ![Kenmerkstroom 3](./media/active-directory-aadconnectsync-change-the-configuration/attributeflowjp3.png)
7. Klik op **toevoegen** de regel op te slaan.
8. Ga naar **Synchronization Service Manager**. Op **Connectors**, selecteert u de connector waar u de regel toegevoegd. Selecteer **uitvoeren**, en selecteer vervolgens **volledige synchronisatie**. Een volledige synchronisatie worden alle objecten opnieuw berekend met behulp van de huidige regels.

Dit is het resultaat voor hetzelfde object met deze aangepaste regel:  
![Kenmerkstroom 4](./media/active-directory-aadconnectsync-change-the-configuration/attributeflowjp4.png)

### <a name="length-of-attributes"></a>Lengte van kenmerken
Tekenreekskenmerken kunnen worden geïndexeerd standaard en de maximale lengte is 448 tekens. Als u met tekenreekskenmerken die meer bevatten werkt mogelijk, zorg ervoor dat het volgende in de kenmerkstroom:  
`attributeName` <- `Left([attributeName],448)`.

### <a name="changing-the-userprincipalsuffix"></a>De userPrincipalSuffix wijzigen
Het kenmerk userPrincipalName in Active Directory is niet altijd bekend door de gebruikers en mogelijk niet geschikt is als de aanmeldings-ID. Met de installatiewizard van Azure AD Connect-synchronisatie kunt u een ander kenmerk--bijvoorbeeld *mail*. Maar in sommige gevallen kan het kenmerk moet worden berekend.

Het bedrijf Contoso heeft bijvoorbeeld twee Azure AD-mappen, één voor productie en één voor het testen. Ze willen dat gebruikers met hun testtenant gebruik van een andere achtervoegsel in de aanmeldings-ID:  
`userPrincipalName` <- `Word([userPrincipalName],1,"@") & "@contosotest.com"`.

In deze expressie nemen alles links van de eerste @-sign (Word) en samenvoegen met een vaste tekenreeks.

### <a name="convert-a-multi-value-attribute-to-single-value"></a>Een kenmerk met meerdere waarden te converteren naar één waarde
Bepaalde kenmerken in Active Directory zijn meerdere waarden in het schema, ondanks dat ze één waarde in Active Directory: gebruikers en Computers zoeken. Een voorbeeld is het beschrijvingskenmerk:  
`description` <- `IIF(IsNullOrEmpty([description]),NULL,Left(Trim(Item([description],1)),448))`.

In deze expressie als het kenmerk een waarde heeft, duren voordat het eerste item (*Item*) in het kenmerk verwijdert voorloopspaties en afsluitende spaties (*Trim*), en u behoudt de eerst 448 tekens (*links* ) in de tekenreeks.

### <a name="do-not-flow-an-attribute"></a>Niet laten doorlopen voor een kenmerk
Zie voor achtergrondinformatie over het scenario voor deze sectie, [bepalen hoe het kenmerk stroom verloopt](active-directory-aadconnectsync-understanding-declarative-provisioning.md#control-the-attribute-flow-process).

Er zijn twee manieren een kenmerk niet stromen. De eerste is met behulp van de installatiewizard [geselecteerde kenmerken verwijderen](active-directory-aadconnect-get-started-custom.md#azure-ad-app-and-attribute-filtering). Deze optie werkt als u nooit het kenmerk voordat hebt gesynchroniseerd. Als u dit kenmerk synchroniseren en later verwijdert met deze functie hebt gestart, worden de synchronisatie-engine stopt het beheren van het kenmerk en de bestaande waarden echter gelaten in Azure AD.

Als u wilt verwijderen van de waarde van een kenmerk en zorg ervoor dat deze wordt niet in de toekomst uitgebreid, moet u een aangepaste regel maken.

In dit scenario Fabrikam hebben we gerealiseerde dat sommige van de kenmerken die wordt gesynchroniseerd met de cloud niet er worden moeten. We willen zeker weten dat deze kenmerken worden verwijderd uit Azure AD.  
![Ongeldige uitbreidingskenmerken](./media/active-directory-aadconnectsync-change-the-configuration/badextensionattribute.png)

1. Maak een nieuwe synchronisatieregel voor binnenkomende en de beschrijving te vullen.
  ![Beschrijvingen](./media/active-directory-aadconnectsync-change-the-configuration/syncruledescription.png)
2. Maken van kenmerkstromen met **expressie** voor **FlowType** en met **AuthoritativeNull** voor **bron**. De letterlijke waarde **AuthoritativeNull** geeft aan dat de waarde leeg in de metaverse, zijn mag zelfs als een lagere prioriteit synchronisatieregel probeert te vullen van de waarde.
  ![Transformatie voor uitbreidingskenmerken](./media/active-directory-aadconnectsync-change-the-configuration/syncruletransformations.png)
3. Sla de synchronisatieregel. Start de **synchronisatieservice**, vinden de connector, selecteert u **uitvoeren**, en selecteer vervolgens **volledige synchronisatie**. Deze stap opnieuw alle kenmerkstromen berekend.
4. Controleer of de gewenste wijzigingen zijn geëxporteerd door te zoeken naar het Connectorgebied overgebracht.
  ![Gefaseerde verwijderen](./media/active-directory-aadconnectsync-change-the-configuration/deletetobeexported.png)

## <a name="create-rules-with-powershell"></a>Regels maken met PowerShell
Wanneer u slechts enkele wijzigingen aanbrengen, werkt goed samen met het gebruik van de regel voor synchronisatie editor. Als u wijzigingen aanbrengen veel wilt, zijn PowerShell een betere optie. Sommige geavanceerde functies zijn alleen beschikbaar met PowerShell.

### <a name="get-the-powershell-script-for-an-out-of-box-rule"></a>Ophalen van het PowerShell-script voor een out-of-box-regel
Om te zien van de PowerShell-script dat een out-of-box-regel gemaakt, selecteert u de regel met de regeleditor synchronisatie en klikt u op **exporteren**. Deze actie geeft u de PowerShell-script dat de regel hebt gemaakt.

### <a name="advanced-precedence"></a>Geavanceerde prioriteit
De regels voor out-of-box synchronisatie starten met een prioriteitswaarde 100. Als u veel forests hebt en u wijzigingen wilt aanbrengen veel aangepaste, vervolgens regels 99 synchronisatie mogelijk niet voldoende.

U kunt opgeven dat de synchronisatie-engine die u wilt dat extra regels invoegen vóór de out-of-box-regels. Als u dit gedrag, de volgende stappen uit:

1. Markeer de eerste out-of-box-synchronisatieregel (**In van het AD-gebruiker toevoegen**) in de regeleditor synchronisatie en selecteer **exporteren**. Kopieer de SR-id-waarde.  
![PowerShell voor wijziging](./media/active-directory-aadconnectsync-change-the-configuration/powershell1.png)  
2. Maak de nieuwe synchronisatieregel. U kunt de regeleditor sync gebruiken om deze te maken. De regel naar een PowerShell-script exporteren.
3. In de eigenschap **PrecedenceBefore**, voeg de id-waarde van de out-of-box-regel. Stel de **voorrang** naar **0**. Zorg ervoor dat het kenmerk-id is uniek en dat u een GUID van een andere regel niet opnieuw gebruikt. Controleer ook of de **ImmutableTag** eigenschap is niet ingesteld. Deze eigenschap mag alleen worden ingesteld voor een out-of-box-regel.
4. Sla het PowerShell-script en voer deze uit. Het resultaat is dat uw aangepaste regel de prioriteitswaarde van 100 is toegewezen en alle andere out-of-box-regels worden verhoogd.  
![PowerShell na wijziging](./media/active-directory-aadconnectsync-change-the-configuration/powershell2.png)  

U kunt veel aangepaste synchronisatie regels hebben met dezelfde **PrecedenceBefore** waarde indien nodig.

## <a name="enable-synchronization-of-usertype"></a>Synchronisatie van UserType inschakelen
Azure AD Connect ondersteunt synchronisatie van de **UserType** kenmerk voor **gebruiker** objecten in versie 1.1.524.0 en hoger. Meer specifiek, zijn de volgende wijzigingen uitgevoerd:

- Het schema van het objecttype **gebruiker** in de Azure AD-Connector is uitgebreid met het UserType-kenmerk dat is van het type tekenreeks en wordt één waarde.
- Het schema van het objecttype **persoon** in de metaverse is uitgebreid met het UserType-kenmerk dat is van het type tekenreeks en wordt één waarde.

Standaard is het UserType-kenmerk niet ingeschakeld voor synchronisatie omdat er geen bijbehorende UserType-kenmerk in de lokale Active Directory. U moet handmatig synchronisatie inschakelen. Voordat u dit doet, moet u een notitie van het volgende gedrag afgedwongen door Azure AD uitvoeren:

- Azure AD kan slechts twee waarden voor het UserType-kenmerk: **lid** en **Gast**.
- Als het UserType-kenmerk is niet ingeschakeld voor Azure AD Connect-synchronisatie, Azure AD-gebruikers die zijn gemaakt met behulp van adreslijstsynchronisatie moet het UserType-kenmerk ingesteld op **lid**.
- Azure AD staat niet toe dat het UserType-kenmerk op bestaande Azure AD-gebruikers met Azure AD Connect worden gewijzigd. Alleen kan worden ingesteld tijdens het maken van de Azure AD-gebruikers.

Voordat de synchronisatie van het UserType-kenmerk is ingeschakeld, moet u eerst bepalen hoe het kenmerk is afgeleid van de lokale Active Directory. Hier volgen de meest voorkomende methoden:

- Een ongebruikt aanwijzen on-premises AD-kenmerk (zoals extensionAttribute1) naar worden gebruikt als het bronkenmerk. De aangewezen lokale AD-kenmerk moet van het type **tekenreeks**, worden één waarde en de waarde bevatten **lid** of **Gast**. 

    Als u deze benadering kiest, moet u ervoor zorgen dat het opgegeven kenmerk is gevuld met de juiste waarde voor alle bestaande gebruikersobjecten in de lokale Active Directory die worden gesynchroniseerd naar Azure AD voordat de synchronisatie van het UserType-kenmerk is ingeschakeld .

- U kunt ook de waarde voor het UserType-kenmerk afleiden van andere eigenschappen. Bijvoorbeeld, u wilt synchroniseren van alle gebruikers als **Gast** als hun on-premises AD userPrincipalName kenmerk eindigt op domeingedeelte  *@partners.fabrikam123.org* . 

    Zoals eerder vermeld, Azure AD Connect niet is toegestaan het UserType-kenmerk op bestaande Azure AD-gebruikers met Azure AD Connect worden gewijzigd. Daarom moet u ervoor zorgen dat de logica die u hebt besloten consistent is met hoe het UserType-kenmerk al is geconfigureerd voor alle Azure AD-gebruikers in uw tenant.

De stappen voor het inschakelen van synchronisatie van het UserType-kenmerk kunnen worden samengevat als:

1.  Schakel de sync scheduler uit en controleer of dat er vindt geen synchronisatie uitgevoerd.
2.  Het bronkenmerk toevoegen aan de on-premises AD-Connector schema.
3.  Het UserType toevoegen aan het schema van de Azure AD-Connector.
4.  Maak een synchronisatieregel voor binnenkomende om de stroom van de waarde van het kenmerk van de lokale Active Directory.
5.  Maak een synchronisatieregel voor uitgaande om de stroom van de waarde van het kenmerk naar Azure AD.
6.  Voer een volledige synchronisatiecyclus uit.
7.  Schakel de sync scheduler.

>[!NOTE]
> De rest van deze sectie bevat informatie over deze stappen. Ze worden beschreven in de context van een Azure AD-implementatie met één forest topologie en zonder aangepaste synchronisatieregels. Als u een topologie met meerdere forests hebt, aangepaste synchronisatieregels geconfigureerd of een tijdelijke server hebt, moet u de stappen overeenkomstig aanpassen.

### <a name="step-1-disable-the-sync-scheduler-and-verify-there-is-no-synchronization-in-progress"></a>Stap 1: Schakel de sync scheduler uit en controleer of dat er vindt geen synchronisatie uitgevoerd
Zorg ervoor dat er geen synchronisatie plaats terwijl u bent in het midden van synchronisatieregels bijwerken om te voorkomen onbedoelde wijzigingen exporteren naar Azure AD. De ingebouwde sync scheduler uitschakelen:

 1. Start een PowerShell-sessie op de Azure AD Connect-server.
 2. Geplande synchronisatie uitschakelen door de cmdlet `Set-ADSyncScheduler -SyncCycleEnabled $false`.
 3. Open Synchronization Service Manager door te gaan naar **Start** > **synchronisatieservice**.
 4. Ga naar de **Operations** tabblad en er is geen bewerking met de status van bevestigen *Bezig*.

### <a name="step-2-add-the-source-attribute-to-the-on-premises-ad-connector-schema"></a>Stap 2: Het bronkenmerk toevoegen aan de on-premises AD-Connector schema
Niet alle Azure AD-kenmerken worden geïmporteerd in de on-premises AD Connectorgebied overgebracht. Het bronkenmerk toevoegen aan de lijst van de geïmporteerde kenmerken:

 1. Ga naar de **Connectors** tabblad Synchronization Service Manager.
 2. Met de rechtermuisknop op de on-premises AD-Connector en selecteer **eigenschappen**.
 3. In het pop-updialoogvenster, gaat u naar de **kenmerken selecteren** tabblad.
 4. Zorg ervoor dat het bronkenmerk is ingeschakeld in de lijst met kenmerken.
 5. Klik op **OK** om op te slaan.
![Bronkenmerk toevoegen met on-premises AD-Connector schema](./media/active-directory-aadconnectsync-change-the-configuration/usertype1.png)

### <a name="step-3-add-the-usertype-to-the-azure-ad-connector-schema"></a>Stap 3: Het UserType toevoegen aan het schema van Azure AD-Connector
Standaard wordt het UserType-kenmerk niet geïmporteerd in de Azure AD Connect-ruimte. Het UserType-kenmerk toevoegen aan de lijst met geïmporteerde kenmerken:

 1. Ga naar de **Connectors** tabblad Synchronization Service Manager.
 2. Met de rechtermuisknop op de **Azure AD-Connector** en selecteer **eigenschappen**.
 3. In het pop-updialoogvenster, gaat u naar de **kenmerken selecteren** tabblad.
 4. Zorg ervoor dat het kenmerk PreferredDataLocation is ingeschakeld in de lijst met kenmerken.
 5. Klik op **OK** om op te slaan.

![Bronkenmerk toevoegen aan Azure AD-Connector schema](./media/active-directory-aadconnectsync-change-the-configuration/usertype2.png)

### <a name="step-4-create-an-inbound-synchronization-rule-to-flow-the-attribute-value-from-on-premises-active-directory"></a>Stap 4: Een synchronisatieregel voor binnenkomende om de stroom van de waarde van het kenmerk van de lokale Active Directory maken
De synchronisatieregel voor binnenkomende wordt toegestaan de waarde van het kenmerk vanuit het bronkenmerk van de lokale Active Directory naar de metaverse:

1. Open de regeleditor synchronisatie door te gaan naar **Start** > **synchronisatie regeleditor**.
2. Het zoekfilter ingesteld **richting** worden **inkomend**.
3. Klik op de **nieuwe regel toevoegen** om te maken van een nieuwe regel voor binnenkomende verbindingen.
4. Onder de **beschrijving** tabblad, bieden de volgende configuratie:

    | Kenmerk | Waarde | Details |
    | --- | --- | --- |
    | Naam | *Geef een naam* | Bijvoorbeeld: *In uit Active Directory-gebruiker UserType* |
    | Beschrijving | *Geef een beschrijving* |  |
    | Verbonden systeem | *Kies de on-premises AD-connector* |  |
    | Verbonden systeem objecttype | **User** |  |
    | Metaverse-objecttype | **Person** |  |
    | Koppelingstype | **Koppelen** |  |
    | Prioriteit | *Kies een getal tussen 1-99* | 1-99 is gereserveerd voor aangepaste synchronisatie regels. Een waarde die wordt gebruikt door andere synchronisatieregels niet opgenomen. |

5. Ga naar de **Scoping filter** tabblad en voeg een **één filter bereikgroep** met de volgende component:

    | Kenmerk | Operator | Waarde |
    | --- | --- | --- |
    | adminDescription | NOTSTARTWITH | Gebruiker\_ |

    Het bereik filter bepaalt welke on-premises AD-objecten dat deze synchronisatieregel voor binnenkomende wordt toegepast. In dit voorbeeld gebruiken we hetzelfde bereik filter gebruikt in de *In uit Active Directory-gebruiker algemene* out of box synchronisatieregel, waardoor de synchronisatieregel wordt toegepast op gebruikersobjecten via de Azure AD-gebruiker is gemaakt Write-back-functie. Mogelijk moet u het bereik filter op basis van uw Azure AD Connect-implementatie aanpassen.

6. Ga naar de **transformatie** tabblad en de gewenste transformatieregel te implementeren. Bijvoorbeeld, als u hebt aangewezen als een ongebruikt lokale AD-kenmerk (zoals extensionAttribute1) als het bronkenmerk voor de UserType, kunt u een directe kenmerkstroom implementeren:

    | Stroomtype | Doelkenmerk | Bron | Eenmaal toepassen | Type samenvoeging |
    | --- | --- | --- | --- | --- |
    | Direct | UserType | extensionAttribute1 | Dit selectievakje is uitgeschakeld | Update |

    In een ander voorbeeld wilt u de waarde voor het UserType-kenmerk worden afgeleid van andere eigenschappen. U wilt bijvoorbeeld alle gebruikers gesynchroniseerd als Gast als hun on-premises AD userPrincipalName kenmerk eindigt op domeingedeelte  *@partners.fabrikam123.org* . U kunt een expressie als volgt implementeren:

    | Stroomtype | Doelkenmerk | Bron | Eenmaal toepassen | Type samenvoeging |
    | --- | --- | --- | --- | --- |
    | Direct | UserType | IIf(IsPresent([userPrincipalName]),IIf(CBool(InStr(LCase([userPrincipalName])'@partners.fabrikam123.org')=0) 'Lid', 'Gast'), fout ('UserPrincipalName is niet aanwezig zijn om te bepalen UserType')) | Dit selectievakje is uitgeschakeld | Update |

7. Klik op **toevoegen** om de binnenkomende regel te maken.

![Synchronisatieregel voor binnenkomende maken](./media/active-directory-aadconnectsync-change-the-configuration/usertype3.png)

### <a name="step-5-create-an-outbound-synchronization-rule-to-flow-the-attribute-value-to-azure-ad"></a>Stap 5: Maak een synchronisatieregel voor uitgaande om de stroom van de waarde van het kenmerk naar Azure AD
De uitgaande synchronisatieregel wordt toegestaan de waarde van het kenmerk stromen van metaverse met het kenmerk PreferredDataLocation in Azure AD:

1. Ga naar de regeleditor synchronisatie.
2. Het zoekfilter ingesteld **richting** worden **uitgaand**.
3. Klik op de **nieuwe regel toevoegen** knop.
4. Onder de **beschrijving** tabblad, bieden de volgende configuratie:

    | Kenmerk | Waarde | Details |
    | ----- | ------ | --- |
    | Naam | *Geef een naam* | Bijvoorbeeld: *buiten het AAD-gebruiker UserType* |
    | Beschrijving | *Geef een beschrijving* ||
    | Verbonden systeem | *Selecteer de AAD-connector* ||
    | Verbonden systeem objecttype | **User** ||
    | Metaverse-objecttype | **Person** ||
    | Koppelingstype | **Koppelen** ||
    | Prioriteit | *Kies een getal tussen 1-99* | 1-99 is gereserveerd voor aangepaste synchronisatie regels. Een waarde die wordt gebruikt door andere synchronisatieregels niet opgenomen. |

5. Ga naar de **Scoping filter** tabblad en voeg een **één filter bereikgroep** met twee componenten:

    | Kenmerk | Operator | Waarde |
    | --- | --- | --- |
    | sourceObjectType | GELIJK ZIJN AAN | Gebruiker |
    | cloudMastered | NOTEQUAL | True |

    Het bereik filter bepaalt die Azure AD-objecten dat deze uitgaande synchronisatieregel wordt toegepast. In dit voorbeeld gebruiken we het filter voor hetzelfde bereik van de *Out naar AD – gebruikersidentiteit* out of box synchronisatieregel. Dit voorkomt dat de synchronisatieregel wordt toegepast op objecten die niet zijn gesynchroniseerd vanaf de lokale Active Directory. Mogelijk moet u het bereik filter op basis van uw Azure AD Connect-implementatie aanpassen.

6. Ga naar de **transformatie** tabblad en de volgende transformatieregel implementeren:

    | Stroomtype | Doelkenmerk | Bron | Eenmaal toepassen | Type samenvoeging |
    | --- | --- | --- | --- | --- |
    | Direct | UserType | UserType | Dit selectievakje is uitgeschakeld | Update |

7. Klik op **toevoegen** de uitgaande regel maken.

![Uitgaande synchronisatieregel maken](./media/active-directory-aadconnectsync-change-the-configuration/usertype4.png)

### <a name="step-6-run-a-full-synchronization-cycle"></a>Stap 6: Een volledige synchronisatiecyclus uitvoeren
In het algemeen is een volledige synchronisatiecyclus vereist omdat we hebben nieuwe kenmerken toegevoegd aan de Active Directory en de Azure AD-Connector schema's en aangepaste synchronisatieregels geïntroduceerd. U wilt controleren of de wijzigingen voordat deze naar Azure AD worden geëxporteerd. 

U kunt de volgende stappen gebruiken om te controleren of de wijzigingen tijdens de stappen die gezamenlijk een volledige synchronisatiecyclus handmatig uit te voeren.

1. Voer een **volledige import** op de **on-premises AD-Connector**:

   1. Ga naar de **Operations** tabblad Synchronization Service Manager.
   2. Met de rechtermuisknop op de **on-premises AD-Connector** en selecteer **uitvoeren**.
   3. Selecteer in het pop-updialoogvenster **volledige Import** en klik vervolgens op **OK**.
   4. Wacht totdat de bewerking te voltooien.

    > [!NOTE]
    > U kunt een volledige import overslaan op de on-premises AD-Connector als het bronkenmerk al in de lijst met opgenomen is kenmerken geïmporteerd. Met andere woorden, u hebt geen aanbrengen van wijzigingen tijdens de [stap 2: het bronkenmerk toevoegen aan de on-premises AD-Connector schema](#step-2-add-the-source-attribute-to-the-on-premises-ad-connector-schema).

2. Voer een **volledige import** op de **Azure AD-Connector**:

   1. Met de rechtermuisknop op de **Azure AD-Connector** en selecteer **uitvoeren**.
   2. Selecteer in het pop-updialoogvenster **volledige Import** en klik vervolgens op **OK**.
   3. Wacht totdat de bewerking te voltooien.

3. Controleer of de wijzigingen in synchronisatie-regel op een bestaande gebruikersobject:

    Het bronkenmerk van lokale Active Directory en de UserType van Azure AD in hun respectieve Connectorspaces zijn geïmporteerd. Voordat u doorgaat met een volledige synchronisatie, doen een **Preview** object op een bestaande gebruiker in de on-premises AD Connectorgebied overgebracht. Het object dat u hebt gekozen, moet het bronkenmerk ingevuld hebben.
    
    Een geslaagde **Preview** is een goede indicatie dat u de synchronisatie hebt geconfigureerd regels correct met het UserType ingevuld in de metaverse. Voor informatie over hoe u een **Preview**, Zie de sectie [Controleer of de wijziging](#verify-the-change).

4. Voer een **volledige synchronisatie** op de **on-premises AD-Connector**:

   1. Met de rechtermuisknop op de **on-premises AD-Connector** en selecteer **uitvoeren**.
   2. Selecteer in het pop-updialoogvenster **volledige synchronisatie** en klik vervolgens op **OK**.
   3. Wacht totdat de bewerking te voltooien.

5. Controleer of **in behandeling zijnde uitvoer** naar Azure AD:

   1. Met de rechtermuisknop op de **Azure AD-Connector** en selecteer **Connectorgebied zoeken**.
   2. In de **Connectorgebied zoeken** pop-updialoogvenster:

      - Stel **bereik** naar **in behandeling zijnde uitvoer**.
      - Selecteer alle drie de selectievakjes: **toevoegen**, **wijzigen**, en **verwijderen**.
      - Klik op de **Search** knop voor de lijst met objecten met wijzigingen worden geëxporteerd. Dubbelklik op het object voor het onderzoeken van de wijzigingen voor een bepaald object.
      - Controleer of dat de wijzigingen worden verwacht.

6. Voer **exporteren** op de **Azure AD-Connector**:

   1. Met de rechtermuisknop op de **Azure AD-Connector** en selecteer **uitvoeren**.
   2. In de **Connector uitvoeren** pop-dialoogvenster, **exporteren** en klik vervolgens op **OK**.
   3. Wacht totdat de export naar Azure AD te voltooien.

> [!NOTE]
> Deze stappen niet zijn van de volledige synchronisatie en exporteren van de stappen op de Azure AD-Connector. Deze stappen zijn niet vereist omdat de kenmerkwaarden van de lokale Active Directory naar Azure AD alleen stromen.

### <a name="step-7-re-enable-the-sync-scheduler"></a>Stap 7: De sync scheduler opnieuw inschakelen
De ingebouwde sync scheduler opnieuw inschakelen:

1. Een PowerShell-sessie starten.
2. Geplande synchronisatie opnieuw inschakelen door de cmdlet `Set-ADSyncScheduler -SyncCycleEnabled $true`.


## <a name="next-steps"></a>Volgende stappen
* Meer informatie over de configuratiemodel in [Understanding declaratieve inrichting](active-directory-aadconnectsync-understanding-declarative-provisioning.md).
* Meer informatie over de expressietaal in [Understanding declaratieve inrichting expressies](active-directory-aadconnectsync-understanding-declarative-provisioning-expressions.md).

**Overzichtsonderwerpen**

* [Azure AD Connect-synchronisatie: inzicht en synchronisatie aanpassen](active-directory-aadconnectsync-whatis.md)
* [Uw on-premises identiteiten integreren met Azure Active Directory](active-directory-aadconnect.md)
