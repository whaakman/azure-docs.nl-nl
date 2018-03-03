---
title: 'Azure Active Directory Connect-synchronisatie: locatie van de gewenste gegevens voor Multi-Geo-mogelijkheden configureren in Office 365 | Microsoft Docs'
description: Beschrijft hoe u uw Office 365-Gebruikersbronnen dicht bij de gebruiker met Azure Active Directory Connect-synchronisatie te plaatsen.
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
ms.date: 01/31/2018
ms.author: billmath
ms.openlocfilehash: a5ebd61539af7116b8f92cdf9404cd2b5cdea193
ms.sourcegitcommit: 782d5955e1bec50a17d9366a8e2bf583559dca9e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/02/2018
---
# <a name="azure-active-directory-connect-sync-configure-preferred-data-location-for-office-365-resources"></a>Azure Active Directory Connect-synchronisatie: locatie van de gewenste gegevens voor Office 365-bronnen configureren
Het doel van dit onderwerp wordt u stapsgewijs door het configureren van het kenmerk voor de gegevenslocatie van de gewenste in Azure Active Directory (Azure AD) Connect-synchronisatie. Wanneer iemand Multi-Geo-mogelijkheden in Office 365 gebruikt, kunt u dit kenmerk gebruiken om aan te wijzen de geografische locatie van de Office 365-gegevens van de gebruiker. (De voorwaarden *regio* en *geo* door elkaar worden gebruikt.)

> [!IMPORTANT]
> Multi-geografisch is momenteel in preview. Als u deelnemen aan het programma preview wilt, neem dan contact op met uw Microsoft-vertegenwoordiger.
>
>

## <a name="enable-synchronization-of-preferred-data-location"></a>Synchronisatie van de locatie van de gewenste inschakelen
Standaard bevinden Office 365-resources voor uw gebruikers zich in de dezelfde geo als uw Azure AD-tenant. Bijvoorbeeld, als uw tenant bevindt zich in Noord-Amerika, zich vervolgens Exchange-postvakken van de gebruikers bevinden eveneens in Noord-Amerika. Voor een meertalige organisatie, kan dit niet optimaal zijn.

Door het kenmerk **preferredDataLocation**, kunt u een gebruiker geo definiëren. U kunt de gebruiker Office 365 bronnen, zoals het postvak en OneDrive, in de dezelfde geo als de gebruiker en nog steeds een tenant voor uw hele organisatie.

> [!IMPORTANT]
> Als u in aanmerking komen voor meerdere Geo, moet u ten minste 5000 plaatsen in uw Office 365-abonnement hebben.
>
>

Een lijst met alle geografische gebieden voor Office 365 vindt u in [waar zijn de gegevens zich bevinden?](https://aka.ms/datamaps).

De geografische gebieden in Office 365 voor meerdere Geo beschikbaar zijn:

| Geografisch gebied | preferredDataLocation waarde |
| --- | --- |
| Azië en Stille Oceaan | APC |
| Australië | AUS |
| Canada | CAN |
| Europese Unie | EUR |
| India | IND |
| Japan | JPN |
| Zuid-Korea | KOR |
| Verenigd Koninkrijk | GBR |
| Verenigde Staten | NAAM |

* Als een geografisch niet wordt vermeld in deze tabel (bijvoorbeeld, Zuid-Amerika), kan niet vervolgens deze worden gebruikt voor meerdere Geo.
* India en Zuid-Korea geografische gebieden zijn alleen beschikbaar voor klanten met facturering adressen en gekochte licenties in deze geografische gebieden.
* Niet alle Office 365-werkbelastingen ondersteunen het gebruik van een gebruiker geo in te stellen.

### <a name="azure-ad-connect-support-for-synchronization"></a>Azure AD Connect-ondersteuning voor synchronisatie

Azure AD Connect ondersteunt synchronisatie van de **preferredDataLocation** kenmerk voor **gebruiker** objecten in versie 1.1.524.0 en hoger. Specifiek:

* Het schema van het objecttype **gebruiker** in de Azure AD-Connector is uitgebreid met de **preferredDataLocation** kenmerk. Het kenmerk is van het type tekenreeks met één waarde.
* Het schema van het objecttype **persoon** in de metaverse is uitgebreid met de **preferredDataLocation** kenmerk. Het kenmerk is van het type tekenreeks met één waarde.

Standaard **preferredDataLocation** is niet ingeschakeld voor synchronisatie. Deze functie is bedoeld voor grotere organisaties. U moet ook een kenmerk voor het opslaan van de Office 365 geo voor uw gebruikers identificeren omdat er geen **preferredDataLocation** kenmerk in de lokale Active Directory. Dit is het verstandig om verschillende voor elke organisatie.

> [!IMPORTANT]
> Azure AD kan de **preferredDataLocation** -kenmerk uit voor **cloud gebruikersobjecten** rechtstreeks worden geconfigureerd met behulp van Azure AD PowerShell. Azure AD kan niet langer de **preferredDataLocation** -kenmerk uit voor **gesynchroniseerd gebruikersobjecten** rechtstreeks worden geconfigureerd met behulp van Azure AD PowerShell. Voor het configureren van dit kenmerk op **gesynchroniseerd gebruikersobjecten**, moet u Azure AD Connect.

Voordat u synchronisatie inschakelt:

* Bepaal welke lokale Active Directory-kenmerk moet worden gebruikt als het bronkenmerk. Parameter moet van het type **één waarde tekenreeks**. In de stappen die volgen, een van de **extensionAttributes** wordt gebruikt.
* Als u eerder hebt geconfigureerd de **preferredDataLocation** kenmerk op bestaande **gesynchroniseerd gebruikersobjecten** in Azure AD met behulp van Azure AD PowerShell, moet u backport het kenmerk naar waarden de bijbehorende **gebruiker** objecten in de lokale Active Directory.

    > [!IMPORTANT]
    > Als u niet backport deze waarden dit, Azure AD Connect Hiermee verwijdert u de bestaande kenmerkwaarden in Azure AD wanneer synchronisatie voor de **preferredDataLocation** kenmerk is ingeschakeld.

* Configureer nu het bronkenmerk op ten minste een aantal objecten voor lokale Active Directory-gebruiker. U kunt deze gebruiken voor verificatie later.

De volgende secties behandelen de stappen voor het inschakelen van synchronisatie van de **preferredDataLocation** kenmerk.

> [!NOTE]
> De stappen worden beschreven in de context van een Azure AD-implementatie met één forest topologie en zonder aangepaste synchronisatieregels. Als u een topologie met meerdere forests hebt, aangepaste synchronisatieregels geconfigureerd of een tijdelijke server hebt, moet u de stappen overeenkomstig aanpassen.

## <a name="step-1-disable-sync-scheduler-and-verify-there-is-no-synchronization-in-progress"></a>Stap 1: Schakel de sync scheduler uit en controleer of dat er vindt geen synchronisatie uitgevoerd
Zorg ervoor dat er geen synchronisatie plaats terwijl u bent in het midden van synchronisatieregels bijwerken om te voorkomen onbedoelde wijzigingen wordt geëxporteerd naar Azure AD. De ingebouwde sync scheduler uitschakelen:

1. Start een PowerShell-sessie op de Azure AD Connect-server.
2. Geplande synchronisatie uitschakelen door deze cmdlet: `Set-ADSyncScheduler -SyncCycleEnabled $false`.
3. Start de **Synchronization Service Manager** door te gaan naar **START** > **synchronisatieservice**.
4. Selecteer de **Operations** tabblad en er is geen bewerking met de status bevestigen *Bezig*.

![Schermopname van Synchronization Service Manager](./media/active-directory-aadconnectsync-feature-preferreddatalocation/preferreddatalocation-step1.png)

## <a name="step-2-add-the-source-attribute-to-the-on-premises-active-directory-connector-schema"></a>Stap 2: Het bronkenmerk toevoegen aan het schema van de lokale Active Directory-Connector
Niet alle Azure AD-kenmerken worden geïmporteerd in de lokale Active Directory-Connector-ruimte. Als u een kenmerk dat niet is gesynchroniseerd standaard gebruiken hebt geselecteerd, moet u het importeren. Het bronkenmerk toevoegen aan de lijst van de geïmporteerde kenmerken:

1. Selecteer de **Connectors** tabblad Synchronization Service Manager.
2. Met de rechtermuisknop op de lokale Active Directory-Connector en selecteer **eigenschappen**.
3. In het pop-updialoogvenster, gaat u naar de **kenmerken selecteren** tabblad.
4. Zorg ervoor dat het bronkenmerk die u hebt geselecteerd om te gebruiken in de kenmerkenlijst is ingeschakeld. Als u het kenmerk niet ziet, selecteert u de **Alles weergeven** selectievakje.
5. Als u wilt opslaan, selecteert u **OK**.

![Dialoogvenster Eigenschappen en schermafbeelding Synchronization Service Manager](./media/active-directory-aadconnectsync-feature-preferreddatalocation/preferreddatalocation-step2.png)

## <a name="step-3-add-preferreddatalocation-to-the-azure-ad-connector-schema"></a>Stap 3: Toevoegen **preferredDataLocation** aan het schema van Azure AD-Connector
Standaard de **preferredDataLocation** kenmerk is niet geïmporteerd in de ruimte van de Azure AD-Connector. Toe te voegen aan de lijst met geïmporteerde kenmerken:

1. Selecteer de **Connectors** tabblad Synchronization Service Manager.
2. Met de rechtermuisknop op de Azure AD-connector en selecteer **eigenschappen**.
3. In het pop-updialoogvenster, gaat u naar de **kenmerken selecteren** tabblad.
4. Selecteer de **preferredDataLocation** kenmerk in de lijst.
5. Als u wilt opslaan, selecteert u **OK**.

![Dialoogvenster Eigenschappen en schermafbeelding Synchronization Service Manager](./media/active-directory-aadconnectsync-feature-preferreddatalocation/preferreddatalocation-step3.png)

## <a name="step-4-create-an-inbound-synchronization-rule"></a>Stap 4: Een synchronisatieregel voor binnenkomende maken
De synchronisatieregel voor binnenkomende kan de waarde van het kenmerk van het bronkenmerk in de lokale Active Directory naar de metaverse stromen.

1. Start de **synchronisatie regeleditor** door te gaan naar **START** > **synchronisatie regeleditor**.
2. Het zoekfilter ingesteld **richting** worden **inkomend**.
3. Als u een nieuwe regel voor binnenkomende verbindingen, selecteert **nieuwe regel toevoegen**.
4. Onder de **beschrijving** tabblad, bieden de volgende configuratie:

    | Kenmerk | Waarde | Details |
    | --- | --- | --- |
    | Naam | *Geef een naam* | Bijvoorbeeld ' In uit Active Directory-gebruiker preferredDataLocation ' |
    | Beschrijving | *Geef een aangepaste beschrijving* |  |
    | Verbonden systeem | *Kies de on-premises Active Directory-Connector* |  |
    | Verbonden systeem objecttype | **User** |  |
    | Metaverse-objecttype | **Person** |  |
    | Koppelingstype | **Koppelen** |  |
    | Prioriteit | *Kies een getal tussen 1-99* | 1-99 is gereserveerd voor aangepaste synchronisatie regels. Een waarde die wordt gebruikt door andere synchronisatieregels niet opgenomen. |

5. Houd de **Scoping filter** leeg is, naar alle objecten worden opgenomen. Mogelijk moet u het bereik filter op basis van uw Azure AD Connect-implementatie aanpassen.
6. Ga naar de **transformatie tabblad**, en de volgende transformatieregel implementeren:

    | Stroomtype | Doelkenmerk | Bron | Eenmaal toepassen | Type samenvoeging |
    | --- | --- | --- | --- | --- |
    |Direct | PreferredDataLocation | Kies het bronkenmerk | Dit selectievakje is uitgeschakeld | Update |

7. Voor het maken van de binnenkomende regel selecteert **toevoegen**.

![Schermopname van inkomende synchronisatieregel maken](./media/active-directory-aadconnectsync-feature-preferreddatalocation/preferreddatalocation-step4.png)

## <a name="step-5-create-an-outbound-synchronization-rule"></a>Stap 5: Maak een synchronisatieregel voor uitgaande
De uitgaande synchronisatieregel wordt toegestaan de waarde van het kenmerk vanuit de metaverse naar de **preferredDataLocation** kenmerk in Azure AD:

1. Ga naar de **synchronisatie regels Editor**.
2. Het zoekfilter ingesteld **richting** worden **uitgaand**.
3. Selecteer **nieuwe regel toevoegen**.
4. Onder de **beschrijving** tabblad, bieden de volgende configuratie:

    | Kenmerk | Waarde | Details |
    | ----- | ------ | --- |
    | Naam | *Geef een naam* | Bijvoorbeeld, 'Out naar Azure AD-gebruiker preferredDataLocation' |
    | Beschrijving | *Geef een beschrijving* ||
    | Verbonden systeem | *Selecteer de Azure AD-Connector* ||
    | Verbonden systeem objecttype | **User** ||
    | Metaverse-objecttype | **Person** ||
    | Koppelingstype | **Koppelen** ||
    | Prioriteit | *Kies een getal tussen 1-99* | 1-99 is gereserveerd voor aangepaste synchronisatie regels. Een waarde die wordt gebruikt door andere synchronisatieregels niet opgenomen. |

5. Ga naar de **Scoping filter** tabblad en toevoegen van een enkele bereikgroep met een filter met twee componenten:

    | Kenmerk | Operator | Waarde |
    | --- | --- | --- |
    | sourceObjectType | GELIJK ZIJN AAN | Gebruiker |
    | cloudMastered | NOTEQUAL | True |

    Bereik filter bepaalt welke deze uitgaande synchronisatieregel wordt toegepast op Azure AD-objecten. In dit voorbeeld gebruiken we het filter voor hetzelfde bereik uit 'Out naar AD-gebruikers-id' synchronisatieregel OOB (out of box). Deze voorkomt dat de synchronisatieregel wordt toegepast op **gebruiker** objecten die niet zijn gesynchroniseerd vanaf de lokale Active Directory. Mogelijk moet u het bereik filter op basis van uw Azure AD Connect-implementatie aanpassen.

6. Ga naar de **transformatie** tabblad en de volgende transformatieregel implementeren:

    | Stroomtype | Doelkenmerk | Bron | Eenmaal toepassen | Type samenvoeging |
    | --- | --- | --- | --- | --- |
    | Direct | PreferredDataLocation | PreferredDataLocation | Dit selectievakje is uitgeschakeld | Update |

7. Sluit **toevoegen** de uitgaande regel maken.

![Schermopname van uitgaande synchronisatieregel maken](./media/active-directory-aadconnectsync-feature-preferreddatalocation/preferreddatalocation-step5.png)

## <a name="step-6-run-full-synchronization-cycle"></a>Stap 6: Een volledige synchronisatiecyclus uitvoeren
In het algemeen is volledige synchronisatiecyclus vereist. Dit is omdat u hebt nieuwe kenmerken toegevoegd aan de Active Directory en de Azure AD-Connector schema en aangepaste synchronisatieregels geïntroduceerd. Controleer of u de wijzigingen voordat deze naar Azure AD worden geëxporteerd. U kunt de volgende stappen gebruiken om te controleren of de wijzigingen tijdens de stappen die gezamenlijk een volledige synchronisatiecyclus handmatig uit te voeren.

1. Voer **volledige import** op de lokale Active Directory-Connector:

   1. Ga naar de **Operations** tabblad Synchronization Service Manager.
   2. Met de rechtermuisknop op de **lokale Active Directory-Connector**, en selecteer **uitvoeren**.
   3. Selecteer in het dialoogvenster **volledige Import**, en selecteer **OK**.
   4. Wacht totdat de bewerking is voltooid.

    > [!NOTE]
    > Als het bronkenmerk al in de lijst met geïmporteerde kenmerken opgenomen is, kunt u de volledige import overslaan op de lokale Active Directory-Connector. Met andere woorden, hebt u geen eventuele wijzigingen aanbrengen tijdens stap 2 eerder in dit artikel.

2. Voer **volledige import** op de Azure AD-Connector:

   1. Met de rechtermuisknop op de **Azure AD-Connector**, en selecteer **uitvoeren**.
   2. Selecteer in het dialoogvenster **volledige Import**, en selecteer **OK**.
   3. Wacht totdat de bewerking is voltooid.

3. Controleer of de wijzigingen in synchronisatie-regel op een bestaande **gebruiker** object.

   Het bronkenmerk van de lokale Active Directory, en **preferredDataLocation** van Azure AD zijn geïmporteerd naar de respectieve connectorgebied overgebracht. Voordat u doorgaat met de stap volledige synchronisatie, doet u een voorbeeld van een bestaand **gebruiker** object in de lokale Active Directory-Connector-ruimte. Het object dat u verzameld, moet het bronkenmerk ingevuld hebben. Een geslaagde preview met **preferredDataLocation** ingevuld in de metaverse is correct regels voor een goede indicatie dat u de synchronisatie hebt geconfigureerd. Zie voor meer informatie over hoe u een voorbeeld doet [Controleer of de wijziging](active-directory-aadconnectsync-change-the-configuration.md#verify-the-change).

4. Voer **volledige synchronisatie** op de on-premises Active Directory-Connector:

   1. Met de rechtermuisknop op de **lokale Active Directory-Connector**, en selecteer **uitvoeren**.
   2. Selecteer in het dialoogvenster **volledige synchronisatie**, en selecteer **OK**.
   3. Wacht totdat de bewerking is voltooid.

5. Controleer of **in behandeling zijnde uitvoer** naar Azure AD:

   1. Met de rechtermuisknop op de **Azure AD-Connector**, en selecteer **Connectorgebied zoeken**.
   2. In de **Connectorgebied zoeken** in het dialoogvenster:

        a. Stel **bereik** naar **in behandeling zijnde uitvoer**.<br>
        b. Selecteer alle drie de selectievakjes, met inbegrip van **toevoegen, wijzigen en verwijderen**.<br>
        c. Als u wilt weergeven in de lijst met objecten met wijzigingen worden geëxporteerd, selecteer **Search**. Dubbelklik op het object voor het onderzoeken van de wijzigingen voor een bepaald object.<br>
        d. Controleer of dat de wijzigingen worden verwacht.

6. Voer **exporteren** op de **Azure AD-Connector**

   1. Met de rechtermuisknop op de **Azure AD-Connector**, en selecteer **uitvoeren**.
   2. In de **Connector uitvoeren** dialoogvenster, **exporteren**, en selecteer **OK**.
   3. Wacht totdat de bewerking is voltooid.

> [!NOTE]
> U zult merken dat de stappen exclusief de stap volledige synchronisatie op de Azure AD-Connector, of de stap exporteren op de Active Directory-Connector zijn. De stappen zijn niet vereist, omdat de kenmerkwaarden van de lokale Active Directory naar Azure AD alleen stromen.

## <a name="step-7-re-enable-sync-scheduler"></a>Stap 7: Sync scheduler opnieuw inschakelen
De ingebouwde sync scheduler opnieuw inschakelen:

1. Een PowerShell-sessie starten.
2. Geplande synchronisatie opnieuw inschakelen door het uitvoeren van deze cmdlet: `Set-ADSyncScheduler -SyncCycleEnabled $true`

## <a name="step-8-verify-the-result"></a>Stap 8: Controleer of het resultaat
Het is nu tijd om te controleren of de configuratie en inschakelen voor uw gebruikers.

1. De geo toevoegen aan het geselecteerde kenmerk van een gebruiker. De lijst met beschikbare geografische gebieden vindt u in [deze tabel](#enable-synchronization-of-preferreddatalocation).  
![Schermopname van AD-kenmerk toegevoegd aan een gebruiker](./media/active-directory-aadconnectsync-feature-preferreddatalocation/preferreddatalocation-adattribute.png)
2. Wachten op het kenmerk moeten worden gesynchroniseerd naar Azure AD.
3. Met behulp van Exchange Online PowerShell, Controleer of de regio van het Postvak correct is ingesteld.  
![Schermopname van Exchange Online PowerShell](./media/active-directory-aadconnectsync-feature-preferreddatalocation/preferreddatalocation-mailboxregion.png)  
Ervan uitgaande dat uw tenant is gemarkeerd als u deze functie wilt gebruiken, wordt het postvak verplaatst naar de juiste geo. Dit kan worden gecontroleerd door te zoeken op naam van de server waar het postvak zich bevindt.
4. Om te controleren of deze instelling is effectieve via veel postvakken, gebruikt u het script in de [TechNet-galerie](https://gallery.technet.microsoft.com/office/PowerShell-Script-to-a6bbfc2e). Dit script heeft ook een lijst met de server voorvoegsels van alle Office 365-datacenters en welke geo deze bevindt zich in. Deze kan worden gebruikt als een verwijzing in de vorige stap om te controleren of de locatie van het postvak.

## <a name="next-steps"></a>Volgende stappen

Meer informatie over Multi-geografisch in Office 365:

* [Multi-Geo-sessies op Ignite](https://aka.ms/MultiGeoIgnite)
* [Multi-Geo in OneDrive](https://aka.ms/OneDriveMultiGeo)
* [Multi-Geo in SharePoint Online](https://aka.ms/SharePointMultiGeo)

Meer informatie over de configuratiemodel in de synchronisatie-engine:

* Meer informatie over de configuratiemodel in [Understanding declaratieve inrichting](active-directory-aadconnectsync-understanding-declarative-provisioning.md).
* Meer informatie over de expressietaal in [Understanding declaratieve inrichting expressies](active-directory-aadconnectsync-understanding-declarative-provisioning-expressions.md).

Van overzichtsonderwerpen:

* [Azure AD Connect-synchronisatie: inzicht en synchronisatie aanpassen](active-directory-aadconnectsync-whatis.md)
* [Uw on-premises identiteiten integreren met Azure Active Directory](active-directory-aadconnect.md)
