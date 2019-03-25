---
title: Wat is Windows Virtual Desktop Preview?  - Azure
description: Een overzicht van Windows Virtual Desktop Preview.
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: overview
ms.date: 03/21/2019
ms.author: helohr
ms.openlocfilehash: 4443d71d5c0b84ac7dbc18129338229c40323d2c
ms.sourcegitcommit: 81fa781f907405c215073c4e0441f9952fe80fe5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/25/2019
ms.locfileid: "58401043"
---
# <a name="what-is-windows-virtual-desktop-preview"></a>Wat is Windows Virtual Desktop Preview? 

Nu beschikbaar in openbare preview en Windows Virtual Desktop Preview is een desktop- en app-virtualisatie-service die wordt uitgevoerd in de cloud.

Dit is wat u kunt doen wanneer u virtuele Windows-bureaublad op Azure uitvoert:

* Instellen van een Windows 10-implementatie voor meerdere sessies die zorgt voor een volledige Windows-10 met schaalbaarheid
* Office 365 ProPlus virtualiseren en deze wordt uitgevoerd in virtuele scenario's voor meerdere gebruikers te optimaliseren
* Virtuele bureaubladen voor Windows 7 voorzien van gratis uitgebreid beveiligingsupdates
* Breng uw bestaande Remote Desktop Services (RDS) en Windows Server-desktops en apps aan een computer
* Virtualiseren zowel desktops en apps
* Beheren van Windows 10, Windows Server en Windows 7-desktops en apps met een geïntegreerde beheerervaring

## <a name="key-capabilities"></a>Belangrijkste mogelijkheden

Met virtuele Windows-bureaublad, kunt u een schaalbare en flexibele omgeving instellen:

* Maak een omgeving met volledige bureaubladvirtualisatie in uw Azure-abonnement zonder eventuele extra gatewayservers uitvoeren.
* Omdat veel toepassingen hosten als u nodig hebt om aan uw diverse werkbelastingen te kunnen publiceren.
* Breng uw eigen installatiekopie voor werkbelastingen voor productie of testen van de Azure-galerie.
* Lagere kosten met gegroepeerde, meerdere sessie bronnen. Met de nieuwe Windows 10 Enterprise meerdere sessie functionaliteit exclusief voor virtuele Windows-bureaublad en Remote Desktop Session Host (RDSH)-rol op Windows Server, die kunt u aanzienlijk verminderen het aantal virtuele machines en het besturingssysteem (OS) overhead terwijl u nog de dezelfde bronnen leveren waarmee uw gebruikers.
* Geef individuele eigendom via persoonlijke (permanent) bureaubladen.

U kunt implementeren en virtuele bureaubladen beheren:

* De Windows virtuele bureaublad PowerShell en REST-interfaces gebruiken om de host-adresgroepen configureren, app-groepen maken, gebruikers toe te wijzen en publiceren van bronnen.
* Volledige bureaublad of afzonderlijke externe apps uit een groep met één host publiceren, afzonderlijke app-groepen voor verschillende sets van gebruikers maken of zelfs gebruikers toewijzen aan meerdere app-groepen om het aantal installatiekopieën te beperken.
* Als u uw omgeving beheert, gebruikt u de ingebouwde gedelegeerde toegang rollen toewijzen en verzamelen van diagnostische gegevens voor meer informatie over diverse fouten van configuratie of de gebruiker.
* De nieuwe diagnostische gegevens service gebruiken voor het oplossen van fouten.
* Alleen de installatiekopie en de virtuele machines, niet op de infrastructuur beheren. U hoeft niet te persoonlijk de extern bureaublad-rollen beheren, net zoals u met extern bureaublad-Services, alleen de virtuele machines in uw Azure-abonnement.

U kunt ook toewijzen en gebruikers verbinding met uw virtuele bureaubladen:

* Wanneer toegewezen, kunnen gebruikers een virtuele Windows-bureaublad-client voor gebruikers verbinding met hun gepubliceerde Windows-desktops en toepassingen starten. Verbinding maken vanaf elk apparaat via een systeemeigen toepassing op uw apparaat of de Windows virtuele bureaublad HTML5-webclient.
* Veilig tot stand brengen gebruikers via de reverse-verbindingen met de service, zodat u nooit te verlaten inkomende poorten openen.

## <a name="requirements"></a>Vereisten

Er zijn enkele dingen die u moet het virtuele Windows-bureaublad instellen en uw gebruikers verbinding maken met hun Windows-desktops en toepassingen.

Controleer eerst of dat u de juiste licenties hebt voor uw gebruikers op basis van het bureaublad en de apps die u van plan bent om te implementeren:

|OS|Vereiste licentie|
|---|---|
|Meerdere sessies van Windows 10 Enterprise of Windows 10-één-sessie|Microsoft E3, E5, A3, A5, Business<br>Windows E3, E5, A3, A5|
|Windows 7|Microsoft E3, E5, A3, A5, Business<br>Windows E3, E5, A3, A5|
|Windows Server 2012 R2, 2016, 2019|Extern bureaublad-services Client Access License (CAL) met Software Assurance|

Uw infrastructuur moet de volgende zaken ter ondersteuning van virtuele Windows-bureaublad:

* An [Azure Active Directory](https://docs.microsoft.com/azure/active-directory/)
* Een Windows Server Active Directory gesynchroniseerd met Azure Active Directory. Dit kan worden ingeschakeld via:
  * Azure AD Connect
  * Azure AD Domain Services
* Een Azure-abonnement, met een virtueel netwerk die bevat of die is verbonden met de Windows Server Active Directory
  
De Azure virtuele machines die u voor virtuele Windows-bureaublad maken moet zijn:

* [Standaard domein](https://docs.microsoft.com/microsoft-desktop-optimization-pack/appv-v4/domain-joined-and-non-domain-joined-clients) of [toegevoegd aan Hybrid AD-join](https://docs.microsoft.com/azure/active-directory/devices/hybrid-azuread-join-plan). Virtuele machines kan niet lid van Azure AD.
* Een van de volgende ondersteunde OS-installatiekopieën uitgevoerd:
  * Windows 10 Enterprise meerdere sessies
  * Windows Server 2016

>[!NOTE]
>Als u een Azure-abonnement nodig hebt, kunt u [zich registreren voor een gratis proefversie van één maand](https://azure.microsoft.com/free/). Als u de gratis evaluatieversie van Azure, moet u Azure AD Domain Services naar uw Windows Server Active Directory met Azure Active Directory gesynchroniseerd houden.

Virtuele Windows-bureaublad bestaat uit de Windows-desktops en apps die u kunt leveren aan gebruikers en de oplossing, die als een service in Azure wordt gehost door Microsoft. Tijdens de openbare preview, desktops en apps kunnen worden geïmplementeerd op virtuele machines (VM's) in een Azure-regio en de oplossing voor het beheer en de gegevens voor deze virtuele machines bevinden zich in de Verenigde Staten (regio VS Oost 2). Dit kan resulteren in overdracht van gegevens naar de Verenigde Staten terwijl u de service tijdens de openbare preview testen. We beginnen om uit de lokalisatie management-oplossing en gegevens voor alle Azure-regio's algemeen beschikbaar vanaf te schalen.

Voor optimale prestaties, zorg ervoor dat uw netwerk voldoet aan de volgende vereisten:

* Latentie van de round-trip (RTT) vanaf het netwerk van de client naar de Azure-regio waar de groepen van de host zijn geïmplementeerd moet minder dan 150 ms.
* Netwerkverkeer kan buiten land randen flow wanneer virtuele machines die als host desktops en apps fungeren verbinding met de management-service maken.
* Om te optimaliseren voor prestaties van het netwerk, wordt u aangeraden dat de sessie-hosten van virtuele machines in dezelfde Azure-regio als de management-service worden samengevoegd.

## <a name="provide-feedback"></a>Feedback geven

Ga naar de [Windows virtuele bureaublad Tech Community](https://techcommunity.microsoft.com/t5/Windows-Virtual-Desktop/bd-p/WindowsVirtualDesktop) bespreken van de virtuele Windows-bureaublad-service met het productteam en actieve communityleden. We zijn niet op dit moment kwesties duurt zolang virtuele Windows-bureaublad in preview.

## <a name="next-steps"></a>Volgende stappen

Als u wilt beginnen, moet u een tenant maken. Voor meer informatie over het maken van een tenant, gaat u naar de zelfstudie van tenant maken.

> [!div class="nextstepaction"]
> [Een tenant maken in Windows Virtual Desktop Preview](tenant-setup-azure-active-directory.md)
