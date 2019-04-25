---
title: Webproxy voor StorSimple 8000-apparaat instellen | Microsoft Docs
description: Leer hoe u Windows PowerShell voor StorSimple gebruiken voor het web proxy-instellingen voor uw StorSimple-apparaat configureren.
services: storsimple
documentationcenter: ''
author: alkohli
manager: ''
editor: ''
ms.assetid: ''
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/19/2017
ms.author: alkohli
ms.openlocfilehash: be5719d2c383c838ef70c6862c1055c3374e05e5
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2019
ms.locfileid: "60362444"
---
# <a name="configure-web-proxy-for-your-storsimple-device"></a>Webproxy voor uw StorSimple-apparaat configureren

## <a name="overview"></a>Overzicht

In deze zelfstudie wordt beschreven hoe u Windows PowerShell voor StorSimple gebruiken om te configureren en weergeven van webproxy-instellingen voor uw StorSimple-apparaat. De webproxy-instellingen worden gebruikt door het StorSimple-apparaat bij het communiceren met de cloud. Een webproxyserver wordt gebruikt om toe te voegen een andere laag van beveiliging, filterinhoud eenvoudig bandbreedtevereisten of zelfs help met analytics-cache.

De instructies in deze zelfstudie geldt alleen voor fysieke apparaten van StorSimple 8000-serie. Webproxyconfiguratie wordt niet ondersteund op de StorSimple Cloud Appliance (8010 en 8020).

Webproxy is een _optionele_ configuratie voor uw StorSimple-apparaat. U kunt de webproxy alleen via Windows PowerShell voor StorSimple. De configuratie is een proces als volgt:

1. U web proxy-instellingen via de installatiewizard of Windows PowerShell voor StorSimple-cmdlets voor het eerst configureren.
2. U vervolgens inschakelen de geconfigureerde web proxy-instellingen via Windows PowerShell voor StorSimple-cmdlets.

Nadat de configuratie van uw webproxy voltooid is, kunt u de geconfigureerde web proxy-instellingen weergeven in de Microsoft Azure StorSimple Device Manager-service en de Windows PowerShell voor StorSimple.

Na het lezen van deze zelfstudie, kunt u zich aan:

* Webproxy configureren met behulp van de wizard setup en de cmdlets.
* Webproxy inschakelen met behulp van cmdlets.
* Webproxy-instellingen weergeven in Azure portal.
* Fouten oplossen tijdens de configuratie van uw webproxy.


## <a name="configure-web-proxy-via-windows-powershell-for-storsimple"></a>Configureren van webproxy via Windows PowerShell voor StorSimple

U kunt een van de volgende web proxy-instellingen configureren:

* Setup-wizard voor het uitvoeren van de configuratiestappen.
* Cmdlets in Windows PowerShell voor StorSimple.

Elk van deze methoden wordt in de volgende secties besproken.

## <a name="configure-web-proxy-via-the-setup-wizard"></a>Webproxy via de installatiewizard configureren

De wizard setup gebruiken om u te begeleiden door de stappen voor de configuratie van uw webproxy. De volgende stappen uitvoeren om webproxy configureren op uw apparaat.

#### <a name="to-configure-web-proxy-via-the-setup-wizard"></a>Webproxy via de installatiewizard configureren

1. Kies in het menu van de seriële console optie 1, **Meld u aan met volledige toegang tot de** en geef de **beheerderswachtwoord**. Typ de volgende opdracht een sessie van de wizard setup te starten:
   
    `Invoke-HcsSetupWizard`
2. Als dit de eerste keer dat u de wizard setup voor device Registration service hebt gebruikt, moet u de vereiste netwerkinstellingen configureren totdat u de configuratie van uw webproxy. Als uw apparaat al is geregistreerd, accepteert u de geconfigureerde netwerkinstellingen totdat u de configuratie van uw webproxy. In de wizard setup wanneer u hierom wordt gevraagd naar het web proxy-instellingen configureren, typt **Ja**.
3. Voor de **webproxy-URL**, geeft u het IP-adres of de volledig gekwalificeerde domeinnaam (FQDN) van uw web proxy-server en de TCP-poort nummer dat u wilt dat uw apparaat te gebruiken bij het communiceren met de cloud. Gebruik de volgende indeling:
   
    `http://<IP address or FQDN of the web proxy server>:<TCP port number>`
   
    Standaard is TCP-poortnummer 8080 opgegeven.
4. Kies het verificatietype als **NTLM**, **Basic**, of **geen**. Standaard is de minst veilige verificatie voor de proxyserver-configuratie. NT LAN Manager (NTLM) is een zeer veilige en complexe verificatieprotocol die gebruikmaakt van een berichtensysteem drie richtingen (soms vier als extra integriteit vereist is) om een gebruiker te verifiëren. De standaardverificatie is NTLM. Zie voor meer informatie, [Basic](http://hc.apache.org/httpclient-3.x/authentication.html) en [NTLM-verificatie](http://hc.apache.org/httpclient-3.x/authentication.html). 
   
   > [!IMPORTANT]
   > **In de StorSimple Device Manager-service de bewakingsgrafieken apparaat werken niet als basis of NTLM-verificatie is ingeschakeld in de proxyserver-configuratie voor het apparaat. Voor de bewakingsgrafieken om te werken, moet u ervoor zorgen dat de verificatie is ingesteld op NONE.**
  
5. Als u de verificatie hebt ingeschakeld, geeft u een **Web Proxygebruikersnaam** en een **Web Proxywachtwoord**. U moet ook het wachtwoord te bevestigen.
   
    ![Webproxy configureren op StorSimple Device1](./media/storsimple-configure-web-proxy/IC751830.png)

Als u uw apparaat voor de eerste keer registreert, gaat u verder met de registratie. Als uw apparaat is al geregistreerd, wordt de wizard sluit. De geconfigureerde instellingen worden opgeslagen.

Webproxy is nu ingeschakeld. U kunt overslaan de [webproxy inschakelen](#enable-web-proxy) stap en gaat u rechtstreeks naar [webproxy-instellingen weergeven in Azure portal](#view-web-proxy-settings-in-the-azure-portal).

## <a name="configure-web-proxy-via-windows-powershell-for-storsimple-cmdlets"></a>Webproxy via Windows PowerShell voor StorSimple-cmdlets configureren

Er is een alternatieve manier om web proxy-instellingen configureren via de Windows PowerShell voor StorSimple-cmdlets. De volgende stappen uitvoeren om webproxy configureren.

#### <a name="to-configure-web-proxy-via-cmdlets"></a>Het configureren van webproxy via-cmdlets
1. Kies in het menu van de seriële console optie 1, **Meld u aan met volledige toegang tot de**. Als u hierom wordt gevraagd, geeft u de **beheerderswachtwoord**. Is het standaardwachtwoord `Password1`.
2. Typ in de opdrachtprompt:
   
    `Set-HcsWebProxy -Authentication NTLM -ConnectionURI "<http://<IP address or FQDN of web proxy server>:<TCP port number>" -Username "<Username for web proxy server>"`
   
    Geef en Bevestig het wachtwoord wanneer hierom wordt gevraagd.
   
    ![Webproxy configureren op StorSimple Device3](./media/storsimple-configure-web-proxy/IC751831.png)

De webproxy is nu geconfigureerd en moet worden ingeschakeld.

## <a name="enable-web-proxy"></a>Webproxy inschakelen

Webproxy is standaard uitgeschakeld. Nadat u de webproxy-instellingen op uw StorSimple-apparaat configureert, gebruikt u de Windows PowerShell voor StorSimple om in te schakelen van de webproxy-instellingen.

> [!NOTE]
> **Deze stap is niet vereist als u de wizard setup van webproxy configureren. Webproxy is na een sessie van de wizard setup standaard automatisch ingeschakeld.**


Voer de volgende stappen uit in Windows PowerShell voor StorSimple webproxy op uw apparaat in te schakelen:

#### <a name="to-enable-web-proxy"></a>Webproxy in te schakelen
1. Kies in het menu van de seriële console optie 1, **Meld u aan met volledige toegang tot de**. Als u hierom wordt gevraagd, geeft u de **beheerderswachtwoord**. Is het standaardwachtwoord `Password1`.
2. Typ in de opdrachtprompt:
   
    `Enable-HcsWebProxy`
   
    U hebt nu de configuratie van de web proxy ingeschakeld op uw StorSimple-apparaat.
   
    ![Webproxy configureren op StorSimple Device4](./media/storsimple-configure-web-proxy/IC751832.png)

## <a name="view-web-proxy-settings-in-the-azure-portal"></a>Webproxy-instellingen weergeven in Azure portal

De webproxy-instellingen worden geconfigureerd via de Windows PowerShell-interface en kunnen niet worden gewijzigd in de portal. U kunt echter deze geconfigureerde instellingen bekijken in de portal. Voer de volgende stappen uit als u wilt weergeven van webproxy.

#### <a name="to-view-web-proxy-settings"></a>Webproxy-instellingen weergeven
1. Navigeer naar **StorSimple Device Manager-service > apparaten**. Selecteer en klik op een apparaat en ga vervolgens naar **apparaatinstellingen > netwerk**.

    ![Klik op netwerk](./media/storsimple-8000-configure-web-proxy/view-web-proxy-1.png)

2. In de **netwerkinstellingen** blade, klikt u op de **Web proxy** tegel.

    ![Klik op de webproxy](./media/storsimple-8000-configure-web-proxy/view-web-proxy-2.png)

3. In de **Web proxy** blade, Controleer de geconfigureerde web proxy-instellingen op uw StorSimple-apparaat.
   
    ![Webproxy-instellingen weergeven](./media/storsimple-8000-configure-web-proxy/view-web-proxy-3.png)


## <a name="errors-during-web-proxy-configuration"></a>Fouten tijdens de webproxyconfiguratie

Als de webproxyinstellingen zijn onjuist geconfigureerd, worden foutberichten weergegeven voor de gebruiker in Windows PowerShell voor StorSimple. De volgende tabel worden enkele van deze foutberichten, hun waarschijnlijke oorzaken en aanbevolen acties uitgelegd.

| Serienummer. | HRESULT-fout Code | Mogelijke hoofdoorzaken | Aanbevolen actie |
|:--- |:--- |:--- |:--- |
| 1. |0x80070001 |Opdracht wordt uitgevoerd vanaf de passieve controller en het is niet kan communiceren met de actieve controller. |Voer de opdracht op de actieve controller. Voor de opdracht uitvoeren vanaf de passieve controller, moet u de verbinding van passieve met actieve controller oplossen. U moet Microsoft Support betrekken als deze verbinding verbroken is. |
| 2. |0x800710dd - de bewerkings-id is niet geldig |Proxy-instellingen worden niet ondersteund op StorSimple Cloud Appliance. |Proxy-instellingen worden niet ondersteund op StorSimple Cloud Appliance. Deze kunnen alleen worden geconfigureerd op een fysieke StorSimple-apparaat. |
| 3. |0x80070057 - ongeldige parameter |Een van de opgegeven parameters voor de proxy-instellingen is niet geldig. |De URI is niet opgegeven in de juiste indeling. Gebruik de volgende indeling: `http://<IP address or FQDN of the web proxy server>:<TCP port number>` |
| 4. |0x800706ba - RPC-server niet beschikbaar |De hoofdoorzaak is een van de volgende opties:</br></br>Cluster is niet actief. </br></br>Gegevenspad-service wordt niet uitgevoerd.</br></br>De opdracht wordt uitgevoerd vanaf de passieve controller en het is niet kan communiceren met de actieve controller. |Contact Microsoft Support om ervoor te zorgen dat het cluster actief is en gegevenspad-service wordt uitgevoerd.</br></br>Voer de opdracht uit vanaf de actieve controller. Als u de opdracht uitvoeren vanaf de passieve controller wilt, moet u ervoor zorgen dat de passieve controller met de actieve controller communiceren kan. U moet Microsoft Support betrekken als deze verbinding verbroken is. |
| 5. |0x800706be - RPC-aanroep is mislukt |Cluster is niet actief. |Contact Microsoft Support om ervoor te zorgen dat het cluster actief is. |
| 6. |0x8007138f - clusterbron is niet gevonden |Platform-service-clusterbron is niet gevonden. Dit kan gebeuren wanneer de installatie niet correct is. |Mogelijk moet u de fabrieksinstellingen terug op uw apparaat. Mogelijk moet u een resource platform maken. Neem contact op met Microsoft Ondersteuning voor volgende stappen. |
| 7. |0x8007138c - cluster-bron niet online |Platform of gegevenspad clusterbronnen zijn niet online. |Neem contact op met Microsoft Support om ervoor te zorgen dat de bron van de service gegevenspad en platform online zijn. |

> [!NOTE]
> * De bovenstaande lijst met foutberichten is niet volledig.
> * Fouten met betrekking tot web proxy-instellingen worden niet weergegeven in de Azure portal in uw StorSimple Device Manager-service. Als er een probleem met webproxy nadat de configuratie is voltooid, de apparaatstatus wordt gewijzigd naar **Offline** in de klassieke portal. |

## <a name="next-steps"></a>Volgende stappen
* Als u problemen ondervindt tijdens het implementeren van uw apparaat of web proxy-instellingen configureren, raadpleegt u [oplossen van de implementatie van uw StorSimple-apparaten](storsimple-troubleshoot-deployment.md).
* Voor informatie over het gebruik van de StorSimple Device Manager-service, gaat u naar [de StorSimple Device Manager-service gebruiken voor het beheren van uw StorSimple-apparaat](storsimple-8000-manager-service-administration.md).

