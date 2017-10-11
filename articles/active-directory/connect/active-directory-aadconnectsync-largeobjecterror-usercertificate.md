---
title: 'Azure AD Connect-synchronisatie: LargeObject afhandeling van fouten als gevolg van userCertificate-kenmerk | Microsoft Docs'
description: Dit onderwerp bevat de herstelstappen voor LargeObject fouten worden veroorzaakt door userCertificate-kenmerk.
services: active-directory
documentationcenter: 
author: cychua
manager: femila
editor: 
ms.assetid: 146ad5b3-74d9-4a83-b9e8-0973a19828d9
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/13/2017
ms.author: billmath
ms.openlocfilehash: 2a5418ff61e07793fceca5a8207c1c5aa18847b4
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/03/2017
---
# <a name="azure-ad-connect-sync-handling-largeobject-errors-caused-by-usercertificate-attribute"></a>Azure AD Connect-synchronisatie: LargeObject afhandeling van fouten als gevolg van userCertificate-kenmerk

Azure AD dwingt een maximumlimiet van **15** waarden van het certificaat op de **userCertificate** kenmerk. Als een object met meer dan 15 waarden Azure AD Connect geëxporteerd naar Azure AD, Azure AD geeft een **LargeObject** fout opgetreden bij het bericht:

>*'Het ingerichte object is te groot. Verklein het aantal kenmerkwaarden voor dit object. De bewerking uit te voeren in de volgende synchronisatiecyclus..."*

De fout LargeObject kan worden veroorzaakt door andere AD-kenmerken. Om te bevestigen inderdaad wordt veroorzaakt door het kenmerk userCertificate, moet u controleren of op basis van het object zich in de on-premises AD dat of in de [Synchronization Service Manager Metaverse zoeken](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnectsync-service-manager-ui-mvsearch).

Gebruik een van de volgende methoden om de lijst met objecten in uw tenant met LargeObject fouten:

 * Als uw tenant wordt ingeschakeld voor Azure AD Connect Health voor synchroniseren, raadpleegt u de [synchronisatie foutenrapport](https://docs.microsoft.com/azure/active-directory/connect-health/active-directory-aadconnect-health-sync#object-level-synchronization-error-report-preview) opgegeven.
 
 * De e-mailmelding voor directory-synchronisatiefouten dat wordt verzonden aan het einde van elke synchronisatiecyclus heeft de lijst met objecten met fouten LargeObject. 
 * De [Synchronization Service Manager Operations tabblad](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnectsync-service-manager-ui-operations) geeft een lijst met objecten met fouten LargeObject als u de meest recente exporteren naar Azure AD-bewerking op.
 
## <a name="mitigation-options"></a>Risicobeperking opties
Totdat de LargeObject-fout is opgelost, kan de wijzigingen van andere kenmerken met hetzelfde object kunnen niet worden geëxporteerd naar Azure AD. Los de fout, kunt u rekening houden met de volgende opties:

 * Upgrade van Azure AD Connect 1.1.524.0 bouwen of na. In Azure AD Connect bouwen 1.1.524.0, de out-of-box-synchronisatie regels zijn bijgewerkt, zodat de kenmerken userCertificate en userSMIMECertificate niet exporteren als meer dan 15 waarden van de kenmerken hebben. Raadpleeg voor meer informatie over het bijwerken van Azure AD Connect artikel [Azure AD Connect: upgraden van een eerdere versie naar de meest recente](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-upgrade-previous-version).

 * Implementeer een **uitgaande synchronisatieregel** in Azure AD Connect die exporteert een **null-waarde in plaats van de werkelijke waarden voor objecten met meer dan 15 certificaat waarden**. Deze optie is geschikt als u een van de waarden van het certificaat worden geëxporteerd naar Azure AD voor objecten met meer dan 15 waarden niet nodig hebt. Raadpleeg voor meer informatie over het implementeren van deze synchronisatieregel volgende sectie [synchronisatieregel implementeren om te exporteren van het kenmerk userCertificate beperken](#implementing-sync-rule-to-limit-export-of-usercertificate-attribute).

 * Verminder het aantal waarden in het certificaat op de on-premises AD-object (15 of minder) door het verwijderen van de waarden die niet langer in gebruik door uw organisatie. Dit is geschikt als het kenmerk gegevensophoping wordt veroorzaakt door verlopen of niet-gebruikte certificaten. U kunt de [PowerShell-script beschikbaar hier](https://gallery.technet.microsoft.com/Remove-Expired-Certificates-0517e34f) om te zoeken, back-up maken en verwijderen van verlopen certificaten in uw on-premises AD. Voordat u de certificaten verwijdert, wordt het aanbevolen om te controleren met de infrastructuur van openbare sleutels beheerders in uw organisatie.

 * Azure AD Connect als u wilt uitsluiten van het kenmerk userCertificate wordt geëxporteerd naar Azure AD configureren. In het algemeen raadzaam niet deze optie omdat het kenmerk kan worden gebruikt door de Microsoft Online Services specifieke scenario's te maken. In het bijzonder:
    * Het kenmerk userCertificate van het gebruikersobject wordt gebruikt door Exchange Online en Outlook-clients voor bericht ondertekening en versleuteling. Raadpleeg voor meer informatie over deze functie, artikel [S/MIME voor bericht ondertekening en versleuteling](https://technet.microsoft.com/library/dn626158(v=exchg.150).aspx).

    * Het kenmerk userCertificate van het computerobject wordt gebruikt door Azure AD Windows 10 lokale domein om apparaten te laten verbinding maken met Azure AD. Raadpleeg voor meer informatie over deze functie, artikel [domein apparaten verbinden met Azure AD voor Windows 10 optreedt](https://docs.microsoft.com/azure/active-directory/active-directory-azureadjoin-devices-group-policy).

## <a name="implementing-sync-rule-to-limit-export-of-usercertificate-attribute"></a>Implementatie van synchronisatieregel voor het exporteren van het kenmerk userCertificate beperken
U kunt de LargeObject fout wordt veroorzaakt door het kenmerk userCertificate oplossen, kunt u een synchronisatieregel voor uitgaande implementeren in Azure AD Connect die worden geëxporteerd een **null-waarde in plaats van de werkelijke waarden voor objecten met meer dan 15 certificaat waarden**. Deze sectie beschrijft de stappen die nodig zijn voor het implementeren van de synchronisatieregel voor **gebruiker** objecten. De stappen kunnen worden aangepast voor **Contact** en **Computer** objecten.

> [!IMPORTANT]
> Exporteren van null-waarde, verwijdert het certificaat waarden eerder is geëxporteerd naar Azure AD.

De stappen kunnen worden samengevat als:

1. Schakel de sync scheduler en controleer of dat er vindt geen synchronisatie uitgevoerd.
3. De bestaande synchronisatieregel voor uitgaande voor userCertificate-kenmerk niet vinden.
4. Maak de synchronisatieregel voor uitgaande vereist.
5. Controleer of de nieuwe synchronisatieregel op een bestaand object met LargeObject fout.
6. De nieuwe synchronisatieregel van toepassing op de resterende objecten met LargeObject fout.
7. Controleer of er zijn geen onverwachte wijzigingen die nog moeten worden geëxporteerd naar Azure AD.
8. De wijzigingen wilt exporteren naar Azure AD.
9. Sync scheduler opnieuw in te schakelen.

### <a name="step-1-disable-sync-scheduler-and-verify-there-is-no-synchronization-in-progress"></a>Step 1. Schakel de sync scheduler uit en controleer of dat er vindt geen synchronisatie uitgevoerd
Zorg ervoor dat er geen synchronisatie plaats terwijl u bent in het midden van de implementatie van een nieuwe synchronisatieregel om onbedoelde wijzigingen wordt geëxporteerd naar Azure AD te voorkomen. De ingebouwde sync scheduler uitschakelen:
1. Start PowerShell-sessie op de Azure AD Connect-server.

2. Geplande synchronisatie uitschakelen door de cmdlet uit te voeren:`Set-ADSyncScheduler -SyncCycleEnabled $false`

> [!Note]
> De voorgaande stappen zijn alleen van toepassing op nieuwere versies (1.1.xxx.x) van Azure AD Connect met de ingebouwde scheduler. Als u gebruikmaakt van oudere versies (1.0.xxx.x) van Azure AD Connect die gebruikmaakt van Windows Taakplanner of u uw eigen aangepaste scheduler (niet-algemene) gebruikt voor het activeren van periodieke synchronisatie, moet u deze dienovereenkomstig uitschakelen.

3. Start de **Synchronization Service Manager** gaat u naar START → Synchronization Service.

4. Ga naar de **Operations** tabblad en er is geen bewerking met de status bevestigen *'wordt uitgevoerd.'*

### <a name="step-2-find-the-existing-outbound-sync-rule-for-usercertificate-attribute"></a>Stap 2. De bestaande synchronisatieregel voor uitgaande vinden voor het kenmerk userCertificate
Er moet een bestaande regel voor synchroniseren die is ingeschakeld en geconfigureerd voor het kenmerk voor gebruikersobjecten userCertificate exporteren naar Azure AD. Zoek deze synchronisatieregel om erachter te komen de **voorrang** en **bereik filter** configuratie:

1. Start de **synchronisatie regeleditor** door naar START → synchronisatie regeleditor te gaan.

2. De search-filters te configureren met de volgende waarden:

    | Kenmerk | Waarde |
    | --- | --- |
    | Richting |**Uitgaand** |
    | MV-objecttype |**Persoon** |
    | Connector |*naam van uw Azure AD-connector* |
    | Connector-objecttype |**gebruiker** |
    | MV-kenmerk |**userCertificate** |

3. Als u OOB-synchronisatie (out of box) regels voor Azure AD-connector gebruikt voor het kenmerk userCertficiate voor gebruikersobjecten exporteren, ontvangt u de *'Uit naar AAD – gebruiker ExchangeOnline'* regel.
4. Noteer de **voorrang** waarde van deze synchronisatieregel.
5. Selecteer de synchronisatieregel en klik op **bewerken**.
6. In de *'Gereserveerde bevestiging regel bewerken'* pop-upvenster, klikt u op **Nee**. (U hoeft niet, niet gaan we een wijziging aanbrengt in deze synchronisatieregel).
7. Selecteer in het scherm bewerken de **Scoping filter** tabblad.
8. Noteer de scope filterconfiguratie. Als u de synchronisatie OOB regel gebruikt, moet exact er **één filter bereikgroep met twee componenten**, waaronder:

    | Kenmerk | Operator | Waarde |
    | --- | --- | --- |
    | Bronobjecttype | GELIJK ZIJN AAN | Gebruiker |
    | cloudMastered | NOTEQUAL | True |

### <a name="step-3-create-the-outbound-sync-rule-required"></a>Stap 3. De synchronisatieregel voor uitgaande vereist maken
De nieuwe synchronisatieregel moet hebben dezelfde **bereik filter** en **hogere prioriteit** dan de bestaande synchronisatieregel. Dit zorgt ervoor dat de nieuwe synchronisatieregel van toepassing op dezelfde set objecten als de bestaande synchronisatieregel en de bestaande synchronisatieregel voor het kenmerk userCertificate overschrijft. De synchronisatieregel maken:
1. Klik met de regeleditor synchronisatie op de **nieuwe regel toevoegen** knop.
2. Onder de **tabblad Beschrijving**, bieden de volgende configuratie:

    | Kenmerk | Waarde | Details |
    | --- | --- | --- |
    | Naam | *Geef een naam* | Bijvoorbeeld *'Uit voor AAD – aangepaste onderdrukking voor userCertificate'* |
    | Beschrijving | *Geef een beschrijving* | Bijvoorbeeld *"Als userCertificate-kenmerk meer dan 15 waarden heeft, exporteren NULL."* |
    | Verbonden systeem | *Selecteer de Azure AD-Connector* |
    | Verbonden systeem objecttype | **gebruiker** | |
    | Metaverse-objecttype | **persoon** | |
    | Koppelingstype | **Koppelen** | |
    | Prioriteit | *Kies een getal tussen 1 en 99 liggen* | Het aantal gekozen mogen niet worden gebruikt door een bestaande synchronisatieregel en heeft een lagere waarde (en dus hogere prioriteit) dan de bestaande synchronisatieregel. |

3. Ga naar de **Scoping filter** tabblad en hetzelfde bereik filter van de bestaande synchronisatieregel gebruikmaakt te implementeren.
4. Overslaan de **Join regels** tabblad.
5. Ga naar de **transformaties** tabblad toevoegen van een nieuwe transformatie met volgende configuratie:

    | Kenmerk | Waarde |
    | --- | --- |
    | Stroomtype |**Expressie** |
    | Doelkenmerk |**userCertificate** |
    | Kenmerk van de gegevensbron |*Gebruik de volgende expressie*:`IIF(IsNullOrEmpty([userCertificate]), NULL, IIF((Count([userCertificate])> 15),AuthoritativeNull,[userCertificate]))` |
    
6. Klik op de **toevoegen** knop om de synchronisatieregel te maken.

### <a name="step-4-verify-the-new-sync-rule-on-an-existing-object-with-largeobject-error"></a>Stap 4. Controleer of de nieuwe synchronisatieregel op een bestaand object met LargeObject fout
Dit is om te controleren of de synchronisatieregel gemaakt correct op een bestaande AD-object met LargeObject fout werkt voordat u deze toe te op andere objecten passen:
1. Ga naar de **Operations** tabblad Synchronization Service Manager.
2. Selecteer de meest recente exporteren naar Azure AD-bewerking en klik op een van de objecten met fouten LargeObject.
3.  Klik in het pop-scherm objecteigenschappen van Connector ruimte op de **Preview** knop.
4. Selecteer in het pop-scherm Preview **volledige synchronisatie** en klik op **doorvoeren Preview**.
5. Sluit het afdrukvoorbeeld en het scherm objecteigenschappen Connector-ruimte.
6. Ga naar de **Connectors** tabblad Synchronization Service Manager.
7. Met de rechtermuisknop op de **Azure AD** Connector en selecteer **uitvoeren...**
8. Selecteer in de pop-up Connector uitvoeren **exporteren** stap en klik op **OK**.
9. Wacht u totdat de Export naar Azure AD voltooid en er is geen meer LargeObject-fout voor dit specifieke object bevestigen.

### <a name="step-5-apply-the-new-sync-rule-to-remaining-objects-with-largeobject-error"></a>Stap 5. De nieuwe synchronisatieregel van toepassing op de resterende objecten met LargeObject fout
Wanneer de synchronisatieregel is toegevoegd, moet u de stap van een volledige synchronisatie uitvoeren voor de AD-Connector:
1. Ga naar de **Connectors** tabblad Synchronization Service Manager.
2. Met de rechtermuisknop op de **AD** Connector en selecteer **uitvoeren...**
3. Selecteer in de pop-up Connector uitvoeren **volledige synchronisatie** stap en klik op **OK**.
4. Wacht tot de volledige synchronisatie stap te voltooien.
5. Herhaal de bovenstaande stappen voor de resterende AD-Connectors hebt u meer dan één AD-Connectors. Meerdere connectors zijn doorgaans vereist als er meerdere on-premises adreslijsten.

### <a name="step-6-verify-there-are-no-unexpected-changes-waiting-to-be-exported-to-azure-ad"></a>Stap 6. Controleer of er zijn geen onverwachte wijzigingen die nog moeten worden geëxporteerd naar Azure AD
1. Ga naar de **Connectors** tabblad Synchronization Service Manager.
2. Met de rechtermuisknop op de **Azure AD** Connector en selecteer **Connectorgebied zoeken**.
3. In de pop-up Connectorgebied zoeken:
    1. Bereik instelt op **in behandeling zijnde uitvoer**.
    2. Controleer alle 3 selectievakjes, met inbegrip van **toevoegen**, **wijzigen** en **verwijderen**.
    3. Klik op **Search** om terug te keren alle objecten met wijzigingen die nog moeten worden geëxporteerd naar Azure AD.
    4. Controleer of dat er zijn geen onverwachte wijzigingen. Als u wilt onderzoeken de wijzigingen voor een bepaald object, dubbelklik op het object.

### <a name="step-7-export-the-changes-to-azure-ad"></a>Stap 7. De wijzigingen wilt exporteren naar Azure AD
De wijzigingen naar Azure AD exporteren:
1. Ga naar de **Connectors** tabblad Synchronization Service Manager.
2. Met de rechtermuisknop op de **Azure AD** Connector en selecteer **uitvoeren...**
4. Selecteer in de pop-up Connector uitvoeren **exporteren** stap en klik op **OK**.
5. Wacht u totdat de Export naar Azure AD voltooid en er zijn geen fouten meer LargeObject zijn bevestigen.

### <a name="step-8-re-enable-sync-scheduler"></a>Stap 8. Sync scheduler opnieuw inschakelen
Nu dat het probleem is opgelost, schakelt u de ingebouwde sync scheduler:
1. Start PowerShell-sessie.
2. Geplande synchronisatie opnieuw inschakelen door de cmdlet uit te voeren:`Set-ADSyncScheduler -SyncCycleEnabled $true`

> [!Note]
> De voorgaande stappen zijn alleen van toepassing op nieuwere versies (1.1.xxx.x) van Azure AD Connect met de ingebouwde scheduler. Als u gebruikmaakt van oudere versies (1.0.xxx.x) van Azure AD Connect die gebruikmaakt van Windows Taakplanner of u uw eigen aangepaste scheduler (niet-algemene) gebruikt voor het activeren van periodieke synchronisatie, moet u deze dienovereenkomstig uitschakelen.

## <a name="next-steps"></a>Volgende stappen
Lees meer over het [integreren van uw on-premises identiteiten met Azure Active Directory](active-directory-aadconnect.md).

