---
title: Verbinding maken, configureren en Azure Data Box-Gateway moet worden geactiveerd in Azure portal | Microsoft Docs
description: Derde zelfstudie voor het implementeren van Data Box Gateway u verbinding maken, instellen en activeren van uw virtuele apparaat.
services: databox
author: alkohli
ms.service: databox
ms.subservice: gateway
ms.topic: article
ms.date: 01/09/2019
ms.author: alkohli
ms.openlocfilehash: 887c1d554cd5bd2b935178a77a2de19e687ca3f2
ms.sourcegitcommit: 9b6492fdcac18aa872ed771192a420d1d9551a33
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/22/2019
ms.locfileid: "54450401"
---
# <a name="tutorial-connect-set-up-activate-azure-data-box-gateway-preview"></a>Zelfstudie: Verbinding maken, instellen, het activeren van Azure Data Box-Gateway (Preview) 

## <a name="introduction"></a>Inleiding

Deze zelfstudie wordt beschreven hoe u verbinding maakt voor het instellen en activeren van uw gegevens in het Gateway-apparaat met behulp van de lokale webgebruikersinterface. 

Het proces van setup en activering kan ongeveer tien minuten duren om uit te voeren. 

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Verbinding maken met het virtuele apparaat
> * Instellen van en het virtuele apparaat activeren

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.


> [!IMPORTANT]
> - Data Box Gateway verkeert in de preview-fase. Lees de [Gebruiksvoorwaarden voor de preview](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) voordat u deze oplossing bestelt en implementeert. 


## <a name="prerequisites"></a>Vereisten

Voordat u configureren en van uw Data Box-Gateway instellen, controleert u of:

* U hebt een virtueel apparaat ingericht en verkregen van een verbonden URL toe zoals beschreven in de [inrichten van een Data Box-Gateway in Hyper-V](data-box-gateway-deploy-provision-hyperv.md) of [inrichten van een Data Box-Gateway in VMware](data-box-gateway-deploy-provision-vmware.md).
* U hebt de activeringssleutel van de gegevens in het Gateway-service die u hebt gemaakt om gegevens in het Gateway-apparaten te beheren. Ga voor meer informatie naar [voorbereidingen voor het implementeren van Azure Data Box Gateway](data-box-gateway-deploy-prep.md).

<!--* If this is the second or subsequent virtual device that you are registering with an existing StorSimple Device Manager service, you should have the service data encryption key. This key was generated when the first device was successfully registered with this service. If you have lost this key, see [Get the service data encryption key](storsimple-ova-web-ui-admin.md#get-the-service-data-encryption-key) for your Data Box Gateway.-->

## <a name="connect-to-the-local-web-ui-setup"></a>Verbinding maken met de installatie van de lokale web-UI 

1. Open een browservenster en maak verbinding met de lokale webgebruikersinterface. Type:
   
   [https://ip-address-of-network-interface](https://ip-address-of-network-interface)
   
   Gebruik de verbindings-URL die u hebt genoteerd in de vorige zelfstudie. U ziet een foutbericht dat aangeeft dat er een probleem met het beveiligingscertificaat van de website is. Klik op **doorgaan naar deze webpagina**. (Deze stappen kunnen afwijken op basis van de browser die wordt gebruikt.)
   
    ![](./media/data-box-gateway-deploy-connect-setup-activate/image2.png)

2. Aanmelden bij de webgebruikersinterface van uw virtuele apparaat. Is het standaardwachtwoord *Wachtwoord1*. 
   
    ![](./media/data-box-gateway-deploy-connect-setup-activate/image3.png)

3. U wordt gevraagd het beheerderswachtwoord voor het apparaat te wijzigen. Typ in een nieuw wachtwoord op dat tussen 8 en 16 tekens bevat. Het wachtwoord moet 3 van de volgende bevatten: hoofdletters, kleine letters, cijfers en speciale tekens.

    ![](./media/data-box-gateway-deploy-connect-setup-activate/image4.png)

U bent nu op de **Dashboard** van uw apparaat.

## <a name="set-up-and-activate-the-virtual-device"></a>Instellen en het virtuele apparaat activeren
 
1. Vanuit het dashboard, kunt u verschillende instellingen vereist voor het configureren en het virtuele apparaat registreren bij de gegevens in het Gateway-service gaan. De **netwerkinstellingen**, **Web proxyinstellingen**, en **tijdinstellingen** zijn optioneel. De enige vereiste instellingen zijn **apparaatnaam** en **Cloudinstellingen**.
   
    ![](./media/data-box-gateway-deploy-connect-setup-activate/image5.png)

2. In de **apparaatnaam** pagina, een beschrijvende naam voor uw apparaat configureren. De beschrijvende naam kan 1 tot en met 15 tekens lang zijn en mag letters, cijfers en afbreekstreepjes bevatten.

    ![](./media/data-box-gateway-deploy-connect-setup-activate/image6.png)

3. (Optioneel) Configureer uw **netwerkinstellingen**. Er is ten minste 1 netwerkinterface en meer, afhankelijk van hoeveel u hebt geconfigureerd in de onderliggende virtuele machine. De **netwerkinstellingen** pagina voor een virtueel apparaat met één netwerkinterface ingeschakeld is, zoals hieronder weergegeven.
    
    ![](./media/data-box-gateway-deploy-connect-setup-activate/image7.png)
   
    Bij het configureren van netwerkinstellingen, houd rekening met het volgende:

    - Als DHCP is ingeschakeld in uw omgeving, worden netwerkinterfaces automatisch geconfigureerd. Daarom kan worden een IP-adres, subnet, gateway en DNS automatisch toegewezen.
    - Als DHCP niet is ingeschakeld, kunt u statische IP-adressen kunt toewijzen, indien nodig.
    - U kunt uw netwerk-interface configureren als IPv4.

    >[!NOTE] 
    > We raden aan dat u het lokale IP-adres van de netwerkinterface van statisch naar DHCP, niet overschakelen, tenzij u een ander IP-adres verbinding maken met het apparaat hebt. Als u met behulp van een netwerkinterface en u overschakelen naar DHCP, zou er geen manier om te bepalen van de DHCP-adres. Als u wijzigen in een DHCP-adres wilt, wachten totdat het apparaat is geregistreerd bij de service en vervolgens wijzigen. U kunt bekijken, de IP-adressen van alle netwerkadapters in de **apparaateigenschappen** in Azure portal voor uw service.

4. (Optioneel) Configureer uw webproxyserver. Hoewel webproxyconfiguratie optioneel is, er rekening mee dat als u een webproxy gebruikt, u alleen deze hier configureren kunt.
   
   ![](./media/data-box-gateway-deploy-connect-setup-activate/image8.png)
   
   In de **Web proxy** pagina:
   
   1. Geef de **Web-URL van proxy** in deze indeling: *http://&lt;host-IP-adres of FQDN-naam&gt;: poortnummer*. Houd er rekening mee dat HTTPS-URL's worden niet ondersteund.
   2. Geef **verificatie** als **Basic** of **geen**.
   3. Als u verificatie gebruikt, ook moet u voor een **gebruikersnaam** en **wachtwoord**.
   4. Klik op **Toepassen**. Dit wordt gevalideerd en de geconfigureerde web proxy-instellingen toepassen.

5. (Optioneel) Configureer de tijdinstellingen voor uw apparaat, zoals tijdzone en de primaire en secundaire NTP-servers. NTP-servers zijn vereist, omdat het apparaat de tijd synchroniseren moet zodat deze kan worden geverifieerd met uw cloud-serviceproviders.
    
    ![](./media/data-box-gateway-deploy-connect-setup-activate/image9.png)
    
    In de **tijdinstellingen** pagina:
    
    1. Selecteer in de vervolgkeuzelijst de **tijdzone** op basis van de geografische locatie waar het apparaat wordt geïmplementeerd. De standaardtijdzone voor uw apparaat is PST. Het apparaat zal deze tijdzone gebruiken voor alle geplande bewerkingen.
    2. Geef een **primaire NTP-server** voor uw apparaat in of accepteer de standaardwaarde van time.windows.com. Zorg ervoor dat in uw netwerk NTP-verkeer kan worden doorgegeven van uw datacenter naar internet.
    3. Geef eventueel een **secundaire NTP-server** voor uw apparaat.
    4. Klik op **Toepassen**. Dit wordt gevalideerd en de geconfigureerde time-instellingen toepassen.

6. In de **Cloudinstellingen** pagina en het activeren van uw apparaat met de gegevens in het Gateway-service in Azure portal.
    
    1. Voer de **activeringscode** die u hebt verkregen [ophalen van de activeringscode](data-box-gateway-deploy-prep.md#get-the-activation-key) voor Data Box-Gateway.

    2. Klik op **Activeren**. 
       
         ![](./media/data-box-gateway-deploy-connect-setup-activate/image10.png)
    
    3. U moet mogelijk Wacht een minuut voordat het apparaat wordt geactiveerd. Na het activeren van, wordt de pagina bijgewerkt om aan te geven dat het apparaat wordt geactiveerd.


## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie bent u meer te weten gekomen over verschillende onderwerpen met betrekking tot Data Box Gateway, zoals:

> [!div class="checklist"]
> * Verbinding maken met het virtuele apparaat
> * Instellen van en het virtuele apparaat activeren


Ga naar de volgende zelfstudie voor meer informatie over het overbrengen van gegevens met uw Data Box-Gateway.

> [!div class="nextstepaction"]
> [Gegevens overdragen met Data Box Gateway](./data-box-gateway-deploy-add-shares.md).
