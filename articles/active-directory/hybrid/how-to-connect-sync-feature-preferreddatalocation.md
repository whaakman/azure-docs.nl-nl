---
title: 'Azure Active Directory Connect-synchronisatie: Gewenste gegevenslocatie voor meerdere geografische gebieden mogelijkheden configureren in Office 365 | Microsoft Docs'
description: Hierin wordt beschreven hoe u de resources van uw Office 365-gebruiker dicht bij de gebruiker met Azure Active Directory Connect-synchronisatie.
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
ms.date: 07/30/2018
ms.component: hybrid
ms.author: billmath
ms.openlocfilehash: 7b54d1b746467695b2c5e6a4ba634c74a3801d4f
ms.sourcegitcommit: 58dc0d48ab4403eb64201ff231af3ddfa8412331
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/26/2019
ms.locfileid: "55078726"
---
# <a name="azure-active-directory-connect-sync-configure-preferred-data-location-for-office-365-resources"></a>Azure Active Directory Connect-synchronisatie: Gewenste gegevenslocatie voor Office 365-resources configureren
Het doel van dit onderwerp wordt u stapsgewijs door het kenmerk voor de gewenste gegevenslocatie configureren in Azure Active Directory (Azure AD) Connect-synchronisatie. Wanneer iemand maakt gebruik van meerdere geografische gebieden mogelijkheden in Office 365, gebruikt u dit kenmerk om de geo-locatie van Office 365-gegevens van de gebruiker toe te wijzen. (De voorwaarden *regio* en *geo* door elkaar worden gebruikt.)

## <a name="enable-synchronization-of-preferred-data-location"></a>Synchronisatie van de gewenste gegevenslocatie inschakelen
Standaard bevinden Office 365-resources voor uw gebruikers zich in hetzelfde geografische gebied als uw Azure AD-tenant. Bijvoorbeeld, als uw tenant in Noord-Amerika bevindt zich, klikt u vervolgens de gebruikers Exchange-postvakken staan ook in Noord-Amerika. Voor een multinationale organisatie, kan dit niet optimaal zijn.

Door het kenmerk **preferredDataLocation**, kunt u geo van een gebruiker definiëren. Je kunt van de gebruiker Office 365 resources, zoals het postvak en OneDrive, in hetzelfde geografische gebied als de gebruiker, en nog één tenant voor uw hele organisatie.

> [!IMPORTANT]
> Meerdere geografische gebieden is momenteel beschikbaar voor klanten met een minimum van 2500 abonnementen voor Office 365-Services. Neem contact op met uw Microsoft-vertegenwoordiger voor meer informatie.
>
>

Een lijst met alle geografische gebieden voor Office 365 kan worden gevonden in [waar zijn uw gegevens zich?](https://aka.ms/datamaps).

De geografische gebieden in Office 365 beschikbaar voor meerdere geografische gebieden zijn:

| Geografisch gebied | preferredDataLocation waarde |
| --- | --- |
| Azië en Stille Oceaan | APC |
| Australië | AUS |
| Canada | KAN |
| Europese Unie | EUR |
| Frankrijk | FRA |
| India | ZOEKEN |
| Japan | JPN |
| Korea | KOR |
| Verenigd Koninkrijk | GBR |
| Verenigde Staten | NAM |

* Worden als een geografisch gebied niet wordt vermeld in deze tabel (bijvoorbeeld, Zuid-Amerika), klikt u vervolgens deze kan niet gebruikt voor meerdere geografische gebieden.

* Niet alle Office 365-werkbelastingen ondersteuning voor het gebruik van het instellen van een gebruiker geo.

### <a name="azure-ad-connect-support-for-synchronization"></a>Azure AD Connect-ondersteuning voor synchronisatie

Azure AD Connect biedt ondersteuning voor synchronisatie van de **preferredDataLocation** voor het kenmerk **gebruiker** objecten in versie 1.1.524.0 en hoger en hoger. Specifiek:

* Het schema van het objecttype **gebruiker** in de Azure AD-Connector wordt uitgebreid zodat u ook de **preferredDataLocation** kenmerk. Het kenmerk is van het type tekenreeks één waarde.
* Het schema van het objecttype **persoon** in de metaverse wordt uitgebreid zodat u ook de **preferredDataLocation** kenmerk. Het kenmerk is van het type tekenreeks één waarde.

Standaard **preferredDataLocation** is niet ingeschakeld voor synchronisatie. Deze functie is bedoeld voor grote organisaties. U moet ook identificeren aan een kenmerk voor het opslaan van het Office 365 geografische gebied voor uw gebruikers, omdat er geen **preferredDataLocation** kenmerk in on-premises Active Directory. Dit is het verstandig om voor elke organisatie anders.

> [!IMPORTANT]
> Azure AD kan de **preferredDataLocation** kenmerk op **gebruikersobjecten in de cloud** rechtstreeks worden geconfigureerd met behulp van Azure AD PowerShell. Azure AD kan niet meer de **preferredDataLocation** kenmerk op **gesynchroniseerd gebruikersobjecten** rechtstreeks worden geconfigureerd met behulp van Azure AD PowerShell. Voor het configureren van dit kenmerk op **gesynchroniseerd gebruikersobjecten**, moet u Azure AD Connect.

Voordat de synchronisatie is ingeschakeld:

* Bepaal welke on-premises Active Directory-kenmerk moet worden gebruikt als het bronkenmerk. Deze moet van het type **één waarde tekenreeks**. In de stappen die volgen, een van de **extensionAttributes** wordt gebruikt.
* Als u eerder hebt geconfigureerd de **preferredDataLocation** kenmerk op bestaande **gesynchroniseerd gebruikersobjecten** in Azure AD met behulp van Azure AD PowerShell, moet u backport het kenmerk naar waarden de bijbehorende **gebruiker** objecten in on-premises Active Directory.

    > [!IMPORTANT]
    > Als u deze waarden niet backport hebt, Azure AD Connect Hiermee verwijdert u de bestaande kenmerkwaarden in Azure AD wanneer de synchronisatie voor de **preferredDataLocation** kenmerk is ingeschakeld.

* Configureer nu het bronkenmerk op ten minste twee on-premises Active Directory-gebruiker objecten. U kunt dit gebruiken om later te verifiëren.

De volgende secties vindt u de stappen voor het inschakelen van synchronisatie van de **preferredDataLocation** kenmerk.

> [!NOTE]
> De stappen worden beschreven in de context van een Azure AD-implementatie topologie met één forest en zonder aangepaste synchronisatieregels. Als u een topologie met meerdere forests hebt, aangepaste synchronisatieregels die zijn geconfigureerd, of een staging-server, moet u de stappen overeenkomstig aanpassen.

## <a name="step-1-disable-sync-scheduler-and-verify-there-is-no-synchronization-in-progress"></a>Stap 1: Synchronisatieplanning uitschakelen en controleer of dat er is geen synchronisatie wordt uitgevoerd
Zorg ervoor dat er geen synchronisatie plaats vindt wanneer u zich in het midden van synchronisatieregels bijwerken om te voorkomen dat onbedoelde wijzigingen wordt geëxporteerd naar Azure AD. De ingebouwde Synchronisatieplanning uitschakelen:

1. Start een PowerShell-sessie op de Azure AD Connect-server.
2. Geplande synchronisatie uitschakelen door het uitvoeren van deze cmdlet: `Set-ADSyncScheduler -SyncCycleEnabled $false`.
3. Start de **Synchronization Service Manager** door te gaan naar **START** > **Synchronization Service**.
4. Selecteer de **Operations** tabblad en controleer of er is geen bewerking met de status *Bezig*.

![Schermafbeelding van de Synchronization Service Manager](./media/how-to-connect-sync-feature-preferreddatalocation/preferreddatalocation-step1.png)

## <a name="step-2-add-the-source-attribute-to-the-on-premises-active-directory-connector-schema"></a>Stap 2: Het bronkenmerk toevoegen aan het schema van de on-premises Active Directory-Connector
Niet alle Azure AD-kenmerken worden geïmporteerd in de ruimte van on-premises Active Directory-Connector. Als u een kenmerk dat niet is gesynchroniseerd standaard gebruiken hebt geselecteerd, klikt u vervolgens moet u om deze te importeren. Het bronkenmerk toevoegen aan de lijst van de geïmporteerde kenmerken:

1. Selecteer de **Connectors** tabblad in de Synchronization Service Manager.
2. Met de rechtermuisknop op de on-premises Active Directory-Connector en selecteer **eigenschappen**.
3. In het pop-updialoogvenster, gaat u naar de **kenmerken selecteren** tabblad.
4. Zorg ervoor dat het bronkenmerk die u hebt geselecteerd om te gebruiken is ingeschakeld in de lijst met kenmerken. Als u het kenmerk niet ziet, selecteert u de **Alles weergeven** selectievakje.
5. Als u wilt opslaan, selecteert u **OK**.

![Schermafbeelding van de Synchronization Service Manager en eigenschappen in het dialoogvenster](./media/how-to-connect-sync-feature-preferreddatalocation/preferreddatalocation-step2.png)

## <a name="step-3-add-preferreddatalocation-to-the-azure-ad-connector-schema"></a>Stap 3: Voeg **preferredDataLocation** aan het schema van de Azure AD-Connector
Standaard de **preferredDataLocation** kenmerk is niet geïmporteerd in de Azure AD-connectorgebied. Toe te voegen aan de lijst met geïmporteerde kenmerken:

1. Selecteer de **Connectors** tabblad in de Synchronization Service Manager.
2. Met de rechtermuisknop op de Azure AD-connector en selecteer **eigenschappen**.
3. In het pop-updialoogvenster, gaat u naar de **kenmerken selecteren** tabblad.
4. Selecteer de **preferredDataLocation** kenmerk in de lijst.
5. Als u wilt opslaan, selecteert u **OK**.

![Schermafbeelding van de Synchronization Service Manager en eigenschappen in het dialoogvenster](./media/how-to-connect-sync-feature-preferreddatalocation/preferreddatalocation-step3.png)

## <a name="step-4-create-an-inbound-synchronization-rule"></a>Stap 4: Maak een regel voor inkomende synchronisatie
De synchronisatieregel voor binnenkomende gegevens kan de waarde van het kenmerk van het bronkenmerk in on-premises Active Directory naar de metaverse stromen.

1. Start de **Synchronization Rules Editor** door te gaan naar **START** > **Synchronization Rules Editor**.
2. Stel het zoekfilter **richting** moet **inkomend**.
3. Selecteer voor het maken van een nieuwe regel voor binnenkomende verbindingen, **nieuwe regel toevoegen**.
4. Onder de **beschrijving** tabblad, geeft u de volgende configuratie:

    | Kenmerk | Waarde | Details |
    | --- | --- | --- |
    | Name | *Geef een naam* | Bijvoorbeeld, "In uit Active Directory-gebruiker preferredDataLocation" |
    | Description | *Geef een aangepaste beschrijving* |  |
    | Verbonden systeem | *Kies de on-premises Active Directory-Connector* |  |
    | Verbonden systeem objecttype | **User** |  |
    | Metaverse-objecttype | **Person** |  |
    | Type koppeling | **Koppelen** |  |
    | Prioriteit | *Kies een getal tussen 1-99* | 1-99 is gereserveerd voor aangepaste synchronisatieregels. Een waarde die wordt gebruikt door een andere regel voor synchronisatie niet verzamelen. |

5. Houd de **Scoping filter** leeg is, om op te nemen van alle objecten. U moet mogelijk de bereikfilter op basis van uw Azure AD Connect-implementatie aanpassen.
6. Ga naar de **transformatie tabblad**, en implementeren van de volgende transformatieregel:

    | Type gebruikersstroom | Doelkenmerk | Bron | Eenmaal toepassen | Type samenvoeging |
    | --- | --- | --- | --- | --- |
    |Rechtstreeks | preferredDataLocation | Kies het bronkenmerk | Dit selectievakje is uitgeschakeld | Update |

7. Voor het maken van de binnenkomende regel, selecteer **toevoegen**.

![Schermafbeelding van de inkomende synchronisatieregel maken](./media/how-to-connect-sync-feature-preferreddatalocation/preferreddatalocation-step4.png)

## <a name="step-5-create-an-outbound-synchronization-rule"></a>Stap 5: Maak een regel voor uitgaande synchronisatie
De regel voor uitgaande synchronisatie kan de waarde van het kenmerk op die moeten stromen van metaverse naar de **preferredDataLocation** kenmerk in Azure AD:

1. Ga naar de **synchronisatie regels Editor**.
2. Stel het zoekfilter **richting** moet **uitgaand**.
3. Selecteer **nieuwe regel toevoegen**.
4. Onder de **beschrijving** tabblad, geeft u de volgende configuratie:

    | Kenmerk | Waarde | Details |
    | ----- | ------ | --- |
    | Name | *Geef een naam* | Bijvoorbeeld, 'Out naar Azure AD-gebruiker preferredDataLocation" |
    | Description | *Geef een beschrijving* ||
    | Verbonden systeem | *Selecteer de Azure AD-Connector* ||
    | Verbonden systeem objecttype | **User** ||
    | Metaverse-objecttype | **Person** ||
    | Type koppeling | **Koppelen** ||
    | Prioriteit | *Kies een getal tussen 1-99* | 1-99 is gereserveerd voor aangepaste synchronisatieregels. Een waarde die wordt gebruikt door een andere regel voor synchronisatie niet verzamelen. |

5. Ga naar de **Scoping filter** tabblad en toevoegen van een enkel bereik filtergroep met twee componenten:

    | Kenmerk | Operator | Waarde |
    | --- | --- | --- |
    | sourceObjectType | GELIJK AAN | Gebruiker |
    | cloudMastered | NOTEQUAL | True |

    Bereikfilter bepaalt welke Azure AD-objecten dat met deze regel voor uitgaande synchronisatie wordt toegepast op. In dit voorbeeld gebruiken we de dezelfde bereikfilter uit "Out met AD-gebruikers-id" synchronisatieregel OOB (out-of-box). Voorkomt dat de synchronisatieregel wordt toegepast op **gebruiker** objecten die niet zijn gesynchroniseerd vanuit on-premises Active Directory. U moet mogelijk de bereikfilter op basis van uw Azure AD Connect-implementatie aanpassen.

6. Ga naar de **transformatie** tabblad en implementeren van de volgende transformatieregel:

    | Type gebruikersstroom | Doelkenmerk | Bron | Eenmaal toepassen | Type samenvoeging |
    | --- | --- | --- | --- | --- |
    | Rechtstreeks | preferredDataLocation | preferredDataLocation | Dit selectievakje is uitgeschakeld | Update |

7. Sluiten **toevoegen** om de uitgaande regel te maken.

![Schermafbeelding van uitgaande synchronisatieregel maken](./media/how-to-connect-sync-feature-preferreddatalocation/preferreddatalocation-step5.png)

## <a name="step-6-run-full-synchronization-cycle"></a>Stap 6: Volledige synchronisatiecyclus uitvoeren
In het algemeen is de volledige synchronisatiecyclus vereist. Dit is omdat u hebt nieuwe kenmerken toegevoegd aan de Active Directory en de Azure AD-Connector-schema en aangepaste synchronisatieregels die zijn geïntroduceerd. Controleer of u de wijzigingen voordat u ze naar Azure AD exporteert. U kunt de volgende stappen uit om te controleren of de wijzigingen bij het handmatig uitvoeren van de stappen die gezamenlijk een volledige synchronisatiecyclus.

1. Voer **volledige import** op de on-premises Active Directory-Connector:

   1. Ga naar de **Operations** tabblad in de Synchronization Service Manager.
   2. Met de rechtermuisknop op de **on-premises Active Directory-Connector**, en selecteer **uitvoeren**.
   3. Selecteer in het dialoogvenster **volledige Import**, en selecteer **OK**.
   4. Wacht tot de bewerking is voltooid.

    > [!NOTE]
    > Als het bronkenmerk al in de lijst met geïmporteerde kenmerken opgenomen is, kunt u de volledige import overslaan op de on-premises Active Directory-Connector. Met andere woorden, hebt u geen op een wijziging aanbrengt tijdens stap 2 eerder in dit artikel.

2. Voer **volledige import** op de Azure AD-Connector:

   1. Met de rechtermuisknop op de **Azure AD-Connector**, en selecteer **uitvoeren**.
   2. Selecteer in het dialoogvenster **volledige Import**, en selecteer **OK**.
   3. Wacht tot de bewerking is voltooid.

3. Controleer de wijzigingen van de regel voor synchronisatie op een bestaande **gebruiker** object.

   Het bronkenmerk van on-premises Active Directory, en **preferredDataLocation** van Azure AD zijn geïmporteerd in elke betreffende connectorgebied. Voordat u doorgaat met de stap volledige synchronisatie, doet u een Preview-versie van een bestaande **gebruiker** -object in de ruimte van on-premises Active Directory-Connector. Het object dat u hebt geselecteerd, moet het bronkenmerk ingevuld hebben. Een geslaagde Preview-versie met **preferredDataLocation** ingevuld in de metaverse is een goede indicatie dat u de synchronisatie hebt geconfigureerd correct regels. Zie voor meer informatie over hoe u een Preview-versie doet [controleren of de wijziging](how-to-connect-sync-change-the-configuration.md#verify-the-change).

4. Voer **volledige synchronisatie** op de on-premises Active Directory-Connector:

   1. Met de rechtermuisknop op de **on-premises Active Directory-Connector**, en selecteer **uitvoeren**.
   2. Selecteer in het dialoogvenster **volledige synchronisatie**, en selecteer **OK**.
   3. Wacht tot de bewerking is voltooid.

5. Controleer of **in behandeling zijnde uitvoer** naar Azure AD:

   1. Met de rechtermuisknop op de **Azure AD-Connector**, en selecteer **Search Connector Space**.
   2. In de **Search Connector Space** in het dialoogvenster:

        a. Stel **bereik** naar **exportbewerking**.<br>
        b. Schakel alle drie selectievakjes, met inbegrip van **toevoegen, wijzigen en verwijderen**.<br>
        c. Als u wilt weergeven in de lijst van objecten met de wijzigingen worden geëxporteerd, selecteer **zoeken**. Dubbelklik op het object voor het onderzoeken van de wijzigingen voor een bepaald object.<br>
        d. Controleer of dat de wijzigingen worden verwacht.

6. Voer **exporteren** op de **Azure AD-Connector**

   1. Met de rechtermuisknop op de **Azure AD-Connector**, en selecteer **uitvoeren**.
   2. In de **Connector uitvoeren** in het dialoogvenster, selecteer **exporteren**, en selecteer **OK**.
   3. Wacht tot de bewerking is voltooid.

> [!NOTE]
> U zult merken dat de stappen de stap volledige synchronisatie op de Azure AD-Connector, of de uitvoer-stap in de Active Directory-Connector omvatten. De stappen zijn niet vereist, omdat de kenmerkwaarden van on-premises Active Directory naar Azure AD alleen stromen.

## <a name="step-7-re-enable-sync-scheduler"></a>Stap 7: Synchronisatieplanning opnieuw in te schakelen
De ingebouwde Synchronisatieplanning opnieuw inschakelen:

1. Start een PowerShell-sessie.
2. Geplande synchronisatie opnieuw inschakelen door het uitvoeren van deze cmdlet: `Set-ADSyncScheduler -SyncCycleEnabled $true`

## <a name="step-8-verify-the-result"></a>Stap 8: Controleer of het resultaat
Het is nu tijd om te controleren of de configuratie en inschakelen voor uw gebruikers.

1. Het geografische gebied aan de geselecteerde kenmerken van een gebruiker toevoegen. De lijst met beschikbare geo's vindt u [deze tabel](#enable-synchronization-of-preferreddatalocation).  
![Schermopname van het AD-kenmerk toegevoegd aan een gebruiker](./media/how-to-connect-sync-feature-preferreddatalocation/preferreddatalocation-adattribute.png)
2. Wachten op het kenmerk worden gesynchroniseerd met Azure AD.
3. Met behulp van Exchange Online PowerShell, Controleer of de regio van het Postvak correct is ingesteld.  
![Schermafbeelding van Exchange Online PowerShell](./media/how-to-connect-sync-feature-preferreddatalocation/preferreddatalocation-mailboxregion.png)  
Ervan uitgaande dat uw tenant is gemarkeerd als u deze functie wilt gebruiken, wordt het postvak verplaatst naar het juiste geografische gebied. Dit kan worden gecontroleerd door te kijken naam van de server waar het postvak dat zich bevindt.
4. Om te controleren of deze instelling effectieve via veel postvakken zijn, gebruikt u het script in de [TechNet-galerie](https://gallery.technet.microsoft.com/office/PowerShell-Script-to-a6bbfc2e). Met dit script heeft ook een lijst van de server-voorvoegsels van alle Office 365-datacenters en welke geo deze bevindt zich in. Het kan worden gebruikt als een verwijzing in de vorige stap om te controleren of de locatie van het postvak.

## <a name="next-steps"></a>Volgende stappen

Meer informatie over meerdere geografische gebieden in Office 365:

* [Meerdere geografische gebieden Ignite-sessies](https://aka.ms/MultiGeoIgnite)
* [Meerdere geografische gebieden in OneDrive](https://aka.ms/OneDriveMultiGeo)
* [Meerdere geografische gebieden in SharePoint Online](https://aka.ms/SharePointMultiGeo)

Meer informatie over de configuratiemodel in de synchronisatie-engine:

* Meer informatie over de configuratiemodel in [Understanding declaratieve inrichting](concept-azure-ad-connect-sync-declarative-provisioning.md).
* Meer informatie over de expressietaal in [Understanding declaratieve inrichting expressies](concept-azure-ad-connect-sync-declarative-provisioning-expressions.md).

Overzicht van onderwerpen:

* [Azure AD Connect-synchronisatie: Begrijpen en aanpassen van synchronisatie](how-to-connect-sync-whatis.md)
* [Uw on-premises identiteiten integreren met Azure Active Directory](whatis-hybrid-identity.md)
