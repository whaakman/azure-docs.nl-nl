---
title: Publiceren van extern bureaublad met Azure AD-toepassingsproxy | Microsoft Docs
description: Bevat informatie over de basisbeginselen van Azure AD-toepassingsproxy connectors.
services: active-directory
documentationcenter: 
author: daveba
manager: mtillman
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/03/2017
ms.author: daveba
ms.custom: it-pro
ms.reviewer: harshja
ms.openlocfilehash: 44b54ad4331d48202044316486a5b1d1ef9202d2
ms.sourcegitcommit: 3cdc82a5561abe564c318bd12986df63fc980a5a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/05/2018
---
# <a name="publish-remote-desktop-with-azure-ad-application-proxy"></a>Extern bureaublad met Azure AD-toepassingsproxy publiceren

Extern bureaublad-services en Azure AD-toepassingsproxy samen ter verbetering van de productiviteit van werknemers die niet op het bedrijfsnetwerk zijn. 

De doelgroep voor dit artikel is:
- Huidige toepassingsproxy-klanten die bieden meer toepassingen voor hun eindgebruikers willen door het publiceren van on-premises toepassingen via Extern bureaublad-Services.
- Huidige extern bureaublad-Services-klanten die willen Verminder de kwetsbaarheid van hun implementatie met behulp van Azure AD-toepassingsproxy. Dit scenario biedt een beperkte set van verificatie in twee stappen en -besturingselementen voor voorwaardelijke toegang tot RDS.

## <a name="how-application-proxy-fits-in-the-standard-rds-deployment"></a>Hoe Application Proxy past in de standaardimplementatie van RDS

Een standaardimplementatie van RDS bevat verschillende functieservices van extern bureaublad met Windows Server. Kijken naar de [extern bureaublad-Services architectuur](https://technet.microsoft.com/windows-server-docs/compute/remote-desktop-services/desktop-hosting-logical-architecture), er zijn meerdere implementatie-opties. In tegenstelling tot andere RDS-implementatie-opties, de [RDS-implementatie met Azure AD-toepassingsproxy](https://technet.microsoft.com/windows-server-docs/compute/remote-desktop-services/desktop-hosting-logical-architecture) (weergegeven in het volgende diagram) heeft een permanente uitgaande verbinding van de server waarop de connector-service wordt uitgevoerd. Andere implementaties laat open binnenkomende verbindingen via een load balancer.

![Toepassingsproxy bevindt zich tussen de RDS-VM en het openbare internet](./media/application-proxy-publish-remote-desktop/rds-with-app-proxy.png)

In een implementatie RDS uitvoeren de extern bureaublad-Webrol en de rol extern bureaublad-Gateway op Internet gerichte machines. Deze eindpunten beschikbaar worden gesteld om de volgende redenen:
- RD Web biedt de gebruiker een openbaar eindpunt om te melden en de verschillende on-premises toepassingen en toegang te krijgen tot bureaubladen weergeven. Een RDP-verbinding wordt gemaakt via de systeemeigen app op het besturingssysteem bij het selecteren van een resource.
- RD-Gateway wordt geleverd in de afbeelding als een gebruiker de RDP-verbinding opent. De RD-Gateway verwerkt versleutelde RDP-verkeer via internet en zet deze om naar de lokale server die de gebruiker verbinding met maakt. In dit scenario is het verkeer van de RD-Gateway ontvangen is afkomstig uit de Azure AD-toepassingsproxy.

>[!TIP]
>Als u RDS voordat u dit nog niet hebt geïmplementeerd, of u meer informatie wilt voordat u begint, informatie over hoe [naadloos implementeren RDS met Azure Resource Manager en Azure Marketplace](https://technet.microsoft.com/windows-server-docs/compute/remote-desktop-services/rds-in-azure).

## <a name="requirements"></a>Vereisten

- Eindpunten van de RD Web- en RD-Gateway moet zich op dezelfde computer en met een algemene basis. RD Web- en RD-Gateway worden gepubliceerd als één toepassing met toepassingsproxy zodat u kunt een eenmalige aanmelding ervaring tussen de twee toepassingen hebben.

- U hebt al [geïmplementeerd RDS](https://technet.microsoft.com/windows-server-docs/compute/remote-desktop-services/rds-in-azure), en [toepassingsproxy hebt ingeschakeld](active-directory-application-proxy-enable.md).

- Dit scenario veronderstelt dat uw eindgebruikers gaan via Internet Explorer op Windows 7 of Windows 10-desktops die verbinding via de extern bureaublad-webpagina maken. Als u nodig hebt voor de ondersteuning van andere besturingssystemen, Zie [ondersteuning voor andere clientconfiguraties](#support-for-other-client-configurations).

- Schakel de RDS ActiveX-invoegtoepassing voor op Internet Explorer.

## <a name="deploy-the-joint-rds-and-application-proxy-scenario"></a>Het gemeenschappelijke RDS en toepassingsproxy-scenario implementeren

Na het instellen van RDS en Azure AD-toepassingsproxy voor uw omgeving, volg de stappen voor het combineren van de twee oplossingen. Deze stappen doorlopen die de twee web gerichte RDS eindpunten (RD Web en RD-Gateway) als toepassingen publiceren en vervolgens het routeren van verkeer op de RDS-toepassingsproxy doorlopen.

### <a name="publish-the-rd-host-endpoint"></a>Publiceren van het eindpunt van de host Extern bureaublad

1. [Een nieuwe Application Proxy-toepassing publiceren](application-proxy-publish-azure-portal.md) met de volgende waarden:
   - Interne URL: https://\<rdhost\>.com / waar \<rdhost\> bevat de algemene die RD Web- en RD-Gateway delen.
   - Externe URL: Dit veld wordt automatisch ingevuld op basis van de naam van de toepassing, maar u kunt deze wijzigen. Uw gebruikers gaat deze URL wanneer ze toegang krijgen RDS. tot
   - Methode voor verificatie vooraf: Azure Active Directory
   - URL-headers vertalen: Nee
2. Gebruikers toewijzen aan de gepubliceerde toepassing van de extern bureaublad. Zorg ervoor dat ze alle toegang tot de RDS, te hebben.
3. Laat de methode voor eenmalige aanmelding voor de toepassing als **Azure AD eenmalige aanmelding uitgeschakeld**. Uw gebruikers worden gevraagd om te verifiëren eenmaal naar Azure AD en eens op RD Web, maar hebben één aanmelding bij de RD-Gateway.
4. Ga naar **Azure Active Directory** > **App registraties** > *uw toepassing* > **instellingen**.
5. Selecteer **eigenschappen** en werk de **start-pagina-URL** veld om te verwijzen naar uw RD Web-eindpunt (zoals https://\<rdhost\>.com/RDWeb).

### <a name="direct-rds-traffic-to-application-proxy"></a>Directe RDS-verkeer naar de toepassingsproxy

Verbinding maken met de RDS-implementatie als beheerder en wijzig de naam van de RD-Gateway-server voor de implementatie. Deze configuratie zorgt ervoor dat de verbindingen gaan via de service Azure AD-toepassingsproxy.

1. Verbinding maken met de RDS-server uitvoeren van de RD Connection Broker-functie.
2. Start **Serverbeheer**.
3. Selecteer **extern bureaublad-Services** in het deelvenster aan de linkerkant.
4. Selecteer **overzicht**.
5. Selecteer de vervolgkeuzelijst in de sectie overzicht van de implementatie en kies **implementatie-eigenschappen bewerken**.
6. Wijzig in het tabblad Extern bureaublad-Gateway de **servernaam** veld naar de externe URL die u voor het eindpunt van de host Extern bureaublad in Application Proxy instelt.
7. Wijzig de **aanmelden methode** veld **wachtwoordverificatie**.

  ![Scherm van de eigenschappen van implementatie van RDS](./media/application-proxy-publish-remote-desktop/rds-deployment-properties.png)

8. Voer deze opdracht voor elke verzameling. Vervang  *\<yourcollectionname\>*  en  *\<proxyfrontendurl\>*  met uw eigen gegevens. Met deze opdracht kunt eenmalige aanmelding tussen RD Web- en RD-Gateway en optimaliseert de prestaties:

   ```
   Set-RDSessionCollectionConfiguration -CollectionName "<yourcollectionname>" -CustomRdpProperty "pre-authentication server address:s:<proxyfrontendurl>`nrequire pre-authentication:i:1"
   ```

   **Bijvoorbeeld:**
   ```
   Set-RDSessionCollectionConfiguration -CollectionName "QuickSessionCollection" -CustomRdpProperty "pre-authentication server address:s:https://remotedesktoptest-aadapdemo.msappproxy.net/`nrequire pre-authentication:i:1"
   ```

9. Om te controleren of de wijziging van de aangepaste RDP-eigenschappen, evenals de inhoud van het RDP-bestand dat zal worden gedownload van RDWeb voor deze verzameling weergeven, moet u de volgende opdracht uitvoeren:
    ```
    (get-wmiobject -Namespace root\cimv2\terminalservices -Class Win32_RDCentralPublishedRemoteDesktop).RDPFileContents
    ```

Nu dat u hebt geconfigureerd dat extern bureaublad, is Azure AD-toepassingsproxy overgenomen als de component internetgerichte van RDS. U kunt de andere openbare internetgerichte eindpunten verwijderen op de RD Web- en RD-Gateway-machines.

## <a name="test-the-scenario"></a>Testen van het scenario

Testen van het scenario met Internet Explorer op een Windows 7 of 10-computer.

1. Ga naar de externe URL die u instelt of vinden van uw toepassing in de [MyApps Configuratiescherm](https://myapps.microsoft.com).
2. U wordt gevraagd om te verifiëren met Azure Active Directory. Gebruik een account dat u hebt toegewezen aan de toepassing.
3. U wordt gevraagd om te verifiëren met extern bureaublad-webservice.
4. Zodra de RDS-verificatie is gelukt, kunt u het bureaublad of de toepassing die u wilt selecteren en aan de slag gaat.

## <a name="support-for-other-client-configurations"></a>Ondersteuning voor andere clientconfiguraties

De configuratie zoals beschreven in dit artikel is bedoeld voor gebruikers in Windows 7 of 10, met Internet Explorer, plus de RDS ActiveX-invoegtoepassing. Als u wilt echter, kunt u ondersteuning voor andere besturingssystemen of browsers. Het verschil is in de verificatiemethode die u gebruikt.

| Verificatiemethode | Ondersteunde client-configuratie |
| --------------------- | ------------------------------ |
| Pre-authenticatie    | Windows 7/10 met behulp van Internet Explorer + RDS ActiveX-invoegtoepassing |
| Passthrough | Een ander besturingssysteem die ondersteuning biedt voor de toepassing Microsoft Extern bureaublad |

De stroom voor pre-authenticatie biedt de voordelen van meer beveiliging dan de passthrough-stroom. U kunt Azure AD authenticatiefuncties zoals het eenmalige aanmelding, voorwaardelijke toegang en verificatie in twee stappen voor uw lokale bronnen gebruiken met vooraf-verificatie. U ook voor zorgen dat alleen geverifieerd verkeer bereikt uw netwerk.

Passthrough om verificatie te gebruiken, moet u er slechts twee wijzigingen in de stappen in dit artikel zijn:
1. In [publiceren van het eindpunt van de host RD](#publish-the-rd-host-endpoint) stap 1, stelt u de methode voor verificatie vooraf op **Passthrough**.
2. In [Direct RDS-verkeer naar de toepassingsproxy](#direct-rds-traffic-to-application-proxy), slaat u stap 8 volledig.

## <a name="next-steps"></a>Volgende stappen

[Externe toegang voor SharePoint met Azure AD-toepassingsproxy inschakelen](application-proxy-enable-remote-access-sharepoint.md)  
[Beveiligingsoverwegingen voor toegang tot de apps op afstand met behulp van Azure AD-toepassingsproxy](application-proxy-security-considerations.md)
