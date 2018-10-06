---
title: Verbinding maken, configureren en Azure Data Box Edge moet worden geactiveerd in Azure portal | Microsoft Docs
description: Derde zelfstudie voor het implementeren van Data Box Edge u verbinding maken en instellen op, en activeer uw fysieke apparaat.
services: databox-edge-gateway
documentationcenter: NA
author: alkohli
manager: twooley
editor: ''
ms.assetid: ''
ms.service: databox-edge-gateway
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 10/08/2018
ms.author: alkohli
ms.custom: ''
Customer intent: As an IT admin, I need to understand how to connect and activate Data Box Edge so I can use it to transfer data to Azure.
ms.openlocfilehash: 83bc1d81eaa930fc16c895f4e3b8b9bf1b1ad28c
ms.sourcegitcommit: 26cc9a1feb03a00d92da6f022d34940192ef2c42
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/06/2018
ms.locfileid: "48832200"
---
# <a name="tutorial-connect-set-up-activate-azure-data-box-edge-preview"></a>Zelfstudie: Verbinding maken, instellen, activeren van Azure Data Box Edge (Preview) 

Deze zelfstudie wordt beschreven hoe u verbinding maakt voor het instellen en activeren van uw gegevens in het Edge-apparaat met behulp van de lokale webgebruikersinterface. 

De installatie en activering kan duren ongeveer 20 minuten om te voltooien. 

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Verbinding maken met een fysiek apparaat
> * Instellen van en het fysieke apparaat activeren

> [!IMPORTANT]
> Data Box Edge is beschikbaar als preview. Lees de [Gebruiksvoorwaarden voor de preview](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) voordat u deze oplossing bestelt en implementeert. 


## <a name="prerequisites"></a>Vereiste onderdelen

Voordat u configureren en van uw gegevens in Edge instellen, zorg ervoor dat:

* U hebt het fysieke apparaat geïnstalleerd zoals beschreven in [Data Box-Edge installeren](data-box-edge-deploy-install.md).
* U hebt de activeringssleutel van de gegevens in het Edge-service die u hebt gemaakt voor het beheren van de gegevens in het Edge-apparaat. Ga voor meer informatie naar [voorbereidingen voor het implementeren van Azure Data Box Edge](data-box-edge-deploy-prep.md).

## <a name="connect-to-the-local-web-ui-setup"></a>Verbinding maken met de installatie van de lokale web-UI 

1. Configureer de Ethernet-adapter op de computer die u gebruikt voor het verbinding maken met het Edge-apparaat met een statisch IP-adres 192.168.100.5 en subnet 255.255.255.0.
2. Verbind de computer op poort 1 op uw apparaat. 
3. Open een browservenster en de toegang tot de lokale webgebruikersinterface van het apparaat op https://192.168.100.10. Deze actie duurt een paar minuten nadat u hebt ingeschakeld op het apparaat. 
4. Er is een fout of een waarschuwing die aangeeft dat er een probleem met het beveiligingscertificaat van de website is. Klik op **doorgaan naar deze webpagina**. (Deze stappen kunnen afwijken op basis van de browser die wordt gebruikt.)
   
    ![](./media/data-box-edge-deploy-connect-setup-activate/image2.png)

2. Aanmelden bij de webgebruikersinterface van uw apparaat. Is het standaardwachtwoord *Wachtwoord1*. 
   
    ![](./media/data-box-edge-deploy-connect-setup-activate/image3.png)

3. U wordt gevraagd het beheerderswachtwoord voor het apparaat te wijzigen. Typ in een nieuw wachtwoord op dat tussen 8 en 16 tekens bevat. Het wachtwoord moet 3 van de volgende tekens bevatten: hoofdletters, kleine letters, cijfers en speciale tekens.

U kunt nu op de **Dashboard** van uw apparaat.

## <a name="set-up-and-activate-the-physical-device"></a>Instellen en het fysieke apparaat activeren
 
1. Vanuit het dashboard, kunt u verschillende instellingen vereist voor het configureren en het fysieke apparaat registreren bij de gegevens in het Edge-service gaan. De **apparaatnaam**, **netwerkinstellingen**, **Web proxyinstellingen**, en **tijdinstellingen** zijn optioneel. De enige vereiste instellingen zijn **Cloudinstellingen**.
   
    ![](./media/data-box-edge-deploy-connect-setup-activate/set-up-activate-1.png)

2. In de **apparaatnaam** pagina, een beschrijvende naam voor uw apparaat configureren. De beschrijvende naam kan 1 tot en met 15 tekens lang zijn en mag letters, cijfers en afbreekstreepjes bevatten.

    ![](./media/data-box-edge-deploy-connect-setup-activate/set-up-activate-2.png)

3. (Optioneel) Configureer uw **netwerkinstellingen**. Op het fysieke apparaat ziet u zes netwerkinterfaces. POORT 1 en 2 van de poort zijn 1 Gbps-netwerkinterfaces. POORT 3, 4 poort, poort 5 en 6 voor poort zijn alle netwerkinterfaces van 25 Gbps. POORT 1 wordt automatisch geconfigureerd als alleen-management-poort, poort 2 tot en met 6 poort zijn alle gegevenspoorten. De **netwerkinstellingen** pagina, zoals hieronder weergegeven.
    
    ![](./media/data-box-edge-deploy-connect-setup-activate/set-up-activate-3.png)
   
    Houd er rekening mee bij het configureren van netwerkinstellingen:

    - Als DHCP is ingeschakeld in uw omgeving, worden netwerkinterfaces automatisch geconfigureerd. Een IP-adres, subnet, gateway en DNS worden automatisch toegewezen.
    - Als DHCP niet is ingeschakeld, kunt u statische IP-adressen kunt toewijzen, indien nodig.
    - U kunt uw netwerk-interface configureren als IPv4.
   
4. (Optioneel) Configureer uw webproxyserver. Hoewel webproxyconfiguratie optioneel is, als u een webproxy gebruikt kunt u alleen het hier configureert.
   
   ![](./media/data-box-edge-deploy-connect-setup-activate/set-up-activate-4.png)
   
   In de **Web proxy** pagina:
   
   1. Geef de **Web-URL van proxy** in deze indeling: `http://host-IP address or FDQN:Port number`. HTTPS-URL's worden niet ondersteund.
   2. Geef **verificatie** als **Basic** of **geen**.
   3. Als u verificatie gebruikt, moet u ook voor een **gebruikersnaam** en **wachtwoord**.
   4. Klik op **toepassen** om te valideren en de geconfigureerde web proxy-instellingen toepassen.

5. (Optioneel) Configureer de tijdinstellingen voor uw apparaat, zoals tijdzone en de primaire en secundaire NTP-servers. NTP-servers zijn vereist, omdat het apparaat de tijd synchroniseren moet zodat deze kan worden geverifieerd met uw cloud-serviceproviders.
    
    ![](./media/data-box-edge-deploy-connect-setup-activate/set-up-activate-5.png)
    
    In de **tijdinstellingen** pagina:
    
    1. Selecteer in de vervolgkeuzelijst de **tijdzone** op basis van de geografische locatie waar het apparaat wordt geïmplementeerd. De standaardtijdzone voor uw apparaat is PST. Het apparaat zal deze tijdzone gebruiken voor alle geplande bewerkingen.
    2. Geef een **primaire NTP-server** voor uw apparaat in of accepteer de standaardwaarde van time.windows.com. Zorg ervoor dat in uw netwerk NTP-verkeer kan worden doorgegeven van uw datacenter naar internet.
    3. Geef eventueel een **secundaire NTP-server** voor uw apparaat.
    4. Klik op **toepassen** om te valideren en de geconfigureerde time-instellingen toepassen.

6. In de **Cloudinstellingen** pagina en het activeren van uw apparaat en de gegevens in het Edge-service in Azure portal.
    
    1. Voer de **activeringscode** die u hebt verkregen [ophalen van de activeringscode](data-box-edge-deploy-prep.md#get-the-activation-key) voor gegevens in Edge.

    2. Klik op **Toepassen**. 
       
         ![](./media/data-box-edge-deploy-connect-setup-activate/set-up-activate-6.png)
    
    3. Nadat het apparaat wordt geactiveerd, krijgt u met connectiviteitsopties modus. Deze instellingen zijn geconfigureerd als u wilt werken met het apparaat in gedeeltelijk niet-verbonden of niet-verbonden modus. 

        ![](./media/data-box-edge-deploy-connect-setup-activate/set-up-activate-7.png)    

Installatie van het apparaat is voltooid. U kunt nu bestandsshares toevoegen op het apparaat.


## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u geleerd over gegevens in het Edge-onderwerpen, zoals:

> [!div class="checklist"]
> * Verbinding maken met een fysiek apparaat
> * Instellen van en het fysieke apparaat activeren


Ga naar de volgende zelfstudie voor meer informatie over het overbrengen van gegevens met uw gegevens in Edge.

> [!div class="nextstepaction"]
> [Gegevens overdragen met gegevens in Edge](./data-box-edge-deploy-add-shares.md).