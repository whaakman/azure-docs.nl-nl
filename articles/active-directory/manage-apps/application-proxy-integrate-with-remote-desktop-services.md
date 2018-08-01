---
title: Publiceren van extern bureaublad met Azure AD-toepassingsproxy | Microsoft Docs
description: Bevat informatie over de basisbeginselen van Azure AD Application Proxy connectors.
services: active-directory
documentationcenter: ''
author: barbkess
manager: mtillman
ms.service: active-directory
ms.component: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 06/27/2018
ms.author: barbkess
ms.custom: it-pro
ms.reviewer: harshja
ms.openlocfilehash: 61ac0d823322b919952b7ea426c447e070a09fc1
ms.sourcegitcommit: f86e5d5b6cb5157f7bde6f4308a332bfff73ca0f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/31/2018
ms.locfileid: "39363193"
---
# <a name="publish-remote-desktop-with-azure-ad-application-proxy"></a>Extern bureaublad met Azure AD Application Proxy publiceren

Extern bureaublad-services en Azure AD-toepassingsproxy werken samen om de productiviteit van werknemers die verwijderd van het bedrijfsnetwerk bevinden zijn. 

De doelgroep voor dit artikel is:
- Huidige Application Proxy-klanten die bieden meer toepassingen voor hun eindgebruikers willen door het publiceren van on-premises toepassingen via Extern bureaublad-Services.
- Huidige extern bureaublad-Services-klanten die willen Verminder de kwetsbaarheid voor aanvallen van de implementatie met behulp van Azure AD-toepassingsproxy. In dit scenario biedt een beperkte set van verificatie in twee stappen en besturingselementen voor voorwaardelijke toegang tot van RDS.

## <a name="how-application-proxy-fits-in-the-standard-rds-deployment"></a>Hoe Application Proxy past in de standaardimplementatie van RDS

Een standaardimplementatie van RDS bevat verschillende functieservices van extern bureaublad op Windows Server. Kijken naar de [extern bureaublad-Services architectuur](https://technet.microsoft.com/windows-server-docs/compute/remote-desktop-services/desktop-hosting-logical-architecture), er zijn meerdere implementatie-opties. In tegenstelling tot andere RDS-implementatie-opties, de [RDS-implementatie met Azure AD-toepassingsproxy](https://technet.microsoft.com/windows-server-docs/compute/remote-desktop-services/desktop-hosting-logical-architecture) (weergegeven in het volgende diagram) is een permanente uitgaande verbinding van de server waarop de connectorservice wordt uitgevoerd. Andere implementaties laat open binnenkomende verbindingen via een load balancer.

![Application Proxy bevindt zich tussen het openbare internet en de RDS-VM](./media/application-proxy-integrate-with-remote-desktop-services/rds-with-app-proxy.png)

In de implementatie van een extern bureaublad-services uitvoeren de RD-Webrol en de rol extern bureaublad-Gateway op machines-internetservices. Deze eindpunten worden weergegeven voor de volgende redenen:
- RD Web biedt de gebruiker een openbaar eindpunt als u wilt aanmelden en de verschillende on-premises toepassingen en bureaubladen die toegang te krijgen tot weergeven. Bij het selecteren van een resource, wordt een RDP-verbinding gemaakt met behulp van de systeemeigen app op het besturingssysteem.
- Extern bureaublad-Gateway wordt geleverd in de afbeelding als een gebruiker de RDP-verbinding wordt gestart. De RD-Gateway versleutelde RDP-verkeer via internet worden verwerkt en wordt deze omgezet in de on-premises server die de gebruiker verbinding met maakt. In dit scenario is het de RD-Gateway ontvangt verkeer afkomstig van de Azure AD-toepassingsproxy.

>[!TIP]
>Als u extern bureaublad-services voordat u dit nog niet hebt geïmplementeerd, of wilt u meer informatie voordat u begint, krijgt u informatie over het [naadloos implementeren extern bureaublad-services met Azure Resource Manager en Azure Marketplace](https://technet.microsoft.com/windows-server-docs/compute/remote-desktop-services/rds-in-azure).

## <a name="requirements"></a>Vereisten

- Een client dan de webclient extern bureaublad gebruiken omdat de webclient biedt geen ondersteuning voor Application Proxy.

- Eindpunten van de RD-Web- en de RD-Gateway moet zich op dezelfde computer, en met een algemene basis. RD-Web- en RD-Gateway worden gepubliceerd als een enkele aanvraag met de toepassingsproxy zodat u kunt een eenmalige aanmelding mogelijk tussen de twee toepassingen hebben.

- U hebt al [extern bureaublad-services geïmplementeerd](https://technet.microsoft.com/windows-server-docs/compute/remote-desktop-services/rds-in-azure), en [toepassingsproxy hebt ingeschakeld](application-proxy-enable.md).

- In dit scenario wordt ervan uitgegaan dat uw eindgebruikers gaan via Internet Explorer op Windows 7 of Windows 10-desktops die verbinding via de extern bureaublad-webpagina wordt weergegeven maken. Als u nodig hebt ter ondersteuning van andere besturingssystemen, raadpleegt u [ondersteuning voor andere clientconfiguraties](#support-for-other-client-configurations).

- Bij het publiceren van RD-Web, wordt het aanbevolen gebruik van dezelfde interne en externe FQDN-naam. Als de interne en externe FQDN's verschillen moet u Header vertaling aanvragen om te voorkomen dat de client ontvangt ongeldige koppelingen uitschakelen. 

- In Internet Explorer, schakelt u de extern bureaublad-services ActiveX-invoegtoepassing.

## <a name="deploy-the-joint-rds-and-application-proxy-scenario"></a>Het gezamenlijke extern bureaublad-services en Application Proxy-scenario implementeren

Na het instellen van extern bureaublad-services en Azure AD-toepassingsproxy voor uw omgeving, volg de stappen voor het combineren van de twee oplossingen. Deze stappen helpen bij het publiceren van de twee eindpunten web gerichte-RDS (extern bureaublad-Web- en RD-Gateway) als toepassingen, en vervolgens routeren van verkeer op de extern bureaublad-services om te gaan via Application Proxy.

### <a name="publish-the-rd-host-endpoint"></a>Het eindpunt van de extern bureaublad-Sessiehost host publiceren

1. [Een nieuwe Application Proxy-toepassing publiceren](application-proxy-publish-azure-portal.md) met de volgende waarden:
   - Interne URL: https://\<rdhost\>.com / waar \<rdhost\> is de algemene basis met RD-Web- en RD-Gateway.
   - Externe URL: Dit veld wordt automatisch ingevuld op basis van de naam van de toepassing, maar u kunt deze wijzigen. Uw gebruikers gaan naar deze URL wanneer ze toegang krijgen van RDS. tot
   - Methode voor verificatie vooraf: Azure Active Directory
   - URL-headers vertalen: Nee
2. Gebruikers toewijzen aan de gepubliceerde toepassing in de extern bureaublad. Zorg ervoor dat ze alle toegang tot extern bureaublad-services, te hebben.
3. Laat de methode voor eenmalige aanmelding voor de toepassing als **Azure AD eenmalige aanmelding uitgeschakeld**. Uw gebruikers wordt gevraagd om te verifiëren eenmaal naar Azure AD en eenmaal op RD Web, maar hebben eenmalige aanmelding voor extern bureaublad-Gateway.
4. Ga naar **Azure Active Directory** > **App-registraties** > *uw toepassing* > **instellingen**.
5. Selecteer **eigenschappen** en werk de **URL van startpagina** veld om te verwijzen naar het eindpunt van de RD Web (zoals https://\<rdhost\>.com/RDWeb).

### <a name="direct-rds-traffic-to-application-proxy"></a>Extern bureaublad-services verkeer naar toepassingsproxy

Verbinding maken met de implementatie van extern bureaublad-services als beheerder en wijzig de naam van de RD-Gateway-server voor de implementatie. Deze configuratie zorgt ervoor dat de verbindingen via de Azure AD Application Proxy-service lopen.

1. Verbinding maken met de extern bureaublad-Services-server met de RD Connection Broker-functie.
2. Start **Serverbeheer**.
3. Selecteer **extern bureaublad-Services** in het deelvenster aan de linkerkant.
4. Selecteer **Overzicht**.
5. Selecteer de vervolgkeuzelijst in de sectie overzicht van de implementatie en kies **implementatie-eigenschappen bewerken**.
6. Wijzig in het tabblad Extern bureaublad-Gateway de **servernaam** veld naar de externe URL die u hebt ingesteld voor het eindpunt van de host Extern bureaublad in Application Proxy.
7. Wijzig de **aanmelden methode** veld **wachtwoordverificatie**.

  ![Scherm van de eigenschappen van implementatie van extern bureaublad-services](./media/application-proxy-integrate-with-remote-desktop-services/rds-deployment-properties.png)

8. Voer deze opdracht uit voor elke verzameling. Vervang *\<yourcollectionname\>* en *\<proxyfrontendurl\>* door uw eigen waarden. Met deze opdracht kunt eenmalige aanmelding tussen RD-Web- en RD-Gateway en optimaliseert de prestaties:

   ```
   Set-RDSessionCollectionConfiguration -CollectionName "<yourcollectionname>" -CustomRdpProperty "pre-authentication server address:s:<proxyfrontendurl>`nrequire pre-authentication:i:1"
   ```

   **Bijvoorbeeld:**
   ```
   Set-RDSessionCollectionConfiguration -CollectionName "QuickSessionCollection" -CustomRdpProperty "pre-authentication server address:s:https://remotedesktoptest-aadapdemo.msappproxy.net/`nrequire pre-authentication:i:1"
   ```

9. Als u wilt controleren of de wijziging van de aangepaste RDP-eigenschappen, evenals de inhoud van het RDP-bestand dat zal worden gedownload van RDWeb voor deze verzameling weergeven, moet u de volgende opdracht uitvoeren:
    ```
    (get-wmiobject -Namespace root\cimv2\terminalservices -Class Win32_RDCentralPublishedRemoteDesktop).RDPFileContents
    ```

Nu dat u extern bureaublad hebt geconfigureerd, heeft Azure AD Application Proxy overgenomen als onderdeel van de internetverbinding van RDS. U kunt de andere openbare internetgerichte eindpunten verwijderen op de RD-Web- en RD-Gateway-machines.

## <a name="test-the-scenario"></a>Testen van het scenario

Testen van het scenario met Internet Explorer op een Windows 7 of 10-computer.

1. Ga naar de externe URL die u instelt of vinden van uw toepassing in de [MyApps deelvenster](https://myapps.microsoft.com).
2. U wordt gevraagd om te verifiëren bij Azure Active Directory. Gebruik een account dat u hebt toegewezen aan de toepassing.
3. U wordt gevraagd om te verifiëren bij de RD-Web.
4. Nadat de verificatie van uw extern bureaublad-services is geslaagd, kunt u het bureaublad of de toepassing die u wilt selecteren en aan de slag gaat.

## <a name="support-for-other-client-configurations"></a>Ondersteuning voor andere clientconfiguraties

De configuratie zoals beschreven in dit artikel is bedoeld voor gebruikers op Windows 7- of 10, met Internet Explorer, plus de extern bureaublad-services ActiveX-invoegtoepassing. Als u wilt echter, kunt u ondersteuning voor andere besturingssystemen of browsers. Het verschil is in de verificatiemethode die u gebruikt.

| Verificatiemethode | Ondersteunde client-configuratie |
| --------------------- | ------------------------------ |
| Pre-authenticatie    | Windows 7/10 met behulp van Internet Explorer + RDS ActiveX-invoegtoepassing |
| Passthrough | Een ander besturingssysteem die ondersteuning biedt voor de Microsoft Extern bureaublad-toepassing |

De stroom voor pre-authenticatie biedt de voordelen van meer beveiliging dan de passthrough-stroom. Met vooraf-verificatie kunt u Azure AD-verificatie-functies, zoals eenmalige aanmelding, voorwaardelijke toegang en verificatie in twee stappen voor uw on-premises resources. U ook voor zorgen dat alleen geverifieerd verkeer bereikt uw netwerk.

Voor het gebruik van passthrough-verificatie, moet u er slechts twee wijzigingen in de stappen in dit artikel zijn:
1. In [publiceren van het eindpunt van de extern bureaublad-Sessiehost host](#publish-the-rd-host-endpoint) stap 1, stelt u de methode voor verificatie vooraf op **Passthrough**.
2. In [Direct RDS-verkeer naar de Application Proxy](#direct-rds-traffic-to-application-proxy), slaat u stap 8 volledig.

## <a name="next-steps"></a>Volgende stappen

[Externe toegang tot SharePoint met Azure AD-toepassingsproxy inschakelen](application-proxy-integrate-with-sharepoint-server.md)  
[Beveiligingsoverwegingen voor toegang tot de apps op afstand met behulp van Azure AD-toepassingsproxy](application-proxy-security.md)
