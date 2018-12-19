---
title: Over het plannen van de join-implementatie van Azure Active Directory (Azure AD) | Microsoft Docs
description: Worden de stappen die nodig zijn voor het implementeren van Azure AD gekoppelde apparaten in uw omgeving.
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: mtillman
editor: ''
ms.component: devices
ms.assetid: 81d4461e-21c8-4fdd-9076-0e4991979f62
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/21/2018
ms.author: markvi
ms.reviewer: sandeo
ms.openlocfilehash: 3154d5401389d46eb1b9fad335aa18362c5f21f7
ms.sourcegitcommit: eb9dd01614b8e95ebc06139c72fa563b25dc6d13
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/12/2018
ms.locfileid: "53310365"
---
# <a name="how-to-plan-your-azure-ad-join-implementation"></a>Procedure: Uw implementatie van Azure AD-deelname plannen


Azure AD join kunt u apparaten koppelen rechtstreeks naar Azure AD-Tenant zonder de noodzaak om te koppelen aan on-premises Active Directory bij om uw gebruikers productief als veilig te houden. Azure AD join is nu klaar voor ondernemingen voor zowel op schaal en binnen het bereik implementaties.   

In dit artikel biedt u de informatie die u moet uw Azure AD join-implementatie te plannen.

 
## <a name="prerequisites"></a>Vereisten

In dit artikel wordt ervan uitgegaan dat u bekend met bent de [Inleiding tot Apparaatbeheer in Azure Active Directory](../device-management-introduction.md).



## <a name="plan-your-implementation"></a>Uw implementatie plannen

Als u wilt uw hybride Azure AD-implementatie te plannen, moet u vertrouwd raken met:

|   |   |
|---|---|
|![Selecteren][1]|Controleer uw scenario 's|
|![Selecteren][1]|Uw infrastructuur voor identiteiten controleren|
|![Selecteren][1]|Beheer van uw apparaten beoordelen|
|![Selecteren][1]|Informatie over overwegingen voor toepassingen en bronnen|
|![Selecteren][1]|Begrijp welke mogelijkheden u inrichting|
|![Selecteren][1]|Enterprise state roaming configureren|
|![Selecteren][1]|Voorwaardelijke toegang configureren|







## <a name="review-your-scenarios"></a>Controleer uw scenario 's 

Terwijl hybride Azure AD join is misschien voorkeur voor bepaalde scenario's kunt koppelen aan Azure AD u overstappen op een cloud-first-model met Windows. Als u van plan bent te moderniseren van uw apparaten beheren en IT-kosten met betrekking tot apparaat, biedt Azure AD join een goede basis voor het bereiken van deze doelstellingen.  

 
U moet rekening houden met de Azure AD join als uw doelstellingen zijn afgestemd op de volgende criteria:

- U inzetten voor uw gebruikers Microsoft 365 als het pakket productiviteit.

- Wilt u apparaten beheren met een cloud-oplossing voor Apparaatbeheer.

- U wilt vereenvoudigen apparaatinrichting voor geografisch verspreide gebruikers.

- U wilt de infrastructuur van uw toepassingen moderniseren.
 

 

## <a name="review-your-identity-infrastructure"></a>Uw infrastructuur voor identiteiten controleren  

Azure AD join werkt met zowel beheerde als federatieve omgevingen.  


### <a name="managed-environment"></a>Beheerde omgeving

Een beheerde omgeving kan worden geïmplementeerd via [Wachtwoordhashsynchronisatie](https://docs.microsoft.com/azure/active-directory/hybrid/how-to-connect-password-hash-synchronization) of [doorgeven via verificatie](https://docs.microsoft.com/azure/active-directory/hybrid/how-to-connect-pta-quick-start) met naadloze eenmalige aanmelding.

Deze scenario's hoeft u niet een federatieserver voor verificatie te configureren.


### <a name="federated-environment"></a>Federatieve omgeving

Een federatieve omgeving moet een id-provider die ondersteuning biedt voor WS-Trust- en WS-Federation-protocollen zijn:

- **WS-Federation:** Dit protocol is vereist voor een apparaat toevoegen aan Azure AD.

- **WS-Trust:** Dit protocol is vereist voor het aanmelden bij een Azure AD gekoppelde apparaat. 

Als uw identiteitsprovider geen ondersteuning biedt voor deze protocollen, werkt Azure AD join niet systeemeigen. Beginnen met Windows 10 1809, uw gebruikers kunnen zich aanmelden met een apparaat dat is toegevoegd aan Azure AD met een op basis van SAML identiteitsprovider via [web-aanmelding op Windows 10](https://docs.microsoft.com/windows/whats-new/whats-new-windows-10-version-1809#web-sign-in-to-windows-10). Op dit moment is webaanmelding een alleen-preview-functie.


### <a name="smartcards-and-certificate-based-authentication"></a>Smartcards en certificaten gebaseerde verificatie

U kunt smartcards of certificaten gebaseerde verificatie niet gebruiken om apparaten aan Azure AD. Smartcards kan echter aanmelden bij Azure AD gekoppelde apparaten hebt u AD FS geconfigureerd worden gebruikt.

**Aanbeveling:** Implementeer Windows Hello voor bedrijven voor sterk, wachtwoord zonder verificatie van Windows 10-apparaten.


### <a name="user-configuration"></a>Gebruikersconfiguratie

Als u gebruikers in uw:

- **On-premises Active Directory**, moet u deze synchroniseren met Azure AD via [Azure AD Connect](https://docs.microsoft.com/azure/active-directory/hybrid/how-to-connect-sync-whatis). 

- **Azure AD**, geen aanvullende installatie vereist is.

[Alternatieve aanmeldings-id's](https://docs.microsoft.com/windows-server/identity/ad-fs/operations/configuring-alternate-login-id) worden niet ondersteund in Azure AD gekoppelde apparaten. Als uw gebruikers een alternatieve aanmeldings-ID gebruikt, moet u van plan bent om over te schakelen met behulp van hun primaire UPN uit Azure AD.



## <a name="assess-your-device-management"></a>Beheer van uw apparaten beoordelen

### <a name="supported-devices"></a>Ondersteunde apparaten

Azure AD join:

- Is alleen van toepassing op Windows 10-apparaten. 

- Is niet van toepassing op eerdere versies van Windows en andere besturingssystemen. Als u Windows 7/8.1-apparaten hebt, moet u upgraden naar Windows 10 voor het implementeren van Azure AD join.
 
**Aanbeveling:** Gebruik altijd de nieuwste versie van Windows 10 om te profiteren van de bijgewerkte functies.


### <a name="management-platform"></a>Het platform voor wachtwoordbeheer

Device management voor Azure AD gekoppelde apparaten is gebaseerd op een MDM-platform, zoals Intune) en MDM CSP's. Windows 10 heeft een ingebouwde MDM-agent die geschikt is voor alle compatibele MDM-oplossingen.

Er zijn twee manieren voor het beheer van Azure AD gekoppelde apparaten:

- **MDM-alleen** -een apparaat wordt uitsluitend beheerd door een MDM-provider, zoals Intune. Alle beleidsregels worden geleverd als onderdeel van de MDM-inschrijvingsproces. MDM-registratie is voor klanten van Azure AD Premium of EMS een geautomatiseerde stap die deel uitmaakt van een Azure AD join.

- **CO-beheer** -een apparaat wordt beheerd door een MDM-provider en SCCM. In deze benadering is de SCCM-agent geïnstalleerd op een apparaat met MDM worden beheerd voor het beheren van bepaalde aspecten.

Omdat Azure AD join apparaten niet zijn verbonden met on-premises Active Directory, Groepsbeleid worden niet ondersteund.


Als u met behulp van Groepsbeleid, evalueren de pariteit van uw MDM-beleid met behulp van de [MDM migratie analyse hulpprogramma (MMAT)](https://github.com/WindowsDeviceManagement/MMAT). 

Controleer de ondersteunde en niet-ondersteunde beleidsregels om te bepalen of u een MDM-oplossing in plaats van Groepsbeleid gebruiken kunt. Voor niet-ondersteunde-beleid, houd rekening met het volgende:

- Het beleid voor niet-ondersteunde nodig zijn voor Azure AD join apparaten of gebruikers?

- De niet-ondersteunde beleidsregels van toepassing zijn in een cloud gestuurde implementatie?

Als uw MDM-oplossing niet beschikbaar via de app-galerie van Azure AD is, kunt u deze volgens de procedure wordt beschreven in toevoegen [Azure Active Directory-integratie met MDM](https://docs.microsoft.com/windows/client-management/mdm/azure-active-directory-integration-with-mdm). 

U kunt via CO-beheer, SCCM gebruiken voor het beheren van bepaalde aspecten van uw apparaten terwijl beleidsregels worden geleverd via uw MDM-platform. Microsoft Intune kunt CO-beheer met SCCM. Zie voor meer informatie, [CO-beheer voor Windows 10-apparaten](https://docs.microsoft.com/sccm/core/clients/manage/co-management-overview). Als u een MDM-product dan Intune gebruikt, neem contact op met uw MDM-provider op CO-beheer van toepassing zijn scenario's.

**Aanbeveling:** Houd rekening met MDM alleen management voor Azure AD apparaten gekoppelde.



## <a name="understand-considerations-for-applications-and-resources"></a>Informatie over overwegingen voor toepassingen en bronnen

We raden u aan te migreren van toepassingen van on-premises naar de cloud voor een betere gebruiker optreden en toegangsbeheer. Echter, Azure AD join apparaten krijgt u toegang tot zowel on-premises en cloud naadloos toepassingen. Zie voor meer informatie, [hoe eenmalige aanmelding voor on-premises resources werkt in Azure AD gekoppelde apparaten](azuread-join-sso.md).

De volgende secties worden de overwegingen voor verschillende soorten toepassingen en bronnen.

### <a name="cloud-based-applications"></a>Cloud-Apps

Als een toepassing wordt toegevoegd aan Azure AD-app-galerie, krijgen gebruikers eenmalige aanmelding via Azure AD gekoppelde apparaten. Er is geen aanvullende configuratie vereist. Gebruikers krijgen eenmalige aanmelding op zowel, Microsoft Edge en Chrome-browser. Voor Chrome, moet u implementeren de [Windows 10-Accounts extensie](https://chrome.google.com/webstore/detail/windows-10-accounts/ppnbnpeolgkicgegkbkbjmhlideopiji). 

Alle Win32-toepassingen die:

- Vertrouw op Web Account Manager (WAM) voor eenmalige aanmelding tokenaanvragen ook op Azure AD gekoppelde apparaten krijgen. 

- Vertrouw niet alleen op WAM kan gebruikers voor de verificatie wordt gevraagd. 


### <a name="on-premises-web-applications"></a>On-premises webtoepassingen

Als uw apps worden gebouwd en/of on-premises gehost moet u ze toevoegen aan vertrouwde sites van uw browser:

- Inschakelen van geïntegreerde Windows-verificatie om te werken 
- Een Nee-prompt SSO-ervaring bieden aan gebruikers. 

Als u AD FS, Zie [controleren en beheren van eenmalige aanmelding met AD FS](https://docs.microsoft.com/previous-versions/azure/azure-services/jj151809(v%3dazure.100)). 

**Aanbeveling:** Houd rekening met hosting in de cloud (bijvoorbeeld Azure) en integreren met Azure AD voor een betere ervaring.

### <a name="on-premises-applications-relying-on-legacy-protocols"></a>On-premises toepassingen die afhankelijk zijn van verouderde protocollen

Gebruikers krijgen eenmalige aanmelding van Azure AD gekoppelde apparaten als het apparaat toegang tot een domeincontroller heeft. 

**Aanbeveling:** Implementeer [Azure AD App proxy](https://docs.microsoft.com/azure/active-directory/manage-apps/application-proxy) om toegang te beveiligen voor deze toepassingen.


### <a name="on-premises-network-shares"></a>On-premises netwerkshares

Wanneer een apparaat toegang tot een on-premises domeincontroller heeft, hebben uw gebruikers eenmalige aanmelding van Azure AD gekoppelde apparaten.

### <a name="printers"></a>Printers

Voor printers, moet u implementeren [hybride cloud print](https://docs.microsoft.com/windows-server/administration/hybrid-cloud-print/hybrid-cloud-print-deploy) voor het detecteren van printers in Azure AD gekoppelde apparaten. 

Terwijl printers kunnen niet in een cloudomgeving alleen automatisch worden gedetecteerd, kunnen uw gebruikers de printers UNC-pad ook gebruiken voor het rechtstreeks toe te voegen. 


### <a name="on-premises-applications-relying-on-machine-authentication"></a>On-premises toepassingen die afhankelijk zijn van verificatie van computer

Azure AD gekoppelde apparaten ondersteunen geen on-premises toepassingen die afhankelijk zijn van verificatie van de machine. 

**Aanbeveling:** Houd rekening met buiten gebruik stellen van deze toepassingen en verplaatst naar hun moderne alternatieven.

### <a name="remote-desktop-services"></a>Externe bureaubladservices

Extern bureaublad verbinding met een Azure AD gekoppelde apparaten is vereist voor de hostmachine naar een Azure AD zijn toegevoegd of toegevoegd aan Hybrid Azure AD. Extern bureaublad van een verwijderd of niet-Windows-apparaat wordt niet ondersteund. Zie voor meer informatie, [verbinding maken met Azure AD externe pc die lid is](https://docs.microsoft.com/windows/client-management/connect-to-remote-aadj-pc)


## <a name="understand-your-provisioning-options"></a>Begrijp welke mogelijkheden u inrichting

U kunt inrichten koppelen aan Azure AD met behulp van de volgende methoden:

- **Selfservice in OOBE-instellingen/** - In de self-service-modus, gebruikers gaan via de Azure AD join procesinstellingen hetzij tijdens de Windows-Out van Box Experience (OOBE) of van Windows. Zie voor meer informatie, [uw werk-apparaat toevoegen aan het netwerk van uw organisatie](https://docs.microsoft.com/azure/active-directory/user-help/user-help-join-device-on-network). 

- **Windows Autopilot** -Windows Autopilot kunt u vooraf configuratie van apparaten voor een soepeler ervaring in OOBE om uit te voeren van een Azure AD join. Zie voor meer informatie de [overzicht van Windows Autopilot](https://docs.microsoft.com/windows/deployment/windows-autopilot/windows-10-autopilot). 

- **Bulkinschrijving** -met bulkinschrijving kunnen een beheerder basis van Azure AD join met behulp van een bulksgewijze inrichting hulpprogramma om apparaten te configureren. Zie voor meer informatie, [bulkinschrijving voor Windows-apparaten](https://docs.microsoft.com/intune/windows-bulk-enroll).
 
 


Hier volgt een vergelijking van deze drie methoden 

 
||Selfservice instellen|Windows Autopilot|Bulkinschrijving|
|---|---|---|---|
|Vereisen dat tussenkomst van de gebruiker om in te stellen|Ja|Ja|Nee|
|IT-inspanning vereisen|Nee|Ja|Ja|
|Van toepassing stromen|OOBE & stellingen|Alleen OOBE|Alleen OOBE|
|Lokale beheerdersrechten voor de primaire gebruiker|Ja, standaard|Configureerbare|Nee|
|Vereisen dat OEM-Apparaatondersteuning|Nee|Ja|Nee|
|Ondersteunde versies|versie 1511 +|1709 +|1703 +|
 
Kies uw implementatiebenadering of benaderingen door de bovenstaande tabel controleren en beoordelen van de volgende overwegingen voor het overstappen op beide benaderingen:  

- Zijn uw technische gebruikers ervaren om te gaan via de instelling zelf? 

    - Self-service kan geschikt is voor deze gebruikers. Houd rekening met Windows Autopilot voor het verbeteren van de gebruikerservaring.  

- Zijn uw gebruikers externe of binnen zakelijke premises? 

    - Selfservice of Autopilot werken het beste voor externe gebruikers voor een probleemloze installatie. 
 
- Hebt u een voorkeur basis van een gebruiker of de configuratie van een beheerder beheerd? 

    - Bulkinschrijving werkt beter voor de beheerder gestuurde implementatie voor het instellen van apparaten voordat via toekent aan gebruikers.     

- U koop apparaten van 1-2 OEM's of hebt u een brede distributie van OEM-apparaten?  

    - Als het kopen van beperkte OEM's die Autopilot worden ook ondersteund, kunt u profiteren van betere integratie met Autopilot. 
 

## <a name="configure-your-device-settings"></a>De apparaatinstellingen configureren

De Azure-portal kunt dat u de controle van de implementatie van Azure AD gekoppelde apparaten in uw organisatie. De gerelateerde instellingen te configureren op de **Azure Active Directory-pagina**, selecteer `Devices > Device settings`.

### <a name="users-may-join-devices-to-azure-ad"></a>Gebruikers mogen apparaten aan Azure AD toevoegen

Deze optie instelt op **alle** of **geselecteerde** op basis van het bereik van uw implementatie en die u wilt toestaan om in te stellen van een Azure AD toegevoegde apparaat. 

![Gebruikers mogen apparaten aan Azure AD toevoegen](./media/azureadjoin-plan/01.png)

### <a name="additional-local-administrators-on-azure-ad-joined-devices"></a>Extra lokale beheerders voor apparaten die zijn toegevoegd aan Azure AD

Kies **geselecteerde** en selecteert u de gebruikers die u wilt toevoegen aan de groep lokale beheerders op alle Azure AD gekoppelde apparaten. 

![Extra lokale beheerders voor apparaten die zijn toegevoegd aan Azure AD](./media/azureadjoin-plan/02.png)


### <a name="require-multi-factor-auth-to-join-devices"></a>Multi-factor Authentication worden toegevoegd aan apparaten vereist

Selecteer **"Ja** als u wilt gebruikers om uit te voeren van MFA dat tijdens het toevoegen van apparaten aan Azure AD. Voor de gebruikers apparaten toevoegen aan Azure AD MFA gebruiken, wordt het apparaat zelf een 2e factor.

![Multi-factor Authentication worden toegevoegd aan apparaten vereist](./media/azureadjoin-plan/03.png)




## <a name="configure-your-mobility-settings"></a>De mobiliteitsinstellingen configureren

Voordat u de mobiliteitsinstellingen configureren kunt, moet u wellicht een MDM-provider, eerst toevoegen.

**Toevoegen van een MDM-provider**:

1. Op de **Azure Active Directory-pagina**, in de **beheren** sectie, klikt u op `Mobility (MDM and MAM)`. 

2. Klik op **toepassing toevoegen**.

3. Selecteer uw MDM-provider in de lijst.

    ![Een toepassing toevoegen](./media/azureadjoin-plan/04.png)

Selecteer uw MDM-provider om de gerelateerde instellingen te configureren. 

### <a name="mdm-user-scope"></a>Gebruikersbereik van MDM

Selecteer **sommige** of **alle** op basis van het bereik van uw implementatie. 

![Gebruikersbereik van MDM](./media/azureadjoin-plan/05.png)

Op basis van uw bereik valt, gebeurt het volgende: 

- **Gebruiker is in MDM bereik**: Als u een Azure AD Premium-abonnement hebt, is MDM-registratie geautomatiseerd, samen met Azure AD join. Alle binnen het bereik gebruikers moeten een juiste licentie hebben voor uw MDM. Als de MDM-registratie mislukt in dit scenario, wordt Azure AD join eveneens teruggedraaid.
    
- **Gebruiker is niet binnen het bereik van MDM**: Als gebruikers zich niet binnen het bereik voor MDM, Azure AD join zonder een MDM-registratie is voltooid. Dit resulteert in een niet-beheerd apparaat.


### <a name="mdm-urls"></a>MDM-URL 's

Er zijn drie URL's die zijn gerelateerd aan uw MDM-configuratie:

- URL voor MDM-gebruiksvoorwaarden

- Detectie-URL voor MDM 

- URL van MDM-naleving


![Een toepassing toevoegen](./media/azureadjoin-plan/06.png)


Elke URL heeft een vooraf gedefinieerde standaardwaarde. Als deze velden leeg zijn, kunt u contact op met uw MDM-provider voor meer informatie.

### <a name="mam-settings"></a>Instellingen voor MAM

MAM geldt niet voor Azure AD join. 


## <a name="configure-enterprise-state-roaming"></a>Enterprise state roaming configureren

Als u inschakelen status roaming naar Azure AD wilt zodat gebruikers hun instellingen via apparaten synchroniseren kunnen, raadpleegt u [inschakelen Enterprise State Roaming in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/devices/enterprise-state-roaming-enable). 

**Aanbeveling**: Schakel deze instelling ook voor hybride Azure AD gekoppelde apparaten.


## <a name="configure-conditional-access"></a>Voorwaardelijke toegang configureren

Als u een MDM-provider die is geconfigureerd voor uw Azure AD gekoppelde apparaten, de provider van het apparaat als compatibel worden gemarkeerd als het apparaat wordt beheerd. 

![Compatibel apparaat](./media/azureadjoin-plan/46.png)

U kunt deze implementatie naar [beheerde apparaten vereisen voor toegang tot cloud-Apps met voorwaardelijke toegang](../conditional-access/require-managed-devices.md).




## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Deelnemen aan een nieuwe Windows 10-apparaat met Azure AD tijdens de uitvoering van een eerste](azuread-joined-devices-frx.md)
> [uw werk-apparaat toevoegen aan het netwerk van uw organisatie](https://docs.microsoft.com/azure/active-directory/user-help/user-help-join-device-on-network)


<!--Image references-->
[1]: ./media/azureadjoin-plan/12.png
