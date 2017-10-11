---
title: Webproxy voor een StorSimple-apparaat instellen | Microsoft Docs
description: Informatie over het gebruik van Windows PowerShell voor StorSimple web proxy-instellingen voor uw StorSimple-apparaat configureren.
services: storsimple
documentationcenter: 
author: alkohli
manager: carmonm
editor: 
ms.assetid: 6c2ca351-a7c6-4da6-ab5e-c081e6d08261
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/17/2016
ms.author: alkohli
ms.openlocfilehash: 66ee6ce15e51b14366eac0512c899d1c425c6092
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="configure-web-proxy-for-your-storsimple-device"></a>Webproxy voor uw StorSimple-apparaat configureren
## <a name="overview"></a>Overzicht
Deze zelfstudie wordt beschreven hoe u Windows PowerShell voor StorSimple gebruiken voor het configureren en weergeven van web proxy-instellingen voor uw StorSimple-apparaat. De web proxy-instellingen worden gebruikt door het StorSimple-apparaat om te communiceren met de cloud. Een webproxyserver wordt gebruikt voor het toevoegen van een extra beveiligingslaag, filterinhoud, cache eenvoudig bandbreedtevereisten of zelfs help met analytics.

Webproxy is een optionele configuratie voor uw StorSimple-apparaat. U kunt de webproxy alleen via Windows PowerShell voor StorSimple. De configuratie is een proces als volgt:

1. U web proxy-instellingen via de installatiewizard of Windows PowerShell voor StorSimple-cmdlets voor het eerst configureren.
2. U vervolgens inschakelen de geconfigureerde web proxy-instellingen via Windows PowerShell voor StorSimple cmdlets.

Nadat de configuratie van uw webproxy voltooid is, kunt u de geconfigureerde web proxy-instellingen weergeven in de Microsoft Azure StorSimple Manager-service en de Windows PowerShell voor StorSimple. 

Na het lezen van deze zelfstudie, kunt u zich kunt:

* Webproxy configureren met behulp van cmdlets en de wizard setup
* Webproxy inschakelen met behulp van cmdlets
* Weergave web proxy-instellingen in de klassieke Azure portal
* Fouten tijdens de webproxyconfiguratie oplossen

## <a name="configure-web-proxy-via-windows-powershell-for-storsimple"></a>Configureren van webproxy via Windows PowerShell voor StorSimple
U kunt een van de volgende web proxy-instellingen configureren:

* De installatiewizard om u te begeleiden de configuratiestappen.
* Cmdlets in Windows PowerShell voor StorSimple.

Elk van deze methoden wordt in de volgende secties besproken.

## <a name="configure-web-proxy-via-the-setup-wizard"></a>Webproxy via de wizard setup configureren
U kunt de wizard setup helpt u bij de stappen voor de configuratie van uw webproxy. De volgende stappen uitvoeren om te webproxy configureren op uw apparaat.

#### <a name="to-configure-web-proxy-via-the-setup-wizard"></a>Voor het configureren van webproxy via de wizard setup
1. Kies in het menu van de seriële console optie 1, **aanmelden met volledige toegang** en geef de **wachtwoord apparaatbeheerder**. Typ de volgende opdracht een sessie van de wizard setup te starten:
   
    `Invoke-HcsSetupWizard`
2. Als dit de eerste keer dat u de wizard setup voor apparaatregistratie hebt gebruikt, moet u de vereiste netwerkinstellingen configureren totdat u de configuratie van uw webproxy. Als uw apparaat al is geregistreerd, kunt u de geconfigureerde netwerkinstellingen accepteren totdat u de configuratie van uw webproxy. In de wizard setup voor het configureren van instellingen voor web proxy Typ desgevraagd **Ja**.
3. Voor de **Web Proxy-URL**, geeft u het IP-adres of de volledig gekwalificeerde domeinnaam (FQDN) van uw web-proxyserver en de TCP-poort number die u wilt dat uw apparaat om te gebruiken om te communiceren met de cloud. Gebruik de volgende notatie:
   
    `http://<IP address or FQDN of the web proxy server>:<TCP port number>`
   
    Standaard is TCP-poortnummer 8080 opgegeven.
4. Kies het verificatietype als **NTLM**, **Basic**, of **geen**. Standaard is de minst veilige verificatie voor de proxyserver-configuratie. NT LAN Manager (NTLM) is een zeer veilige en complexe verificatieprotocol die gebruikmaakt van een drievoudige berichtensysteem (soms vier als extra integriteit vereist is) om een gebruiker te verifiëren. De standaardverificatie is NTLM. Zie voor meer informatie [Basic](http://hc.apache.org/httpclient-3.x/authentication.html) en [NTLM-verificatie](http://hc.apache.org/httpclient-3.x/authentication.html). 
   
   > [!IMPORTANT]
   > **In de StorSimple Manager-service de bewaking grafieken apparaat werken niet als basis of NTLM-verificatie is ingeschakeld in de configuratie van de proxyserver voor het apparaat. Voor de bewaking grafieken werken, moet u ervoor te zorgen dat de verificatie is ingesteld op NONE.**
   > 
   > 
5. Als u verificatie gebruikt, geeft u een **Web Proxy Username** en een **Web Proxywachtwoord**. U moet ook het wachtwoord te bevestigen.
   
    ![Webproxy configureren op StorSimple Device1](./media/storsimple-configure-web-proxy/IC751830.png)

Als u uw apparaat voor de eerste keer registreert, gaat u verder met de registratie. Als uw apparaat is al geregistreerd, wordt de wizard wordt afgesloten. De geconfigureerde instellingen worden opgeslagen.

Webproxy nu ook ingeschakeld. U kunt doorgaan de [webproxy inschakelen](#enable-web-proxy) stap en rechtstreeks naar [web proxy-instellingen bekijken in de klassieke Azure portal](#view-web-proxy-settings-in-the-azure-classic-portal).

## <a name="configure-web-proxy-via-windows-powershell-for-storsimple-cmdlets"></a>Configureren van webproxy via Windows PowerShell voor StorSimple-cmdlets
Er is een andere manier om web proxy-instellingen te configureren via de Windows PowerShell voor StorSimple-cmdlets. Voer de volgende stappen uit om webproxy te configureren.

#### <a name="to-configure-web-proxy-via-cmdlets"></a>Webproxy via cmdlets configureren
1. Kies in het menu van de seriële console optie 1, **aanmelden met volledige toegang**. Geef desgevraagd de **wachtwoord apparaatbeheerder**. Is het standaardwachtwoord `Password1`.
2. Typ het volgende achter de opdrachtprompt:
   
    `Set-HcsWebProxy -Authentication NTLM -ConnectionURI "<http://<IP address or FQDN of web proxy server>:<TCP port number>" -Username "<Username for web proxy server>"`
   
    Geef en Bevestig het wachtwoord als u wordt gevraagd, zoals hieronder wordt weergegeven.
   
    ![Webproxy configureren op StorSimple Device3](./media/storsimple-configure-web-proxy/IC751831.png)

De webproxy is nu geconfigureerd en moet worden ingeschakeld.

## <a name="enable-web-proxy"></a>Webproxy inschakelen
Webproxy is standaard uitgeschakeld. Nadat u de web proxy-instellingen op uw StorSimple-apparaat configureert, moet u gebruik van de Windows PowerShell voor StorSimple om in te schakelen van de web proxy-instellingen.

> [!NOTE]
> **Deze stap wordt niet vereist als u de wizard setup gebruikt voor het configureren van webproxy. Webproxy is na een sessie van de wizard setup standaard automatisch ingeschakeld.**
> 
> 

De volgende stappen uitvoeren in Windows PowerShell voor StorSimple webproxy op uw apparaat in te schakelen:

#### <a name="to-enable-web-proxy"></a>Webproxy in te schakelen
1. Kies in het menu van de seriële console optie 1, **aanmelden met volledige toegang**. Geef desgevraagd de **wachtwoord apparaatbeheerder**. Is het standaardwachtwoord `Password1`.
2. Typ het volgende achter de opdrachtprompt:
   
    `Enable-HcsWebProxy`
   
    U hebt nu de configuratie van de web proxy ingeschakeld op uw StorSimple-apparaat.
   
    ![Webproxy configureren op StorSimple Device4](./media/storsimple-configure-web-proxy/IC751832.png)

## <a name="view-web-proxy-settings-in-the-azure-classic-portal"></a>Weergave web proxy-instellingen in de klassieke Azure portal
De web proxy-instellingen worden geconfigureerd via de Windows PowerShell-interface en kunnen niet worden gewijzigd in de klassieke portal. U kunt echter deze geconfigureerde instellingen bekijken in de klassieke portal. De volgende stappen om weer te geven van webproxy uitvoeren.

#### <a name="to-view-web-proxy-settings"></a>Web proxy-instellingen te bekijken
1. Navigeer naar **StorSimple Manager-service > apparaten**. Selecteer en klik op een apparaat en ga vervolgens naar **configureren**.
2. Schuif vervolgens omlaag op de **configureren** naar pagina **Web proxy-instellingen** sectie. U kunt de geconfigureerde web proxy-instellingen weergeven op uw StorSimple-apparaat, zoals hieronder wordt weergegeven.
   
    ![De webproxy weergeven in de beheerportal](./media/storsimple-configure-web-proxy/ViewWebProxyPortal_M.png)

## <a name="errors-during-web-proxy-configuration"></a>Fouten tijdens de webproxyconfiguratie
Als de web proxy-instellingen niet juist zijn geconfigureerd, worden foutberichten weergegeven voor de gebruiker in Windows PowerShell voor StorSimple. De volgende tabel worden enkele van deze foutberichten, hun mogelijke oorzaken en aanbevolen acties.

| Serienummer. | Fout HRESULT Code | Mogelijke hoofdoorzaken | Aanbevolen actie |
|:--- |:--- |:--- |:--- |
| 1. |0x80070001 |De opdracht wordt uitgevoerd van de passieve controller en is het niet kan communiceren met de actieve controller. |Voer de opdracht op de actieve controller. Als u wilt de opdracht uitvoert vanaf de passieve controller, moet u de verbinding tussen passieve en actieve controller herstellen. U moet Microsoft Support benaderen als deze de verbinding verbroken is. |
| 2. |0x800710dd - de bewerkings-id is niet geldig |Proxy-instellingen worden niet ondersteund op virtuele StorSimple-apparaat. |Proxy-instellingen worden niet ondersteund op virtuele StorSimple-apparaat. Deze kunnen alleen worden geconfigureerd op een fysieke StorSimple-apparaat. |
| 3. |0x80070057 - ongeldige parameter |Een van de opgegeven parameters voor de proxy-instellingen is niet geldig. |De URI is niet opgegeven in de juiste indeling. Gebruik de volgende notatie:`http://<IP address or FQDN of the web proxy server>:<TCP port number>` |
| 4. |0x800706ba - RPC-server niet beschikbaar |De hoofdoorzaak is een van de volgende opties:</br></br>Cluster is niet actief.</br></br>Gegevenspad-service wordt niet uitgevoerd.</br></br>De opdracht wordt uitgevoerd vanaf een passieve controller en is het niet kan communiceren met de actieve controller. |Neem benaderen Microsoft Support om ervoor te zorgen dat het cluster actief is en gegevenspad-service wordt uitgevoerd.</br></br>Voer de opdracht uit vanaf de actieve controller. Als u uitvoeren van de opdracht van de passieve controller wilt, moet u om te controleren of dat de passieve controller kan communiceren met de actieve controller. U moet Microsoft Support benaderen als deze de verbinding verbroken is. |
| 5. |0x800706be - RPC-aanroep is mislukt |Cluster is niet actief. |Neem benaderen Microsoft Support om ervoor te zorgen dat het cluster actief is. |
| 6. |0x8007138f - clusterbron is niet gevonden |Clusterbron voor platform-service is niet gevonden. Dit kan gebeuren wanneer de installatie niet correct is. |U wilt u de fabrieksinstellingen op uw apparaat. U moet mogelijk een platform-resource maken. Neem contact op met Microsoft Support voor volgende stappen. |
| 7. |0x8007138c - cluster-bron niet online |Platform of gegevenspad clusterbronnen zijn niet online. |Neem contact op met Microsoft Support om ervoor te zorgen dat de bron van de service gegevenspad en platform online zijn. |

> [!NOTE]
> * De bovenstaande lijst van foutberichten is niet volledig. 
> * Fouten met betrekking tot web proxy-instellingen worden niet weergegeven in de klassieke Azure portal in uw StorSimple Manager-service. Als er een probleem met de webproxy nadat de configuratie is voltooid, de status van het apparaat wordt gewijzigd naar **Offline** in de klassieke portal. |
> 
> 

## <a name="next-steps"></a>Volgende stappen
* Als u problemen ondervindt tijdens het implementeren van uw apparaat of web proxy-instellingen configureren, raadpleegt u [uw StorSimple-apparaat-implementatie oplossen](storsimple-troubleshoot-deployment.md).
* Voor informatie over het gebruik van de StorSimple Manager-service, gaat u naar [de StorSimple Manager-service gebruiken voor het beheren van uw StorSimple-apparaat](storsimple-manager-service-administration.md).

