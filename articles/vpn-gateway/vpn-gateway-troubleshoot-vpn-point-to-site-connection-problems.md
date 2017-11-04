---
title: Problemen met Azure punt-naar-site-verbinding | Microsoft Docs
description: Informatie over het oplossen van problemen met de punt-naar-site-verbinding.
services: vpn-gateway
documentationcenter: na
author: chadmath
manager: cshepard
editor: 
tags: 
ms.service: vpn-gateway
ms.devlang: na
ms.topic: troubleshooting
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/23/2017
ms.author: genli
ms.openlocfilehash: 76ab1600903705aad7f18f48f41cb7119c3c09bf
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="troubleshooting-azure-point-to-site-connection-problems"></a>Voor probleemoplossing: Problemen met de Azure-punt-naar-site-verbinding

Dit artikel worden veelvoorkomende problemen met de punt-naar-site-verbinding die u kunt ervaren. Ook wordt beschreven mogelijke oorzaken en oplossingen voor deze problemen.

## <a name="vpn-client-error-a-certificate-could-not-be-found"></a>VPN-Clientfout: kan certificaat niet vinden.

### <a name="symptom"></a>Symptoom

Wanneer u probeert verbinding maken met een Azure-netwerk met behulp van de VPN-client, wordt het volgende foutbericht weergegeven:

**Een certificaat kan niet worden gevonden die kan worden gebruikt met dit Extensible Authentication Protocol. (Fout 798)**

### <a name="cause"></a>Oorzaak

Dit probleem treedt op als het clientcertificaat ontbreekt in **Certificaten - Huidige gebruiker\Persoonlijk\Certificaten**.

### <a name="solution"></a>Oplossing

U lost dit probleem, de volgende stappen uit:

1. Zorg ervoor dat de volgende certificaten in de juiste locatie:

    | Certificaat | Locatie |
    | ------------- | ------------- |
    | AzureClient.pfx  | Huidige gebruiker\Persoonlijk\Certificaten |
    | Azuregateway -*GUID*. cloudapp.net  | Huidige User\Trusted basiscertificeringsinstanties|
    | AzureGateway -*GUID*. cloudapp.net, AzureRoot.cer    | Lokale Computer\Trusted Root Certification Authorities|

2. Ga naar gebruikers\<UserName > \AppData\Roaming\Microsoft\Network\Connections\Cm\<GUID >, handmatig het certificaat (*.cer-bestand) op de gebruiker en archief van de computer installeren.

Zie voor meer informatie over het installeren van het clientcertificaat [genereren en exporteren van certificaten voor punt-naar-site-verbindingen](vpn-gateway-certificates-point-to-site.md).

> [!NOTE]
> Bij het importeren van het clientcertificaat niet selecteert de **zware beveiliging van persoonlijke sleutel inschakelen** optie.

## <a name="vpn-client-error-the-message-received-was-unexpected-or-badly-formatted"></a>VPN-Clientfout: het ontvangen bericht is onverwacht of onjuist ingedeeld

### <a name="symptom"></a>Symptoom

Wanneer u probeert verbinding maken met een Azure-netwerk met behulp van de VPN-client, wordt het volgende foutbericht weergegeven:

**Het ontvangen bericht is onverwacht of onjuist ingedeeld. (Fout 0x80090326)**

### <a name="cause"></a>Oorzaak

Dit probleem treedt op als de openbare sleutel van het root-certificaat is niet geüpload naar de Azure VPN-gateway. Het kan ook optreden als de sleutel is beschadigd of verlopen.

### <a name="solution"></a>Oplossing

U lost dit probleem, Controleer de status van het basiscertificaat in de Azure portal om te zien of deze is ingetrokken. Als deze niet is ingetrokken, kunt u proberen te verwijderen van het basiscertificaat en reupload. Zie voor meer informatie [certificaten maken](vpn-gateway-howto-point-to-site-classic-azure-portal.md#generatecerts).

## <a name="vpn-client-error-a-certificate-chain-processed-but-terminated"></a>VPN-Clientfout: een certificaatketen verwerkt, maar is beëindigd 

### <a name="symptom"></a>Symptoom 

Wanneer u probeert verbinding maken met een Azure-netwerk met behulp van de VPN-client, wordt het volgende foutbericht weergegeven:

**Een certificaatketen verwerkt, maar in een basiscertificaat dat niet wordt vertrouwd door de provider is beëindigd.**

### <a name="solution"></a>Oplossing

1. Zorg ervoor dat de volgende certificaten in de juiste locatie:

    | Certificaat | Locatie |
    | ------------- | ------------- |
    | AzureClient.pfx  | Huidige gebruiker\Persoonlijk\Certificaten |
    | Azuregateway -*GUID*. cloudapp.net  | Huidige User\Trusted basiscertificeringsinstanties|
    | AzureGateway -*GUID*. cloudapp.net, AzureRoot.cer    | Lokale Computer\Trusted Root Certification Authorities|

2. Als de certificaten al op de locatie zijn, kunt u de certificaten verwijderen en installeert u deze opnieuw. De  **azuregateway -*GUID*. cloudapp.net** certificaat bevindt zich in het configuratiepakket voor VPN-client die u hebt gedownload van de Azure-portal. Bestand archivers kunt u de bestanden extraheren uit het pakket.

## <a name="file-download-error-target-uri-is-not-specified"></a>Fout bij het downloaden bestand: doel-URI is niet opgegeven.

### <a name="symptom"></a>Symptoom

U ontvangt het volgende foutbericht weergegeven:

**Fout bij het downloaden van het bestand. Doel-URI is niet opgegeven.**

### <a name="cause"></a>Oorzaak 

Dit probleem treedt vanwege een onjuiste Gatewaytype. 

### <a name="solution"></a>Oplossing

Het type VPN-gateway moet **VPN**, en het VPN-type moet **RouteBased**.

## <a name="vpn-client-error-azure-vpn-custom-script-failed"></a>VPN-Clientfout: aangepaste Azure VPN-script is mislukt 

### <a name="symptom"></a>Symptoom

Wanneer u probeert verbinding maken met een Azure-netwerk met behulp van de VPN-client, wordt het volgende foutbericht weergegeven:

**Aangepast script (om bij te werken uw routeringstabel) is mislukt. (Fout 8007026f)**

### <a name="cause"></a>Oorzaak

Dit probleem kan optreden als u probeert te openen van de site-naar-punt VPN-verbinding via een snelkoppeling.

### <a name="solution"></a>Oplossing 

Hiermee opent u het VPN-pakket rechtstreeks in plaats van openen vanuit de snelkoppeling.

## <a name="cannot-install-the-vpn-client"></a>De VPN-client installeren niet

### <a name="cause"></a>Oorzaak 

Een aanvullend certificaat is vereist voor het vertrouwen van de VPN-gateway voor het virtuele netwerk. Het certificaat is opgenomen in het configuratiepakket voor VPN-client die wordt gegenereerd vanuit de Azure-portal.

### <a name="solution"></a>Oplossing

Pak het configuratiepakket voor VPN-client en het .cer-bestand niet vinden. Volg deze stappen voor het installeren van het certificaat:

1. Open mmc.exe.
2. Voeg de **certificaten** -module.
3. Selecteer de **Computer** account voor de lokale computer.
4. Met de rechtermuisknop op de **Trusted Root Certification Authorities** knooppunt. Klik op **All-taak** > **importeren**, en blader naar het cer-bestand dat u hebt opgehaald uit de configuratie van VPN-clientpakket.
5. De computer opnieuw opstarten. 
6. Probeer de VPN-client te installeren.

## <a name="azure-portal-error-failed-to-save-the-vpn-gateway-and-the-data-is-invalid"></a>Azure portal-fout: kan niet opslaan van de VPN-gateway en de gegevens zijn ongeldig

### <a name="symptom"></a>Symptoom

Wanneer u probeert de wijzigingen wilt opslaan voor de VPN-gateway in de Azure portal, wordt het volgende foutbericht weergegeven:

**Virtuele netwerkgateway opslaan is mislukt &lt;* gatewaynaam*&gt;. Gegevens voor certificaat &lt; *-certificaat-ID* &gt; is invalid.* *

### <a name="cause"></a>Oorzaak 

Dit probleem kan optreden als hoofdmap openbare sleutel van het certificaat dat u hebt geüpload een ongeldig teken, zoals een spatie bevat.

### <a name="solution"></a>Oplossing

Zorg ervoor dat de gegevens in het certificaat bevat geen ongeldige tekens, zoals regeleinden (regelterugloop). De volledige waarde moet een lange regel. De volgende tekst is een voorbeeld van het certificaat:

    -----BEGIN CERTIFICATE-----
    MIIC5zCCAc+gAwIBAgIQFSwsLuUrCIdHwI3hzJbdBjANBgkqhkiG9w0BAQsFADAW
    MRQwEgYDVQQDDAtQMlNSb290Q2VydDAeFw0xNzA2MTUwMjU4NDZaFw0xODA2MTUw
    MzE4NDZaMBYxFDASBgNVBAMMC1AyU1Jvb3RDZXJ0MIIBIjANBgkqhkiG9w0BAQEF
    AAOCAQ8AMIIBCgKCAQEAz8QUCWCxxxTrxF5yc5uUpL/bzwC5zZ804ltB1NpPa/PI
    sa5uwLw/YFb8XG/JCWxUJpUzS/kHUKFluqkY80U+fAmRmTEMq5wcaMhp3wRfeq+1
    G9OPBNTyqpnHe+i54QAnj1DjsHXXNL4AL1N8/TSzYTm7dkiq+EAIyRRMrZlYwije
    407ChxIp0stB84MtMShhyoSm2hgl+3zfwuaGXoJQwWiXh715kMHVTSj9zFechYd7
    5OLltoRRDyyxsf0qweTFKIgFj13Hn/bq/UJG3AcyQNvlCv1HwQnXO+hckVBB29wE
    sF8QSYk2MMGimPDYYt4ZM5tmYLxxxvGmrGhc+HWXzMeQIDAQABozEwLzAOBgNVHQ8B
    Af8EBAMCAgQwHQYDVR0OBBYEFBE9zZWhQftVLBQNATC/LHLvMb0OMA0GCSqGSIb3
    DQEBCwUAA4IBAQB7k0ySFUQu72sfj3BdNxrXSyOT4L2rADLhxxxiK0U6gHUF6eWz
    /0h6y4mNkg3NgLT3j/WclqzHXZruhWAXSF+VbAGkwcKA99xGWOcUJ+vKVYL/kDja
    gaZrxHlhTYVVmwn4F7DWhteFqhzZ89/W9Mv6p180AimF96qDU8Ez8t860HQaFkU6
    2Nw9ZMsGkvLePZZi78yVBDCWMogBMhrRVXG/xQkBajgvL5syLwFBo2kWGdC+wyWY
    U/Z+EK9UuHnn3Hkq/vXEzRVsYuaxchta0X2UNRzRq+o706l+iyLTpe6fnvW6ilOi
    e8Jcej7mzunzyjz4chN0/WVF94MtxbUkLkqP
    -----END CERTIFICATE-----

## <a name="azure-portal-error-failed-to-save-the-vpn-gateway-and-the-resource-name-is-invalid"></a>Azure portal-fout: kan niet opslaan van de VPN-gateway en de resourcenaam is ongeldig

### <a name="symptom"></a>Symptoom

Wanneer u probeert de wijzigingen wilt opslaan voor de VPN-gateway in de Azure portal, wordt het volgende foutbericht weergegeven: 

**Virtuele netwerkgateway opslaan is mislukt &lt;* gatewaynaam*&gt;. Resourcenaam &lt; *certificaatnaam die u probeert te uploaden* &gt; is ongeldig **.

### <a name="cause"></a>Oorzaak

Dit probleem treedt op omdat de naam van het certificaat een ongeldig teken, zoals een spatie bevat. 

## <a name="azure-portal-error-vpn-package-file-download-error-503"></a>Azure portal-fout: VPN-pakket bestand Downloadfout 503

### <a name="symptom"></a>Symptoom

Wanneer u de configuratie van VPN-clientpakket downloaden, wordt het volgende foutbericht weergegeven:

**Download het bestand is mislukt. Details van fout: fout 503. De server is bezet.**
 
### <a name="solution"></a>Oplossing

Deze fout kan worden veroorzaakt door een tijdelijk netwerkprobleem. Probeer de VPN-pakket te downloaden na een paar minuten opnieuw.

## <a name="azure-vpn-gateway-upgrade-all-p2s-clients-are-unable-to-connect"></a>Upgrade uitvoeren voor Azure VPN-Gateway: alle P2S-clients kunnen geen verbinding maken

### <a name="cause"></a>Oorzaak

Als het certificaat meer dan 50 procent is via hun levensduur, het certificaat wordt overgeschakeld.

### <a name="solution"></a>Oplossing

U lost dit probleem, maken en distribueren van nieuwe certificaten aan de VPN-clients. 

## <a name="too-many-vpn-clients-connected-at-once"></a>Te veel VPN-clients tegelijk verbonden

Voor elke VPN-gateway is het maximum aantal toegestane verbindingen 128. Hier ziet u het totale aantal verbonden clients in de Azure portal.

## <a name="point-to-site-vpn-incorrectly-adds-a-route-for-100008-to-the-route-table"></a>Punt-naar-site VPN wordt een route voor 10.0.0.0/8 onjuist toegevoegd aan de routetabel

### <a name="symptom"></a>Symptoom

Wanneer u de VPN-verbinding op de client punt-naar-site kiest, moet de VPN-client een route naar de virtuele Azure-netwerk toevoegen. De IP-helper-service moet een route voor het subnet van de VPN-clients toevoegen. 

Het VPN-client bereik behoort tot een kleinere 10.0.0.0/8, zoals 10.0.12.0/24-subnet. In plaats van een route voor 10.0.12.0/24, wordt een route voor 10.0.0.0/8 toegevoegd die een hogere prioriteit heeft. 

Deze onjuiste route verbroken connectiviteit met andere on-premises-netwerken die deel kan uitmaken van een ander subnet binnen het bereik 10.0.0.0/8, zoals 10.50.0.0/24, waarvoor geen een specifieke route gedefinieerd. 

### <a name="cause"></a>Oorzaak

Dit gedrag is inherent aan het ontwerp voor Windows-clients. Wanneer de client het PPP IPCP-protocol gebruikt, ontvangt deze het IP-adres voor de tunnelinterface van de server (de VPN-gateway in dit geval). Echter, vanwege een beperking in het protocol, de client heeft geen het subnetmasker. Omdat er geen andere manier om toegang te krijgen, wordt de client probeert te raden het subnetmasker op basis van de klasse van het IP-adres van de tunnel-interface. 

Een route is daarom toegevoegd op basis van de volgende statische toewijzing: 

Als het adres behoort tot klasse A-->/8 die van toepassing

Als het adres behoort tot klasse B--> /16 toepassen

Als het adres behoort tot klasse C--> /24 toepassen

## <a name="vpn-client-cannot-access-network-file-shares"></a>VPN-client heeft geen toegang tot netwerkbestandsshares

### <a name="symptom"></a>Symptoom

De VPN-client is verbonden met het Azure-netwerk. Echter, de client geen toegang tot netwerkshares.

### <a name="cause"></a>Oorzaak

Het SMB-protocol wordt gebruikt voor bestandsshare-toegang. Wanneer de verbinding wordt gestart, wordt de VPN-client de referenties van de sessie wordt toegevoegd en wordt de fout zich voordoet. Nadat de verbinding tot stand is gebracht, wordt de client gedwongen de cache-referenties gebruiken voor Kerberos-verificatie. Dit proces initieert query's naar de Key Distribution Center (domeincontroller) om op te halen van een token. Omdat de client verbinding vanaf het Internet maakt, het niet mogelijk om te bereiken van de domeincontroller. Daarom kan geen de client failover van Kerberos naar NTLM. 

Het enige ogenblik waarop de client wordt gevraagd om een referentie wanneer is er een geldig certificaat (met SAN = UPN) uitgegeven door het domein waaraan deze is gekoppeld. De client moet ook fysiek verbonden zijn met het domeinnetwerk. In dit geval wordt de client probeert om het certificaat te gebruiken en gezocht naar de domeincontroller. De Key Distribution Center wordt vervolgens een 'KDC_ERR_C_PRINCIPAL_UNKNOWN'-fout geretourneerd. Failover naar NTLM geforceerd op de client. 

### <a name="solution"></a>Oplossing

U kunt het probleem omzeilen, uitschakelen in cache opslaan van referenties van het domein van de volgende registersubsleutel: 

    HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Lsa\DisableDomainCreds - Set the value to 1 


## <a name="cannot-find-the-point-to-site-vpn-connection-in-windows-after-reinstalling-the-vpn-client"></a>De punt-naar-site VPN-verbinding kan niet zoeken in Windows nadat de VPN-client opnieuw is geïnstalleerd

### <a name="symptom"></a>Symptoom

U verwijdert de punt-naar-site VPN-verbinding en de VPN-client opnieuw installeren. In dit geval kan is de VPN-verbinding niet correct geconfigureerd. U ziet geen de VPN-verbinding in de **netwerkverbindingen** instellingen in Windows.

### <a name="solution"></a>Oplossing

Los het probleem op door de oude VPN-client configuratiebestanden verwijderen van **C:\Users\TheUserName\AppData\Roaming\Microsoft\Network\Connections**, en voer het installatieprogramma van de VPN-client vervolgens opnieuw.
