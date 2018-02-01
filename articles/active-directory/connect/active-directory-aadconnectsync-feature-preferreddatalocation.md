---
title: 'Azure AD Connect-synchronisatie: locatie van de gewenste gegevens voor Office 365-gebruikers configureren | Microsoft Docs'
description: Beschrijft hoe u uw Office 365-Gebruikersbronnen dicht bij de gebruiker met Azure AD Connect-synchronisatie.
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
ms.date: 01/13/2018
ms.author: billmath
ms.openlocfilehash: 73b9b8d208b5eac2e62f62ab786efafa056e3cb4
ms.sourcegitcommit: ded74961ef7d1df2ef8ffbcd13eeea0f4aaa3219
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2018
---
# <a name="azure-ad-connect-sync-configure-preferred-data-location-for-office-365-resources"></a>Azure AD Connect-synchronisatie: locatie van de gewenste gegevens voor Office 365-bronnen configureren
Het doel als dit onderwerp helpt u bij het configureren van preferredDataLocation in Azure AD Connect-synchronisatie. Dit kenmerk wordt gebruikt om aan te geven op Office 365 waarin de gebruiker zich bevindt, zodat de resources dicht bij de gebruiker kunnen worden geplaatst. Deze functie is bedoeld voor grotere klanten.

> [!IMPORTANT]
> Deze functie is momenteel in preview en in de cloud standaard uitgeschakeld. Als u deelnemen aan het programma preview wilt, klikt u vervolgens contact op met uw Microsoft-vertegenwoordiger.
>
>

## <a name="enable-synchronization-of-preferreddatalocation"></a>Synchronisatie van PreferredDataLocation inschakelen
Standaard bevinden Office 365-resources voor uw gebruikers zich in dezelfde regio bevinden als uw Azure AD-tenant. Bijvoorbeeld, als uw tenant bevindt zich in Noord-Amerika zich vervolgens de Exchange-postvakken van gebruikers bevinden eveneens in Noord-Amerika. Voor een organisatie meerdere nationale dit mogelijk niet optimaal. Door het instellen van het kenmerk preferredDataLocation kan regio van de gebruiker worden gedefinieerd.

Instellingen voor dit kenmerk kunt u de gebruiker Office 365-resources, zoals de postvak en OneDrive, hebben in dezelfde regio bevinden als de gebruiker en nog steeds een tenant voor uw hele organisatie.

> [!IMPORTANT]
> U moet ten minste 5000 plaatsen in uw Office 365-abonnement hebben voor deze functie in aanmerking komt.
>
>

De regio's in Office 365 zijn:

| Regio | Beschrijving |
| --- | --- |
| NAAM | Noord-Amerika |
| EUR | Europa |
| APC | Azië en Stille Oceaan |
| JPN | Japan |
| AUS | Australië |
| CAN | Canada |
| GBR | Groot-Brittannië |
| LAM | Latijns-Amerika |

Niet alle Office 365-werkbelastingen ondersteunt het gebruik van het instellen van een gebruiker regio.

Azure AD Connect ondersteunt synchronisatie van de **PreferredDataLocation** kenmerk voor **gebruiker** objecten in versie 1.1.524.0 en na. Meer specifiek, zijn de volgende wijzigingen geïntroduceerd:

* Het schema van het objecttype **gebruiker** in de Azure AD-Connector is uitgebreid met PreferredDataLocation kenmerk van het type één waarde tekenreeks.

* Het schema van het objecttype **persoon** in de Metaverse is uitgebreid met PreferredDataLocation kenmerk is van het type tekenreeks en één waarde.

Het kenmerk PreferredDataLocation is standaard niet ingeschakeld voor synchronisatie. Deze functie is bedoeld voor grote organisaties en niet iedereen wilt profiteren van deze. U moet ook een kenmerk voor het opslaan van de Office 365-regio voor uw gebruikers, omdat er geen kenmerk PreferredDataLocation in de lokale Active Directory is identificeren. Dit is het verstandig om verschillende voor elke organisatie.

> [!IMPORTANT]
> Azure AD kan op dit moment is dat het kenmerk PreferredDataLocation op gesynchroniseerde gebruikersobjecten en cloud gebruiker objecten rechtstreeks worden geconfigureerd met behulp van Azure AD PowerShell. Wanneer u de synchronisatie van het kenmerk PreferredDataLocation hebt ingeschakeld, moet u stoppen met Azure AD PowerShell voor het configureren van het kenmerk op **gesynchroniseerd gebruikersobjecten** als Azure AD Connect overschrijft toe op basis van de bron-kenmerkwaarden in de lokale Active Directory.

> [!IMPORTANT]
> Sinds 1 September 2017 Azure AD niet meer kan het kenmerk PreferredDataLocation op **gesynchroniseerd gebruikersobjecten** rechtstreeks worden geconfigureerd met behulp van Azure AD PowerShell. Als u wilt configureren PreferredLocation-kenmerk op gesynchroniseerde gebruikersobjecten, moet u Azure AD Connect.

Voordat u de synchronisatie van het kenmerk PreferredDataLocation inschakelt, moet u het volgende doen:

 * Bepaal eerst welke lokale Active Directory-kenmerk moet worden gebruikt als het bronkenmerk. Deze moet van het type **één waarde tekenreeks**. In de stappen onder een van de extensionAttributes wordt gebruikt.

 * Als u het kenmerk PreferredDataLocation eerder hebt geconfigureerd op bestaande gebruikersobjecten gesynchroniseerd in Azure AD dat gebruikmaakt van Azure AD PowerShell, moet u **backport** de kenmerkwaarden voor de bijbehorende gebruikersobjecten in de lokale Active Directory.

    > [!IMPORTANT]
    > Als u niet backport de kenmerkwaarden voor de bijbehorende gebruikersobjecten in de lokale Active Directory dit, wordt de bestaande kenmerkwaarden in Azure AD Connect verwijderd in Azure AD wanneer synchronisatie voor het kenmerk PreferredDataLocation is ingeschakeld.

 * Het wordt aanbevolen configureren van het bronkenmerk op ten minste twee lokale AD-gebruiker objecten nu, die kan worden gebruikt voor verificatie later.

De stappen voor het inschakelen van synchronisatie van het kenmerk PreferredDataLocation kunnen worden samengevat als:

1. Schakel de sync scheduler uit en controleer of dat er vindt geen synchronisatie uitgevoerd
2. Het bronkenmerk toevoegen aan het schema van lokale ADDS-connector
3. PreferredDataLocation toevoegen aan het schema van Azure AD-connector
4. Maken van een synchronisatieregel voor binnenkomende om de stroom van de waarde van het kenmerk van de lokale Active Directory
5. Een uitgaande synchronisatieregel maken om te laten doorlopen van de waarde van het kenmerk naar Azure AD
6. Volledige synchronisatiecyclus uitvoeren
7. Sync scheduler inschakelen
8. Controleer of het resultaat

> [!NOTE]
> De rest van deze sectie bevat informatie over deze stappen in de details. Ze worden beschreven in de context van een Azure AD-implementatie met één forest topologie en zonder aangepaste synchronisatieregels. Als u een topologie met meerdere forests hebt, aangepaste synchronisatieregels geconfigureerd of een tijdelijke server hebt, moet u de stappen overeenkomstig aanpassen.

## <a name="step-1-disable-sync-scheduler-and-verify-there-is-no-synchronization-in-progress"></a>Stap 1: Schakel de sync scheduler uit en controleer of dat er vindt geen synchronisatie uitgevoerd
Zorg ervoor dat er geen synchronisatie plaats terwijl u bent in het midden van het bijwerken van synchronisatieregels om onbedoelde wijzigingen wordt geëxporteerd naar Azure AD te voorkomen. De ingebouwde sync scheduler uitschakelen:

1. Start een PowerShell-sessie op de Azure AD Connect-server.
2. Geplande synchronisatie uitschakelen door de cmdlet: `Set-ADSyncScheduler -SyncCycleEnabled $false`.
3. Start de **Synchronization Service Manager** door te gaan naar **START** > **synchronisatieservice**.
4. Ga naar de **Operations** tabblad en er is geen bewerking met de status bevestigen *Bezig*.

![Controleer de Synchronization Service Manager - er zijn geen bewerkingen uitgevoerd](./media/active-directory-aadconnectsync-feature-preferreddatalocation/preferreddatalocation-step1.png)

### <a name="step-2-add-the-source-attribute-to-the-on-premises-adds-connector-schema"></a>Stap 2: Het bronkenmerk toevoegen aan het schema van lokale ADDS-connector
Niet alle AD-kenmerken worden geïmporteerd in de on-premises AD Connectorgebied overgebracht. Als u hebt geselecteerd om te gebruiken van een kenmerk is niet standaard gesynchroniseerd, moet u het importeren. Het bronkenmerk toevoegen aan de lijst van de geïmporteerde kenmerken:

1. Ga naar de **Connectors** tabblad Synchronization Service Manager.
2. Met de rechtermuisknop op de **on-premises AD-Connector** en selecteer **eigenschappen**.
3. In het pop-updialoogvenster, gaat u naar de **kenmerken selecteren** tabblad.
4. Zorg ervoor dat het bronkenmerk die u hebt geselecteerd om te gebruiken in de kenmerkenlijst is ingeschakeld. Als u het kenmerk niet ziet, klikt u op het selectievakje 'Alles weergeven'.
5. Klik op **OK** om op te slaan.

![Bronkenmerk toevoegen met on-premises AD-Connector schema](./media/active-directory-aadconnectsync-feature-preferreddatalocation/preferreddatalocation-step2.png)

## <a name="step-3-add-preferreddatalocation-to-the-azure-ad-connector-schema"></a>Stap 3: PreferredDataLocation toevoegen aan het schema van Azure AD-connector
Standaard wordt het kenmerk PreferredDataLocation niet geïmporteerd in de ruimte van Azure AD-connector. Het kenmerk PreferredDataLocation toevoegen aan de lijst met geïmporteerde kenmerken:

1. Ga naar de **Connectors** tabblad Synchronization Service Manager.
2. Met de rechtermuisknop op de **Azure AD-connector** en selecteer **eigenschappen**.
3. In het pop-updialoogvenster, gaat u naar de **kenmerken selecteren** tabblad.
4. Selecteer het preferredDataLocation-kenmerk in de lijst met kenmerken.
5. Klik op **OK** om op te slaan.

![Bronkenmerk toevoegen aan Azure AD-Connector schema](./media/active-directory-aadconnectsync-feature-preferreddatalocation/preferreddatalocation-step3.png)

### <a name="step-4-create-an-inbound-synchronization-rule-to-flow-the-attribute-value-from-on-premises-active-directory"></a>Stap 4: Een synchronisatieregel voor binnenkomende om de stroom van de waarde van het kenmerk van de lokale Active Directory maken
De synchronisatieregel voor binnenkomende wordt toegestaan de waarde van het kenmerk vanuit het bronkenmerk van de lokale Active Directory naar de Metaverse:

1. Start de **synchronisatie regeleditor** door te gaan naar **START** > **synchronisatie regeleditor**.
2. Het zoekfilter ingesteld **richting** worden **inkomend**.
3. Klik op **nieuwe regel toevoegen** om te maken van een nieuwe regel voor binnenkomende verbindingen.
4. Onder de **beschrijving** tabblad, bieden de volgende configuratie:

    | Kenmerk | Waarde | Details |
    | --- | --- | --- |
    | Naam | *Geef een naam* | Bijvoorbeeld: *' In uit Active Directory-gebruiker PreferredDataLocation '* |
    | Beschrijving | *Geef een aangepaste beschrijving* |  |
    | Verbonden systeem | *Kies de on-premises AD-connector* |  |
    | Verbonden systeem objecttype | **User** |  |
    | Metaverse-objecttype | **Person** |  |
    | Koppelingstype | **Koppelen** |  |
    | Prioriteit | *Kies een getal tussen 1-99* | 1-99 is gereserveerd voor aangepaste synchronisatie regels. Een waarde die wordt gebruikt door andere synchronisatieregels niet opgenomen. |

5. Houd de **Scoping filter** leeg voor alle objecten worden opgenomen. Mogelijk moet u het bereik filter op basis van uw Azure AD Connect-implementatie aanpassen.
6. Ga naar de **transformatie tabblad** en implementeren van de volgende transformatieregel:

    | Stroomtype | Doelkenmerk | Bron | Eenmaal toepassen | Type samenvoeging |
    | --- | --- | --- | --- | --- |
    |Direct | PreferredDataLocation | Kies het bronkenmerk | Dit selectievakje is uitgeschakeld | Update |

7. Klik op **toevoegen** om de binnenkomende regel te maken.

![Synchronisatieregel voor binnenkomende maken](./media/active-directory-aadconnectsync-feature-preferreddatalocation/preferreddatalocation-step4.png)

## <a name="step-5-create-an-outbound-synchronization-rule-to-flow-the-attribute-value-to-azure-ad"></a>Stap 5: Maak een synchronisatieregel voor uitgaande om de stroom van de waarde van het kenmerk naar Azure AD
De uitgaande synchronisatieregel wordt toegestaan de waarde van het kenmerk stromen van Metaverse met het kenmerk PreferredDataLocation in Azure AD:

1. Ga naar de **synchronisatieregels** Editor.
2. Het zoekfilter ingesteld **richting** worden **uitgaand**.
3. Klik op **nieuwe regel toevoegen** knop.
4. Onder de **beschrijving** tabblad, bieden de volgende configuratie:

    | Kenmerk | Waarde | Details |
    | ----- | ------ | --- |
    | Naam | *Geef een naam* | Bijvoorbeeld 'uit naar AAD – gebruiker PreferredDataLocation' |
    | Beschrijving | *Geef een beschrijving* ||
    | Verbonden systeem | *Selecteer de AAD-connector* ||
    | Verbonden systeem objecttype | Gebruiker ||
    | Metaverse-objecttype | **Person** ||
    | Koppelingstype | **Koppelen** ||
    | Prioriteit | *Kies een getal tussen 1-99* | 1-99 is gereserveerd voor aangepaste synchronisatie regels. Een waarde die wordt gebruikt door andere synchronisatieregels niet opgenomen. |

5. Ga naar de **Scoping filter** tabblad en voeg een **één filter bereikgroep met twee componenten**:

    | Kenmerk | Operator | Waarde |
    | --- | --- | --- |
    | sourceObjectType | GELIJK ZIJN AAN | Gebruiker |
    | cloudMastered | NOTEQUAL | True |

    Bereik filter bepaalt welke deze uitgaande synchronisatieregel wordt toegepast op Azure AD-objecten. In dit voorbeeld gebruiken we het filter voor hetzelfde bereik uit 'Out naar AD-gebruikers-id' OOB-synchronisatieregel. Dit voorkomt dat de synchronisatieregel wordt toegepast op gebruikers-objecten die niet zijn gesynchroniseerd vanuit de lokale Active Directory. Mogelijk moet u het bereik filter op basis van uw Azure AD Connect-implementatie aanpassen.

6. Ga naar de **transformatie** tabblad en de volgende transformatieregel implementeren:

    | Stroomtype | Doelkenmerk | Bron | Eenmaal toepassen | Type samenvoeging |
    | --- | --- | --- | --- | --- |
    | Direct | PreferredDataLocation | PreferredDataLocation | Dit selectievakje is uitgeschakeld | Update |

7. Sluit **toevoegen** de uitgaande regel maken.

![Uitgaande synchronisatieregel maken](./media/active-directory-aadconnectsync-feature-preferreddatalocation/preferreddatalocation-step5.png)

## <a name="step-6-run-full-synchronization-cycle"></a>Stap 6: De volledige synchronisatie uitvoeren cyclus
In het algemeen volledige synchronisatiecyclus is vereist, omdat we nieuwe kenmerken hebt toegevoegd aan zowel de advertentie en Azure AD-Connector schema en geïntroduceerd aangepaste synchronisatieregels. Het is raadzaam om de wijzigingen te controleren voordat deze naar Azure AD worden geëxporteerd. U kunt de volgende stappen gebruiken om te controleren of de wijzigingen tijdens de stappen die gezamenlijk een volledige synchronisatiecyclus handmatig uit te voeren.

1. Voer **volledige import** stap op het **on-premises AD-Connector**:

   1. Ga naar de **Operations** tabblad Synchronization Service Manager.
   2. Met de rechtermuisknop op de **on-premises AD-Connector** en selecteer **uitvoeren...** .
   3. Selecteer in het pop-updialoogvenster **volledige Import** en klik op **OK**.
   4. Wacht u totdat de bewerking is voltooid.

    > [!NOTE]
    > U kunt de volledige Import overslaan op de on-premises AD-Connector als het bronkenmerk al in de lijst met opgenomen is kenmerken geïmporteerd. Met andere woorden, u hebt geen wijzigt tijdens [stap 2: het bronkenmerk toevoegen aan de on-premises AD-Connector schema](#step-2-add-the-source-attribute-to-the-on-premises-adds-connector-schema).

2. Voer **volledige import** stap op het **Azure AD-Connector**:

   1. Met de rechtermuisknop op de **Azure AD-Connector** en selecteer **uitvoeren...**
   2. Selecteer in het pop-updialoogvenster **volledige Import** en klik op **OK**.
   3. Wacht u totdat de bewerking is voltooid.

3. Controleer of de wijzigingen in synchronisatie-regel op een bestaande gebruikersobject:

Het bronkenmerk van lokale Active Directory en PreferredDataLocation van Azure AD zijn geïmporteerd naar de respectieve connectorgebied overgebracht. Voordat u doorgaat met de stap volledige synchronisatie wordt aanbevolen dat u wilt een **Preview** object op een bestaande gebruiker in de on-premises AD connectorgebied overgebracht. Het object dat u verzameld, moet het bronkenmerk ingevuld hebben. Een geslaagde **Preview** is een goede indicatie dat u de synchronisatie hebt geconfigureerd correct regels met de PreferredDataLocation ingevuld in de Metaverse. Voor informatie over hoe u een **Preview**, Raadpleeg het gedeelte [Controleer of de wijziging](active-directory-aadconnectsync-change-the-configuration.md#verify-the-change).

4. Voer **volledige synchronisatie** stap op het **on-premises AD-Connector**:

   1. Met de rechtermuisknop op de **on-premises AD-Connector** en selecteer **uitvoeren...** .
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

   1. Met de rechtermuisknop op de **Azure AD-Connector** en selecteer **uitvoeren...** .
   2. Selecteer in het pop-upvenster Connector uitvoeren **exporteren** en klik op **OK**.
   3. Wacht u totdat de Export naar Azure AD te voltooien.

> [!NOTE]
> U merkt u wellicht dat de stappen geen de stap volledige synchronisatie op de Azure AD-connector en exporteren van de AD-connector bevatten. De stappen zijn niet vereist omdat de kenmerkwaarden van de lokale Active Directory naar Azure AD alleen stromen.

## <a name="step-7-re-enable-sync-scheduler"></a>Stap 7: Sync scheduler opnieuw inschakelen
De ingebouwde sync scheduler opnieuw inschakelen:

1. Start PowerShell-sessie.
2. Geplande synchronisatie opnieuw inschakelen door de cmdlet uit te voeren:`Set-ADSyncScheduler -SyncCycleEnabled $true`

## <a name="step-8-verify-the-result"></a>Stap 8: Controleer of het resultaat
Het is nu tijd om te controleren of de configuratie en inschakelen voor uw gebruikers.

1. De regio aan het geselecteerde kenmerk van een gebruiker toevoegen. De lijst met beschikbare regio's vindt u in [deze tabel](#enable-synchronization-of-preferreddatalocation).  
![AD-kenmerk toegevoegd aan een gebruiker](./media/active-directory-aadconnectsync-feature-preferreddatalocation/preferreddatalocation-adattribute.png)
2. Wachten op het kenmerk moeten worden gesynchroniseerd naar Azure AD.
3. Met behulp van Exchange Online PowerShell, Controleer of de regio van het Postvak correct is ingesteld.  
![Postvak regio ingesteld voor een gebruiker een Exchange Online](./media/active-directory-aadconnectsync-feature-preferreddatalocation/preferreddatalocation-mailboxregion.png)  
Ervan uitgaande dat uw tenant is gemarkeerd als u deze functie wilt gebruiken, wordt het postvak verplaatst naar de juiste regio. Dit kan worden gecontroleerd door te zoeken op naam van de server waar het postvak zich bevindt.
4. Om te controleren of deze instelling is effectieve via veel postvakken, gebruikt u het script in de [Technet-galerie](https://gallery.technet.microsoft.com/office/PowerShell-Script-to-a6bbfc2e). Dit script heeft ook een lijst met alle Office 365 datacenters server voorvoegsels en welke regio bevindt het zich in. Deze kan worden gebruikt als een verwijzing in de vorige stap om te controleren of de locatie van het postvak.

## <a name="next-steps"></a>Volgende stappen
* Meer informatie over de configuratiemodel in [Understanding declaratieve inrichting](active-directory-aadconnectsync-understanding-declarative-provisioning.md).
* Meer informatie over de expressietaal in [Understanding declaratieve inrichting expressies](active-directory-aadconnectsync-understanding-declarative-provisioning-expressions.md).

**Overzichtsonderwerpen**

* [Azure AD Connect-synchronisatie: inzicht en synchronisatie aanpassen](active-directory-aadconnectsync-whatis.md)
* [Uw on-premises identiteiten integreren met Azure Active Directory](active-directory-aadconnect.md)
