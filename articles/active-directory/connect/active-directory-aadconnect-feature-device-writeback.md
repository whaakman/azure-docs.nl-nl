---
title: 'Azure AD Connect: Apparaat terugschrijven inschakelen | Microsoft Docs'
description: In dit document worden het apparaat terugschrijven met Azure AD Connect inschakelen
services: active-directory
documentationcenter: ''
author: billmath
manager: femila
editor: curtand
ms.assetid: c0ff679c-7ed5-4d6e-ac6c-b2b6392e7892
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/08/2018
ms.component: hybrid
ms.author: billmath
ms.openlocfilehash: fa8cdaf1a21a59a5bb695e3be90382f1e33823a2
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/01/2018
ms.locfileid: "34590586"
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
> <li>Slechts één apparaat registratie configuration-object kan worden toegevoegd aan de lokale Active Directory-forest. Deze functie is niet compatibel met een topologie waarbij de lokale Active Directory worden gesynchroniseerd met meerdere Azure AD-mappen.</li>> 

## <a name="part-1-install-azure-ad-connect"></a>Deel 1: Installeer Azure AD Connect
Installeer Azure AD Connect met aangepaste of snelle instellingen. Microsoft raadt aan om te beginnen met alle gebruikers en groepen zijn gesynchroniseerd voordat u Write-back van apparaat inschakelt.

## <a name="part-2-enable-device-writeback-in-azure-ad-connect"></a>Deel 2: Inschakelen apparaat terugschrijven in Azure AD Connect
1. Voer de installatiewizard opnieuw uit. Selecteer **Apparaatopties configureren** pagina van de aanvullende taken en klik op **volgende**. 

    ![Apparaatopties configureren](./media/active-directory-aadconnect-feature-device-writeback/deviceoptions.png)

    >[!NOTE]
    > De nieuwe opties voor het apparaat configureren is alleen beschikbaar in versie 1.1.819.0 en nieuwer.

2. Selecteer op de optiepagina voor apparaat **Write-back van apparaat configureren**. Optie voor **apparaat terugschrijven uitschakelen** niet meer beschikbaar totdat Write-back van apparaat is ingeschakeld. Klik op **volgende** verplaatsen naar de volgende pagina in de wizard.
    ![Hebt gekozen apparaat bewerking](./media/active-directory-aadconnect-feature-device-writeback/configuredevicewriteback1.png)

3. Op de pagina Write-back ziet u het opgegeven domein als de standaard apparaat Write-back-forest.
   ![Aangepaste installatie Write-back van apparaat doelforest](./media/active-directory-aadconnect-feature-device-writeback/writebackforest.png)

4. **Apparaat-container** pagina biedt de mogelijkheid van de voorbereiding van de active directory met behulp van een van de twee beschikbare opties:

    a. **Enterprise-beheerdersreferenties opgeven**: als de ondernemingsbeheerderreferenties worden opgegeven voor het forest waarin de apparaten moeten worden teruggeschreven, Azure AD Connect bereidt het forest automatisch tijdens de configuratie van Write-back van apparaat.

    b. **Download de PowerShell-script**: Azure AD Connect automatisch-genereert een PowerShell-script waarmee de active directory kan worden voorbereid voor write-back van apparaat. Als de ondernemingsbeheerderreferenties kunnen niet worden opgegeven in Azure AD Connect, wordt u aangeraden om te downloaden van het PowerShell-script. De gedownloade PowerShell-script bieden **CreateDeviceContainer.psq** aan de beheerder van de onderneming van het forest waarin apparaten worden teruggeschreven naar.
    ![Active Directory-forest voorbereiden](./media/active-directory-aadconnect-feature-device-writeback/devicecontainercreds.png)
    
    De volgende bewerkingen worden uitgevoerd voor het voorbereiden van het active directory-forest:
    * Als ze niet al bestaat, maakt en configureert u de nieuwe containers en objecten onder CN = configuratie van apparaatregistratie, CN = Services, CN = configuratie, [forest-dn].
    * Als ze niet al bestaat, maakt en configureert u de nieuwe containers en objecten onder CN = Geregistreerdeapparaten, [domein-dn]. Apparaatobjecten wordt gemaakt in deze container.
    * Benodigde machtigingen ingesteld op het Azure AD-Connector-account voor het beheren van apparaten in uw Active Directory.
    * Alleen moet worden uitgevoerd op één forest, zelfs als Azure AD Connect wordt geïnstalleerd op meerdere forests.

## <a name="verify-devices-are-synchronized-to-active-directory"></a>Controleer of de dat apparaten worden gesynchroniseerd met Active Directory
Apparaat terugschrijven moet nu goed werkt. Let erop dat het duren tot 3 uur voor apparaatobjecten voordat kan worden geschreven terug naar AD.  Om te controleren of uw apparaten correct zijn wordt gesynchroniseerd, het volgende doen nadat de synchronisatie-regels hebt voltooid:

1. Start Active Directory-beheercentrum.
2. Vouw Geregistreerdeapparaten binnen het domein dat federated is.

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

## <a name="additional-information"></a>Aanvullende gegevens
* [Risico beheren met voorwaardelijke toegang](../active-directory-conditional-access-azure-portal.md)
* [Instellen van On-premises voorwaardelijke toegang met behulp van Azure Active Directory-apparaatregistratie](../active-directory-device-registration-on-premises-setup.md)

## <a name="next-steps"></a>Volgende stappen
Lees meer over het [integreren van uw on-premises identiteiten met Azure Active Directory](active-directory-aadconnect.md).

