---
title: 'Azure AD Connect-synchronisatie: filtering configureren | Microsoft Docs'
description: Legt uit hoe u configureert filtering in Azure AD Connect-synchronisatie.
services: active-directory
documentationcenter: 
author: billmath
manager: mtillman
editor: 
ms.assetid: 880facf6-1192-40e9-8181-544c0759d506
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: billmath
ms.openlocfilehash: 5af82e889a80994dd47d4fc3b89f8eece2201355
ms.sourcegitcommit: f1c1789f2f2502d683afaf5a2f46cc548c0dea50
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/18/2018
---
# <a name="azure-ad-connect-sync-configure-filtering"></a>Azure AD Connect-synchronisatie: filtering configureren
U gebruikt voor het filteren, kunt u bepalen welke objecten worden weergegeven in Azure Active Directory (Azure AD) van uw on-premises directory. De standaardconfiguratie wordt van alle objecten in alle domeinen in de geconfigureerde forests. Dit is in het algemeen zijn de aanbevolen configuratie. Gebruikers met behulp van Office 365-werkbelastingen, zoals Exchange Online en Skype voor bedrijven profiteren van een volledige lijst met globale adressen zodat ze kunnen e-mail verzenden en iedereen. Met de standaardconfiguratie hebben ze dezelfde ervaring die ze met een lokale implementatie van Exchange- of Lync hebben zou.

In sommige gevallen echter je vereist een aantal wijzigingen aanbrengen in de standaardconfiguratie. Hier volgen enkele voorbeelden:

* U wilt gebruiken de [meerdere Azure AD-directory-topologie](active-directory-aadconnect-topologies.md#each-object-only-once-in-an-azure-ad-tenant). Moet u een filter toepassen om te bepalen welke objecten worden gesynchroniseerd naar een bepaalde Azure AD-directory.
* U een test uitvoeren voor Azure of Office 365 en u wilt dat alleen een subset van gebruikers in Azure AD. In de kleine test is niet belangrijk dat u hebt een volledige algemene adreslijst ter illustratie van de functionaliteit.
* U hebt veel serviceaccounts en andere alle niet-persoonlijke accounts die u niet dat in Azure AD wilt.
* U kunt elke gebruiker accounts lokale niet verwijdert om wettelijke redenen. U alleen uitschakelen deze. Maar in Azure AD kunt u alleen actieve accounts aanwezig zijn.

In dit artikel bevat informatie over het configureren van de verschillende methoden voor filteren.

> [!IMPORTANT]
> Microsoft biedt geen ondersteuning te wijzigen of het besturingssysteem van Azure AD Connect-synchronisatie buiten de acties die formeel zijn gedocumenteerd. Een van deze acties kan leiden tot een inconsistente of niet-ondersteunde status van Azure AD Connect-synchronisatie. Microsoft kan niet als gevolg hiervan technische ondersteuning bieden voor dergelijke implementaties.

## <a name="basics-and-important-notes"></a>Basisbeginselen en belangrijke opmerkingen
In Azure AD Connect-synchronisatie, kunt u filteren op elk gewenst moment inschakelen. Als u met een standaardconfiguratie van directory-synchronisatie begint en filtering configureren, worden de objecten die worden gefilterd niet meer gesynchroniseerd naar Azure AD. Vanwege deze wijziging worden alle objecten in Azure AD die eerder zijn gesynchroniseerd, maar vervolgens zijn gefilterd verwijderd in Azure AD.

Voordat u wijzigingen aanbrengt filteren, zorg ervoor dat u [uitschakelen van de geplande taak](#disable-scheduled-task) zodat u wijzigingen die u nog niet hebt gecontroleerd om alleen correct als per ongeluk niet exporteren.

Omdat filteren, veel objecten op hetzelfde moment verwijderen kan, die u wilt zorgen dat uw nieuwe filters juist zijn voordat u begint met het exporteren van wijzigingen naar Azure AD. Nadat u de configuratiestappen hebt voltooid, wordt aangeraden dat u volgt de [verificatiestappen](#apply-and-verify-changes) voordat u exporteert en wijzigingen in Azure AD aanbrengen.

Om u te beschermen veel objecten verwijderen per ongeluk, de functie '[onopzettelijk verwijderen voorkomen](active-directory-aadconnectsync-feature-prevent-accidental-deletes.md)' is standaard ingeschakeld. Als u veel objecten vanwege filteren (500 standaard) verwijdert, moet u de stappen in dit artikel om toe te staan de verwijderingen te doorlopen naar Azure AD.

Als u een build vóór November 2015 ([1.0.9125](active-directory-aadconnect-version-history.md#1091250)), een wijziging aanbrengt in een filterconfiguratie en Wachtwoordsynchronisatie, gebruiken, moet u een volledige synchronisatie van alle wachtwoorden activeren nadat u de configuratie hebt voltooid. Zie voor stappen voor het activeren van een volledige Wachtwoordsynchronisatie [activeren van een volledige synchronisatie van wachtwoorden voor alle](active-directory-aadconnectsync-troubleshoot-password-synchronization.md#trigger-a-full-sync-of-all-passwords). Als u van build 1.0.9125 gebruikmaakt of hoger, klikt u vervolgens de normale **volledige synchronisatie** actie berekent ook Hiermee wordt aangegeven of wachtwoorden moeten worden gesynchroniseerd en als deze extra stap niet langer vereist is.

Als **gebruiker** objecten onbedoeld in Azure AD zijn verwijderd vanwege een fout filteren, kunt u opnieuw maken de gebruikersobjecten in Azure AD door het verwijderen van uw filteren configuraties. Vervolgens kunt u uw adreslijsten opnieuw synchroniseren. Deze actie worden de gebruikers hersteld uit de Prullenbak in Azure AD. U kunt andere objecttypen echter kan niet ongedaan. Bijvoorbeeld, als u per ongeluk een beveiligingsgroep verwijdert en deze is gebruikt voor de Toegangsbeheerlijst van kunnen niet een resource, de groep en de ACL's worden hersteld.

Azure AD Connect verwijdert u alleen objecten die deze heeft één keer beschouwd als in het bereik. Er zijn objecten die zijn gemaakt door een andere synchronisatie-engine in Azure AD en deze objecten niet binnen het bereik, verwijderen toe te voegen filteren niet. Bijvoorbeeld, als u begint met een DirSync-server die een volledige kopie van uw gehele map in Azure AD gemaakt en installeren van een nieuwe Azure AD Connect-synchronisatieserver parallel met filtering is ingeschakeld vanaf het begin, Azure AD Connect niet de extra objecten verwijderen die zijn gemaakt door DirSync.

De configuratie van de filterregel blijft behouden wanneer u installeren of naar een nieuwere versie van Azure AD Connect upgraden. Het is altijd een best practice om te controleren dat de configuratie is niet per ongeluk is gewijzigd na een upgrade naar een nieuwere versie voordat u de eerste synchronisatiecyclus.

Als u meer dan één forest hebt, kunt u de filteren configuraties die worden beschreven in dit onderwerp voor elk forest (ervan uitgaande dat u wilt dat dezelfde configuratie voor alle paden) moet toepassen.

### <a name="disable-the-scheduled-task"></a>De geplande taak uitschakelen
Schakel de ingebouwde planner waarmee een synchronisatiecyclus elke 30 minuten wordt geactiveerd als volgt:

1. Ga naar een PowerShell-prompt.
2. Voer `Set-ADSyncScheduler -SyncCycleEnabled $False` uitschakelen van de planner.
3. Breng de wijzigingen die zijn beschreven in dit artikel.
4. Voer `Set-ADSyncScheduler -SyncCycleEnabled $True` opnieuw inschakelen van de planner.

**Als u een Azure AD Connect-build voordat 1.1.105.0**  
Schakel de geplande taak die een synchronisatiecyclus elke drie uur activeert de volgende stappen uit:

1. Start **Task Scheduler** van de **Start** menu.
2. Direct onder **bibliotheek voor Taakplanner**, zoek de taak met de naam **Azure AD Sync Scheduler**, klik met de rechtermuisknop en selecteer **uitschakelen**.  
   ![Taakplanner](./media/active-directory-aadconnectsync-configure-filtering/taskscheduler.png)  
3. U kunt nu configuratiewijzigingen aanbrengen en uitvoeren van de synchronisatie-engine handmatig van de **Synchronization Service Manager** console.

Nadat u uw filteren wijzigingen hebt voltooid, vergeet niet om u te keert u terug en **inschakelen** de taak opnieuw.

## <a name="filtering-options"></a>Opties voor het filteren
U kunt de volgende configuratie-typen filters toepassen op het hulpprogramma directory-synchronisatie:

* [**Op basis van een groep**](#group-based-filtering): filteren op basis van een groep kan alleen worden geconfigureerd op de eerste installatie met behulp van de installatiewizard.
* [**Op basis van een domein**](#domain-based-filtering): deze optie gebruikt, kunt u selecteren welke domeinen naar Azure AD synchroniseren. U kunt ook toevoegen en verwijderen uit de configuratie van de synchronisatie-engine wanneer u wijzigingen aan uw on-premises infrastructuur aanbrengt nadat u Azure AD Connect-synchronisatie hebt geïnstalleerd.
* [**Organisatie-eenheid (OE) – op basis van**](#organizational-unitbased-filtering): met deze optie kunt u selecteren welke organisatie-eenheden naar Azure AD synchroniseren. Deze optie is voor alle objecttypen in de geselecteerde OE's.
* [**Op kenmerken gebaseerde**](#attribute-based-filtering): deze optie gebruikt, kunt u objecten op basis van de kenmerkwaarden voor de objecten filteren. U kunt ook verschillende filters voor verschillende objecttypen hebben.

U kunt meerdere filteropties tegelijkertijd gebruiken. Bijvoorbeeld, kunt u filteren op basis van organisatie-eenheid alleen objecten opnemen in een organisatie-eenheid. Op hetzelfde moment kunt u filteren op basis van het kenmerk voor het filteren van de objecten verder. Wanneer u meerdere filtermethoden, gebruik de filters een logische 'en' tussen de filters.

## <a name="domain-based-filtering"></a>Filteren op basis van een domein
Deze sectie biedt u de stappen voor het configureren van uw domein-filter. Als u toegevoegd of verwijderd van domeinen in uw forest nadat u Azure AD Connect geïnstalleerd, hebt u ook filteren configuratie bijwerken.

De beste manier om te filteren op basis van een domein wijzigen door de wizard installeren en te wijzigen is [domein en OE filteren](active-directory-aadconnect-get-started-custom.md#domain-and-ou-filtering). De installatiewizard automatiseert de taken die worden beschreven in dit onderwerp.

Volg deze stappen alleen als u de installatiewizard uitvoeren om een bepaalde reden niet.

Filteren configuratie op basis van een domein bestaat uit de volgende stappen uit:

1. [Selecteer de domeinen](#select-domains-to-be-synchronized) die u wilt opnemen in de synchronisatie.
2. Aan elk toegevoegd en verwijderd domein, pas de [uitvoeringsprofielen](#update-run-profiles).
3. [Van toepassing en controleer of wijzigingen](#apply-and-verify-changes).

### <a name="select-the-domains-to-be-synchronized"></a>Selecteer de domeinen kunnen worden gesynchroniseerd
Voer de volgende stappen uit te stellen de domein-filter:

1. Aanmelden bij de server waarop Azure AD Connect-synchronisatie met een account dat lid is van de **ADSyncAdmins** beveiligingsgroep.
2. Start **synchronisatieservice** van de **Start** menu.
3. Selecteer **Connectors**, en in de **Connectors** , selecteert u de Connector met het type **Active Directory Domain Services**. In **acties**, selecteer **eigenschappen**.  
   ![Connectoreigenschappen](./media/active-directory-aadconnectsync-configure-filtering/connectorproperties.png)  
4. Klik op **mappartities configureren**.
5. In de **mappartities selecteren** optie en domeinen Schakel indien nodig. Controleer of dat alleen de partities die u wilt synchroniseren zijn geselecteerd.  
   ![Partities](./media/active-directory-aadconnectsync-configure-filtering/connectorpartitions.png)  
   Als u hebt gewijzigd van uw on-premises Active Directory-infrastructuur en toegevoegd of verwijderd van de domeinen van het forest, klikt u op de **vernieuwen** knop ophalen van een bijgewerkte lijst. Wanneer u vernieuwt, wordt u gevraagd om referenties. Geef referenties met leestoegang naar Windows Server Active Directory. Heeft geen moet de gebruiker die vooraf is ingevuld in het dialoogvenster.  
   ![Vernieuwen vereist](./media/active-directory-aadconnectsync-configure-filtering/refreshneeded.png)  
6. Wanneer u bent klaar, sluit u de **eigenschappen** dialoogvenster door te klikken op **OK**. Als u domeinen verwijderd uit het forest, bericht een pop-dat een domein is verwijderd en dat de configuratie worden opgeschoond.
7. Doorgaan om aan te passen de [uitvoeringsprofielen](#update-run-profiles).

### <a name="update-the-run-profiles"></a>Bijwerken van de profielen uitvoeren
Als u uw domein filter hebt bijgewerkt, moet u ook de uitvoeringsprofielen bijwerken.

1. In de **Connectors** lijst, zorg ervoor dat de Connector die u in de vorige stap hebt gewijzigd is geselecteerd. In **acties**, selecteer **Uitvoeringsprofielen configureren**.  
   ![Connector uitvoeringsprofielen 1](./media/active-directory-aadconnectsync-configure-filtering/connectorrunprofiles1.png)  
2. Vinden en identificeren van de volgende profielen:
    * Volledige Import
    * Volledige synchronisatie
    * Delta-Import
    * Deltasynchronisatie
    * Exporteren
3. Aanpassen voor elk profiel de **toegevoegd** en **verwijderd** domeinen.
    1. Voor elk van de vijf volgt u de volgende stappen uit voor elk **toegevoegd** domein:
        1. Selecteer het uitvoeringsprofiel en klik op **nieuwe stap**.
        2. Op de **stap configureren** pagina in de **Type** vervolgkeuzelijst, selecteer de stap typen met dezelfde naam als het profiel dat u configureert. Klik op **Volgende**.  
        ![Connector uitvoeringsprofielen 2](./media/active-directory-aadconnectsync-configure-filtering/runprofilesnewstep1.png)  
        3. Op de **connectorconfiguratie** pagina in de **partitie** vervolgkeuzelijst, selecteer de naam van het domein dat u hebt toegevoegd aan uw domein-filter.  
        ![Connector uitvoeringsprofielen 3](./media/active-directory-aadconnectsync-configure-filtering/runprofilesnewstep2.png)  
        4. Sluit de **Uitvoeringsprofiel configureren** dialoogvenster, klikt u op **voltooien**.
    2. Voor elk van de vijf volgt u de volgende stappen uit voor elk **verwijderd** domein:
        1. Selecteer het profiel uitvoeren.
        2. Als de **waarde** van de **partitie** kenmerk is een GUID, selecteert u de stappen van uitvoering en klikt u op **verwijderen stap**.  
        ![Connector uitvoeringsprofielen 4](./media/active-directory-aadconnectsync-configure-filtering/runprofilesdeletestep.png)  
    3. Controleer of de wijziging. Elk domein dat u wilt synchroniseren moet worden vermeld als een stap voor elk uitvoeringsprofiel.
4. Sluit de **Uitvoeringsprofielen configureren** dialoogvenster, klikt u op **OK**.
5.  Voor het voltooien van de configuratie, moet u uitvoeren een **volledige import** en een **Deltasynchronisatie**. Doorgaan met het lezen van de sectie [toepassen en controleer of de wijzigingen](#apply-and-verify-changes).

## <a name="organizational-unitbased-filtering"></a>Organisatie-eenheid – gebaseerd filteren
De beste manier om te filteren op basis van organisatie-eenheid wijzigen door de wizard installeren en te wijzigen is [domein en OE filteren](active-directory-aadconnect-get-started-custom.md#domain-and-ou-filtering). De installatiewizard automatiseert de taken die worden beschreven in dit onderwerp.

Volg deze stappen alleen als u de installatiewizard uitvoeren om een bepaalde reden niet.

Voer de volgende stappen uit voor het configureren van de organisatie-eenheid – gebaseerd filteren:

1. Aanmelden bij de server waarop Azure AD Connect-synchronisatie met een account dat lid is van de **ADSyncAdmins** beveiligingsgroep.
2. Start **synchronisatieservice** van de **Start** menu.
3. Selecteer **Connectors**, en in de **Connectors** , selecteert u de Connector met het type **Active Directory Domain Services**. In **acties**, selecteer **eigenschappen**.  
   ![Connectoreigenschappen](./media/active-directory-aadconnectsync-configure-filtering/connectorproperties.png)  
4. Klik op **mappartities configureren**, selecteert u het domein dat u wilt configureren en klik vervolgens op **Containers**.
5. Wanneer u wordt gevraagd, referenties lezen toegang bieden tot uw lokale Active Directory. Heeft geen moet de gebruiker die vooraf is ingevuld in het dialoogvenster.
6. In de **Containers selecteren** dialoogvenster vak, schakelt u de organisatie-eenheden die u niet wilt synchroniseren met de clouddirectory, en klik vervolgens op **OK**.  
   ![OE's in het dialoogvenster Containers selecteren](./media/active-directory-aadconnectsync-configure-filtering/ou.png)  
   * De **Computers** container moet worden geselecteerd voor uw Windows 10-computers worden gesynchroniseerd naar Azure AD. Als uw domein computers bevinden zich in een andere organisatie-eenheden, zorg er dan voor dat die zijn geselecteerd.
   * De container **ForeignSecurityPrincipals** moet zijn geselecteerd als er meerdere forests met vertrouwensrelaties zijn. Dankzij deze container kan het lidmaatschap van de beveiligingsgroep van verschillende forests worden omgezet.
   * De **Geregistreerdeapparaten** organisatie-eenheid moet worden geselecteerd als u de functie van de Write-back van apparaat is ingeschakeld. Als u een andere Write-back-functie, zoals Write-back van groep controleert u of dat deze locaties zijn geselecteerd.
   * Selecteer een andere organisatie-eenheid waar gebruikers, iNetOrgPersons, groepen, contactpersonen en Computers zich bevinden. In de afbeelding bevinden deze organisatie-eenheden zich in de ManagedObjects OU.
   * Als u filteren op basis van een groep, moet klikt u vervolgens de organisatie-eenheid waar de groep bevindt worden opgenomen.
   * Houd er rekening mee dat u configureren kunt of nieuwe OE's die worden toegevoegd nadat de filteren configuratie is voltooid die zijn gesynchroniseerd of niet gesynchroniseerd. Zie de volgende sectie voor meer informatie.
7. Wanneer u bent klaar, sluit u de **eigenschappen** dialoogvenster door te klikken op **OK**.
8. Voor het voltooien van de configuratie, moet u uitvoeren een **volledige import** en een **Deltasynchronisatie**. Doorgaan met het lezen van de sectie [toepassen en controleer of de wijzigingen](#apply-and-verify-changes).

### <a name="synchronize-new-ous"></a>Synchroniseren van nieuwe organisatie-eenheden
Nieuwe OE's die zijn gemaakt nadat het filteren is geconfigureerd, worden standaard gesynchroniseerd. Deze status wordt aangegeven door het selectievakje is ingeschakeld. U kunt ook het vakje sommige sub-OU's. Als u dit gedrag, klikt u op het vak totdat deze wit met een blauw vinkje (de standaardstatus) verandert. Vervolgens moet een sub-OE's die u niet wilt synchroniseren verwijderen.

Als alle sub-OU's worden gesynchroniseerd, klikt u vervolgens is het selectievakje wit met een blauw vinkje.  
![Organisatie-eenheid met alle selectievakjes zijn geselecteerd](./media/active-directory-aadconnectsync-configure-filtering/ousyncnewall.png)

Als sommige sub-OU's hebt niet geselecteerd, is het selectievakje grijs met een wit vinkje.  
![Organisatie-eenheid met sommige sub-OU's uitgeschakeld](./media/active-directory-aadconnectsync-configure-filtering/ousyncnew.png)

Met deze configuratie is een nieuwe organisatie-eenheid die is gemaakt onder ManagedObjects gesynchroniseerd.

De Azure AD Connect-installatiewizard wordt altijd gemaakt voor deze configuratie.

### <a name="dont-synchronize-new-ous"></a>Nieuwe OE's niet synchroniseren
U kunt de synchronisatie-engine voor het nieuwe OE's niet synchroniseren nadat de filteren configuratie is voltooid. Deze status wordt aangegeven in de gebruikersinterface door het selectievakje effen grijs zonder vinkje worden weergegeven. Als u dit gedrag, klikt u op het vak totdat deze wit zonder vinkje verandert. Selecteer vervolgens de sub-OE's die u wilt synchroniseren.

![Organisatie-eenheid met de hoofdmap niet geselecteerd](./media/active-directory-aadconnectsync-configure-filtering/oudonotsyncnew.png)

Met deze configuratie is niet een nieuwe organisatie-eenheid die is gemaakt onder ManagedObjects gesynchroniseerd.

## <a name="attribute-based-filtering"></a>Filteren op basis van het kenmerk
Zorg ervoor dat u de November 2015 ([1.0.9125](active-directory-aadconnect-version-history.md#1091250)) of later bouwen voor deze stappen om te werken.

Filteren op basis van het kenmerk is de meest flexibele manier om filterobjecten. U kunt de kracht van [declaratieve inrichting](active-directory-aadconnectsync-understanding-declarative-provisioning.md) bijna elk aspect van wanneer een object wordt gesynchroniseerd naar Azure AD beheren.

U kunt toepassen [inkomende](#inbound-filtering) filteren uit Active Directory naar de metaverse en [uitgaande](#outbound-filtering) voor het filteren van metaverse naar Azure AD. Het is raadzaam dat u inkomende filteren toepassen omdat dit het gemakkelijkst te onderhouden. Gebruik alleen uitgaande filteren als dit is vereist voor het toevoegen van objecten uit meer dan één forest voordat de evaluatie kan plaatsvinden.

### <a name="inbound-filtering"></a>Inkomende filteren
Inkomende filteren maakt gebruik van de standaardconfiguratie, waarbij objecten naar Azure AD de metaverse-kenmerk cloudFiltered niet is ingesteld op een waarde kunnen worden gesynchroniseerd moeten hebben. Als de waarde van dit kenmerk is ingesteld op **True**, en vervolgens het object is niet gesynchroniseerd. Mag niet worden ingesteld op **False**, aan het ontwerp. Om er zeker van te zijn andere regels hebben de mogelijkheid om bij te dragen van een waarde, moet dit kenmerk alleen de waarden **True** of **NULL** (afwezig).

In het inkomende filteren, gebruikt u de kracht van **bereik** om te bepalen welke objecten worden gesynchroniseerd of niet synchroniseren. Dit is waar u aanpassingen aan de behoeften van uw eigen organisatie maken. De scope-module heeft een **groep** en een **component** om te bepalen wanneer een synchronisatieregel is binnen het bereik. Een groep bevat een of meer componenten. Er is een logische 'en' tussen meerdere componenten en een logische 'of' tussen meerdere groepen.

Laat het ons Bekijk een voorbeeld:  
![Bereik](./media/active-directory-aadconnectsync-configure-filtering/scope.png)  
Dit moet worden gelezen als **(afdeling = IT) of (afdeling = verkoop-en c = US)**.

In de volgende voorbeelden en stappen, gebruik van het gebruikersobject als voorbeeld, maar u kunt deze gebruiken voor alle objecttypen.

In de volgende voorbeelden wordt de prioriteitswaarde begint met 50. Dit is een nummer niet gebruikt, maar moet lager zijn dan 100 zijn.

#### <a name="negative-filtering-do-not-sync-these"></a>Negatieve filteren: 'Synchroniseer niet deze'
In het volgende voorbeeld wordt u uitfilteren (niet synchroniseren) alle gebruikers waar **extensionAttribute15** heeft de waarde **NoSync**.

1. Aanmelden bij de server waarop Azure AD Connect-synchronisatie met een account dat lid is van de **ADSyncAdmins** beveiligingsgroep.
2. Start **synchronisatie regeleditor** van de **Start** menu.
3. Zorg ervoor dat **inkomend** is geselecteerd en klik op **nieuwe regel toevoegen**.
4. Geef de regel een beschrijvende naam, zoals '*In uit Active Directory-gebruiker DoNotSyncFilter*'. Selecteer het juiste forest, selecteer **gebruiker** als de **CS objecttype**, en selecteer **persoon** als de **MV-objecttype**. In **koppelingstype**, selecteer **Join**. In **voorrang**, typt u een waarde die niet wordt momenteel gebruikt door andere synchronisatieregels (bijvoorbeeld 50) en klik vervolgens op **volgende**.  
   ![Inkomende 1 Beschrijving](./media/active-directory-aadconnectsync-configure-filtering/inbound1.png)  
5. In **Scoping filter**, klikt u op **groep toevoegen**, en klik op **component toevoegen**. In **kenmerk**, selecteer **ExtensionAttribute15**. Zorg ervoor dat **Operator** is ingesteld op **gelijk**, en typ de waarde **NoSync** in de **waarde** vak. Klik op **Volgende**.  
   ![Inkomende 2 bereik](./media/active-directory-aadconnectsync-configure-filtering/inbound2.png)  
6. Laat de **Join** regels leeg en klik vervolgens op **volgende**.
7. Klik op **transformatie toevoegen**, selecteer de **FlowType** als **constante**, en selecteer **cloudFiltered** als de **doel Kenmerk**. In de **bron** in het tekstvak **True**. Klik op **toevoegen** de regel op te slaan.  
   ![Inkomende 3 transformatie](./media/active-directory-aadconnectsync-configure-filtering/inbound3.png)
8. Voor het voltooien van de configuratie, moet u uitvoeren een **volledige synchronisatie**. Doorgaan met het lezen van de sectie [toepassen en controleer of de wijzigingen](#apply-and-verify-changes).

#### <a name="positive-filtering-only-sync-these"></a>Filteren van positieve: 'alleen synchroniseren deze'
Uitdrukken positief filteren worden meer lastig omdat u ook hebt rekening houden met objecten die niet kunnen worden gesynchroniseerd, zoals vergaderruimten duidelijk. U gaat ook het standaardfilter in de out-of-box-regel onderdrukken **In uit Active Directory - gebruiker toevoegen**. Wanneer u een aangepaste filter maakt, zorg ervoor dat geen essentiële systeemobjecten, replicatieobjecten conflict, speciale postvakken en de serviceaccounts voor Azure AD Connect.

Het positieve filteroptie vereist twee regels voor synchronisatie. U moet één regel (of meerdere) met het juiste bereik van objecten om te synchroniseren. U moet ook een tweede synchronisatieregel catch all-gefilterd op alle objecten die nog niet hebt is geïdentificeerd als een object dat moet worden gesynchroniseerd.

In het volgende voorbeeld wordt u gebruikersobjecten waar het kenmerk voor afdeling de waarde heeft alleen synchroniseren **verkoop**.

1. Aanmelden bij de server waarop Azure AD Connect-synchronisatie met een account dat lid is van de **ADSyncAdmins** beveiligingsgroep.
2. Start **synchronisatie regeleditor** van de **Start** menu.
3. Zorg ervoor dat **inkomend** is geselecteerd en klik op **nieuwe regel toevoegen**.
4. Geef de regel een beschrijvende naam, zoals '*In uit Active Directory-gebruiker verkoop synchroniseren*'. Selecteer het juiste forest, selecteer **gebruiker** als de **CS objecttype**, en selecteer **persoon** als de **MV-objecttype**. In **koppelingstype**, selecteer **Join**. In **voorrang**, typt u een waarde die niet wordt momenteel gebruikt door andere synchronisatieregels (bijvoorbeeld 51) en klik vervolgens op **volgende**.  
   ![Inkomende 4 beschrijving](./media/active-directory-aadconnectsync-configure-filtering/inbound4.png)  
5. In **Scoping filter**, klikt u op **groep toevoegen**, en klik op **component toevoegen**. In **kenmerk**, selecteer **afdeling**. Zorg ervoor dat de Operator is ingesteld op **gelijk**, en typ de waarde **verkoop** in de **waarde** vak. Klik op **Volgende**.  
   ![Inkomende 5 bereik](./media/active-directory-aadconnectsync-configure-filtering/inbound5.png)  
6. Laat de **Join** regels leeg en klik vervolgens op **volgende**.
7. Klik op **transformatie toevoegen**, selecteer **constante** als de **FlowType**, en selecteer de **cloudFiltered** als de **doel Kenmerk**. In de **bron** in het vak **False**. Klik op **toevoegen** de regel op te slaan.  
   ![Inkomende 6 transformatie](./media/active-directory-aadconnectsync-configure-filtering/inbound6.png)  
   Dit is een speciaal geval waar u expliciet cloudFiltered instellen op **False**.
8. We hebben nu de catch all-synchronisatieregel maken. Geef de regel een beschrijvende naam, zoals '*In uit Active Directory-gebruiker Catch all-filter*'. Selecteer het juiste forest, selecteer **gebruiker** als de **CS objecttype**, en selecteer **persoon** als de **MV-objecttype**. In **koppelingstype**, selecteer **Join**. In **voorrang**, typt u een waarde die niet wordt momenteel gebruikt door andere synchronisatieregels (bijvoorbeeld 99). U kunt een prioriteit die hogere (lagere prioriteit) dan de vorige synchronisatieregel hebt geselecteerd. Maar u hebt ook ruimte zodat u meer synchronisatie-filterregels later toevoegen kunt als u wilt beginnen met het synchroniseren van aanvullende afdelingen. Klik op **Volgende**.  
   ![Inkomende 7 beschrijving](./media/active-directory-aadconnectsync-configure-filtering/inbound7.png)  
9. Laat **Scoping filter** leeg en klik op **volgende**. Een filternaam is leeg geeft aan dat de regel moet worden toegepast op alle objecten.
10. Laat de **Join** regels leeg en klik vervolgens op **volgende**.
11. Klik op **transformatie toevoegen**, selecteer **constante** als de **FlowType**, en selecteer **cloudFiltered** als de **doel Kenmerk**. In de **bron** in het vak **True**. Klik op **toevoegen** de regel op te slaan.  
    ![Inkomende 3 transformatie](./media/active-directory-aadconnectsync-configure-filtering/inbound3.png)  
12. Voor het voltooien van de configuratie, moet u uitvoeren een **volledige synchronisatie**. Doorgaan met het lezen van de sectie [toepassen en controleer of de wijzigingen](#apply-and-verify-changes).

Als u wilt, kunt u meer regels van het eerste type waar u meer objecten in de synchronisatie opnemen.

### <a name="outbound-filtering"></a>Uitgaande filteren
In sommige gevallen hoeft te doen het filter alleen nadat de objecten in de metaverse hebt toegevoegd. Bijvoorbeeld mogelijk om te kijken naar het e-mailkenmerk vanuit het bronforest en het kenmerk userPrincipalName uit het accountforest, om te bepalen of een object moet worden gesynchroniseerd. In dergelijke gevallen maakt u filteren op de uitgaande regel.

In dit voorbeeld wijzigt u de filteren zodat alleen gebruikers die hun e-mail en de userPrincipalName eindigt op @contoso.com worden gesynchroniseerd:

1. Aanmelden bij de server waarop Azure AD Connect-synchronisatie met een account dat lid is van de **ADSyncAdmins** beveiligingsgroep.
2. Start **synchronisatie regeleditor** van de **Start** menu.
3. Onder **regels Type**, klikt u op **uitgaand**.
4. Afhankelijk van de versie van Connect dat u gebruikt, ofwel de regel voor licentiecontrole vinden **buiten het AAD-gebruiker toevoegen** of **uit naar de AAD - gebruiker toevoegen SOAInAD**, en klik op **bewerken**.
5. In het pop-upvenster beantwoorden **Ja** om een kopie van de regel te maken.
6. Op de **beschrijving** pagina, wijzigt u **voorrang** op een niet-gebruikte waarde, zoals 50.
7. Klik op **Scoping filter** op de linkernavigatiebalk en klik vervolgens op **toevoegen component**. In **kenmerk**, selecteer **mail**. In **Operator**, selecteer **ENDSWITH**. In **waarde**, type  **@contoso.com** , en klik vervolgens op **toevoegen component**. In **kenmerk**, selecteer **userPrincipalName**. In **Operator**, selecteer **ENDSWITH**. In **waarde**, type  **@contoso.com** .
8. Klik op **Opslaan**.
9. Voor het voltooien van de configuratie, moet u uitvoeren een **volledige synchronisatie**. Doorgaan met het lezen van de sectie [toepassen en controleer of de wijzigingen](#apply-and-verify-changes).

## <a name="apply-and-verify-changes"></a>Van toepassing en controleer of wijzigingen
Nadat u uw wijzigingen hebt aangebracht, moet u deze toepassen op de objecten die al aanwezig in het systeem zijn. Het kan ook zijn dat de objecten die zich momenteel niet in de synchronisatie-engine moeten worden verwerkt (en de synchronisatie-engine moet het bronsysteem opnieuw uit om te controleren of de inhoud lezen).

Als u de configuratie hebt gewijzigd met behulp van **domein** of **organisatie-eenheid** filteren, en vervolgens moet u een **volledige import**, gevolgd door **Delta synchronisatie**.

Als u de configuratie hebt gewijzigd met behulp van **kenmerk** filteren, en vervolgens moet u een **volledige synchronisatie**.

Voer de volgende stappen uit:

1. Start **synchronisatieservice** van de **Start** menu.
2. Selecteer **Connectors**. In de **Connectors** , selecteert u de Connector waar u eerder een configuratiewijziging hebt gemaakt. In **acties**, selecteer **uitvoeren**.  
   ![Connector uitvoeren](./media/active-directory-aadconnectsync-configure-filtering/connectorrun.png)  
3. In **Uitvoeringsprofielen**, selecteer de bewerking die is vermeld in de vorige sectie. Als u twee acties uitvoeren moet, voert u de seconde na de eerste bewerking is voltooid. (De **status** kolom is **inactief** voor de geselecteerde verbindingslijn.)

Alle wijzigingen zijn na de synchronisatie wordt voorbereid om te worden geëxporteerd. Voordat u daadwerkelijk wijzigingen in Azure AD aanbrengen, die u wilt controleren of alle wijzigingen in deze juist zijn.

1. Start een opdrachtprompt en Ga naar `%Program Files%\Microsoft Azure AD Sync\bin`.
2. Voer `csexport "Name of Connector" %temp%\export.xml /f:x` uit.  
   De naam van de Connector is in de synchronisatieservice. Een naam die vergelijkbaar is met 'contoso.com – AAD' voor Azure AD.
3. Voer `CSExportAnalyzer %temp%\export.xml > %temp%\export.csv` uit.
4. U hebt nu een bestand in % temp % met de naam export.csv die kan worden onderzocht in Microsoft Excel. Dit bestand bevat alle wijzigingen die zullen worden uitgevoerd.
5. Breng de benodigde wijzigingen aan de gegevens of configuratie en voer deze stappen opnieuw (importeren, synchroniseren en controleer of) totdat de wijzigingen die zullen worden uitgevoerd zijn wat u verwacht.

Wanneer u tevreden bent, exporteert u de wijzigingen naar Azure AD.

1. Selecteer **Connectors**. In de **Connectors** , selecteert u de Azure AD-Connector. In **acties**, selecteer **uitvoeren**.
2. In **Uitvoeringsprofielen**, selecteer **exporteren**.
3. Als uw configuratiewijzigingen veel objecten verwijderen, klikt u vervolgens ziet u een fout in de uitvoer wanneer het aantal hoger dan de geconfigureerde drempelwaarde (standaard 500 is). Als u deze fout te zien, moet u tijdelijk de '[onopzettelijk verwijderen voorkomen](active-directory-aadconnectsync-feature-prevent-accidental-deletes.md)' functie.

Nu is het tijd om de planner opnieuw inschakelen.

1. Start **Task Scheduler** van de **Start** menu.
2. Direct onder **bibliotheek voor Taakplanner**, zoek de taak met de naam **Azure AD Sync Scheduler**, klik met de rechtermuisknop en selecteer **inschakelen**.

## <a name="group-based-filtering"></a>Filteren op basis van een groep
U kunt filteren op basis van een groep van de eerste keer dat u Azure AD Connect met behulp van installeert [aangepaste installatie](active-directory-aadconnect-get-started-custom.md#sync-filtering-based-on-groups). Het bedoeld voor een proef-implementatie waar u een kleine set objecten worden gesynchroniseerd. Wanneer u filteren op basis van een groep uitschakelt, kunnen deze kan niet worden ingeschakeld. Deze heeft *niet ondersteund* te filteren op basis van een groep in een aangepaste configuratie gebruiken. Deze functie configureren met behulp van de installatiewizard wordt alleen ondersteund. Wanneer u uw proefproject hebt voltooid, voert u een van de filteropties in dit onderwerp. Wanneer u filteren op basis van organisatie-eenheid in combinatie met filteren op basis van een groep, is de OU('s) waar de groep en de bijbehorende leden zich bevinden moeten worden opgenomen.

Bij het synchroniseren van meerdere AD-forests, kunt u filteren op basis van een groep door op te geven van een andere groep voor elk AD-connector kunt configureren. Als u om te synchroniseren van een gebruiker in één AD-forest en dezelfde gebruiker heeft een of meer overeenkomende objecten in andere AD-forests, moet u ervoor zorgen dat de gebruikersobject en alle bijbehorende objecten binnen de groep op basis van een gefilterd bereik. Voor voorbeelden:

* Er is een gebruiker in een forest met een bijbehorende FSP (afwijkende beveiligings-Principal)-object in een ander forest. Beide objecten moeten worden binnen groepen gebaseerd filteren bereik. Anders wordt de gebruiker niet worden gesynchroniseerd naar Azure AD.

* Er is een gebruiker in een forest met een bijbehorende resourceaccount (bijvoorbeeld gekoppeld postvak) in een ander forest. Bovendien hebt u Azure AD Connect om te koppelen van de gebruiker met de resource-account geconfigureerd. Beide objecten moeten worden binnen groepen gebaseerd filteren bereik. Anders wordt de gebruiker niet worden gesynchroniseerd naar Azure AD.

* Er is een gebruiker in een forest met een overeenkomende e-mailbericht contactpersoon in een ander forest. U kunt Azure AD Connect als u wilt koppelen aan de gebruiker de e-mail contact op met verder hebt geconfigureerd. Beide objecten moeten worden binnen groepen gebaseerd filteren bereik. Anders wordt de gebruiker niet worden gesynchroniseerd naar Azure AD.


## <a name="next-steps"></a>Volgende stappen
- Meer informatie over [Azure AD Connect-synchronisatie](active-directory-aadconnectsync-whatis.md) configuratie.
- Meer informatie over [uw on-premises identiteiten integreren met Azure AD](active-directory-aadconnect.md).
