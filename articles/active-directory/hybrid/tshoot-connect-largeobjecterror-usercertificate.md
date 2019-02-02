---
title: Azure AD Connect - LargeObject-fouten die zijn veroorzaakt door userCertificate kenmerk | Microsoft Docs
description: Dit onderwerp bevat de herstelstappen voor LargeObject-fouten die zijn veroorzaakt door userCertificate kenmerk.
services: active-directory
documentationcenter: ''
author: billmath
manager: daveba
editor: ''
ms.assetid: 146ad5b3-74d9-4a83-b9e8-0973a19828d9
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/13/2017
ms.subservice: hybrid
ms.author: billmath
ms.custom: seohack1
ms.openlocfilehash: e0d5e466da8804e89ffecd38dd2db6e37a3cb334
ms.sourcegitcommit: de32e8825542b91f02da9e5d899d29bcc2c37f28
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/02/2019
ms.locfileid: "55663635"
---
# <a name="azure-ad-connect-sync-handling-largeobject-errors-caused-by-usercertificate-attribute"></a>Azure AD Connect-synchronisatie: Afhandeling van LargeObject-fouten die zijn veroorzaakt door userCertificate kenmerk

Azure AD een maximumlimiet van afgedwongen **15** waarden van het certificaat op de **userCertificate** kenmerk. Als Azure AD Connect Hiermee exporteert u een object met meer dan 15 waarden aan Azure AD, Azure AD als resultaat een **LargeObject** fout opgetreden bij het bericht:

>*"Het ingerichte object is te groot. Verklein het aantal kenmerkwaarden voor dit object. De bewerking wordt opnieuw uitgevoerd tijdens de volgende synchronisatiecyclus...'*

De LargeObject-fout kan worden veroorzaakt door andere AD-kenmerken. Om te bevestigen dat het inderdaad wordt veroorzaakt door het kenmerk userCertificate, moet u controleren of het object in on-premises AD of in de [Metaverse Search van Synchronization Service Manager](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnectsync-service-manager-ui-mvsearch).

Gebruik een van de volgende methoden om de lijst van objecten in uw tenant met LargeObject-fouten:

 * Als uw tenant is ingeschakeld voor Azure AD Connect Health voor synchroniseren, kunt u verwijzen naar de [synchronisatie foutenrapport](https://docs.microsoft.com/azure/active-directory/connect-health/active-directory-aadconnect-health-sync) opgegeven.
 
 * De e-mailmelding voor directory synchronisatiefouten die wordt verzonden aan het einde van elke synchronisatiecyclus heeft de lijst met objecten met LargeObject-fouten. 
 * De [Synchronization Service Manager Operations-tabblad](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnectsync-service-manager-ui-operations) wordt de lijst met objecten met LargeObject-fouten weergegeven als u klikt op de meest recente exporteren naar Azure AD-bewerking.
 
## <a name="mitigation-options"></a>Opties voor risicobeperking
Totdat de LargeObject-fout is opgelost, kan de wijzigingen van andere kenmerken op hetzelfde object kunnen niet worden geëxporteerd naar Azure AD. U kunt de fout oplossen, kunt u de volgende opties overwegen:

 * Azure AD Connect voor het bouwen van 1.1.524.0 en hoger upgraden of na. Bouw 1.1.524.0 en hoger, de out-of-box-synchronisatie regels om te exporteren niet kenmerken userCertificate en usersmimecertificate wordt door als de kenmerken meer dan 15 waarden hebben zijn bijgewerkt in Azure AD Connect. Raadpleeg voor meer informatie over het bijwerken van Azure AD Connect het artikel [Azure AD Connect: Een upgrade uitvoeren van een eerdere versie naar de meest recente](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-upgrade-previous-version).

 * Implementeer een **uitgaande synchronisatieregel** in Azure AD Connect die Hiermee exporteert u een **null-waarde in plaats van de werkelijke waarden voor objecten met meer dan 15 certificaatwaarden**. Deze optie is geschikt als u een van de certificaatwaarden om te worden geëxporteerd naar Azure AD voor objecten met meer dan 15 waarden niet nodig hebt. Raadpleeg voor meer informatie over het implementeren van deze synchronisatieregel, volgende sectie [synchronisatieregel implementeren naar de export van het kenmerk userCertificate beperken](#implementing-sync-rule-to-limit-export-of-usercertificate-attribute).

 * Verminder het aantal certificaatwaarden in het op de on-premises AD-object (15 of minder) door het verwijderen van de waarden die niet langer in gebruik door uw organisatie. Dit is geschikt als het kenmerk onnodig groot bestand wordt veroorzaakt door verlopen of niet-gebruikte certificaten. U kunt de [PowerShell-script beschikbaar hier](https://gallery.technet.microsoft.com/Remove-Expired-Certificates-0517e34f) om te zoeken, back-up maken en verwijderen van verlopen certificaten in uw on-premises AD. Voordat u de certificaten verwijdert, wordt het aanbevolen om te controleren met de beheerders van de infrastructuur van openbare sleutels in uw organisatie.

 * Azure AD Connect om uit te sluiten van het kenmerk userCertificate wordt geëxporteerd naar Azure AD configureren. In het algemeen niet aangeraden deze optie omdat het kenmerk kan worden gebruikt door Microsoft Online Services om in te schakelen specifieke scenario's. In het bijzonder:
    * Het kenmerk userCertificate van het gebruikersobject wordt gebruikt door de Exchange Online en Outlook-clients voor Berichtondertekening en versleuteling. Raadpleeg het artikel voor meer informatie over deze functie [S/MIME voor Berichtondertekening en versleuteling](https://technet.microsoft.com/library/dn626158(v=exchg.150).aspx).

    * Het kenmerk userCertificate van het computerobject wordt gebruikt door Azure AD om toe te staan van Windows 10 on-premises domein apparaten verbinding maken met Azure AD. Raadpleeg voor meer informatie over deze functie, artikel [domein apparaten verbinden met Azure AD voor Windows 10-ervaringen](https://docs.microsoft.com/azure/active-directory/active-directory-azureadjoin-devices-group-policy).

## <a name="implementing-sync-rule-to-limit-export-of-usercertificate-attribute"></a>Implementatie van synchronisatieregel naar de export van het kenmerk userCertificate beperken
Om op te lossen de LargeObject-fout veroorzaakt door het kenmerk userCertificate, kunt u een synchronisatieregel voor uitgaande implementeren in Azure AD Connect die Hiermee exporteert u een **null-waarde in plaats van de werkelijke waarden voor objecten met meer dan 15 certificaatwaarden**. In deze sectie beschrijft de stappen die nodig zijn voor het implementeren van de synchronisatieregel voor **gebruiker** objecten. De stappen kunnen worden aangepast voor **Neem contact op met** en **Computer** objecten.

> [!IMPORTANT]
> Exporteren van null-waarde, verwijdert u het certificaat waarden eerder is geëxporteerd naar Azure AD.

De stappen kunnen worden samengevat als:

1. Synchronisatieplanning uitschakelen en controleer of dat er is geen synchronisatie uitgevoerd.
3. De bestaande synchronisatieregel voor uitgaande voor kenmerk userCertificate niet vinden.
4. Maak de synchronisatieregel voor uitgaande vereist is.
5. Controleer of de nieuwe synchronisatieregel op een bestaand object met LargeObject-fout.
6. De nieuwe synchronisatieregel van toepassing op de resterende objecten met LargeObject-fout.
7. Controleer of er zijn geen onverwachte wijzigingen die nog moeten worden geëxporteerd naar Azure AD.
8. Exporteer de wijzigingen naar Azure AD.
9. Synchronisatieplanning opnieuw in te schakelen.

### <a name="step-1-disable-sync-scheduler-and-verify-there-is-no-synchronization-in-progress"></a>Step 1. Synchronisatieplanning uitschakelen en controleer of dat er is geen synchronisatie wordt uitgevoerd
Zorg ervoor dat er geen synchronisatie vindt plaats wanneer u zich in het midden van implementatie van een nieuwe synchronisatieregel voor om te voorkomen dat onbedoelde wijzigingen wordt geëxporteerd naar Azure AD. De ingebouwde Synchronisatieplanning uitschakelen:
1. PowerShell-sessie starten op de Azure AD Connect-server.

2. Geplande synchronisatie uitschakelen door de cmdlet uit te voeren: `Set-ADSyncScheduler -SyncCycleEnabled $false`

> [!Note]
> De voorgaande stappen zijn alleen van toepassing zijn op nieuwere versies (1.1.xxx.x) van Azure AD Connect met behulp van ingebouwde scheduler. Als u oudere versies (1.0.xxx.x) van Azure AD Connect die gebruikmaakt van Windows Taakplanner gebruikt, of u uw eigen aangepaste scheduler (niet-algemene) gebruikt voor het activeren van periodieke synchronisatie, moet u deze dienovereenkomstig uitschakelen.

3. Start de **Synchronization Service Manager** door te gaan naar de synchronisatieservice START →.

4. Ga naar de **Operations** tabblad en controleer of er is geen bewerking met de status *'wordt uitgevoerd.'*

### <a name="step-2-find-the-existing-outbound-sync-rule-for-usercertificate-attribute"></a>Stap 2. De bestaande synchronisatieregel voor uitgaande vinden voor het kenmerk userCertificate
Er moet een bestaande regel voor synchroniseren die is ingeschakeld en geconfigureerd voor het kenmerk userCertificate voor gebruikersobjecten exporteren naar Azure AD. Zoek deze synchronisatieregel om erachter te komen de **prioriteit** en **bereikfilter** configuratie:

1. Start de **Synchronization Rules Editor** door te gaan naar START → Synchronization Rules Editor.

2. De search-filters te configureren met de volgende waarden:

    | Kenmerk | Value |
    | --- | --- |
    | Richting |**Uitgaande** |
    | MV-objecttype |**Person** |
    | Connector |*naam van uw Azure AD-connector* |
    | Connector-objecttype |**user** |
    | MV-kenmerk |**userCertificate** |

3. Als u OOB (out-of-box)-synchronisatieregels met Azure AD-connector gebruikt voor het exporteren van userCertficiate-kenmerk voor gebruikersobjecten, je weer toegang te krijgen de *"Uit naar AAD – gebruiker ExchangeOnline"* regel.
4. Noteer de **prioriteit** waarde van deze synchronisatieregel.
5. Selecteer de synchronisatieregel en klik op **bewerken**.
6. In de *'Gereserveerde bevestiging regel bewerken'* pop-upvenster, klikt u op **Nee**. (Geen zorgen, we gaan niet een wijziging aanbrengt in deze synchronisatieregel).
7. Selecteer in het scherm voor bewerken, de **Scoping filter** tabblad.
8. Noteer de configuratie van het bereik. Als u van de synchronisatieregel OOB gebruikmaakt, precies moet er **één filter bereikgroep met twee componenten**, waaronder:

    | Kenmerk | Operator | Value |
    | --- | --- | --- |
    | sourceObjectType | GELIJK AAN | Gebruiker |
    | cloudMastered | NOTEQUAL | True |

### <a name="step-3-create-the-outbound-sync-rule-required"></a>Stap 3. Maken van de uitgaande synchronisatieregel vereist
De nieuwe synchronisatieregel moet hebben dezelfde **bereikfilter** en **hogere prioriteit** dan de bestaande synchronisatieregel. Dit zorgt ervoor dat de nieuwe synchronisatieregel van toepassing op dezelfde set objecten als de bestaande synchronisatieregel en de bestaande synchronisatieregel voor het kenmerk userCertificate overschrijft. De synchronisatieregel maken:
1. Klik in de Synchronization Rules Editor op de **nieuwe regel toevoegen** knop.
2. Onder de **tabblad Beschrijving**, bieden de volgende configuratie:

    | Kenmerk | Value | Details |
    | --- | --- | --- |
    | Name | *Geef een naam* | Bijvoorbeeld, *"Uit voor AAD-aangepaste onderdrukking voor userCertificate"* |
    | Description | *Geef een beschrijving* | Bijvoorbeeld, *"Als kenmerk userCertificate meer dan 15 waarden heeft, exporteren null zijn."* |
    | Verbonden systeem | *Selecteer de Azure AD-Connector* |
    | Verbonden systeem objecttype | **user** | |
    | Metaverse-objecttype | **person** | |
    | Type koppeling | **Koppelen** | |
    | Prioriteit | *Een getal tussen 1 en 99 gekozen* | Het aantal gekozen mogen niet worden gebruikt door een bestaande synchronisatieregel en heeft een lagere waarde (en dus hogere prioriteit) dan de bestaande synchronisatieregel. |

3. Ga naar de **Scoping filter** tabblad en implementeren van de dezelfde bereikfilter wordt met behulp van de bestaande synchronisatieregel.
4. Overslaan de **Join regels** tabblad.
5. Ga naar de **transformaties** tabblad om toe te voegen een nieuwe transformatie van de volgende configuratie:

    | Kenmerk | Value |
    | --- | --- |
    | Type gebruikersstroom |**expressie** |
    | Doelkenmerk |**userCertificate** |
    | Bronkenmerk |*Gebruik de volgende expressie*: `IIF(IsNullOrEmpty([userCertificate]), NULL, IIF((Count([userCertificate])> 15),AuthoritativeNull,[userCertificate]))` |
    
6. Klik op de **toevoegen** om te maken van de synchronisatieregel.

### <a name="step-4-verify-the-new-sync-rule-on-an-existing-object-with-largeobject-error"></a>Stap 4. Controleer of de nieuwe synchronisatieregel op een bestaand object met LargeObject-fout
Dit is om te controleren of de synchronisatieregel gemaakt correct op een bestaande AD-object met LargeObject-fout werkt voordat u deze op andere objecten toepassen:
1. Ga naar de **Operations** tabblad in de Synchronization Service Manager.
2. Selecteer de meest recente exporteren naar Azure AD-bewerking en klik op een van de objecten met LargeObject-fouten.
3.  Klik in het pop-scherm van de eigenschappen van het Object Connectorgebied op de **Preview** knop.
4. Selecteer in het pop-scherm van de Preview-versie **volledige synchronisatie** en klikt u op **doorvoeren Preview**.
5. Sluit de Preview-scherm en de eigenschappen van het Object Connectorgebied-scherm.
6. Ga naar de **Connectors** tabblad in de Synchronization Service Manager.
7. Met de rechtermuisknop op de **Azure AD** -Connector en selecteer **uitvoeren...**
8. Selecteer in de pop-up Connector uitvoeren **exporteren** stap en klikt u op **OK**.
9. Wacht totdat de Export naar Azure AD om te voltooien en controleer of dat er geen meer LargeObject-fout is opgetreden op het specifieke object.

### <a name="step-5-apply-the-new-sync-rule-to-remaining-objects-with-largeobject-error"></a>Stap 5. De nieuwe synchronisatieregel van toepassing op de resterende objecten met LargeObject-fout
Nadat de synchronisatieregel is toegevoegd, moet u de stap van een volledige synchronisatie worden uitgevoerd op de AD-Connector:
1. Ga naar de **Connectors** tabblad in de Synchronization Service Manager.
2. Met de rechtermuisknop op de **AD** -Connector en selecteer **uitvoeren...**
3. Selecteer in de pop-up Connector uitvoeren **volledige synchronisatie** stap en klikt u op **OK**.
4. Wacht tot de volledige synchronisatie stap te voltooien.
5. Herhaal de bovenstaande stappen voor de resterende AD-Connectors hebt u meer dan één AD-Connectors. Meerdere connectors moeten normaal gesproken hebt u meerdere on-premises adreslijsten.

### <a name="step-6-verify-there-are-no-unexpected-changes-waiting-to-be-exported-to-azure-ad"></a>Stap 6. Controleer of er zijn geen onverwachte wijzigingen die nog moeten worden geëxporteerd naar Azure AD
1. Ga naar de **Connectors** tabblad in de Synchronization Service Manager.
2. Met de rechtermuisknop op de **Azure AD** -Connector en selecteer **Search Connector Space**.
3. In de pop-up Search Connector Space:
    1. Bereik ingesteld op **exportbewerking**.
    2. Schakel alle selectievakjes in 3, met inbegrip van **toevoegen**, **wijzigen** en **verwijderen**.
    3. Klik op **zoeken** om terug te keren van alle objecten met wijzigingen die nog moeten worden geëxporteerd naar Azure AD.
    4. Controleer of dat er zijn geen onverwachte wijzigingen. Dubbelklik op het object voor het onderzoeken van de wijzigingen voor een bepaald object.

### <a name="step-7-export-the-changes-to-azure-ad"></a>Stap 7. De wijzigingen wilt exporteren naar Azure AD
De wijzigingen naar Azure AD exporteren:
1. Ga naar de **Connectors** tabblad in de Synchronization Service Manager.
2. Met de rechtermuisknop op de **Azure AD** -Connector en selecteer **uitvoeren...**
4. Selecteer in de pop-up Connector uitvoeren **exporteren** stap en klikt u op **OK**.
5. Wacht totdat de Export naar Azure AD om te voltooien en controleer of dat er geen meer LargeObject-fouten zijn.

### <a name="step-8-re-enable-sync-scheduler"></a>Stap 8. Synchronisatieplanning opnieuw in te schakelen
Nu dat het probleem is opgelost, moet u opnieuw de ingebouwde Synchronisatieplanning inschakelen:
1. PowerShell-sessie starten.
2. Geplande synchronisatie opnieuw inschakelen door de cmdlet uit te voeren: `Set-ADSyncScheduler -SyncCycleEnabled $true`

> [!Note]
> De voorgaande stappen zijn alleen van toepassing zijn op nieuwere versies (1.1.xxx.x) van Azure AD Connect met behulp van ingebouwde scheduler. Als u oudere versies (1.0.xxx.x) van Azure AD Connect die gebruikmaakt van Windows Taakplanner gebruikt, of u uw eigen aangepaste scheduler (niet-algemene) gebruikt voor het activeren van periodieke synchronisatie, moet u deze dienovereenkomstig uitschakelen.

## <a name="next-steps"></a>Volgende stappen
Lees meer over het [integreren van uw on-premises identiteiten met Azure Active Directory](whatis-hybrid-identity.md).

