---
title: 'Azure AD Connect: Apparaat terugschrijven inschakelen | Microsoft Docs'
description: Dit document wordt uitgelegd hoe apparaat terugschrijven met Azure AD Connect inschakelen
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
ms.topic: conceptual
ms.date: 05/08/2018
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: b7ed6fd0a4a1de3fb02b3d8583c0e5c0cecac211
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/13/2019
ms.locfileid: "56205508"
---
# <a name="azure-ad-connect-enabling-device-writeback"></a>Azure AD Connect: Apparaat terugschrijven inschakelen
> [!NOTE]
> Een abonnement op Azure AD Premium is vereist voor write-back van apparaat.
> 
> 

De volgende documentatie bevat informatie over het inschakelen van de functie apparaat terugschrijven van wachtwoorden in Azure AD Connect. Write-back van apparaat wordt gebruikt in de volgende scenario's:

* Inschakelen van voorwaardelijke toegang op basis van apparaten naar AD FS (2012 R2 of hoger) toepassingen (relying party trusts) die zijn beveiligd.

Dit biedt extra beveiliging en zekerheid dat de toegang tot toepassingen te tot vertrouwde apparaten krijgen. Zie voor meer informatie over voorwaardelijke toegang, [risico beheren met voorwaardelijke toegang](../active-directory-conditional-access-azure-portal.md) en [instellen van On-premises voorwaardelijke toegang met behulp van Azure Active Directory-apparaatregistratie](../../active-directory/active-directory-device-registration-on-premises-setup.md).

> [!IMPORTANT]
> <li>Apparaten moeten zich in hetzelfde forest als de gebruikers. Omdat apparaten moeten worden teruggeschreven naar één forest, ondersteunt deze functie momenteel geen een implementatie met meerdere forests van de gebruiker.</li>
> <li>Slechts één apparaat registratie configuratieobject kan worden toegevoegd aan de on-premises Active Directory-forest. Deze functie is niet compatibel is met een topologie waarbij de on-premises Active Directory is gesynchroniseerd met meerdere Azure AD-mappen.</li>

## <a name="part-1-install-azure-ad-connect"></a>Deel 1: Azure AD Connect installeren
Installeer Azure AD Connect met aangepaste of snelle instellingen. Microsoft raadt aan om te beginnen met alle gebruikers en groepen zijn gesynchroniseerd voordat u Write-back van apparaat inschakelen.

## <a name="part-2-enable-device-writeback-in-azure-ad-connect"></a>Deel 2: Apparaat terugschrijven van wachtwoorden in Azure AD Connect inschakelen
1. Voer de installatiewizard opnieuw uit. Selecteer **Apparaatopties configureren** pagina van de aanvullende taken en klik op **volgende**. 

    ![Apparaatopties configureren](./media/how-to-connect-device-writeback/deviceoptions.png)

    >[!NOTE]
    > De nieuwe opties voor het apparaat configureren is alleen beschikbaar in versie 1.1.819.0 en nieuwer.

2. Selecteer op de pagina van de opties voor apparaat **Write-back van apparaat configureren**. Optie voor **Write-back van apparaat uitschakelen** niet meer beschikbaar totdat Write-back van apparaat is ingeschakeld. Klik op **volgende** te verplaatsen naar de volgende pagina in de wizard.
    ![Gekozen apparaat bewerking](./media/how-to-connect-device-writeback/configuredevicewriteback1.png)

3. Op de pagina Write-back ziet u het opgegeven domein als de standaard apparaat terugschrijven-forest.
   ![Aangepaste installatie Write-back van apparaat doelforest](./media/how-to-connect-device-writeback/writebackforest.png)

4. **Apparaatcontainer** pagina biedt de mogelijkheid van de voorbereiding van de active directory met behulp van een van de twee beschikbare opties:

    a. **Enterprise-beheerdersreferenties opgeven**: Als de referenties voor ondernemingsadministrator worden opgegeven voor het forest waarin apparaten moeten worden teruggeschreven, wordt Azure AD Connect het forest automatisch voorbereid tijdens de configuratie van Write-back van apparaat.

    b. **PowerShell-script downloaden**: Azure AD Connect wordt automatisch gegenereerd een PowerShell-script dat de active directory voor write-back van apparaat voorbereiden kunt. Als de referenties voor ondernemingsadministrator kunnen niet worden opgegeven in de Azure AD Connect, is het nodig om te downloaden van het PowerShell-script. De gedownloade PowerShell-script bieden **CreateDeviceContainer.psq** naar de enterprise-beheerder van het forest waarin apparaten worden teruggeschreven naar.
    ![Active directory-forest voorbereiden](./media/how-to-connect-device-writeback/devicecontainercreds.png)
    
    De volgende bewerkingen worden uitgevoerd voor het voorbereiden van de active directory-forest:
    * Als ze niet al bestaan, maakt en configureert u de nieuwe containers en objecten onder CN = registratie apparaatconfiguratie, CN = Services, CN = configuratie, [forest-DN-naam].
    * Als ze niet al bestaan, maakt en configureert u de nieuwe containers en objecten onder CN = RegisteredDevices, [domein-DN-naam]. Apparaatobjecten wordt in deze container gemaakt.
    * Hiermee stelt u vereiste machtigingen op het Azure AD Connector-account voor het beheren van apparaten in uw Active Directory.
    * Alleen moet worden uitgevoerd op één forest, zelfs als Azure AD Connect wordt geïnstalleerd op meerdere forests.

## <a name="verify-devices-are-synchronized-to-active-directory"></a>Controleer of de dat apparaten worden gesynchroniseerd met Active Directory
Write-back van apparaat, moet nu goed werkt. Let erop dat duurt tot 3 uur voor apparaatobjecten om te worden geschreven terug naar AD.  Om te controleren of uw apparaten correct zijn die wordt gesynchroniseerd, het volgende doen nadat de synchronisatieregels hebt voltooid:

1. Start Active Directory-beheercentrum.
2. Vouw RegisteredDevices, binnen het domein dat wordt is gefedereerd.

   ![Active Directory-beheercentrum ingeschreven apparaten](./media/how-to-connect-device-writeback/devicewriteback5.png)

3. Er wordt het huidige geregistreerde apparaten weergegeven.

   ![Active Directory-beheercentrum geregistreerd lijst met apparaten](./media/how-to-connect-device-writeback/devicewriteback6.png)

## <a name="enable-conditional-access"></a>Voorwaardelijke toegang inschakelen (Engelstalig artikel)
Gedetailleerde instructies voor het inschakelen van dit scenario zijn beschikbaar binnen [instellen van On-premises voorwaardelijke toegang met behulp van Azure Active Directory-apparaatregistratie](../../active-directory/active-directory-device-registration-on-premises-setup.md).

## <a name="troubleshooting"></a>Problemen oplossen
### <a name="the-writeback-checkbox-is-still-disabled"></a>Het selectievakje terugschrijven is nog steeds uitgeschakeld
Als het selectievakje voor write-back van apparaat is niet ingeschakeld, zelfs als u de bovenstaande stappen hebt gevolgd, de volgende stappen leidt u door wat de installatiewizard wordt gecontroleerd voordat u het selectievakje is ingeschakeld.

Eerste dingen eerste:

* Het forest waar de apparaten aanwezig zijn, moet het forestschema bijgewerkt naar Windows 2012 R2-niveau, zodat het apparaatobject en de bijbehorende kenmerken aanwezig zijn.
* Als de installatiewizard wordt al uitgevoerd, worden klikt u vervolgens eventuele wijzigingen niet gedetecteerd. In dit geval, voltooi de installatiewizard en start deze opnieuw.
* Zorg ervoor dat het account dat u in het script voor initialisatie opgeeft daadwerkelijk de juiste gebruiker door de Active Directory-Connector gebruikt. U kunt dit controleren door de volgende stappen uit:
  * Open in het startmenu **Synchronization Service**.
  * Open de **Connectors** tabblad.
  * De Connector met het type Active Directory Domain Services zoeken en te selecteren.
  * Onder **acties**, selecteer **eigenschappen**.
  * Ga naar **verbinding maken met Active Directory-Forest**. Controleer of dat door de naam van het domein en de gebruikersnaam worden opgegeven op deze overeenkomst scherm die door het account dat is opgegeven voor het script.
    ![Connector-account in Sync Service Manager](./media/how-to-connect-device-writeback/connectoraccount.png)

Controleer of de configuratie in Active Directory:

* Controleren dat de Device Registration Service bevindt zich in de onderstaande locatie (CN DeviceRegistrationService, CN = apparaat registratieservices, CN = registratie apparaatconfiguratie, CN = Services, CN = configuratie) onder de configuratienaamgevingscontext.

![Oplossen, DeviceRegistrationService in de configuratie van naamruimte](./media/how-to-connect-device-writeback/troubleshoot1.png)

* Controleer of er is slechts één configuratieobject door te zoeken naar de naamruimte van de configuratie. Als er meer dan één, verwijder het duplicaat.

![Problemen oplossen, de dubbele objecten zoeken](./media/how-to-connect-device-writeback/troubleshoot2.png)

* Controleer of het kenmerk msDS-DeviceLocation aanwezig is en heeft een waarde op het object Device Registration Service. Opzoeken van deze locatie en zorg ervoor dat het met het objectType msDS-DeviceContainer aanwezig is.

![Oplossen, msDS-DeviceLocation](./media/how-to-connect-device-writeback/troubleshoot3.png)

![Oplossen, RegisteredDevices objectklasse](./media/how-to-connect-device-writeback/troubleshoot4.png)

* Controleer of het account voor de Active Directory-Connector heeft de vereiste machtigingen voor de container voor geregistreerde apparaten die met de vorige stap is gevonden. Dit is de verwachte machtigingen voor deze container:

![Problemen op, Controleer de machtigingen op de container](./media/how-to-connect-device-writeback/troubleshoot5.png)

* Controleer of de Active Directory-account machtigingen heeft op de CN = registratie apparaatconfiguratie, CN = Services, CN = Configuration-object.

![Problemen op, Controleer de machtigingen op de configuratie van apparaatregistratie](./media/how-to-connect-device-writeback/troubleshoot6.png)

## <a name="additional-information"></a>Aanvullende informatie
* [Risico beheren met voorwaardelijke toegang](../active-directory-conditional-access-azure-portal.md)
* [Instellen van On-premises voorwaardelijke toegang met behulp van Azure Active Directory-apparaatregistratie](../../active-directory/active-directory-device-registration-on-premises-setup.md)

## <a name="next-steps"></a>Volgende stappen
Lees meer over het [integreren van uw on-premises identiteiten met Azure Active Directory](whatis-hybrid-identity.md).

