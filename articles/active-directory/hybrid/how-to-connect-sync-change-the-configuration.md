---
title: 'Azure AD Connect-synchronisatie: Een configuratiewijziging in Azure AD Connect-synchronisatie wijzigen | Microsoft Docs'
description: Helpt u bij het maken van een wijziging aan de configuratie in Azure AD Connect-synchronisatie.
services: active-directory
documentationcenter: ''
author: billmath
manager: daveba
editor: ''
ms.assetid: 7b9df836-e8a5-4228-97da-2faec9238b31
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 08/30/2018
ms.subservice: hybrid
ms.author: billmath
ms.openlocfilehash: ebc32f59539e4e4e94f7f68d5d164cf2b04cc4c2
ms.sourcegitcommit: 5978d82c619762ac05b19668379a37a40ba5755b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/31/2019
ms.locfileid: "55488523"
---
# <a name="azure-ad-connect-sync-make-a-change-to-the-default-configuration"></a>Azure AD Connect-synchronisatie: Breng een wijziging aan in de standaardconfiguratie
Het doel van dit artikel is waarmee u stapsgewijs hoe u wijzigingen aanbrengen in de standaardconfiguratie in Azure Active Directory (Azure AD) Connect-synchronisatie. Het biedt stappen voor enkele algemene scenario's. Met deze kennis moet u het volgende kunnen eenvoudige wijzigingen aanbrengen in uw eigen configuratie op basis van uw eigen bedrijfsregels.

> [!WARNING]
> Als u wijzigingen in de standaardregels voor de synchronisatie aanbrengt vervolgens deze wijzigingen overschreven de volgende keer dat Azure AD Connect is bijgewerkt, wat resulteert in de synchronisatie van onverwachte en waarschijnlijk ongewenste resultaten.
>
> De out-of-box-synchronisatieregels hebben een vingerafdruk. Als u een wijziging in deze regels aanbrengt, wordt de vingerafdruk van het niet meer overeen. U kunt problemen ondervinden in de toekomst wanneer u probeert toe te passen van een nieuwe versie van Azure AD Connect. Alleen wijzigingen aanbrengen de manier waarop die wordt beschreven in dit artikel.

## <a name="synchronization-rules-editor"></a>Synchronization Rules Editor
De Synchronization Rules Editor wordt gebruikt om te zien en de standaardconfiguratie wijzigen. U kunt vinden op de **Start** menu onder de **Azure AD Connect** groep.  
![Startmenu met de regeleditor voor synchronisatie](./media/how-to-connect-sync-change-the-configuration/startmenu2.png)

Wanneer u de editor opent, ziet u de standaardregels voor out-of-box.

![Synchronisatie Rule Editor](./media/how-to-connect-sync-change-the-configuration/sre2.png)

### <a name="navigating-in-the-editor"></a>Navigeren in de editor
Met behulp van de vervolgkeuzelijsten boven aan de editor, kunt u snel vinden een specifieke regel. Als u zien van de regels waarin het kenmerk proxyAddresses is opgenomen wilt, kunt u de vervolgkeuzelijsten wijzigen op het volgende:  
![SRE filteren](./media/how-to-connect-sync-change-the-configuration/filtering.png)  
Als u wilt herstellen filteren en een nieuwe configuratie laden, druk op F5 op het toetsenbord.

In de rechterbovenhoek wordt de **nieuwe regel toevoegen** knop. Deze knop kunt u uw eigen aangepaste regel maken.

Aan de onderkant zijn knoppen voor fungeert voor een geselecteerde synchronisatieregel. **Bewerken** en **verwijderen** doen wat u verwacht. **Exporteren** produceert een PowerShell-script voor het opnieuw maken van de synchronisatieregel. Met deze procedure kunt u een synchronisatieregel voor van de ene server verplaatsen naar een andere.

## <a name="create-your-first-custom-rule"></a>Uw eerste aangepaste regel maken
De meest voorkomende wijzigingen zijn aan het kenmerkstromen. De gegevens in uw bronmap mogelijk niet hetzelfde als in Azure AD. In het voorbeeld in deze sectie, zorg ervoor dat de opgegeven naam van een gebruiker zich altijd in *juiste hoofdlettergebruik*.

### <a name="disable-the-scheduler"></a>De scheduler uitschakelen
De [scheduler](how-to-connect-sync-feature-scheduler.md) wordt standaard elke 30 minuten uitgevoerd. Zorg ervoor dat deze wordt niet gestart tijdens het aanbrengen van wijzigingen en het oplossen van uw nieuwe regels. Als u wilt de scheduler tijdelijk uitschakelen, start PowerShell en voer `Set-ADSyncScheduler -SyncCycleEnabled $false`.

![De scheduler uitschakelen](./media/how-to-connect-sync-change-the-configuration/schedulerdisable.png)  

### <a name="create-the-rule"></a>De regel maken
1. Klik op **nieuwe regel toevoegen**.
2. Op de **beschrijving** pagina en voer de volgende:  
   ![Inkomende regel filteren](./media/how-to-connect-sync-change-the-configuration/description2.png)  
   * **Naam**: Geef een beschrijvende naam voor de regel.
   * **Beschrijving**: Geef enkele verduidelijkingen zodat iemand anders wat de regel is begrijpen kan voor.
   * **Verbonden systeem**: Dit is het systeem waarin het object kan worden gevonden. In dit geval selecteren **Active Directory-Connector**.
   * **Verbonden systeem/Metaverse-objecttype**: Selecteer **gebruiker** en **persoon**, respectievelijk.
   * **Type koppeling**: Wijzig deze waarde in **Join**.
   * **Prioriteit**: Geef een waarde die uniek is in het systeem. Een lagere numerieke waarde geeft een hogere prioriteit.
   * **Tag**: Laat dit leeg. Alleen out-of-box regels van Microsoft, moeten dit vak gevuld met een waarde hebben.
3. Op de **Scoping filter** pagina **givenName ISNOTNULL**.  
   ![De binnenkomende regel bereikfilter](./media/how-to-connect-sync-change-the-configuration/scopingfilter.png)  
   In deze sectie wordt gebruikt om te definiëren welke objecten de regel moet worden toegepast. Als deze leeg wordt gelaten, zou de regel van toepassing op alle gebruikersobjecten. Dat zou echter vergaderruimten, service-accounts en andere niet-mensen gebruikersobjecten bevatten.
4. Op de **Join regels** pagina, laat het veld leeg.
5. Op de **transformaties** pagina, wijzigt u **FlowType** naar **expressie**. Voor **doelkenmerk**, selecteer **givenName**. En voor **bron**, voer **PCase([givenName])**.
   ![Regel voor binnenkomende verbindingen transformaties](./media/how-to-connect-sync-change-the-configuration/transformations.png)  
   De synchronisatie-engine is hoofdlettergevoelig zijn voor zowel de naam van de functie en de naam van het kenmerk. Als u er iets mis typt, ziet u een waarschuwing wanneer u de regel toevoegt. U kunt opslaan en doorgaan, maar u wilt openen en de regel te corrigeren.
6. Klik op **toevoegen** op de regel niet opslaan.

De nieuwe aangepaste regel moet worden weergegeven met de andere synchronisatieregels in het systeem.

### <a name="verify-the-change"></a>Controleer of de wijziging
Met deze nieuwe wijziging die u wilt controleren of deze werkt zoals verwacht en wordt eventuele fouten niet genereren. Afhankelijk van het aantal objecten dat u hebt, zijn er twee manieren om te doen in deze stap:

- Een volledige synchronisatie worden uitgevoerd op alle objecten.
- Een Preview-versie en een volledige synchronisatie uitvoeren op een enkel object.

Open de **Synchronization Service** uit de **Start** menu. De stappen in deze sectie worden alle in dit hulpprogramma.

**Volledige synchronisatie van alle objecten**  

   1. Selecteer **Connectors** aan de bovenkant. Identificeren van de connector die u in de vorige sectie (in dit geval, Active Directory Domain Services) hebt gewijzigd, en selecteer deze. 
   2. Voor **acties**, selecteer **uitvoeren**.
   3. Selecteer **volledige synchronisatie**, en selecteer vervolgens **OK**.
   ![Volledige synchronisatie](./media/how-to-connect-sync-change-the-configuration/fullsync.png)  
   De objecten zijn nu bijgewerkt in de metaverse. Controleer uw wijzigingen door te kijken naar het object in de metaverse.

**Preview-versie en een volledige synchronisatie op een enkel object**  

   1. Selecteer **Connectors** aan de bovenkant. Identificeren van de connector die u in de vorige sectie (in dit geval, Active Directory Domain Services) hebt gewijzigd, en selecteer deze.
   2. Selecteer **Connectorgebied zoeken**. 
   3. Gebruik **bereik** om een object dat u wilt gebruiken voor het testen van de wijziging te vinden. Selecteer het object en klikt u op **Preview**. 
   4. Selecteer op het nieuwe scherm **doorvoeren Preview**.  
   ![Preview-versie doorvoeren](./media/how-to-connect-sync-change-the-configuration/commitpreview.png)  
   De wijziging is nu toegewezen aan de metaverse.

**Het object in de metaverse weergeven**  

1. Kies een paar voorbeeld objecten om ervoor te zorgen dat de waarde wordt verwacht en dat de regel toegepast. 
2. Selecteer **Metaverse zoeken** vanaf de bovenkant. Voeg alle filters die u nodig hebt om de relevante objecten te vinden. 
3. Open een object in de zoekresultaten. Ga naar de kenmerkwaarden weer en Controleer ook of de **synchronisatieregels** kolom die de regel toegepast zoals verwacht.  
![Metaverse zoeken](./media/how-to-connect-sync-change-the-configuration/mvsearch.png)  

### <a name="enable-the-scheduler"></a>De scheduler inschakelen
Als alles is zoals verwacht, kunt u de scheduler opnieuw inschakelen. Voer vanuit PowerShell, `Set-ADSyncScheduler -SyncCycleEnabled $true`.

## <a name="other-common-attribute-flow-changes"></a>Andere algemene kenmerkwijzigingen voor stroom
De vorige sectie wordt beschreven hoe u wijzigingen aanbrengt aan een kenmerkstroom. In deze sectie vindt u enkele extra voorbeelden. De stappen voor het maken van de synchronisatieregel is afgekort, maar de volledige stappen vindt u in de vorige sectie.

### <a name="use-an-attribute-other-than-the-default"></a>Een kenmerk dan de standaardwaarde gebruiken
In dit scenario Fabrikam is er een forest waarbij het lokale alfabet wordt gebruikt voor de voornaam, achternaam en weergavenaam. De Latijnse tekens representatie van deze kenmerken kan worden gevonden in de extensiekenmerken. Voor het bouwen van een globale-adres van de lijst in Azure AD en Office 365, de organisatie wil deze kenmerken in plaats daarvan gebruiken.

Met een standaardconfiguratie is een object uit het lokale forest ziet er als volgt:  
![Kenmerkstroom 1](./media/how-to-connect-sync-change-the-configuration/attributeflowjp1.png)

Voor het maken van een regel met andere kenmerkstromen, het volgende doen:

1. Open de **Synchronization Rules Editor** uit de **Start** menu.
2. Met **inkomend** aan de linkerkant nog steeds is geselecteerd, klikt u op de **nieuwe regel toevoegen** knop.
3. Geef de regel een naam en beschrijving. Selecteer de on-premises Active Directory-exemplaar en de relevante objecttypen. In **koppelingstype**, selecteer **Join**. Voor **prioriteit**, kies een getal dat niet door een andere regel gebruikt wordt. De regels voor out-of-box beginnen met 100, zodat de waarde 50 kan worden gebruikt in dit voorbeeld.
  ![Kenmerkstroom 2](./media/how-to-connect-sync-change-the-configuration/attributeflowjp2.png)
4. Laat **Scoping filter** leeg zijn. (Dat wil zeggen, deze moet van toepassing op alle gebruikersobjecten in het forest.)
5. Laat **Join regels** leeg zijn. (Dat wil zeggen, laat de out-of-box-regel joins worden verwerkt.)
6. In **transformaties**, maken de volgende stromen:  
  ![Kenmerkstroom 3](./media/how-to-connect-sync-change-the-configuration/attributeflowjp3.png)
7. Klik op **toevoegen** op de regel niet opslaan.
8. Ga naar **Synchronization Service Manager**. Op **Connectors**, selecteert u de connector waar u de regel toegevoegd. Selecteer **uitvoeren**, en selecteer vervolgens **volledige synchronisatie**. Een volledige synchronisatie worden alle objecten met behulp van de huidige regels opnieuw berekend.

Dit is het resultaat voor hetzelfde object met deze aangepaste regel:  
![Kenmerkstroom 4](./media/how-to-connect-sync-change-the-configuration/attributeflowjp4.png)

### <a name="length-of-attributes"></a>Lengte van kenmerken
Tekenreekskenmerken kunnen worden geïndexeerd standaard, en de maximale lengte is 448 tekens. Als u met tekenreekskenmerken die meer bevatten werkt kunnen, zorg ervoor dat het volgende in de kenmerkstroom:  
`attributeName` <- `Left([attributeName],448)`.

### <a name="changing-the-userprincipalsuffix"></a>De userPrincipalSuffix wijzigen
Het kenmerk userPrincipalName in Active Directory is niet altijd door de gebruikers bekend en is mogelijk niet geschikt is als de aanmeldings-ID. Met de wizard Azure AD Connect sync-installatie, kunt u een ander kenmerk, bijvoorbeeld *e-mail*. Maar in sommige gevallen kan het kenmerk moet worden berekend.

Het bedrijf Contoso heeft bijvoorbeeld twee Azure AD-directory's, één voor productie en één voor het testen. Ze willen de gebruikers in hun testtenant het gebruik van een andere achtervoegsel in de aanmeldings-ID:  
`userPrincipalName` <- `Word([userPrincipalName],1,"@") & "@contosotest.com"`.

In deze expressie duren voordat alles links van de eerste @-sign (Word) en samenvoegen met een vaste tekenreeks.

### <a name="convert-a-multi-value-attribute-to-single-value"></a>Een kenmerk meerdere waarden te converteren naar één waarde
Bepaalde kenmerken in Active Directory zijn meerdere waarden in het schema, ook al zien ze er één waarde in Active Directory: gebruikers en Computers. Een voorbeeld is het beschrijvingskenmerk:  
`description` <- `IIF(IsNullOrEmpty([description]),NULL,Left(Trim(Item([description],1)),448))`.

Als het kenmerk een waarde heeft, kunt u in deze expressie wordt het eerste item (*Item*) in het kenmerk verwijdert voorloopspaties en afsluitende spaties (*Trim*), en u behoudt de eerste 448 tekens (*links* ) in de tekenreeks.

### <a name="do-not-flow-an-attribute"></a>Een kenmerk niet flow
Zie voor achtergrondinformatie over het scenario voor het in deze sectie, [bepalen van het proces van de stroom kenmerk](concept-azure-ad-connect-sync-declarative-provisioning.md#control-the-attribute-flow-process).

Er zijn twee manieren een kenmerk niet stromen. De eerste is met behulp van de installatiewizard aan [verwijderen van geselecteerde kenmerken](how-to-connect-install-custom.md#azure-ad-app-and-attribute-filtering). Deze optie werkt als u het kenmerk voordat nooit hebt gesynchroniseerd. Als u dit kenmerk synchroniseren en deze later verwijderen met deze functie hebt gestart, worden de synchronisatie-engine stopt beheren van het kenmerk en de bestaande waarden echter links in Azure AD.

Als u wilt verwijderen van de waarde van een kenmerk en zorg ervoor dat deze wordt niet in de toekomst uitgebreid, moet u een aangepaste regel maken.

In dit scenario Fabrikam hebben we gerealiseerde dat enkele van de kenmerken die we naar de cloud synchroniseren niet er zijn mag. We willen er zeker van te zijn dat deze kenmerken worden verwijderd uit Azure AD.  
![Ongeldige extensiekenmerken](./media/how-to-connect-sync-change-the-configuration/badextensionattribute.png)

1. Een nieuwe regel voor inkomende synchronisatie maken en vullen van de beschrijving.
  ![Beschrijvingen](./media/how-to-connect-sync-change-the-configuration/syncruledescription.png)
2. Maken van kenmerkstromen met **expressie** voor **FlowType** en met **AuthoritativeNull** voor **bron**. De letterlijke waarde **AuthoritativeNull** geeft aan dat de waarde moet leeg zijn in de metaverse is, zelfs als een lagere prioriteit synchronisatieregel wordt geprobeerd om in te vullen van de waarde.
  ![Transformatie voor extensiekenmerken](./media/how-to-connect-sync-change-the-configuration/syncruletransformations.png)
3. De synchronisatieregel niet opslaan. Start de **Synchronization Service**, de connector vinden, selecteer **uitvoeren**, en selecteer vervolgens **volledige synchronisatie**. Deze stap opnieuw alle kenmerkstromen berekend.
4. Controleer of de beoogde wijzigingen zijn op het punt te worden geëxporteerd door te zoeken naar het Connectorgebied.
  ![Gefaseerde verwijderen](./media/how-to-connect-sync-change-the-configuration/deletetobeexported.png)

## <a name="create-rules-with-powershell"></a>Regels maken met PowerShell
Met behulp van de regel voor synchronisatie editor werkt prima wanneer u slechts een paar wijzigingen aanbrengen. Als u nodig hebt om u te veel wijzigingen aanbrengt, zijn PowerShell een betere optie. Enkele geavanceerde functies zijn alleen beschikbaar met PowerShell.

### <a name="get-the-powershell-script-for-an-out-of-box-rule"></a>Het PowerShell-script voor een out-of-box-regel ophalen
Om te zien van de PowerShell-script dat een out-of-box-regel hebt gemaakt, selecteert u de regel de regeleditor synchronisatie en klikt u op **exporteren**. Hiermee geeft u de PowerShell-script dat de regel hebt gemaakt.

### <a name="advanced-precedence"></a>Geavanceerde prioriteit
De out-of-box-synchronisatieregels beginnen met een prioriteit van 100. Als u veel forests hebt en u moet veel aangepaste wijzigingen aanbrengen, klikt u vervolgens 99 synchronisatieregels mogelijk niet voldoende.

U kunt de opdracht geven voor de synchronisatie-engine die u aanvullende regels ingevoegd vóór de out-of-box-regels wilt. Als u dit gedrag, de volgende stappen uit:

1. Markeren van de eerste synchronisatieregel van de out-of-box-(**In van het AD-gebruiker toevoegen**) in de synchronisatie regeleditor en selecteer **exporteren**. Kopieer de SR-id-waarde.  
![PowerShell voor wijziging](./media/how-to-connect-sync-change-the-configuration/powershell1.png)  
2. Maak de nieuwe synchronisatieregel. U kunt de regeleditor sync gebruiken om deze te maken. De regel exporteren naar een PowerShell-script.
3. In de eigenschap **PrecedenceBefore**, de id-waarde uit de out-of-box-regel invoegen. Stel de **prioriteit** naar **0**. Zorg ervoor dat het kenmerk-id is uniek en dat u een GUID van een andere regel niet opnieuw worden gebruikt. Controleer ook of de **ImmutableTag** eigenschap niet is ingesteld. Deze eigenschap moet alleen worden ingesteld voor een out-of-box-regel.
4. Sla het PowerShell-script en voer deze uit. Het resultaat is dat uw aangepaste regel de prioriteitswaarde van 100 is toegewezen en alle andere out-of-box-regels worden verhoogd.  
![PowerShell na wijziging](./media/how-to-connect-sync-change-the-configuration/powershell2.png)  

U kunt veel aangepaste synchronisatieregels hebben met behulp van dezelfde **PrecedenceBefore** waarde wanneer dat nodig is.

## <a name="enable-synchronization-of-usertype"></a>Synchronisatie van UserType inschakelen
Azure AD Connect biedt ondersteuning voor synchronisatie van de **UserType** voor het kenmerk **gebruiker** objecten in versie 1.1.524.0 en hoger en hoger. Meer specifiek, zijn de volgende wijzigingen geïntroduceerd:

- Het schema van het objecttype **gebruiker** in de Azure AD-Connector wordt uitgebreid zodat u ook het UserType-kenmerk dat is van het typetekenreeks en één waarde.
- Het schema van het objecttype **persoon** in de metaverse wordt uitgebreid zodat u ook het UserType-kenmerk dat is van het typetekenreeks en één waarde.

Standaard is het UserType-kenmerk niet ingeschakeld voor synchronisatie omdat er geen overeenkomend UserType-kenmerk in on-premises Active Directory. U moet synchronisatie handmatig inschakelen. Voordat u dit doet, moet u Let op het volgende gedrag afgedwongen door Azure AD:

- Azure AD kan slechts twee waarden voor het UserType-kenmerk: **Lid** en **Gast**.
- Als het UserType-kenmerk is niet ingeschakeld voor Azure AD Connect-synchronisatie, Azure AD-gebruikers die zijn gemaakt via adreslijstsynchronisatie moet het UserType-kenmerk ingesteld op **lid**.
- Azure AD is niet toegestaan voor het UserType-kenmerk op bestaande Azure AD-gebruikers worden gewijzigd door Azure AD Connect. Het kan alleen worden ingesteld tijdens het maken van de Azure AD-gebruikers.

Voordat u synchronisatie van het UserType-kenmerk is ingeschakeld, moet u eerst bepalen hoe het kenmerk is afgeleid van on-premises Active Directory. Hier volgen de meest voorkomende methoden:

- Wijst een niet-gebruikte on-premises AD-kenmerk (zoals extensionAttribute1) worden gebruikt als het bronkenmerk. De on-premises AD-kenmerk moet van het type **tekenreeks**, worden één waarde en de waarde bevatten **lid** of **Gast**. 

    Als u deze aanpak kiest, moet u ervoor zorgen dat het opgegeven kenmerk is gevuld met de juiste waarde voor alle bestaande gebruikersobjecten in on-premises Active Directory die zijn gesynchroniseerd met Azure AD voordat de synchronisatie van het UserType-kenmerk is ingeschakeld .

- U kunt ook de waarde voor het UserType-kenmerk afgeleid van andere eigenschappen. Bijvoorbeeld, u wilt synchroniseren van alle gebruikers als **Gast** als hun on-premises AD userPrincipalName-kenmerk eindigt met domeingedeelte <em>@partners.fabrikam123.org</em>. 

    Zoals eerder vermeld, is Azure AD Connect het UserType-kenmerk niet toegestaan op bestaande Azure AD-gebruikers worden gewijzigd door Azure AD Connect. Daarom moet u zorgen dat de logica die u hebt besloten consistent is met hoe het UserType-kenmerk voor alle bestaande Azure AD-gebruikers in uw tenant al is geconfigureerd.

De stappen voor het inschakelen van synchronisatie van het UserType-kenmerk kunnen als worden samengevat:

1.  De synchronisatieplanning uitschakelen en controleer of dat er is geen synchronisatie uitgevoerd.
2.  Het bronkenmerk toevoegen aan de on-premises AD-Connector-schema.
3.  Het UserType toevoegen aan het schema van de Azure AD-Connector.
4.  Maak een regel voor inkomende synchronisatie voor de stroom van de waarde van het kenmerk van on-premises Active Directory.
5.  Maak een regel voor uitgaande synchronisatie om de stroom van de waarde van het kenmerk met Azure AD.
6.  Een volledige synchronisatiecyclus uitvoeren.
7.  Schakel de synchronisatieplanning.

>[!NOTE]
> De rest van deze sectie bevat informatie over deze stappen. Ze worden beschreven in de context van een Azure AD-implementatie met één forest topologie en zonder aangepaste synchronisatieregels. Hebt u een topologie met meerdere forests, aangepaste synchronisatieregels die zijn geconfigureerd, of een staging-server, moet u de stappen overeenkomstig aanpassen.

### <a name="step-1-disable-the-sync-scheduler-and-verify-there-is-no-synchronization-in-progress"></a>Stap 1: De synchronisatieplanning uitschakelen en controleer of dat er is geen synchronisatie wordt uitgevoerd
Zorg ervoor dat er geen synchronisatie plaats vindt wanneer u zich in het midden van synchronisatieregels bijwerken om te voorkomen dat onbedoelde wijzigingen te exporteren naar Azure AD. De ingebouwde Synchronisatieplanning uitschakelen:

 1. Start een PowerShell-sessie op de Azure AD Connect-server.
 2. Geplande synchronisatie uitschakelen door de cmdlet `Set-ADSyncScheduler -SyncCycleEnabled $false`.
 3. Open Synchronization Service Manager door te gaan naar **Start** > **Synchronization Service**.
 4. Ga naar de **Operations** tabblad en controleer of er is geen bewerking met de status van *Bezig*.

### <a name="step-2-add-the-source-attribute-to-the-on-premises-ad-connector-schema"></a>Stap 2: Het bronkenmerk toevoegen aan de on-premises AD-Connector-schema
Niet alle Azure AD-kenmerken worden geïmporteerd in de on-premises AD-Connectorgebied. Het bronkenmerk toevoegen aan de lijst van de geïmporteerde kenmerken:

 1. Ga naar de **Connectors** tabblad in de Synchronization Service Manager.
 2. Met de rechtermuisknop op de on-premises AD-Connector en selecteer **eigenschappen**.
 3. In het pop-updialoogvenster, gaat u naar de **kenmerken selecteren** tabblad.
 4. Zorg ervoor dat het bronkenmerk is ingeschakeld in de lijst met kenmerken.
 5. Klik op **OK** om op te slaan.
![Kenmerk van de gegevensbron toevoegen naar on-premises AD-Connector-schema](./media/how-to-connect-sync-change-the-configuration/usertype1.png)

### <a name="step-3-add-the-usertype-to-the-azure-ad-connector-schema"></a>Stap 3: Het UserType toevoegen aan het schema van de Azure AD-Connector
Standaard is het UserType-kenmerk niet geïmporteerd in de Azure AD Connect-ruimte. Het UserType-kenmerk toevoegen aan de lijst met geïmporteerde kenmerken:

 1. Ga naar de **Connectors** tabblad in de Synchronization Service Manager.
 2. Met de rechtermuisknop op de **Azure AD-Connector** en selecteer **eigenschappen**.
 3. In het pop-updialoogvenster, gaat u naar de **kenmerken selecteren** tabblad.
 4. Zorg ervoor dat het UserType-kenmerk is ingeschakeld in de lijst met kenmerken.
 5. Klik op **OK** om op te slaan.

![Kenmerk van de gegevensbron toevoegen aan Azure AD-Connector-schema](./media/how-to-connect-sync-change-the-configuration/usertype2.png)

### <a name="step-4-create-an-inbound-synchronization-rule-to-flow-the-attribute-value-from-on-premises-active-directory"></a>Stap 4: Maak een regel voor inkomende synchronisatie voor de stroom van de waarde van het kenmerk van on-premises Active Directory
De synchronisatieregel voor binnenkomende gegevens kan de waarde van het kenmerk vanuit het bronkenmerk van on-premises Active Directory naar de metaverse:

1. Open de Synchronization Rules Editor door te gaan naar **Start** > **Synchronization Rules Editor**.
2. Stel het zoekfilter **richting** moet **inkomend**.
3. Klik op de **nieuwe regel toevoegen** om te maken van een nieuwe regel voor binnenkomende verbindingen.
4. Onder de **beschrijving** tabblad, geeft u de volgende configuratie:

    | Kenmerk | Value | Details |
    | --- | --- | --- |
    | Name | *Geef een naam* | Bijvoorbeeld, *In uit Active Directory-gebruiker UserType* |
    | Description | *Geef een beschrijving* |  |
    | Verbonden systeem | *Kies de on-premises AD-connector* |  |
    | Verbonden systeem objecttype | **User** |  |
    | Metaverse-objecttype | **Person** |  |
    | Type koppeling | **Koppelen** |  |
    | Prioriteit | *Kies een getal tussen 1-99* | 1-99 is gereserveerd voor aangepaste synchronisatieregels. Een waarde die wordt gebruikt door een andere regel voor synchronisatie niet verzamelen. |

5. Ga naar de **Scoping filter** tabblad en voeg een **één filter bereikgroep** met de volgende component:

    | Kenmerk | Operator | Value |
    | --- | --- | --- |
    | adminDescription | NOTSTARTWITH | Gebruiker\_ |

    De bereikfilter bepaalt welke on-premises AD-objecten dat deze synchronisatieregel voor binnenkomende gegevens wordt toegepast. In dit voorbeeld gebruiken we de dezelfde bereikfilter gebruikt in de *In uit Active Directory-gebruiker algemene* out-of-box synchronization rule, waarmee wordt voorkomen dat de synchronisatieregel wordt toegepast op objecten die zijn gemaakt via de Azure AD-gebruiker Write-back-functie. U moet mogelijk de bereikfilter op basis van uw Azure AD Connect-implementatie aanpassen.

6. Ga naar de **transformatie** tabblad en implementeren van de gewenste transformatie-regel. Bijvoorbeeld, als u hebt aangewezen als een niet-gebruikte on-premises AD-kenmerk (zoals extensionAttribute1) als het bronkenmerk voor de UserType, kunt u een directe kenmerkstroom implementeren:

    | Type gebruikersstroom | Doelkenmerk | Bron | Eenmaal toepassen | Type samenvoeging |
    | --- | --- | --- | --- | --- |
    | Rechtstreeks | UserType | extensionAttribute1 | Dit selectievakje is uitgeschakeld | Update |

    In een ander voorbeeld wilt u de waarde voor het UserType-kenmerk zijn afgeleid van andere eigenschappen. Bijvoorbeeld, u wilt synchroniseren van alle gebruikers als Gast als hun on-premises AD userPrincipalName-kenmerk eindigt met domeingedeelte <em>@partners.fabrikam123.org</em>. U kunt een expressie als volgt implementeren:

    | Type gebruikersstroom | Doelkenmerk | Bron | Eenmaal toepassen | Type samenvoeging |
    | --- | --- | --- | --- | --- |
    | Rechtstreeks | UserType | IIf(IsPresent([userPrincipalName]),IIf(CBool(InStr(LCase([userPrincipalName]),"@partners.fabrikam123.org")=0), 'Lid', 'Guest'), fout ("UserPrincipalName is niet aanwezig zijn om te bepalen UserType")) | Dit selectievakje is uitgeschakeld | Update |

7. Klik op **toevoegen** om de binnenkomende regel te maken.

![Synchronisatieregel voor binnenkomende gegevens maken](./media/how-to-connect-sync-change-the-configuration/usertype3.png)

### <a name="step-5-create-an-outbound-synchronization-rule-to-flow-the-attribute-value-to-azure-ad"></a>Stap 5: Een regel voor uitgaande synchronisatie om de stroom van de waarde van het kenmerk met Azure AD maken
De regel voor uitgaande synchronisatie kan de waarde van het kenmerk op die moeten stromen van metaverse aan het UserType-kenmerk in Azure AD:

1. Ga naar de Synchronization Rules Editor.
2. Stel het zoekfilter **richting** moet **uitgaand**.
3. Klik op de **nieuwe regel toevoegen** knop.
4. Onder de **beschrijving** tabblad, geeft u de volgende configuratie:

    | Kenmerk | Value | Details |
    | ----- | ------ | --- |
    | Name | *Geef een naam* | Bijvoorbeeld, *Out voor AAD-gebruiker UserType* |
    | Description | *Geef een beschrijving* ||
    | Verbonden systeem | *Selecteer de AAD-connector* ||
    | Verbonden systeem objecttype | **User** ||
    | Metaverse-objecttype | **Person** ||
    | Type koppeling | **Koppelen** ||
    | Prioriteit | *Kies een getal tussen 1-99* | 1-99 is gereserveerd voor aangepaste synchronisatieregels. Een waarde die wordt gebruikt door een andere regel voor synchronisatie niet verzamelen. |

5. Ga naar de **Scoping filter** tabblad en voeg een **één filter bereikgroep** met twee componenten:

    | Kenmerk | Operator | Value |
    | --- | --- | --- |
    | sourceObjectType | GELIJK AAN | Gebruiker |
    | cloudMastered | NOTEQUAL | True |

    De bereikfilter bepaalt welke Azure AD-objecten dat met deze regel voor uitgaande synchronisatie wordt toegepast. In dit voorbeeld gebruiken we de dezelfde bereikfilter uit de *aan AD-gebruikers-id* out-of-box synchronisatieregel. Voorkomt dat de synchronisatieregel wordt toegepast op objecten die niet zijn gesynchroniseerd vanuit on-premises Active Directory. U moet mogelijk de bereikfilter op basis van uw Azure AD Connect-implementatie aanpassen.

6. Ga naar de **transformatie** tabblad en implementeren van de volgende transformatieregel:

    | Type gebruikersstroom | Doelkenmerk | Bron | Eenmaal toepassen | Type samenvoeging |
    | --- | --- | --- | --- | --- |
    | Rechtstreeks | UserType | UserType | Dit selectievakje is uitgeschakeld | Update |

7. Klik op **toevoegen** om de uitgaande regel te maken.

![Regel voor uitgaande synchronisatie maken](./media/how-to-connect-sync-change-the-configuration/usertype4.png)

### <a name="step-6-run-a-full-synchronization-cycle"></a>Stap 6: Een volledige synchronisatiecyclus uitvoeren
In het algemeen is een volledige synchronisatiecyclus vereist omdat we hebben nieuwe kenmerken toegevoegd aan de Active Directory en de Azure AD-Connector schema's en aangepaste synchronisatieregels die zijn geïntroduceerd. U wilt controleren of de wijzigingen voordat u ze naar Azure AD exporteert. 

U kunt de volgende stappen uit om te controleren of de wijzigingen bij het handmatig uitvoeren van de stappen die gezamenlijk een volledige synchronisatiecyclus.

1. Voer een **volledige import** op de **on-premises AD-Connector**:

   1. Ga naar de **Operations** tabblad in de Synchronization Service Manager.
   2. Met de rechtermuisknop op de **on-premises AD-Connector** en selecteer **uitvoeren**.
   3. Selecteer in het pop-updialoogvenster **volledige Import** en klik vervolgens op **OK**.
   4. Wacht tot de bewerking is voltooid.

    > [!NOTE]
    > U kunt een volledige import overslaan op de on-premises AD-Connector als het bronkenmerk al opgenomen in de lijst met is kenmerken geïmporteerd. Met andere woorden, u hebt geen te wijzigen tijdens de [stap 2: Het bronkenmerk toevoegen aan de on-premises AD-Connector schema](#step-2-add-the-source-attribute-to-the-on-premises-ad-connector-schema).

2. Voer een **volledige import** op de **Azure AD-Connector**:

   1. Met de rechtermuisknop op de **Azure AD-Connector** en selecteer **uitvoeren**.
   2. Selecteer in het pop-updialoogvenster **volledige Import** en klik vervolgens op **OK**.
   3. Wacht tot de bewerking is voltooid.

3. Controleer of de wijzigingen van de regel voor synchronisatie op een bestaande gebruikersobject:

    Het bronkenmerk van on-premises Active Directory en het UserType van Azure AD zijn geïmporteerd in hun respectieve Connectorspaces. Voordat u doorgaat met een volledige synchronisatie kan een **Preview** op een bestaande gebruiker van het object in de on-premises AD-Connectorgebied. Het object dat u hebt gekozen, moet het bronkenmerk ingevuld hebben.
    
    Een geslaagde **Preview** is een goede indicatie dat u de synchronisatie hebt geconfigureerd correct regels met de UserType ingevuld in de metaverse. Voor informatie over hoe u doet een **Preview**, Zie de sectie [controleren of de wijziging](#verify-the-change).

4. Voer een **volledige synchronisatie** op de **on-premises AD-Connector**:

   1. Met de rechtermuisknop op de **on-premises AD-Connector** en selecteer **uitvoeren**.
   2. Selecteer in het pop-updialoogvenster **volledige synchronisatie** en klik vervolgens op **OK**.
   3. Wacht tot de bewerking is voltooid.

5. Controleer of **in behandeling zijnde uitvoer** naar Azure AD:

   1. Met de rechtermuisknop op de **Azure AD-Connector** en selecteer **Search Connector Space**.
   2. In de **Search Connector Space** pop-updialoogvenster:

      - Stel **bereik** naar **exportbewerking**.
      - Selecteer alle drie de selectievakjes: **Voeg**, **wijzigen**, en **verwijderen**.
      - Klik op de **zoeken** knop aan de lijst van objecten met de wijzigingen worden geëxporteerd. Dubbelklik op het object voor het onderzoeken van de wijzigingen voor een bepaald object.
      - Controleer of dat de wijzigingen worden verwacht.

6. Voer **exporteren** op de **Azure AD-Connector**:

   1. Met de rechtermuisknop op de **Azure AD-Connector** en selecteer **uitvoeren**.
   2. In de **Connector uitvoeren** pop-upvenster in het dialoogvenster, selecteer **exporteren** en klik vervolgens op **OK**.
   3. Wacht totdat de export naar Azure AD om te voltooien.

> [!NOTE]
> Deze stappen niet zijn de volledige synchronisatie en exporteren van de stappen in de Azure AD-Connector. Deze stappen zijn niet vereist omdat de kenmerkwaarden van on-premises Active Directory naar Azure AD alleen stromen.

### <a name="step-7-re-enable-the-sync-scheduler"></a>Stap 7: De synchronisatieplanning opnieuw inschakelen
De ingebouwde Synchronisatieplanning opnieuw inschakelen:

1. Start een PowerShell-sessie.
2. Geplande synchronisatie weer inschakelen door de cmdlet `Set-ADSyncScheduler -SyncCycleEnabled $true`.


## <a name="next-steps"></a>Volgende stappen
* Meer informatie over de configuratiemodel in [Understanding declaratieve inrichting](concept-azure-ad-connect-sync-declarative-provisioning.md).
* Meer informatie over de expressietaal in [Understanding declaratieve inrichting expressies](concept-azure-ad-connect-sync-declarative-provisioning-expressions.md).

**Overzichtsonderwerpen**

* [Azure AD Connect-synchronisatie: Begrijpen en aanpassen van synchronisatie](how-to-connect-sync-whatis.md)
* [Uw on-premises identiteiten integreren met Azure Active Directory](whatis-hybrid-identity.md)
