---
title: Verbinding maken, configureren en Azure Data Box-Gateway moet worden geactiveerd in Azure portal | Microsoft Docs
description: Derde zelfstudie voor het implementeren van Data Box Gateway u verbinding maken, instellen en activeren van uw virtuele apparaat.
services: databox
author: alkohli
ms.service: databox
ms.subservice: gateway
ms.topic: article
ms.date: 03/18/2019
ms.author: alkohli
ms.openlocfilehash: de2ef4908260a62acf28a270dda6ad738a1760b9
ms.sourcegitcommit: 81fa781f907405c215073c4e0441f9952fe80fe5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/25/2019
ms.locfileid: "58402324"
---
# <a name="tutorial-connect-set-up-activate-azure-data-box-gateway"></a>Zelfstudie: Verbinding maken, instellen, Azure Data Box Gateway activeren

## <a name="introduction"></a>Inleiding

Deze zelfstudie wordt beschreven hoe u verbinding maken met, instellen en uw gegevens in het Gateway-apparaat activeren met behulp van de lokale webgebruikersinterface. 

Het proces van setup en activering kan ongeveer tien minuten duren om uit te voeren. 

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Verbinding maken met een virtueel apparaat
> * Instellen en het virtuele apparaat activeren

## <a name="prerequisites"></a>Vereisten

Voordat u configureren en van uw Data Box-Gateway instellen, controleert u of:

* U hebt een virtueel apparaat ingericht en verkregen van een verbonden URL toe zoals beschreven in de [inrichten van een Data Box-Gateway in Hyper-V](data-box-gateway-deploy-provision-hyperv.md) of [inrichten van een Data Box-Gateway in VMware](data-box-gateway-deploy-provision-vmware.md).
* U hebt de activeringssleutel van de gegevens in het Gateway-service die u hebt gemaakt om gegevens in het Gateway-apparaten te beheren. Ga voor meer informatie naar [voorbereidingen voor het implementeren van Azure Data Box Gateway](data-box-gateway-deploy-prep.md).


## <a name="connect-to-the-local-web-ui-setup"></a>Verbinding maken met de installatie van de lokale web-UI 

1. Open een browservenster en de toegang tot de lokale webgebruikersinterface van het apparaat op:
   
   [https://ip-address-of-network-interface](https://ip-address-of-network-interface)
   
   Gebruik de verbindings-URL die u hebt genoteerd in de vorige zelfstudie. Er is een fout of een waarschuwing die aangeeft dat er een probleem met het beveiligingscertificaat van de website is.

2. Selecteer **doorgaan naar deze webpagina**. Deze stappen kunnen afwijken afhankelijk van de browser die u gebruikt.
   
    ![Foutbericht voor website security-certificaat](./media/data-box-gateway-deploy-connect-setup-activate/image2.png)

3. Aanmelden bij de webgebruikersinterface van uw virtuele apparaat. Is het standaardwachtwoord *Wachtwoord1*. 
   
    ![Aanmelden bij de lokale web-UI](./media/data-box-gateway-deploy-connect-setup-activate/image3.png)

4. Wijzig bij de prompt wachtwoord voor het apparaat. Het nieuwe wachtwoord moet tussen 8 en 16 tekens bevatten. Deze moet 3 van de volgende waarden bevatten: hoofdletters, kleine letters, cijfers en speciale tekens.

    ![Wachtwoord voor apparaat wijzigen](./media/data-box-gateway-deploy-connect-setup-activate/image4.png)

U kunt nu op de **Dashboard** van uw apparaat.

## <a name="set-up-and-activate-the-virtual-device"></a>Instellen en het virtuele apparaat activeren
 
Uw dashboard vindt u de verschillende instellingen die nodig zijn voor het configureren en registreren van het virtuele apparaat met de gegevens in het Gateway-service. De **apparaatnaam**, **netwerkinstellingen**, **Web proxyinstellingen**, en **tijdinstellingen** zijn optioneel. De enige vereiste instellingen zijn **Cloudinstellingen**.
   
![Lokale web-UI 'Dashboard' pagina](./media/data-box-gateway-deploy-connect-setup-activate/image5.png)

1. Selecteer in het linkerdeelvenster **apparaatnaam**, en voer vervolgens een beschrijvende naam voor uw apparaat. De beschrijvende naam moet bevatten van 1 tot 15 tekens lang zijn en zijn letters, cijfers en afbreekstreepjes bevatten.

    ![Lokale web-UI 'Apparaatnaam' pagina](./media/data-box-gateway-deploy-connect-setup-activate/image6.png)

2. (Optioneel) Selecteer in het linkerdeelvenster **netwerkinstellingen** en configureer vervolgens de instellingen. Op uw virtuele apparaat ziet u ten minste één netwerkinterface en meer, afhankelijk van hoeveel u hebt geconfigureerd in de onderliggende virtuele machine. De **netwerkinstellingen** pagina voor een virtueel apparaat met één netwerkinterface ingeschakeld is, zoals hieronder weergegeven.
    
    ![Lokale web-UI "Netwerkinstellingen" pagina](./media/data-box-gateway-deploy-connect-setup-activate/image7.png)
   
    Als u netwerkinstellingen configureert, houd er rekening mee:

    - Als DHCP is ingeschakeld in uw omgeving, worden netwerkinterfaces automatisch geconfigureerd. Een IP-adres, subnet, gateway en DNS worden automatisch toegewezen.
    - Als DHCP niet is ingeschakeld, kunt u statische IP-adressen kunt toewijzen, indien nodig.
    - U kunt uw netwerk-interface configureren als IPv4.

     >[!NOTE] 
     > We raden aan dat u het lokale IP-adres van de netwerkinterface van statisch naar DHCP, niet overschakelen, tenzij u een ander IP-adres verbinding maken met het apparaat hebt. Als u met behulp van een netwerkinterface en u overschakelen naar DHCP, zou er geen manier om te bepalen van de DHCP-adres. Als u wijzigen in een DHCP-adres wilt, wachten totdat het apparaat is geregistreerd bij de service en vervolgens wijzigen. U kunt bekijken, de IP-adressen van alle netwerkadapters in de **apparaateigenschappen** in Azure portal voor uw service.

3. Configureer desgewenst uw webproxyserver. Hoewel de configuratie van uw webproxy is optioneel, als u een webproxy gebruikt kunt u deze alleen op deze pagina configureren.
   
   ![Lokale web-UI 'Webproxy-instellingen' pagina](./media/data-box-gateway-deploy-connect-setup-activate/image8.png)
   
   Op de **Web proxy** pagina, de volgende handelingen uit:
   
   1. In de **Web-URL van proxy** voert u de URL in deze indeling: `http://&lt;host-IP address or FQDN&gt;:Port number`. HTTPS-URL's worden niet ondersteund.
   2. Onder **verificatie**, selecteer **geen** of **NTLM**.
   3. Als u verificatie gebruikt, voert u een **gebruikersnaam** en **wachtwoord**.
   4. Als u wilt valideren en de geconfigureerde web proxy-instellingen toepassen, selecteert u **toepassen**.

4. (Optioneel) Selecteer in het linkerdeelvenster **tijdinstellingen**, en configureer vervolgens de tijdzone en de primaire en secundaire NTP-servers voor uw apparaat. 

    NTP-servers zijn vereist, omdat het apparaat de tijd synchroniseren moet zodat deze kan worden geverifieerd met uw cloud-serviceproviders.
    
    ![Lokale web-UI "Tijdinstellingen" pagina](./media/data-box-gateway-deploy-connect-setup-activate/image9.png)
    
    In de **tijdinstellingen** pagina, de volgende handelingen uit:
    
    1. In de **tijdzone** vervolgkeuzelijst, selecteert u de tijdzone die overeenkomt met de geografische locatie waarin het apparaat is geïmplementeerd.
        De standaardtijdzone voor uw apparaat is PST. Het apparaat zal deze tijdzone gebruiken voor alle geplande bewerkingen.

    2. Geef een **primaire NTP-server** voor uw apparaat in of accepteer de standaardwaarde van `time.windows.com`.   
        Zorg ervoor dat in uw netwerk NTP-verkeer kan worden doorgegeven van uw datacenter naar internet.

    3. (Optioneel) in de **secundaire NTP-server** voert u een secundaire server voor uw apparaat.

    4. Als u wilt valideren en de geconfigureerde time-instellingen toepassen, selecteert u **toepassen**.

6. Selecteer in het linkerdeelvenster **Cloudinstellingen**, en vervolgens het apparaat met de gegevens in het Gateway-service in Azure-portal te activeren.
    
    1. In de **activeringscode** voert u de **activeringscode** die u hebt verkregen [ophalen van de activeringscode](data-box-gateway-deploy-prep.md#get-the-activation-key) voor Data Box-Gateway.

    2. Selecteer **activeren**.
       
         ![Lokale web-UI ' Cloud ' instellingenpagina](./media/data-box-gateway-deploy-connect-setup-activate/image10a.png)
    
    3. Het apparaat wordt geactiveerd en essentiële updates, indien beschikbaar, worden automatisch toegepast. U ziet een melding in die zin. Controleer de voortgang van de update via de Azure-portal.

        ![Lokale web-UI ' Cloud ' instellingenpagina](./media/data-box-gateway-deploy-connect-setup-activate/image12.png)
        
        **Het dialoogvenster heeft ook een herstelsleutel die u moet het kopiëren en opslaan op een veilige locatie. Deze sleutel wordt gebruikt voor het herstellen van uw gegevens in het geval dat het apparaat niet kan opstarten.**


    4. U moet mogelijk Wacht enkele minuten voor de update te voltooien. Na de update is voltooid, zich aanmelden bij het apparaat. De **Cloudinstellingen** pagina updates om aan te geven dat het apparaat wordt geactiveerd.

        ![Lokale web-UI ' Cloud ' instellingenpagina bijgewerkt](./media/data-box-gateway-deploy-connect-setup-activate/image13.png)

Installatie van het apparaat is voltooid. U kunt nu bestandsshares toevoegen op het apparaat.

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie heeft u het volgende geleerd:

> [!div class="checklist"]
> * Verbinding maken met een virtueel apparaat
> * Instellen en het virtuele apparaat activeren

Voor meer informatie over het overdragen van gegevens met uw Data Box-Gateway, Zie:

> [!div class="nextstepaction"]
> [Gegevens overdragen met Data Box Gateway](./data-box-gateway-deploy-add-shares.md).
