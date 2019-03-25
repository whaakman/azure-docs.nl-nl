---
title: Verbinding maken, configureren en activeren van een Azure Data Box-Edge-apparaat in Azure portal | Microsoft Docs
description: Derde zelfstudie voor het implementeren van Data Box Edge u verbinding maken en instellen op, en activeer uw fysieke apparaat.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: article
ms.date: 03/08/2019
ms.author: alkohli
Customer intent: As an IT admin, I need to understand how to connect and activate Data Box Edge so I can use it to transfer data to Azure.
ms.openlocfilehash: 0aaee4b0582f8acb9302c75463c0bc52698acaeb
ms.sourcegitcommit: 81fa781f907405c215073c4e0441f9952fe80fe5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/25/2019
ms.locfileid: "58403465"
---
# <a name="tutorial-connect-set-up-and-activate-azure-data-box-edge"></a>Zelfstudie: Verbinding maken en activeren van Azure Data Box Edge instellen 

Deze zelfstudie wordt beschreven hoe u verbinding kunt maken, instellen en uw Azure Data Box-Edge-apparaat activeren met behulp van de lokale webgebruikersinterface.

De installatie en activering kan duren ongeveer 20 minuten om te voltooien.

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Verbinding maken met een fysiek apparaat
> * Instellen en het fysieke apparaat activeren

## <a name="prerequisites"></a>Vereisten

Voordat u configureren en van uw gegevens in het Edge-apparaat instellen, zorg ervoor dat:

* U hebt het fysieke apparaat geïnstalleerd zoals beschreven in [Data Box-Edge installeren](data-box-edge-deploy-install.md).
* U hebt de activeringssleutel van de gegevens in het Edge-service die u hebt gemaakt voor het beheren van de gegevens in het Edge-apparaat. Ga voor meer informatie naar [voorbereidingen voor het implementeren van Azure Data Box Edge](data-box-edge-deploy-prep.md).

## <a name="connect-to-the-local-web-ui-setup"></a>Verbinding maken met de installatie van de lokale web-UI 

1. Configureer de Ethernet-adapter op uw computer verbinding maken met de gegevens in het Edge-apparaat met een statisch IP-adres 192.168.100.5 en subnet 255.255.255.0.

2. Verbind de computer op poort 1 op uw apparaat. Gebruik de volgende afbeelding om te identificeren van poort 1 op uw apparaat.

    ![Achterpaneel van een bekabeld apparaat](./media/data-box-edge-deploy-install/backplane-cabled.png)


3. Open een browservenster en de toegang tot de lokale webgebruikersinterface van het apparaat op https://192.168.100.10.  
    Deze actie duurt een paar minuten nadat u hebt ingeschakeld op het apparaat. 

    Er is een fout of een waarschuwing die aangeeft dat er een probleem met het beveiligingscertificaat van de website is. 
   
    ![Foutbericht voor website security-certificaat](./media/data-box-edge-deploy-connect-setup-activate/image2.png)

4. Selecteer **doorgaan naar deze webpagina**.  
    Deze stappen kunnen afwijken afhankelijk van de browser die u gebruikt.

5. Aanmelden bij de webgebruikersinterface van uw apparaat. Is het standaardwachtwoord *Wachtwoord1*. 
   
    ![Gegevens in het Edge-apparaat aanmeldingspagina opgeven](./media/data-box-edge-deploy-connect-setup-activate/image3.png)

6. Wijzig het beheerderswachtwoord voor het apparaat bij de prompt.  
    Het nieuwe wachtwoord moet tussen 8 en 16 tekens bevatten. Deze moet drie van de volgende tekens bevatten: hoofdletters, kleine letters, cijfers en speciale tekens.

U kunt nu op het dashboard van uw apparaat.

## <a name="set-up-and-activate-the-physical-device"></a>Instellen en het fysieke apparaat activeren
 
Uw dashboard vindt u de verschillende instellingen die nodig zijn om te configureren en het fysieke apparaat registreren bij de gegevens in het Edge-service. De **apparaatnaam**, **netwerkinstellingen**, **Web proxyinstellingen**, en **tijdinstellingen** zijn optioneel. De enige vereiste instellingen zijn **Cloudinstellingen**.
   
![Lokale web-UI 'Dashboard' pagina](./media/data-box-edge-deploy-connect-setup-activate/set-up-activate-1.png)

1. Selecteer in het linkerdeelvenster **apparaatnaam**, en voer vervolgens een beschrijvende naam voor uw apparaat.  
    De beschrijvende naam moet tussen 1 tot 15 tekens bevatten en hebben letters, cijfers en afbreekstreepjes bevatten.

    ![Lokale web-UI 'Apparaatnaam' pagina](./media/data-box-edge-deploy-connect-setup-activate/set-up-activate-2.png)

2. (Optioneel) Selecteer in het linkerdeelvenster **netwerkinstellingen** en configureer vervolgens de instellingen.  
    Zijn zes netwerkinterfaces op uw fysieke apparaat. POORT 1 en 2 van de poort zijn 1 Gbps-netwerkinterfaces. POORT 3, 4-poort, poort 5 en 6 poort zijn alle 25 Gbps netwerkinterfaces die kunnen ook dienen als 10 Gbps-netwerkinterfaces. POORT 1 wordt automatisch geconfigureerd als alleen-management-poort en poort 2 tot en met 6 poort zijn alle gegevenspoorten. De **netwerkinstellingen** pagina is, zoals hieronder weergegeven.
    
    ![Lokale web-UI "Netwerkinstellingen" pagina](./media/data-box-edge-deploy-connect-setup-activate/set-up-activate-3.png)
   
    Als u de netwerkinstellingen configureren, houd er rekening mee:

   - Als DHCP is ingeschakeld in uw omgeving, worden netwerkinterfaces automatisch geconfigureerd. Een IP-adres, subnet, gateway en DNS worden automatisch toegewezen.
   - Als DHCP niet is ingeschakeld, kunt u statische IP-adressen kunt toewijzen, indien nodig.
   - U kunt uw netwerk-interface configureren als IPv4.

     >[!NOTE] 
     > We raden aan dat u het lokale IP-adres van de netwerkinterface van statisch naar DCHP, niet overschakelen, tenzij u een ander IP-adres verbinding maken met het apparaat hebt. Als u met behulp van een netwerkinterface en u overschakelen naar DHCP, zou er geen manier om te bepalen van de DHCP-adres. Als u wijzigen in een DHCP-adres wilt, wachten totdat het apparaat is geregistreerd bij de service en vervolgens wijzigen. U kunt bekijken, de IP-adressen van alle netwerkadapters in de **apparaateigenschappen** in Azure portal voor uw service.

3. (Optioneel) Selecteer in het linkerdeelvenster **Web proxyinstellingen**, en configureer vervolgens uw webproxyserver. Hoewel de configuratie van uw webproxy is optioneel, als u een webproxy gebruikt kunt u deze alleen op deze pagina configureren.
   
   ![Lokale web-UI 'Webproxy-instellingen' pagina](./media/data-box-edge-deploy-connect-setup-activate/set-up-activate-4.png)
   
   Op de **Web proxyinstellingen** pagina, de volgende handelingen uit:
   
   a. In de **Web-URL van proxy** voert u de URL in deze indeling: `http://host-IP address or FQDN:Port number`. HTTPS-URL's worden niet ondersteund.

   b. Onder **verificatie**, selecteer **geen** of **NTLM**.

   c. Als u verificatie gebruikt, voert u een gebruikersnaam en wachtwoord.

   d. Als u wilt valideren en de geconfigureerde web proxy-instellingen toepassen, selecteert u **instellingen toepassen**.

4. (Optioneel) Selecteer in het linkerdeelvenster **tijdinstellingen**, en configureer vervolgens de tijdzone en de primaire en secundaire NTP-servers voor uw apparaat.  
    NTP-servers zijn vereist, omdat het apparaat de tijd synchroniseren moet zodat deze kan worden geverifieerd met uw cloud-serviceproviders.
       
    Op de **tijdinstellingen** pagina, de volgende handelingen uit:
    
    1. In de **tijdzone** vervolgkeuzelijst, selecteert u de tijdzone die overeenkomt met de geografische locatie waar het apparaat wordt geïmplementeerd.
        De standaardtijdzone voor uw apparaat is PST. Het apparaat zal deze tijdzone gebruiken voor alle geplande bewerkingen.

    2. In de **primaire NTP-server** vak, voer de primaire server voor uw apparaat of accepteer de standaardwaarde van time.windows.com.  
        Zorg ervoor dat uw netwerk NTP-verkeer kan worden doorgegeven van uw datacenter met het internet toestaat.

    3. (Optioneel) in de **secundaire NTP-server** voert u een secundaire server voor uw apparaat.

    4. Als u wilt valideren en de geconfigureerde time-instellingen toepassen, selecteert u **instellingen toepassen**.

        ![Lokale web-UI "Tijdinstellingen" pagina](./media/data-box-edge-deploy-connect-setup-activate/set-up-activate-5.png)

5. Selecteer in het linkerdeelvenster **Cloudinstellingen**, en vervolgens uw apparaat en de gegevens in het Edge-service in Azure portal te activeren.
    
    1. In de **activeringscode** voert de activeringscode die u hebt verkregen in [ophalen van de activeringscode](data-box-edge-deploy-prep.md#get-the-activation-key) voor gegevens in Edge.
    2. Selecteer **Toepassen**.
       
        ![Lokale web-UI ' Cloud ' instellingenpagina](./media/data-box-edge-deploy-connect-setup-activate/set-up-activate-6.png)

    3. Eerst wordt het apparaat geactiveerd. Het apparaat wordt vervolgens gescand voor alle essentiële updates en indien beschikbaar, de updates automatisch worden toegepast. U ziet een melding in die zin.

        Het dialoogvenster heeft ook een herstelsleutel of moet u kopiëren en sla deze op een veilige locatie. Deze sleutel wordt gebruikt voor het herstellen van uw gegevens in het geval dat het apparaat niet kan opstarten.

        ![Lokale web-UI ' Cloud ' instellingenpagina bijgewerkt](./media/data-box-edge-deploy-connect-setup-activate/set-up-activate-7.png)

    4. U moet mogelijk Wacht enkele minuten nadat de update is voltooid. De pagina bijgewerkt om aan te geven dat het apparaat wordt geactiveerd.

        ![Lokale web-UI ' Cloud ' instellingenpagina bijgewerkt](./media/data-box-edge-deploy-connect-setup-activate/set-up-activate-8.png)

Installatie van het apparaat is voltooid. U kunt nu bestandsshares toevoegen op het apparaat.

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie heeft u het volgende geleerd:

> [!div class="checklist"]
> * Verbinding maken met een fysiek apparaat
> * Instellen en het fysieke apparaat activeren

Voor meer informatie over het overdragen van gegevens met uw gegevens in het Edge-apparaat, Zie:

> [!div class="nextstepaction"]
> [Gegevens overdragen met gegevens in Edge](./data-box-edge-deploy-add-shares.md).
