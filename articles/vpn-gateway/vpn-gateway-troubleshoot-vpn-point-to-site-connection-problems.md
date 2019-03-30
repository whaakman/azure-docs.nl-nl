---
title: Problemen met Azure point-to-site-verbinding | Microsoft Docs
description: Informatie over het oplossen van problemen met punt-naar-site-verbinding.
services: vpn-gateway
documentationcenter: na
author: chadmath
manager: cshepard
editor: ''
tags: ''
ms.service: vpn-gateway
ms.devlang: na
ms.topic: troubleshooting
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/28/2018
ms.author: genli
ms.openlocfilehash: 7990a98e0e2d688456db054e3cdfa447e1ed1043
ms.sourcegitcommit: 956749f17569a55bcafba95aef9abcbb345eb929
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/29/2019
ms.locfileid: "58630469"
---
# <a name="troubleshooting-azure-point-to-site-connection-problems"></a>Probleemoplossing: Problemen met Azure point-to-site-verbinding

Dit artikel worden veelvoorkomende problemen met punt-naar-site-verbinding die u kunt tegenkomen. Hierin worden ook mogelijke oorzaken en oplossingen voor deze problemen.

## <a name="vpn-client-error-a-certificate-could-not-be-found"></a>Fout bij de VPN-client: Een certificaat kan niet worden gevonden.

### <a name="symptom"></a>Symptoom

Wanneer u probeert verbinding maken met een Azure-netwerk met behulp van de VPN-client, ontvangt u de volgende strekking weergegeven:

**Een certificaat kan niet worden gevonden die kunnen worden gebruikt met deze Extensible Authentication Protocol. (Fout 798)**

### <a name="cause"></a>Oorzaak

Dit probleem treedt op als het clientcertificaat ontbreekt in **Certificaten - Huidige gebruiker\Persoonlijk\Certificaten**.

### <a name="solution"></a>Oplossing

U lost dit probleem, de volgende stappen uit:

1. Open Certificate Manager: Klik op **Start**, type **computercertificaten beheren**, en klik vervolgens op **computercertificaten beheren** in de zoekresultaten.

2. Zorg ervoor dat de volgende certificaten op de juiste locatie zijn:

    | Certificaat | Locatie |
    | ------------- | ------------- |
    | AzureClient.pfx  | Huidige gebruiker\Persoonlijk\Certificaten |
    | Azuregateway-*GUID*.cloudapp.net  | Huidige User\Trusted Root Certification Authorities|
    | AzureGateway-*GUID*.cloudapp.net, AzureRoot.cer    | Lokale Computer\Trusted Root Certification Authorities|

3. Ga naar C:\Users\<gebruikersnaam > \AppData\Roaming\Microsoft\Network\Connections\Cm\<GUID >, handmatig het certificaat (*.cer-bestand) op de gebruiker en archief van de computer installeren.

Zie voor meer informatie over het installeren van het clientcertificaat [genereren en exporteren van certificaten voor punt-naar-site-verbindingen](vpn-gateway-certificates-point-to-site.md).

> [!NOTE]
> Wanneer u het certificaat importeert, schakel niet het **sterke beveiliging met persoonlijke sleutel inschakelen** optie.

## <a name="vpn-client-error-the-message-received-was-unexpected-or-badly-formatted"></a>Fout bij de VPN-client: Het ontvangen bericht is onverwacht of onjuist ingedeeld

### <a name="symptom"></a>Symptoom

Wanneer u probeert verbinding maken met een Azure-netwerk met behulp van de VPN-client, ontvangt u de volgende strekking weergegeven:

**Het ontvangen bericht is onverwacht of onjuist ingedeeld. (Error 0x80090326)**

### <a name="cause"></a>Oorzaak

Dit probleem doet zich voor als een van de volgende voorwaarden voldaan wordt:

- Het gebruik gebruiker gedefinieerde routes (UDR) met standaard-route op het Gatewaysubnet is onjuist ingesteld.
- De openbare sleutel van het root-certificaat is niet geüpload naar de Azure VPN-gateway. 
- De sleutel is beschadigd of verlopen.

### <a name="solution"></a>Oplossing

U lost dit probleem, de volgende stappen uit:

1. Verwijder de UDR in het Gatewaysubnet. Zorg ervoor dat de dat UDR stuurt al het verkeer correct.
2. Controleer de status van het basiscertificaat in de Azure-portal om te zien of deze is ingetrokken. Als niet is ingetrokken, probeert u het basiscertificaat en reupload te verwijderen. Zie voor meer informatie, [certificaten maken](vpn-gateway-howto-point-to-site-classic-azure-portal.md#generatecerts).

## <a name="vpn-client-error-a-certificate-chain-processed-but-terminated"></a>Fout bij de VPN-client: Een certificaatketen verwerkt maar dat is beëindigd 

### <a name="symptom"></a>Symptoom 

Wanneer u probeert verbinding maken met een Azure-netwerk met behulp van de VPN-client, ontvangt u de volgende strekking weergegeven:

**Een certificaatketen verwerkt, maar in een basiscertificaat dat niet wordt vertrouwd door de provider is beëindigd.**

### <a name="solution"></a>Oplossing

1. Zorg ervoor dat de volgende certificaten op de juiste locatie zijn:

    | Certificaat | Locatie |
    | ------------- | ------------- |
    | AzureClient.pfx  | Huidige gebruiker\Persoonlijk\Certificaten |
    | Azuregateway-*GUID*.cloudapp.net  | Huidige User\Trusted Root Certification Authorities|
    | AzureGateway-*GUID*.cloudapp.net, AzureRoot.cer    | Lokale Computer\Trusted Root Certification Authorities|

2. Als de certificaten al op de locatie zijn, kunt u de certificaten verwijderen en ze opnieuw installeren. De **azuregateway -*GUID*. cloudapp.net** certificaat bevindt zich in de VPN-clientconfiguratiepakket dat u hebt gedownload van de Azure-portal. U kunt bestand archivers gebruiken om op te halen van de bestanden van het pakket.

## <a name="file-download-error-target-uri-is-not-specified"></a>Fout bij het downloaden van het bestand: Doel-URI is niet opgegeven.

### <a name="symptom"></a>Symptoom

U ontvangt de volgende strekking weergegeven:

**Fout bij het downloaden van het bestand. Doel-URI is niet opgegeven.**

### <a name="cause"></a>Oorzaak 

Dit probleem treedt op vanwege een onjuiste Gatewaytype. 

### <a name="solution"></a>Oplossing

Het type van de VPN-gateway moet **VPN**, en de VPN-type moet **RouteBased**.

## <a name="vpn-client-error-azure-vpn-custom-script-failed"></a>Fout bij de VPN-client: Azure VPN-aangepaste script is mislukt 

### <a name="symptom"></a>Symptoom

Wanneer u probeert verbinding maken met een Azure-netwerk met behulp van de VPN-client, ontvangt u de volgende strekking weergegeven:

**Aangepast script (om bij te werken uw routeringstabel) is mislukt. (Error 8007026f)**

### <a name="cause"></a>Oorzaak

Dit probleem kan optreden als u probeert te openen van de site-naar-punt VPN-verbinding met behulp van een snelkoppeling.

### <a name="solution"></a>Oplossing 

Open het VPN-clientpakket rechtstreeks in plaats van via de snelkoppeling te openen.

## <a name="cannot-install-the-vpn-client"></a>Kan de VPN-client niet installeren

### <a name="cause"></a>Oorzaak 

Een aanvullend certificaat is vereist voor het vertrouwen van de VPN-gateway voor het virtuele netwerk. Het certificaat is opgenomen in de VPN-clientconfiguratiepakket dat is gegenereerd op basis van de Azure-portal.

### <a name="solution"></a>Oplossing

Pak het configuratiepakket voor de VPN-client en het cer-bestand niet vinden. Volg deze stappen voor het installeren van het certificaat:

1. Open mmc.exe.
2. Voeg de **certificaten** -module.
3. Selecteer de **Computer** account voor de lokale computer.
4. Met de rechtermuisknop op de **Trusted Root Certification Authorities** knooppunt. Klik op **alle-taak** > **importeren**, en blader naar het cer-bestand dat u hebt opgehaald uit het configuratiepakket voor de VPN-client.
5. De computer opnieuw opstarten. 
6. Probeer de VPN-client installeren.

## <a name="azure-portal-error-failed-to-save-the-vpn-gateway-and-the-data-is-invalid"></a>Azure portal-fout: Opslaan van de VPN-gateway is mislukt en de gegevens zijn ongeldig

### <a name="symptom"></a>Symptoom

Wanneer u probeert de wijzigingen wilt opslaan voor de VPN-gateway in Azure portal, ontvangt u de volgende strekking weergegeven:

**Kan niet opslaan van de virtuele netwerkgateway &lt; *gatewaynaam*&gt;. Gegevens voor certificaat &lt; *certificaat-ID* &gt; is ongeldig.**

### <a name="cause"></a>Oorzaak 

Dit probleem kan optreden als root openbare sleutel van het certificaat dat u hebt geüpload een ongeldig teken, zoals een spatie bevat.

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

## <a name="azure-portal-error-failed-to-save-the-vpn-gateway-and-the-resource-name-is-invalid"></a>Azure portal-fout: Opslaan van de VPN-gateway is mislukt en de resourcenaam is ongeldig

### <a name="symptom"></a>Symptoom

Wanneer u probeert de wijzigingen wilt opslaan voor de VPN-gateway in Azure portal, ontvangt u de volgende strekking weergegeven: 

**Kan niet opslaan van de virtuele netwerkgateway &lt; *gatewaynaam*&gt;. Resourcenaam &lt; *certificaatnaam die u probeert te uploaden* &gt; is ongeldig**.

### <a name="cause"></a>Oorzaak

Dit probleem treedt op omdat de naam van het certificaat een ongeldig teken, zoals een spatie bevat. 

## <a name="azure-portal-error-vpn-package-file-download-error-503"></a>Azure portal-fout: VPN-pakket downloaden bestandsfout 503

### <a name="symptom"></a>Symptoom

Wanneer u probeert te downloaden van het VPN-clientconfiguratiepakket, ontvangt u de volgende strekking weergegeven:

**Downloaden van het bestand is mislukt. Details van fout: 503-fout. De server is bezet.**
 
### <a name="solution"></a>Oplossing

Deze fout kan worden veroorzaakt door een tijdelijk probleem met het netwerk. Probeer de VPN-clientpakket downloaden na een paar minuten opnieuw.

## <a name="azure-vpn-gateway-upgrade-all-point-to-site-clients-are-unable-to-connect"></a>Azure VPN-Gateway-upgrade: Er zijn allemaal verwijzen naar de Site clients kan geen verbinding maken

### <a name="cause"></a>Oorzaak

Als het certificaat meer dan 50 procent is tot en met hun levensduur, het certificaat wordt overgedragen.

### <a name="solution"></a>Oplossing

U lost dit probleem, opnieuw downloaden en implementeren van de punt aan Site-pakket op alle clients.

## <a name="too-many-vpn-clients-connected-at-once"></a>Te veel VPN-clients tegelijk verbonden

Het maximum aantal toegestane verbindingen is bereikt. Hier ziet u het totale aantal verbonden clients in de Azure-portal.

## <a name="point-to-site-vpn-incorrectly-adds-a-route-for-100008-to-the-route-table"></a>Punt-naar-site VPN een route voor 10.0.0.0/8 wordt onjuist wordt toegevoegd aan de routetabel

### <a name="symptom"></a>Symptoom

Wanneer u de VPN-verbinding op het punt-naar-site-client moet, moet de VPN-client een route naar de Azure-netwerk toevoegen. De IP-helper-service moet toevoegen een route voor het subnet van de VPN-clients. 

Het bereik van VPN-client behoort tot een kleiner subnet 10.0.0.0/8, zoals 10.0.12.0/24. In plaats van een route voor 10.0.12.0/24, wordt een route voor 10.0.0.0/8 toegevoegd die een hogere prioriteit heeft. 

Deze onjuiste route verbroken connectiviteit met andere on-premises-netwerken die mogelijk deel uitmaken van een ander subnet binnen het bereik 10.0.0.0/8, zoals 10.50.0.0/24, waarvoor geen een specifieke route die is gedefinieerd. 

### <a name="cause"></a>Oorzaak

Dit gedrag is inherent aan het ontwerp voor Windows-clients. Wanneer de client het PPP IPCP-protocol gebruikt, wordt het IP-adres voor de tunnelinterface wordt verkregen van de server (in dit geval de VPN-gateway). Echter, vanwege een beperking in het protocol, de client heeft geen het subnetmasker. Omdat er geen andere manier om dit probleem te zijn, wordt de client probeert te raden het subnetmasker op basis van de klasse van het IP-adres van de tunnel-interface. 

Daarom wordt een route toegevoegd op basis van de volgende statische toewijzing: 

Als het adres behoort tot klasse A-->/8 die van toepassing

Als het adres behoort tot klasse B--> /16 toepassen

Als het adres behoort tot klasse C--> /24 toepassen

### <a name="solution"></a>Oplossing

Routes voor andere netwerken in de routeringstabel worden toegevoegd met de langste voorvoegselovereenkomst of lager metrische gegevens (daarom hogere prioriteit) dan de punt aan Site hebben. 

## <a name="vpn-client-cannot-access-network-file-shares"></a>VPN-client heeft geen toegang tot netwerkbestandsshares

### <a name="symptom"></a>Symptoom

De VPN-client is verbonden met het Azure-netwerk. Echter, de client geen toegang tot netwerkshares.

### <a name="cause"></a>Oorzaak

Het SMB-protocol wordt gebruikt voor bestandsshare-toegang. Wanneer de verbinding wordt gestart, wordt de VPN-client de referenties van de sessie wordt toegevoegd en wordt de fout zich voordoet. Nadat de verbinding tot stand is gebracht, wordt de client gedwongen de referenties van de cache voor Kerberos-verificatie gebruiken. Dit proces wordt gestart voor query's naar de Key Distribution Center (een domeincontroller) om op te halen van een token. Omdat de client verbinding vanaf het Internet maakt, kan deze niet kunnen bereiken van de domeincontroller zijn. Daarom kan geen de client failover van Kerberos naar NTLM. 

De enige keer dat de client wordt gevraagd om een referentie wanneer is er een geldig certificaat (met SAN = UPN) dat is uitgegeven door het domein waaraan deze is gekoppeld. De client moet ook worden fysiek verbonden met het domeinnetwerk. In dit geval de client probeert om het certificaat te gebruiken en de domeincontroller zoekt. De Key Distribution Center wordt vervolgens een 'KDC_ERR_C_PRINCIPAL_UNKNOWN'-fout geretourneerd. De client gedwongen om te fungeren als failover voor NTLM. 

### <a name="solution"></a>Oplossing

Als tijdelijke oplossing voor het probleem, schakel opslaan in de cache van de domeinreferenties van de volgende registersubsleutel: 

    HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Lsa\DisableDomainCreds - Set the value to 1 


## <a name="cannot-find-the-point-to-site-vpn-connection-in-windows-after-reinstalling-the-vpn-client"></a>Na het opnieuw installeren van de VPN-client vinden de punt-naar-site VPN-verbinding niet in Windows

### <a name="symptom"></a>Symptoom

U verwijdert de punt-naar-site VPN-verbinding en vervolgens opnieuw de VPN-client te installeren. In dit geval is de VPN-verbinding niet geconfigureerd is. U ziet geen de VPN-verbinding in de **netwerkverbindingen** instellingen in Windows.

### <a name="solution"></a>Oplossing

Het probleem op te lossen de oude VPN client configuratiebestanden verwijderen van **C:\Users\UserName\AppData\Roaming\Microsoft\Network\Connections\<id >**, en voer het installatieprogramma van de VPN-client opnieuw uit .

## <a name="point-to-site-vpn-client-cannot-resolve-the-fqdn-of-the-resources-in-the-local-domain"></a>Punt-naar-site VPN-client de FQDN-naam van de resources in het lokale domein kan niet worden omgezet

### <a name="symptom"></a>Symptoom

Wanneer de client verbinding met Azure maakt met behulp van punt-naar-site VPN-verbinding, kan niet worden omgezet de FQDN-naam van de resources in uw lokale domein.

### <a name="cause"></a>Oorzaak

Punt-naar-site VPN-client maakt gebruik van Azure DNS-servers die zijn geconfigureerd in de Azure-netwerk. De Azure DNS-servers hebben voorrang op de lokale DNS-servers die zijn geconfigureerd in de client, zodat alle DNS-query's worden verzonden naar de Azure DNS-servers. Als de Azure DNS-servers geen records voor de lokale bronnen, de query is mislukt.

### <a name="solution"></a>Oplossing

Het probleem is opgelost, zorg ervoor dat de Azure DNS-servers die in de Azure-netwerk gebruikt kan de DNS-records voor lokale bronnen worden opgelost. Om dit te doen, kunt u DNS-doorstuurservers of voorwaardelijke doorstuurservers. Zie voor meer informatie, [naamomzetting met uw eigen DNS-server](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-that-uses-your-own-dns-server)

## <a name="the-point-to-site-vpn-connection-is-established-but-you-still-cannot-connect-to-azure-resources"></a>De punt-naar-site VPN-verbinding tot stand is gebracht, maar u nog steeds geen verbinding met Azure-resources 

### <a name="cause"></a>Oorzaak

Dit probleem kan optreden als de VPN-client heeft de routes niet verkrijgen van de Azure VPN-gateway.

### <a name="solution"></a>Oplossing

Dit probleem op te lossen [Azure VPN-gateway opnieuw instellen](vpn-gateway-resetgw-classic.md). Om ervoor te zorgen dat de nieuwe routes worden gebruikt, moeten de punt-naar-Site VPN-clients opnieuw worden gedownload nadat peering op virtueel netwerk is geconfigureerd.

## <a name="error-the-revocation-function-was-unable-to-check-revocation-because-the-revocation-server-was-offlineerror-0x80092013"></a>Fout: 'De functie intrekken kon niet intrekkingscontrole omdat de certificaatintrekkingsserver offline is. (Fout 0x80092013)"

### <a name="causes"></a>Oorzaken
Deze fout treedt op als de client geen toegang tot http://crl3.digicert.com/ssca-sha2-g1.crl en http://crl4.digicert.com/ssca-sha2-g1.crl.  De controle van certificaatintrekking vereist toegang tot deze twee sites.  Dit probleem treedt meestal op de client met de proxyserver is geconfigureerd. In sommige omgevingen, als de aanvragen niet meer via de proxyserver, wordt deze geweigerd op de randfirewall.

### <a name="solution"></a>Oplossing

Controleer de instellingen van de proxyserver, zorg ervoor dat de client toegang heeft tot http://crl3.digicert.com/ssca-sha2-g1.crl en http://crl4.digicert.com/ssca-sha2-g1.crl.

## <a name="vpn-client-error-the-connection-was-prevented-because-of-a-policy-configured-on-your-rasvpn-server-error-812"></a>Fout bij de VPN-Client: De verbinding is voorkomen vanwege een dat is geconfigureerd op de RAS-/ VPN-server. (Error 812)

### <a name="cause"></a>Oorzaak

Deze fout treedt op als de RADIUS-server die u hebt gebruikt voor het verifiëren van VPN-client verkeerde instellingen heeft of Azure-Gateway kan de Radius-server niet bereiken.

### <a name="solution"></a>Oplossing

Zorg ervoor dat de RADIUS-server correct is geconfigureerd. Zie voor meer informatie, [integreren RADIUS-verificatie met Azure multi-factor Authentication-Server](../active-directory/authentication/howto-mfaserver-dir-radius.md).

## <a name="error-405-when-you-download-root-certificate-from-vpn-gateway"></a>"Error 405" wanneer u een basiscertificaat downloaden van VPN-Gateway

### <a name="cause"></a>Oorzaak

Basiscertificaat is niet geïnstalleerd. Het basiscertificaat is geïnstalleerd in van de client **certificaten van vertrouwde** opslaan.

## <a name="vpn-client-error-the-remote-connection-was-not-made-because-the-attempted-vpn-tunnels-failed-error-800"></a>Fout bij de VPN-Client: De externe verbinding is niet gemaakt omdat de poging tot VPN-tunnels is mislukt. (Fout 800) 

### <a name="cause"></a>Oorzaak

Het NIC-stuurprogramma is verouderd.

### <a name="solution"></a>Oplossing

Het NIC-stuurprogramma-update:

1. Klik op **Start**, type **Apparaatbeheer**, en selecteer deze in de lijst met resultaten. Als u wordt gevraagd om een beheerderswachtwoord of een bevestiging, typt u het wachtwoord of geeft u de bevestiging.
2. In de **netwerkadapters** categorieën, vinden de NIC die u wilt bijwerken.  
3. Dubbelklik op de naam van het apparaat, selecteert u **stuurprogramma bijwerken**, selecteer **zoeken voor bijgewerkte stuurprogramma's automatisch**.
4. Als Windows geen nieuw stuurprogramma wordt gevonden, kunt u kijkt u voor één op de website van de apparaatfabrikant en volg de instructies.
5. Start de computer opnieuw op en probeer het vervolgens opnieuw.

## <a name="error-file-download-error-target-uri-is-not-specified"></a>Fout: 'Fout bij het downloaden die doel-URI is niet opgegeven het bestand'

### <a name="cause"></a>Oorzaak

Dit wordt veroorzaakt door een onjuiste gateway is geconfigureerd.

### <a name="solution"></a>Oplossing

Het type van de Azure VPN-gateway moet VPN en de VPN-type moet **RouteBased**.

## <a name="vpn-package-installer-doesnt-complete"></a>VPN-installatieprogramma voltooien niet

### <a name="cause"></a>Oorzaak

Dit probleem kan worden veroorzaakt door de vorige installatie van de VPN-clients. 

### <a name="solution"></a>Oplossing

Verwijder de oude configuratiebestanden van een VPN-client van **C:\Users\UserName\AppData\Roaming\Microsoft\Network\Connections\<id >** en voer het installatieprogramma van de VPN-client opnieuw uit. 

## <a name="the-vpn-client-hibernates-or-sleep-after-some-time"></a>De VPN-client in de slaapstand of slaapstand na enige tijd

### <a name="solution"></a>Oplossing

Controleer de slaapstand en stand-bymodus instellingen op de computer waarop de VPN-client wordt uitgevoerd.
