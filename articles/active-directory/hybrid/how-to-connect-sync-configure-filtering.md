---
title: 'Azure AD Connect-synchronisatie: Filtering configureren | Microsoft Docs'
description: Wordt uitgelegd hoe u in Azure AD Connect-synchronisatie-filtering configureren.
services: active-directory
documentationcenter: ''
author: billmath
manager: daveba
editor: ''
ms.assetid: 880facf6-1192-40e9-8181-544c0759d506
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.subservice: hybrid
ms.author: billmath
ms.openlocfilehash: 28f35fd1098e055b22dada703cd7e68de591eea7
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2019
ms.locfileid: "55192618"
---
# <a name="azure-ad-connect-sync-configure-filtering"></a>Azure AD Connect-synchronisatie: Filtering configureren
Met behulp van filteren, kunt u bepalen welke objecten worden weergegeven in Azure Active Directory (Azure AD) vanuit uw on-premises directory. De standaardconfiguratie wordt van alle objecten in alle domeinen in de geconfigureerde forests. Dit is in het algemeen is de aanbevolen configuratie. Gebruikers met behulp van Office 365-werkbelastingen, zoals Exchange Online en Skype voor bedrijven, profiteren van een volledige lijst met algemene adres zodat ze kunnen e-mailbericht verzenden en iedereen. Met de standaardconfiguratie, zouden ze hebben dezelfde ervaring die ze met een on-premises implementatie van Exchange- of Lync hebben zouden.

In sommige gevallen echter, u bent verplicht u enkele wijzigingen in de standaardconfiguratie. Hier volgen enkele voorbeelden:

* U wilt gebruiken de [met meerdere Azure AD-directory-topologie](plan-connect-topologies.md#each-object-only-once-in-an-azure-ad-tenant). Moet u het pas een filter om te bepalen welke objecten worden gesynchroniseerd met een bepaald Azure AD-directory.
* U een test uitvoeren voor Azure of Office 365 en u wilt dat alleen een subset van gebruikers in Azure AD. Aan de kleine pilot is het niet belangrijk dat u hebt een volledige globale adreslijst om de functionaliteit te demonstreren.
* U hebt veel serviceaccounts en andere alle niet-persoonlijke accounts die u niet dat in Azure AD wilt.
* Om wettelijke redenen, kunt u een gebruiker computeraccounts on-premises niet verwijdert. U alleen uitschakelen deze. Maar in Azure AD kunt u wilt dat alleen actieve accounts aanwezig zijn.

In dit artikel bevat informatie over het configureren van de verschillende methoden voor filteren.

> [!IMPORTANT]
> Microsoft biedt geen ondersteuning voor het wijzigen of uitvoeren van Azure AD Connect-synchronisatie anders dan op de manier die officieel is gedocumenteerd. Alle andere acties kunnen resulteren in een inconsistente of niet-ondersteunde status van de Azure AD Connect-synchronisatie. Daarom biedt Microsoft geen technische ondersteuning voor dergelijke implementaties.

## <a name="basics-and-important-notes"></a>Basisbeginselen en belangrijke opmerkingen
In Azure AD Connect-synchronisatie kunt u filteren op elk gewenst moment. Als u met een standaardconfiguratie van directory-synchronisatie beginnen en configureer vervolgens filteren, worden de objecten die zijn gefilterd niet meer gesynchroniseerd met Azure AD. Vanwege deze wijziging, worden alle objecten in Azure AD die eerder zijn gesynchroniseerd, maar vervolgens zijn gefilterd verwijderd in Azure AD.

Voordat u begint met het aanbrengen van wijzigingen voor het filteren, zorg ervoor dat u [uitschakelen van de geplande taak](#disable-the-scheduled-task) , zodat u niet per ongeluk wijzigingen die u nog niet hebt gecontroleerd om juiste exporteren.

Omdat filteren, veel objecten op hetzelfde moment verwijderen kan, wilt u om ervoor te zorgen dat uw nieuwe filters juist zijn voordat u begint met het exporteren van wijzigingen naar Azure AD. Nadat u de configuratiestappen hebt voltooid, wordt aangeraden dat u volgt de [verificatiestappen](#apply-and-verify-changes) voordat u exporteert en wijzigingen in Azure AD aanbrengen.

Om te voorkomen dat u veel objecten verwijderen per ongeluk, de functie '[onopzettelijke verwijderingen voorkomen](how-to-connect-sync-feature-prevent-accidental-deletes.md)' is standaard ingeschakeld. Als u veel objecten vanwege filteren (500 standaard) verwijdert, moet u de stappen in dit artikel om toe te staan de verwijdert via naar Azure AD.

Als u een build vóór November 2015 ([1.0.9125](reference-connect-version-history.md#1091250)), een wijziging aanbrengt in de filterconfiguratie van een en gebruiken van wachtwoord-hashsynchronisatie, moet u een volledige synchronisatie van alle wachtwoorden activeren nadat u de configuratie hebt voltooid. Zie voor instructies over het activeren van een volledige synchronisatie van wachtwoord, [activeren van een volledige synchronisatie van alle wachtwoorden](tshoot-connect-password-hash-synchronization.md#trigger-a-full-sync-of-all-passwords). Als u van build 1.0.9125 gebruikmaakt of hoger, klikt u vervolgens het normale **volledige synchronisatie** actie wordt ook berekend of wachtwoorden moeten worden gesynchroniseerd en als deze extra stap niet langer vereist is.

Als **gebruiker** objecten per ongeluk in Azure AD zijn verwijderd vanwege een fout filteren, kunt u de gebruikersobjecten opnieuw in Azure AD door het verwijderen van de configuraties van uw filteren. Vervolgens kunt u uw adreslijsten opnieuw synchroniseren. Deze actie worden hersteld door de gebruikers uit de Prullenbak in Azure AD. U kunt geen echter ongedaan maken andere objecttypen. Bijvoorbeeld, als u per ongeluk een beveiligingsgroep verwijdert en het is gebruikt voor de Toegangsbeheerlijst van worden een resource, de groep en de ACL's niet hersteld.

Azure AD Connect verwijdert alleen objecten die deze heeft één keer beschouwd als binnen het bereik. Er zijn in Azure AD-objecten die zijn gemaakt door een andere synchronisatie-engine en deze objecten niet binnen het bereik, verwijderen toe te voegen filteren niet. Bijvoorbeeld, als u begint met een DirSync-server die een volledige kopie van uw hele directory in Azure AD hebt gemaakt en u een nieuwe Azure AD Connect-synchronisatieserver parallel met filtering is ingeschakeld vanaf het begin installeren, Azure AD Connect niet de extra objecten verwijderen die door DirSync worden gemaakt.

De configuratie van de filters worden bewaard wanneer u installeert of naar een nieuwere versie van Azure AD Connect bijwerkt. Het is altijd een best practice om te controleren dat de configuratie is niet per ongeluk is gewijzigd na een upgrade naar een nieuwere versie voordat u de eerste synchronisatiecyclus uitvoert.

Als u meer dan één forest hebt, kunt u de filters gebruiken om configuraties die worden beschreven in dit onderwerp voor elk forest (ervan uitgaande dat u wilt dat de dezelfde configuratie voor al deze) moet toepassen.

### <a name="disable-the-scheduled-task"></a>De geplande taak uitschakelen
Als u wilt uitschakelen met de ingebouwde scheduler die een synchronisatiecyclus elke 30 minuten activeert, de volgende stappen uit:

1. Ga naar een PowerShell-prompt.
2. Voer `Set-ADSyncScheduler -SyncCycleEnabled $False` om uit te schakelen van de scheduler.
3. Breng de wijzigingen die worden beschreven in dit artikel.
4. Voer `Set-ADSyncScheduler -SyncCycleEnabled $True` opnieuw zodat de scheduler.

**Als u een Azure AD Connect-build voordat 1.1.105.0**  
Als u wilt uitschakelen van de geplande taak die wordt geactiveerd een synchronisatiecyclus elke drie uur, de volgende stappen uit:

1. Start **Task Scheduler** uit de **Start** menu.
2. Direct onder de **bibliotheek voor Taakplanner**, de taak met de naam niet vinden **Azure AD Sync Scheduler**, klik met de rechtermuisknop en selecteer **uitschakelen**.  
   ![Task Scheduler](./media/how-to-connect-sync-configure-filtering/taskscheduler.png)  
3. U kunt nu configuratiewijzigingen aanbrengen en uitvoeren van de synchronisatie-engine handmatig vanuit de **Synchronization Service Manager** console.

Nadat u alle filters gebruiken om wijzigingen hebt voltooid, vergeet dan niet te keert u terug en **inschakelen** de taak opnieuw.

## <a name="filtering-options"></a>Opties voor het filteren
U kunt de volgende filters gebruiken om configuratie-typen toepassen op het hulpprogramma directory-synchronisatie:

* [**Op basis van een groep**](#group-based-filtering): Filteren op basis van één groep kan alleen worden geconfigureerd op de eerste installatie met behulp van de installatiewizard.
* [**Op basis van een domein**](#domain-based-filtering): U kunt met deze optie kunt selecteren welke domeinen synchroniseren met Azure AD. U kunt ook toevoegen en domeinen verwijderen uit de synchronisatie-engine-configuratie wanneer u wijzigingen in uw on-premises infrastructuur aanbrengen nadat u Azure AD Connect-synchronisatie installeren.
* [**Organisatie-eenheid (OE) – op basis van**](#organizational-unitbased-filtering): Met deze optie, kunt u selecteren welke OE's gesynchroniseerd met Azure AD. Deze optie is voor alle objecttypen in geselecteerde organisatie-eenheden.
* [**Op kenmerken gebaseerde**](#attribute-based-filtering): Deze optie gebruikt, kunt u objecten op basis van kenmerkwaarden voor de objecten te filteren. U kunt ook verschillende filters voor verschillende objecttypen hebben.

U kunt meerdere opties voor het filteren op hetzelfde moment. U kunt bijvoorbeeld filteren op basis van organisatie-eenheid gebruiken om op te nemen alleen objecten in een organisatie-eenheid. Op hetzelfde moment, kunt u filteren op basis van een kenmerk voor het filteren van de objecten verder. Wanneer u meerdere filtermethoden gebruiken, gebruik de filters een logische 'en' tussen de filters.

## <a name="domain-based-filtering"></a>Filteren op basis van een domein
In deze sectie biedt u de stappen voor het configureren van uw domein-filter. Als u toegevoegd of verwijderd van domeinen in uw forest nadat u Azure AD Connect geïnstalleerd, hebt u ook filters gebruiken om de configuratie bijwerken.

De beste manier om te filteren op basis van een domein wijzigen is door de installatiewizard uitvoeren en wijzigen van [domein en OE filteren](how-to-connect-install-custom.md#domain-and-ou-filtering). De installatiewizard automatiseert de taken die worden beschreven in dit onderwerp.

Volg deze stappen alleen als u niet om uit te voeren van de installatiewizard om een bepaalde reden.

Filters gebruiken om configuratie op basis van een domein bestaat uit de volgende stappen uit:

1. [Selecteer de domeinen](#select-domains-to-be-synchronized) die u wilt opnemen in de synchronisatie.
2. Voor elke toegevoegd en verwijderd domein, passen de [uitvoeringsprofielen](#update-run-profiles).
3. [Toepassen en wijzigingen controleren](#apply-and-verify-changes).

### <a name="select-the-domains-to-be-synchronized"></a>Selecteer de domeinen moeten worden gesynchroniseerd
Als u wilt de domein-filter kunt instellen, moet u de volgende stappen uitvoeren:

1. Aanmelden bij de server waarop Azure AD Connect-synchronisatie is uitgevoerd met behulp van een account dat lid is van de **ADSyncAdmins** beveiligingsgroep.
2. Start **Synchronization Service** uit de **Start** menu.
3. Selecteer **Connectors**, en klik in de **Connectors** , selecteert u de Connector met het type **Active Directory Domain Services**. In **acties**, selecteer **eigenschappen**.  
   ![Eigenschappen van de connector](./media/how-to-connect-sync-configure-filtering/connectorproperties.png)  
4. Klik op **mappartities configureren**.
5. In de **mappartities selecteren** lijst, selecteer en hef de selectie van domeinen naar behoefte. Controleer of dat alleen de partities die u wilt synchroniseren zijn geselecteerd.  
   ![Partities](./media/how-to-connect-sync-configure-filtering/connectorpartitions.png)  
   Als u hebt gewijzigd van uw on-premises Active Directory-infrastructuur en toegevoegd of verwijderd van domeinen van het forest, klikt u op de **vernieuwen** knop waarmee een bijgewerkte lijst. Wanneer u vernieuwt, wordt u gevraagd om referenties. Geef referenties met leestoegang naar Windows Server Active Directory. Dit hoeft niet te worden van de gebruiker die vooraf is ingevuld in het dialoogvenster.  
   ![Er moet worden vernieuwd](./media/how-to-connect-sync-configure-filtering/refreshneeded.png)  
6. Wanneer u klaar bent, sluit u de **eigenschappen** dialoogvenster door te klikken op **OK**. Als u domeinen van het forest verwijderd, wordt een pop-bericht zegt dat een domein is verwijderd en dat de configuratie worden opgeschoond.
7. Doorgaan om aan te passen de [uitvoeringsprofielen](#update-run-profiles).

### <a name="update-the-run-profiles"></a>De uitvoerprofielen bijwerken
Als u uw filter domein hebt bijgewerkt, moet u ook de uitvoerprofielen bijwerken.

1. In de **Connectors** lijst, zorg ervoor dat de Connector die u hebt gewijzigd in de vorige stap is geselecteerd. In **acties**, selecteer **Uitvoeringsprofielen configureren**.  
   ![Connector uitvoeringsprofielen 1](./media/how-to-connect-sync-configure-filtering/connectorrunprofiles1.png)  
2. Zoeken en identificeren van de volgende profielen:
    * Volledige Import
    * Volledige synchronisatie
    * Delta-Import
    * Deltasynchronisatie
    * Exporteren
3. Voor elk profiel aanpassen de **toegevoegd** en **verwijderd** domeinen.
    1. Voor elk van de vijf profielen, voert u de volgende stappen uit voor elk **toegevoegd** domein:
        1. Selecteer het uitvoeringsprofiel en klik op **nieuwe stap**.
        2. Op de **configureren stap** pagina, in de **Type** vervolgkeuzelijst, selecteer de stap type met dezelfde naam als het profiel dat u configureert. Klik op **Volgende**.  
        ![Connector uitvoeringsprofielen 2](./media/how-to-connect-sync-configure-filtering/runprofilesnewstep1.png)  
        3. Op de **connectorconfiguratie** pagina, in de **partitie** vervolgkeuzelijst, selecteer de naam van het domein dat u hebt toegevoegd aan uw domein-filter.  
        ![Connector uitvoeringsprofielen 3](./media/how-to-connect-sync-configure-filtering/runprofilesnewstep2.png)  
        4. Sluit de **Uitvoeringsprofiel configureren** dialoogvenster, klikt u op **voltooien**.
    2. Voor elk van de vijf profielen, voert u de volgende stappen uit voor elk **verwijderd** domein:
        1. Selecteer het profiel uitvoeren.
        2. Als de **waarde** van de **partitie** kenmerk is een GUID, selecteert u de stappen van uitvoering en klikt u op **stap verwijderen**.  
        ![Connector uitvoeringsprofielen 4](./media/how-to-connect-sync-configure-filtering/runprofilesdeletestep.png)  
    3. Controleer of de wijziging. Elk domein dat u wilt synchroniseren moet worden vermeld als een stap in elk uitvoeringsprofiel.
4. Sluit de **Uitvoeringsprofielen configureren** dialoogvenster, klikt u op **OK**.
5.  De configuratie te voltooien, moet u uitvoeren een **volledige import** en een **Deltasynchronisatie**. Lees ook de sectie [toepassen en controleer of de wijzigingen](#apply-and-verify-changes).

## <a name="organizational-unitbased-filtering"></a>Organisatie-eenheid – gebaseerd filteren
De beste manier om te filteren op basis van organisatie-eenheid wijzigen is door de installatiewizard uitvoeren en wijzigen van [domein en OE filteren](how-to-connect-install-custom.md#domain-and-ou-filtering). De installatiewizard automatiseert de taken die worden beschreven in dit onderwerp.

Volg deze stappen alleen als u niet om uit te voeren van de installatiewizard om een bepaalde reden.

Voor het configureren van organisatie-eenheid – gebaseerd filteren, voer de volgende stappen uit:

1. Aanmelden bij de server waarop Azure AD Connect-synchronisatie is uitgevoerd met behulp van een account dat lid is van de **ADSyncAdmins** beveiligingsgroep.
2. Start **Synchronization Service** uit de **Start** menu.
3. Selecteer **Connectors**, en klik in de **Connectors** , selecteert u de Connector met het type **Active Directory Domain Services**. In **acties**, selecteer **eigenschappen**.  
   ![Eigenschappen van de connector](./media/how-to-connect-sync-configure-filtering/connectorproperties.png)  
4. Klik op **mappartities configureren**, selecteert u het domein dat u wilt configureren en klik vervolgens op **Containers**.
5. Wanneer u wordt gevraagd, bieden u geen referenties met leestoegang tot uw on-premises Active Directory. Dit hoeft niet te worden van de gebruiker die vooraf is ingevuld in het dialoogvenster.
6. In de **Containers selecteren** dialoogvenster vak, schakelt u de organisatie-eenheden die u niet wilt synchroniseren met de cloud-map en klik vervolgens op **OK**.  
   ![OE's in het dialoogvenster Containers selecteren](./media/how-to-connect-sync-configure-filtering/ou.png)  
   * De **Computers** container moet worden geselecteerd voor uw Windows 10-computers om te worden gesynchroniseerd naar Azure AD. Als uw domein computers bevinden zich in een andere organisatie-eenheden, zorg er dan voor dat die zijn geselecteerd.
   * De container **ForeignSecurityPrincipals** moet zijn geselecteerd als er meerdere forests met vertrouwensrelaties zijn. Dankzij deze container kan het lidmaatschap van de beveiligingsgroep van verschillende forests worden omgezet.
   * De **RegisteredDevices** organisatie-eenheid moet worden geselecteerd als u de functie van de Write-back van apparaat is ingeschakeld. Als u een andere Write-back-functie, zoals Write-back van groep, zorg ervoor dat deze locaties zijn geselecteerd.
   * Selecteer een andere organisatie-eenheid waar gebruikers, iNetOrgPersons, groepen, contactpersonen en Computers zich bevinden. In de gaten, bevinden deze organisatie-eenheden zich in de OU ManagedObjects.
   * Als u filteren op basis van een groep, moet klikt u vervolgens de organisatie-eenheid waar bevindt zich de groep worden opgenomen.
   * Houd er rekening mee dat u configureren kunt of nieuwe OE's die worden toegevoegd nadat de filters gebruiken om configuratie is voltooid, worden gesynchroniseerd of niet is gesynchroniseerd. Zie de volgende sectie voor meer informatie.
7. Wanneer u klaar bent, sluit u de **eigenschappen** dialoogvenster door te klikken op **OK**.
8. De configuratie te voltooien, moet u uitvoeren een **volledige import** en een **Deltasynchronisatie**. Lees ook de sectie [toepassen en controleer of de wijzigingen](#apply-and-verify-changes).

### <a name="synchronize-new-ous"></a>Nieuwe OE's synchroniseren
Nieuwe OE's die zijn gemaakt nadat filteren is geconfigureerd, worden standaard gesynchroniseerd. Deze status wordt aangegeven door het selectievakje is ingeschakeld. U kunt ook de selectie opheffen voor sommige sub-OE's. Als u dit gedrag, klikt u op het vak totdat deze wit met een blauw vinkje (de standaardstatus heeft). Vervolgens Hef de selectie van eventuele sub-OE's die u niet wilt synchroniseren.

Als alle sub-OE's worden gesynchroniseerd, klikt u vervolgens is het selectievakje wit met een blauw vinkje.  
![Organisatie-eenheid met alle selectievakjes zijn geselecteerd](./media/how-to-connect-sync-configure-filtering/ousyncnewall.png)

Als sommige sub-OE's hebt niet geselecteerd, is het selectievakje grijs weergegeven met een wit vinkje.  
![Organisatie-eenheid met sommige sub-OE's niet geselecteerd](./media/how-to-connect-sync-configure-filtering/ousyncnew.png)

Met deze configuratie wordt een nieuwe organisatie-eenheid die is gemaakt onder ManagedObjects gesynchroniseerd.

De Azure AD Connect-installatiewizard wordt altijd gemaakt voor deze configuratie.

### <a name="dont-synchronize-new-ous"></a>Nieuwe OE's niet synchroniseren
De synchronisatie-engine voor het nieuwe OE's niet gesynchroniseerd nadat de filters gebruiken om configuratie is voltooid, kunt u configureren. Deze status wordt aangegeven in de gebruikersinterface van het vak effen grijs met geen vinkje weergegeven. Als u dit gedrag, klikt u op het vak totdat deze wit met geen selectievakje is ingeschakeld. Selecteer vervolgens de sub-OE's die u wilt synchroniseren.

![Organisatie-eenheid met de hoofdmap niet geselecteerd](./media/how-to-connect-sync-configure-filtering/oudonotsyncnew.png)

Met deze configuratie is niet een nieuwe organisatie-eenheid die is gemaakt onder ManagedObjects gesynchroniseerd.

## <a name="attribute-based-filtering"></a>Filteren op basis van een kenmerk
Zorg ervoor dat u de November 2015 ([1.0.9125](reference-connect-version-history.md#1091250)) of later bouwen voor deze stappen om te werken.

> [!IMPORTANT]
>Microsoft raadt aan om te wijzigen niet de standaardregels die zijn gemaakt door **Azure AD Connect**. Als u wijzigen van de regel wilt, deze klonen en de oorspronkelijke regel uitschakelen. Breng wijzigingen aan de gekloonde regel. Houd er rekening mee dat op deze manier (oorspronkelijke regel uitschakelen) u mist eventuele oplossingen voor problemen of functies die door deze regel is ingeschakeld.

Filteren op basis van het kenmerk is de meest flexibele manier om filterobjecten. U kunt de kracht van [declaratieve inrichting](concept-azure-ad-connect-sync-declarative-provisioning.md) voor het beheren van vrijwel elk aspect van wanneer een object wordt gesynchroniseerd met Azure AD.

U kunt toepassen [inkomende](#inbound-filtering) filteren uit Active Directory naar de metaverse en [uitgaande](#outbound-filtering) filteren van metaverse naar Azure AD. Het is raadzaam dat u toepassing inkomende filteren omdat dit het gemakkelijkst te onderhouden. Gebruik alleen uitgaande filteren als dit is vereist voor deelname aan objecten uit meer dan één forest voordat de evaluatie kan plaatsvinden.

### <a name="inbound-filtering"></a>Binnenkomende filters
Filteren van binnenkomende maakt gebruik van de standaardconfiguratie, waarbij objecten gaan met Azure AD moet de metaverse-kenmerk cloudFiltered niet is ingesteld op een waarde kunnen worden gesynchroniseerd. Als de waarde van dit kenmerk is ingesteld op **waar**, en vervolgens het object is niet gesynchroniseerd. Deze mag niet worden ingesteld op **False**, standaard. Om er zeker van te zijn andere regels hebben de mogelijkheid om bij te dragen van een waarde, moet dit kenmerk alleen de waarden hebben **waar** of **NULL** (afwezig).

In het inkomende filteren, gebruikt u de kracht van **bereik** om te bepalen welke objecten om te synchroniseren of niet synchroniseren. Dit is waar u aanpassingen om te voldoen aan de vereisten van uw eigen organisatie. De scope-module bevat een **groep** en een **component** om te bepalen wanneer een synchronisatieregel is binnen het bereik. Een groep bevat een of meer van de EU. Er is een logische 'en' tussen meerdere componenten en een logische 'of' tussen meerdere groepen.

We bekijken een voorbeeld:  
![Bereik](./media/how-to-connect-sync-configure-filtering/scope.png)  
Deze moet worden gelezen als **(afdeling = IT) of (afdeling = verkoop-en c = US)**.

In de volgende voorbeelden en stappen, gebruikt u de gebruikersobject als een voorbeeld, maar u kunt deze gebruiken voor alle objecttypen.

In de volgende voorbeelden wordt de prioriteitswaarde begint met 50. Dit is een nummer niet gebruikt, maar moet lager zijn dan 100 zijn.

#### <a name="negative-filtering-do-not-sync-these"></a>Negatieve filteren: "worden niet gesynchroniseerd deze"
In het volgende voorbeeld u wegfilteren (niet synchroniseren) alle gebruikers waar **extensionAttribute15** heeft de waarde **NoSync**.

1. Aanmelden bij de server waarop Azure AD Connect-synchronisatie is uitgevoerd met behulp van een account dat lid is van de **ADSyncAdmins** beveiligingsgroep.
2. Start **Synchronization Rules Editor** uit de **Start** menu.
3. Zorg ervoor dat **inkomend** is geselecteerd en klik op **nieuwe regel toevoegen**.
4. Geef de regel een beschrijvende naam, zoals '*In uit Active Directory-gebruiker DoNotSyncFilter*'. Het juiste forest selecteert, selecteert u **gebruiker** als de **CS objecttype**, en selecteer **persoon** als de **MV-objecttype**. In **koppelingstype**, selecteer **Join**. In **prioriteit**, typ een waarde die momenteel niet wordt gebruikt door andere synchronisatieregels (bijvoorbeeld 50), en klik vervolgens op **volgende**.  
   ![Inkomende 1 Beschrijving](./media/how-to-connect-sync-configure-filtering/inbound1.png)  
5. In **Scoping filter**, klikt u op **groep toevoegen**, en klikt u op **component toevoegen**. In **kenmerk**, selecteer **ExtensionAttribute15**. Zorg ervoor dat **Operator** is ingesteld op **gelijk**, en typ de waarde **NoSync** in de **waarde** vak. Klik op **volgende**.  
   ![Inkomende 2 bereik](./media/how-to-connect-sync-configure-filtering/inbound2.png)  
6. Laat de **Join** regels leeg en klik vervolgens op **volgende**.
7. Klik op **transformatie toevoegen**, selecteer de **FlowType** als **constante**, en selecteer **cloudFiltered** als de **doel Kenmerk**. In de **bron** in het tekstvak **waar**. Klik op **toevoegen** op de regel niet opslaan.  
   ![Inkomende 3 transformatie](./media/how-to-connect-sync-configure-filtering/inbound3.png)
8. De configuratie te voltooien, moet u uitvoeren een **Full sync**. Lees ook de sectie [toepassen en controleer of de wijzigingen](#apply-and-verify-changes).

#### <a name="positive-filtering-only-sync-these"></a>Positieve filteren: "alleen synchroniseren deze"
Uitdrukken positieve filteren kan lastig zijn meer hebt u ook rekening houden met objecten die niet kunnen worden gesynchroniseerd, zoals vergaderruimten duidelijk. U gaat ook het standaardfilter in de out-of-box-regel onderdrukken **In uit Active Directory - gebruiker toevoegen**. Wanneer u uw aangepaste filter maakt, zorg ervoor dat u geen essentiële systeemobjecten, replicatie conflict objecten, speciale postvakken en de service-accounts voor Azure AD Connect.

Het positieve filteroptie vereist twee synchronisatieregels. U moet één regel (of verschillende) met het juiste bereik van objecten te synchroniseren. U moet ook een tweede catch-all synchronisatieregel gefilterd op alle objecten die nog niet hebt zijn geïdentificeerd als een object dat moet worden gesynchroniseerd.

In het volgende voorbeeld wordt u alleen objecten waarin het kenmerk voor afdeling de waarde heeft synchroniseren **verkoop**.

1. Aanmelden bij de server waarop Azure AD Connect-synchronisatie is uitgevoerd met behulp van een account dat lid is van de **ADSyncAdmins** beveiligingsgroep.
2. Start **Synchronization Rules Editor** uit de **Start** menu.
3. Zorg ervoor dat **inkomend** is geselecteerd en klik op **nieuwe regel toevoegen**.
4. Geef de regel een beschrijvende naam, zoals '*In uit Active Directory-gebruiker verkoop synchroniseren*'. Het juiste forest selecteert, selecteert u **gebruiker** als de **CS objecttype**, en selecteer **persoon** als de **MV-objecttype**. In **koppelingstype**, selecteer **Join**. In **prioriteit**, typ een waarde die momenteel niet wordt gebruikt door andere synchronisatieregels (bijvoorbeeld 51) en klik vervolgens op **volgende**.  
   ![Inkomende 4 beschrijving](./media/how-to-connect-sync-configure-filtering/inbound4.png)  
5. In **Scoping filter**, klikt u op **groep toevoegen**, en klikt u op **component toevoegen**. In **kenmerk**, selecteer **afdeling**. Zorg ervoor dat de Operator is ingesteld op **gelijk**, en typ de waarde **verkoop** in de **waarde** vak. Klik op **volgende**.  
   ![Inkomende 5 bereik](./media/how-to-connect-sync-configure-filtering/inbound5.png)  
6. Laat de **Join** regels leeg en klik vervolgens op **volgende**.
7. Klik op **transformatie toevoegen**, selecteer **constante** als de **FlowType**, en selecteer de **cloudFiltered** als de **doel Kenmerk**. In de **bron** in het vak **False**. Klik op **toevoegen** op de regel niet opslaan.  
   ![Inkomende 6 transformatie](./media/how-to-connect-sync-configure-filtering/inbound6.png)  
   Dit is een speciaal geval waar u expliciet cloudFiltered instellen op **False**.
8. We hebben nu de catch-all synchronisatieregel maken. Geef de regel een beschrijvende naam, zoals '*In uit Active Directory-gebruiker Catch-all-filter*'. Het juiste forest selecteert, selecteert u **gebruiker** als de **CS objecttype**, en selecteer **persoon** als de **MV-objecttype**. In **koppelingstype**, selecteer **Join**. In **prioriteit**, typ een waarde die momenteel niet wordt gebruikt door andere synchronisatieregels (bijvoorbeeld 99). U kunt een prioriteitswaarde die hoger (lagere prioriteit) dan de vorige synchronisatieregel hebt geselecteerd. Maar u hebt ook wat ruimte maken zodat u meer synchronisatie-filterregels later toevoegen kunt als u wilt beginnen met het synchroniseren van extra diensten. Klik op **volgende**.  
   ![Inkomende 7 beschrijving](./media/how-to-connect-sync-configure-filtering/inbound7.png)  
9. Laat **Scoping filter** leeg en klik op **volgende**. Een leeg filter geeft aan dat de regel moet worden toegepast op alle objecten.
10. Laat de **Join** regels leeg en klik vervolgens op **volgende**.
11. Klik op **transformatie toevoegen**, selecteer **constante** als de **FlowType**, en selecteer **cloudFiltered** als de **doel Kenmerk**. In de **bron** in het vak **waar**. Klik op **toevoegen** op de regel niet opslaan.  
    ![Inkomende 3 transformatie](./media/how-to-connect-sync-configure-filtering/inbound3.png)  
12. De configuratie te voltooien, moet u uitvoeren een **Full sync**. Lees ook de sectie [toepassen en controleer of de wijzigingen](#apply-and-verify-changes).

Als u wilt, kunt u meer regels van het eerste type waar u meer objecten in de synchronisatie opnemen.

### <a name="outbound-filtering"></a>Uitgaande filteren
In sommige gevallen is het nodig zijn om u te doen het filter alleen nadat de objecten hebt toegevoegd in de metaverse. Het kan bijvoorbeeld nodig zijn om te kijken naar het e-mailkenmerk uit de bron-forest en het kenmerk userPrincipalName uit het accountforest, om te bepalen als een object moet worden gesynchroniseerd zijn. In dergelijke gevallen maakt u filteren op de regel voor uitgaande.

In dit voorbeeld wijzigt u de filteren zodanig dat alleen gebruikers die hebben hun e-mail en de userPrincipalName hebben die eindigt op @contoso.com worden gesynchroniseerd:

1. Aanmelden bij de server waarop Azure AD Connect-synchronisatie is uitgevoerd met behulp van een account dat lid is van de **ADSyncAdmins** beveiligingsgroep.
2. Start **Synchronization Rules Editor** uit de **Start** menu.
3. Onder **regels Type**, klikt u op **uitgaand**.
4. Afhankelijk van de versie van Connect u gebruikt, ofwel de regel met de naam vinden **Out voor AAD-gebruiker toevoegen** of **uit voor AAD - gebruiker toevoegen SOAInAD**, en klikt u op **bewerken**.
5. In het pop-upvenster beantwoorden **Ja** om een kopie van de regel te maken.
6. Op de **beschrijving** pagina, wijzigt u **prioriteit** op een niet-gebruikte waarde, zoals 50.
7. Klik op **Scoping filter** op de navigatiebalk aan de linkerkant en klik vervolgens op **toevoegen component**. In **kenmerk**, selecteer **e-mail**. In **Operator**, selecteer **ENDSWITH**. In **waarde**, type **@contoso.com**, en klik vervolgens op **toevoegen component**. In **kenmerk**, selecteer **userPrincipalName**. In **Operator**, selecteer **ENDSWITH**. In **waarde**, type **@contoso.com**.
8. Klik op **Opslaan**.
9. De configuratie te voltooien, moet u uitvoeren een **Full sync**. Lees ook de sectie [toepassen en controleer of de wijzigingen](#apply-and-verify-changes).

## <a name="apply-and-verify-changes"></a>Toepassen en wijzigingen controleren
Nadat u uw wijzigingen hebt aangebracht, moet u deze toepassen op de objecten die al aanwezig zijn in het systeem. Het kan ook zijn dat de objecten die zich niet op dit moment in de synchronisatie-engine moeten worden verwerkt (en de synchronisatie-engine moet het bronsysteem opnieuw uit om te controleren of de inhoud lezen).

Als u de configuratie hebt gewijzigd met behulp van **domein** of **organisatie-eenheid** filteren, moet u een **volledige import**, gevolgd door **Delta synchronisatie**.

Als u de configuratie hebt gewijzigd met behulp van **kenmerk** filteren, moet u een **volledige synchronisatie**.

Voer de volgende stappen uit:

1. Start **Synchronization Service** uit de **Start** menu.
2. Selecteer **Connectors**. In de **Connectors** , selecteert u de Connector waar u eerder een configuratiewijziging hebt gemaakt. In **acties**, selecteer **uitvoeren**.  
   ![Connector uitvoeren](./media/how-to-connect-sync-configure-filtering/connectorrun.png)  
3. In **Uitvoeringsprofielen**, selecteer de bewerking die is vermeld in de vorige sectie. Als u twee acties uitgevoerd moet, voert u de tweede nadat het eerste item is voltooid. (De **status** kolom **niet-actief** voor de geselecteerde connector.)

Na de synchronisatie worden alle wijzigingen klaargezet om te worden geëxporteerd. Voordat u daadwerkelijk de wijzigingen in Azure AD, die u wilt controleren of dergelijke wijzigingen correct zijn.

1. Start een opdrachtprompt en Ga naar `%Program Files%\Microsoft Azure AD Sync\bin`.
2. Voer `csexport "Name of Connector" %temp%\export.xml /f:x` uit.  
   De naam van de Connector is in de synchronisatie-Service. Er is een naam die lijkt op 'contoso.com – AAD' voor Azure AD.
3. Voer `CSExportAnalyzer %temp%\export.xml > %temp%\export.csv` uit.
4. U hebt nu een bestand in % temp % met de naam export.csv die kunnen worden onderzocht in Microsoft Excel. Dit bestand bevat alle wijzigingen die moeten worden geëxporteerd.
5. Breng de benodigde wijzigingen aan de gegevens of configuratie en voer deze stappen opnieuw (importeren, synchroniseren en controleer of) totdat de wijzigingen die zullen worden uitgevoerd, wat u verwacht.

Wanneer u tevreden bent, exporteert u de wijzigingen naar Azure AD.

1. Selecteer **Connectors**. In de **Connectors** , selecteert u de Azure AD-Connector. In **acties**, selecteer **uitvoeren**.
2. In **Uitvoeringsprofielen**, selecteer **exporteren**.
3. Als uw configuratiewijzigingen veel objecten verwijderen, klikt u vervolgens ziet u een fout in de uitvoer wanneer het aantal hoger dan de geconfigureerde drempelwaarde (standaard 500 is). Als u deze fout ziet, moet u tijdelijk uit te schakelen de "[onopzettelijke verwijderingen voorkomen](how-to-connect-sync-feature-prevent-accidental-deletes.md)" functie.

Het is nu tijd om de scheduler opnieuw inschakelen.

1. Start **Task Scheduler** uit de **Start** menu.
2. Direct onder de **bibliotheek voor Taakplanner**, de taak met de naam niet vinden **Azure AD Sync Scheduler**, klik met de rechtermuisknop en selecteer **inschakelen**.

## <a name="group-based-filtering"></a>Groep filteren
U kunt configureren op basis van een groep filteren van de eerste keer dat u Azure AD Connect met behulp van installeert [aangepaste installatie](how-to-connect-install-custom.md#sync-filtering-based-on-groups). Het bedoeld voor een testimplementatie waar u alleen een kleine set van objecten moeten worden gesynchroniseerd. Wanneer u de groep filteren uitschakelt, kunnen deze kan niet worden ingeschakeld. Er *niet ondersteund* te gebruiken op basis van een groep filteren in een aangepaste configuratie. Deze functie configureren met behulp van de installatiewizard wordt alleen ondersteund. Wanneer u uw proefproject hebt voltooid, voert u een van de andere opties voor het filteren in dit onderwerp. Wanneer u filteren op basis van organisatie-eenheid in combinatie met filteren op basis van een groep, is de OU('s) waar de groep en de bijbehorende leden zich bevinden moet worden opgenomen.

Bij het synchroniseren van meerdere AD-forests, kunt u filteren op basis van een groep door op te geven van een andere groep voor elk AD-connector kunt configureren. Als u wilt om te synchroniseren van een gebruiker in een AD-forest en dezelfde gebruiker heeft een of meer van de bijbehorende objecten in andere AD-forests, moet u ervoor zorgen dat het gebruikersobject en alle bijbehorende objecten binnen de groep op basis van filter bereik. Voor voorbeelden:

* Er is een gebruiker in een forest met een bijbehorende FSP (afwijkende beveiligings-Principal)-object in een ander forest. Beide objecten moeten worden binnen op basis van een groep filteren bereik. De gebruiker worden, anders niet gesynchroniseerd naar Azure AD.

* Er is een gebruiker in een forest met een bijbehorende resource-account (bijvoorbeeld gekoppeld postvak) in een ander forest. Bovendien hebt u Azure AD Connect voor het koppelen van de gebruiker met het resource-account geconfigureerd. Beide objecten moeten worden binnen op basis van een groep filteren bereik. De gebruiker worden, anders niet gesynchroniseerd naar Azure AD.

* Er is een gebruiker in een forest met een bijbehorende e-mail contact op met in een ander forest. Bovendien hebt u Azure AD Connect voor het koppelen van de gebruiker met de e-mail contact op met geconfigureerd. Beide objecten moeten worden binnen op basis van een groep filteren bereik. De gebruiker worden, anders niet gesynchroniseerd naar Azure AD.


## <a name="next-steps"></a>Volgende stappen
- Meer informatie over [Azure AD Connect-synchronisatie](how-to-connect-sync-whatis.md) configuratie.
- Meer informatie over [uw on-premises identiteiten integreren met Azure AD](whatis-hybrid-identity.md).
