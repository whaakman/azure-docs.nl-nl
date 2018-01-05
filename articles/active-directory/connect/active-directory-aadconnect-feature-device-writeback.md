---
title: 'Azure AD Connect: Apparaat terugschrijven inschakelen | Microsoft Docs'
description: In dit document worden het apparaat terugschrijven met Azure AD Connect inschakelen
services: active-directory
documentationcenter: 
author: billmath
manager: mtillman
editor: curtand
ms.assetid: c0ff679c-7ed5-4d6e-ac6c-b2b6392e7892
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/02/2018
ms.author: billmath
ms.openlocfilehash: fddbbeda50764ade149e8a8f370bf7341da01736
ms.sourcegitcommit: 3cdc82a5561abe564c318bd12986df63fc980a5a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/05/2018
---
# <a name="azure-ad-connect-enabling-device-writeback"></a>Azure AD Connect: Apparaat terugschrijven inschakelen
> [!NOTE]
> Een abonnement op Azure AD Premium is vereist voor write-back van apparaat.
>
>

De volgende documentatie bevat informatie over het inschakelen van de functie van de Write-back van apparaat in Azure AD Connect. Write-back van apparaat wordt gebruikt in de volgende scenario's:

* Inschakelen van voorwaardelijke toegang op basis van apparaten naar ADFS (2012 R2 of hoger) beveiligde toepassingen (relying partyvertrouwensrelaties).

Dit biedt extra beveiliging en zekerheid dat toegang tot toepassingen alleen voor vertrouwde apparaten wordt verleend. Zie voor meer informatie over voorwaardelijke toegang [risico beheren met voorwaardelijke toegang](../active-directory-conditional-access-azure-portal.md) en [instellen van On-premises voorwaardelijke toegang met behulp van Azure Active Directory-apparaatregistratie](../active-directory-conditional-access-automatic-device-registration-setup.md).

> [!IMPORTANT]
> <li>Apparaten moeten zich in hetzelfde forest als de gebruikers. Omdat apparaten moeten worden teruggeschreven naar één forest, ondersteunt deze functie momenteel geen een implementatie met meerdere forests van de gebruiker.</li>
> <li>Slechts één apparaat registratie configuration-object kan worden toegevoegd aan de lokale Active Directory-forest. Deze functie is niet compatibel met een topologie waarbij de lokale Active Directory worden gesynchroniseerd met meerdere Azure AD-tenants.</li>
>

## <a name="part-1-install-azure-ad-connect"></a>Deel 1: Installeer Azure AD Connect
1. Installeer Azure AD Connect met aangepaste of snelle instellingen. Microsoft raadt aan om te beginnen met alle gebruikers en groepen zijn gesynchroniseerd voordat u Write-back van apparaat inschakelt.

## <a name="part-2-prepare-active-directory"></a>Deel 2: Active Directory voorbereiden
Gebruik de volgende stappen uit om voor te bereiden voor het gebruik van Write-back van apparaat.

1. Start PowerShell met verhoogde bevoegdheid op de machine waarop Azure AD Connect is geïnstalleerd.
2. Als de Active Directory PowerShell-module niet is geïnstalleerd, installeert u de Remote Server Administration Tools, die de AD PowerShell-module en dsacls.exe die vereist is voor het uitvoeren van het script bevat. Voer de volgende opdracht uit:

   ``` powershell
   Add-WindowsFeature RSAT-AD-Tools
   ```

3. Als de Azure Active Directory PowerShell-module niet is geïnstalleerd, download en installeer vervolgens uit [Azure Active Directory-Module voor Windows PowerShell (64-bits versie)](http://go.microsoft.com/fwlink/p/?linkid=236297). Dit onderdeel heeft een afhankelijkheid op de aanmeldhulp, die met Azure AD Connect is geïnstalleerd.  
4. Met enterprise-beheerdersreferenties, voer de volgende opdrachten en sluit vervolgens af PowerShell.

   ``` powershell
   Import-Module 'C:\Program Files\Microsoft Azure Active Directory Connect\AdPrep\AdSyncPrep.psm1'
   ```

   ``` powershell
   Initialize-ADSyncDeviceWriteback {Optional:–DomainName [name] Optional:-AdConnectorAccount [account]}
   ```

Enterprise-beheerdersreferenties zijn vereist, omdat wijzigingen in de naamruimte van de configuratie zijn vereist. De domeinbeheerder van een heeft niet voldoende machtigingen.

![PowerShell voor het apparaat terugschrijven inschakelen](./media/active-directory-aadconnect-feature-device-writeback/powershell.png)  

Beschrijving:

* Als ze niet al bestaat, maakt en configureert u de nieuwe containers en objecten onder CN = configuratie van apparaatregistratie, CN = Services, CN = configuratie, [forest-dn].
* Als ze niet al bestaat, maakt en configureert u de nieuwe containers en objecten onder CN = Geregistreerdeapparaten, [domein-dn]. Apparaatobjecten wordt gemaakt in deze container.
* Benodigde machtigingen ingesteld op het Azure AD-Connector-account voor het beheren van apparaten in uw Active Directory.
* Alleen moet worden uitgevoerd op één forest, zelfs als Azure AD Connect wordt geïnstalleerd op meerdere forests.

Parameters:

* Domeinnaam: Active Directory-domein waar apparaatobjecten wordt gemaakt. Opmerking: Alle apparaten voor een opgegeven Active Directory-forest worden gemaakt in één domein.
* AdConnectorAccount: Active Directory-account dat wordt gebruikt door Azure AD Connect om objecten in de directory te beheren. Dit is het account dat wordt gebruikt door Azure AD Connect-synchronisatie verbinding maken met AD. Als u met expresinstellingen hebt geïnstalleerd, is het account dat wordt voorafgegaan door MSOL_.

## <a name="part-3-enable-device-writeback-in-azure-ad-connect"></a>Deel 3: Schakel apparaat terugschrijven in Azure AD Connect
Gebruik de volgende procedure apparaat terugschrijven in Azure AD Connect inschakelen.

1. Voer de installatiewizard opnieuw uit. Selecteer **aanpassen Synchronisatieopties** pagina van de aanvullende taken en klik op **volgende**.
   ![Aangepaste installatie aanpassen synchronisatie-opties](./media/active-directory-aadconnect-feature-device-writeback/devicewriteback2.png)
2. Op de pagina optionele functies Write-back van apparaat wordt niet meer worden lichter gekleurd weergegeven. Neem wordt er rekening mee dat als de Azure AD Connect prep stappen niet voltooide apparaat terugschrijven uit op de pagina optionele functies beschikbaar. Schakel het selectievakje voor write-back van apparaat en klikt u op **volgende**. Als het selectievakje is nog uitgeschakeld, raadpleegt u de [sectie troubleshooting](#the-writeback-checkbox-is-still-disabled).
   ![Aangepaste installatie optionele functies voor write-back van apparaat](./media/active-directory-aadconnect-feature-device-writeback/devicewriteback3.png)
3. Op de pagina Write-back ziet u het opgegeven domein als de standaard apparaat Write-back-forest.
   ![Aangepaste installatie Write-back van apparaat doelforest](./media/active-directory-aadconnect-feature-device-writeback/devicewriteback4.png)
4. Voltooi de installatie van de Wizard zonder aanvullende configuratiewijzigingen. Indien nodig, verwijzen naar [aangepaste installatie van Azure AD Connect.](active-directory-aadconnect-get-started-custom.md)
5. Als u hebt ingeschakeld [filteren](active-directory-aadconnectsync-configure-filtering.md) in Azure AD Connect, Controleer of de zojuist gemaakte container CN = Geregistreerdeapparaten is opgenomen in uw bereik valt.

## <a name="part-4-verify-devices-are-synchronized-to-active-directory"></a>Deel 4: Controleren of apparaten zijn gesynchroniseerd met Active Directory
Apparaat terugschrijven moet nu goed werkt. Let erop dat het duren tot 3 uur voor apparaatobjecten voordat kan worden geschreven terug naar AD. Om te controleren of uw apparaten correct zijn wordt gesynchroniseerd, moet u het volgende doen nadat de synchronisatie is voltooid:

1. Start Active Directory-beheercentrum.
2. Vouw Geregistreerdeapparaten, binnen het domein dat is geconfigureerd in [deel 2](#part-2-prepare-active-directory).  

   ![Active Directory-beheercentrum ingeschreven apparaten](./media/active-directory-aadconnect-feature-device-writeback/devicewriteback5.png)  
   
3. Huidige geregistreerde apparaten worden er weergegeven.  

   ![Active Directory-beheercentrum lijst met apparaten geregistreerd](./media/active-directory-aadconnect-feature-device-writeback/devicewriteback6.png)  

## <a name="enable-conditional-access"></a>Voorwaardelijke toegang inschakelen (Engelstalig artikel)
   Gedetailleerde instructies voor het inschakelen van dit scenario zijn beschikbaar binnen [instellen van On-premises voorwaardelijke toegang met behulp van Azure Active Directory-apparaatregistratie](../active-directory-conditional-access-automatic-device-registration-setup.md).

## <a name="troubleshooting"></a>Problemen oplossen
### <a name="the-writeback-checkbox-is-still-disabled"></a>Het selectievakje Write-back is nog uitgeschakeld
Als het selectievakje voor write-back van apparaat is niet ingeschakeld, zelfs als u de bovenstaande stappen hebt gevolgd, de volgende stappen leidt u door wat de installatiewizard controleren voordat u het selectievakje is ingeschakeld.

Eerste dingen eerste:

* Zorg ervoor dat ten minste één forest Windows Server 2012R2. Het objecttype van het apparaat moet aanwezig zijn.
* Als de installatiewizard wordt al uitgevoerd, klikt u vervolgens wijzigingen niet gedetecteerd. In dit geval wordt de installatiewizard voltooien en voer dit opnieuw.
* Zorg ervoor dat het account dat u in het script voor initialisatie opgeeft daadwerkelijk de juiste gebruiker gebruikt door de Active Directory-Connector. U kunt dit controleren door de volgende stappen uit:
  * Open in het startmenu **synchronisatieservice**.
  * Open de **Connectors** tabblad.
  * De Connector met het type Active Directory Domain Services zoeken en te selecteren.
  * Onder **acties**, selecteer **eigenschappen**.
  * Ga naar **verbinding maken met Active Directory-Forest**. Controleer of de naam van de domein- en gebruikersnaam opgegeven op dit scherm overeen met het account dat is opgegeven voor het script.  
  
    ![Connector-account in Sync Service Manager](./media/active-directory-aadconnect-feature-device-writeback/connectoraccount.png)

Controleer of de configuratie in Active Directory:

* Controleren dat de Device Registration Service bevindt zich in de onderstaande locatie (CN DeviceRegistrationService, CN = Services in de apparaat-registratie, CN = configuratie van apparaatregistratie, CN = Services, CN = = Configuration) onder de configuratienaamgevingscontext.

![Oplossen, DeviceRegistrationService in de naamruimte van de configuratie](./media/active-directory-aadconnect-feature-device-writeback/troubleshoot1.png)

* Controleer of er is slechts één configuratieobject door te zoeken in de configuratie-naamruimte. Als er meer dan één, verwijder het duplicaat.

![Problemen oplossen, zoekt u de dubbele objecten](./media/active-directory-aadconnect-feature-device-writeback/troubleshoot2.png)

* Controleer of het kenmerk msDS-DeviceLocation aanwezig is en een waarde op het object Device Registration Service. Lookup deze locatie en zorg ervoor dat het met het objectType msDS-DeviceContainer aanwezig is.

![Oplossen, msDS-DeviceLocation](./media/active-directory-aadconnect-feature-device-writeback/troubleshoot3.png)

![Oplossen, Geregistreerdeapparaten objectklasse](./media/active-directory-aadconnect-feature-device-writeback/troubleshoot4.png)

* Controleer of het account dat wordt gebruikt door de Active Directory-Connector heeft de vereiste machtigingen voor de container voor geregistreerde apparaten gevonden door de vorige stap. Dit is de verwachte machtigingen voor deze container:

![Problemen oplossen, Controleer de machtigingen voor de container](./media/active-directory-aadconnect-feature-device-writeback/troubleshoot5.png)

* Controleer of de Active Directory-account machtigingen heeft op de CN = configuratie van apparaatregistratie, CN = Services, CN = Configuration-object.

![Problemen oplossen, Controleer de machtigingen voor de configuratie van apparaatregistratie](./media/active-directory-aadconnect-feature-device-writeback/troubleshoot6.png)

## <a name="additional-information"></a>Aanvullende informatie
* [Risico beheren met voorwaardelijke toegang](../active-directory-conditional-access-azure-portal.md)
* [Instellen van On-premises voorwaardelijke toegang met behulp van Azure Active Directory-apparaatregistratie](../active-directory-device-registration-on-premises-setup.md)

## <a name="next-steps"></a>Volgende stappen
Lees meer over het [integreren van uw on-premises identiteiten met Azure Active Directory](active-directory-aadconnect.md).
