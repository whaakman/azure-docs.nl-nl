---
title: Gegevens overdragen met Azure Data Box Edge | Microsoft Docs
description: Informatie over het toevoegen van en verbinding maken met shares op een Data Box Edge-apparaat.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: tutorial
ms.date: 10/08/2018
ms.author: alkohli
Customer intent: As an IT admin, I need to understand how to add and connect to shares on Data Box Edge so I can use it to transfer data to Azure.
ms.openlocfilehash: 6810818e48329d883961c840fa83857d84b98fd4
ms.sourcegitcommit: fec0e51a3af74b428d5cc23b6d0835ed0ac1e4d8
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/12/2019
ms.locfileid: "56112866"
---
# <a name="tutorial-transfer-data-with-azure-data-box-edge-preview"></a>Zelfstudie: Gegevens overdragen met Azure Data Box Edge (preview)

In deze zelfstudie wordt beschreven hoe u shares toevoegt aan een Data Box Edge-apparaat en er verbinding mee maakt. Nadat u de shares hebt toegevoegd, kunnen via Data Box Edge gegevens worden overgedragen naar Azure.

Deze procedure neemt in totaal ongeveer tien minuten in beslag. 

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Een share toevoegen
> * Verbinding maken met de share

> [!IMPORTANT]
> Data Box Edge is in de preview-fase. Bekijk de [Azure-gebruiksvoorwaarden voor de preview](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) voordat u deze oplossing bestelt en implementeert. 
 
## <a name="prerequisites"></a>Vereisten

Voordat u shares aan Data Box Edge gaat toevoegen, controleert u het volgende:
* U hebt uw fysieke apparaat geïnstalleerd zoals beschreven in [Azure Data Box Edge installeren](data-box-edge-deploy-install.md). 

* U hebt het fysieke apparaat geactiveerd zoals beschreven in [Azure Data Box Edge aansluiten, instellen en activeren](data-box-edge-deploy-connect-setup-activate.md). 

* Het apparaat is klaar om bestandsshares te maken en gegevens over te dragen.


## <a name="add-a-share"></a>Een share toevoegen

Ga als volgt te werk om een share te maken:

1. Ga in de [Azure-portal](https://portal.azure.com/) naar **Alle resources**, en zoek vervolgens naar uw Data Box Edge-resource.
    
1. Selecteer uw Data Box Edge-resource in de gefilterde lijst met resources.

1. Selecteer in het linkerdeelvenster **Overzicht** en selecteer vervolgens **Share toevoegen**.
   
   ![Een share toevoegen](./media/data-box-edge-deploy-add-shares/click-add-share.png)

1. Ga als volgt te werk in het deelvenster **Share delen**:

    a. Geef in het vak **Naam** een unieke naam op voor de share.  
    De naam van de share mag alleen bestaan uit kleine letters, cijfers en afbreekstreepjes. De naam moet tussen de 3 en 63 tekens lang zijn en beginnen met een letter of cijfer. Afbreekstreepjes moeten worden voorafgegaan door een letter of cijfer.
    
    b. Selecteer een **Type** voor de share.  
    Het type kan **SMB** of **NFS** zijn. SMB is het standaardtype. SMB is het standaardtype voor Windows-clients; NFS wordt gebruikt voor Linux-clients.  
    De andere opties verschillen enigszins, afhankelijk van of u kiest voor SMB- of NFS-shares. 

    c. Bied een opslagaccount waarin de share wordt opgeslagen.  
    Als er nog geen container bestaat, wordt in het opslagaccount een container gemaakt met de naam van de zojuist gemaakte share. Als er al een container bestaat, wordt de bestaande container gebruikt. 
    
    d. Selecteer in de vervolgkeuzelijst **Storage-service** de optie **Blok-blob**, **Pagina-blob** of **Bestanden**.  
    Het type service dat u selecteert, is afhankelijk van de gewenste indeling voor de gegevens in Azure. In dit voorbeeld selecteren we **Blok-blob** omdat we de gegevens willen opslaan als blob-blokken. Als u Pagina-blob selecteert, moet u ervoor zorgen dat uw gegevens zijn uitgelijnd op 512 bytes. VHDX is bijvoorbeeld altijd op 512 bytes uitgelijnd.
   
    e. Voer een van de volgende stappen uit, afhankelijk van of u een SMB-share of een NFS-share hebt gemaakt: 
     
    - **SMB-share**: Selecteer onder **Lokale gebruiker met Alle machtigingen** de optie **Nieuwe maken** of **Bestaande gebruiken**. Als u een nieuwe lokale gebruiker maakt, voert u een gebruikersnaam en wachtwoord in. Vervolgens bevestigt u het wachtwoord. Met deze actie worden machtigingen toegewezen aan de lokale gebruiker. Nadat u de machtigingen hier hebt toegewezen, kunt u Verkenner gebruiken om ze aan te passen.

        Als u het selectievakje **Alleen leesbewerkingen toestaan** selecteert voor deze sharegegevens, kunt u gebruikers met het kenmerk Alleen-lezen opgeven.

        ![SMB-share toevoegen](./media/data-box-edge-deploy-add-shares/add-share-smb-1.png)
   
    - **NFS-share**: Voer de IP-adressen van de clients in die toegang hebben tot de share.

        ![NFS-share toevoegen](./media/data-box-edge-deploy-add-shares/add-share-nfs-1.png)
   
1. Selecteer **Maken** om de share te maken. 
    
    U ontvangt een melding als wordt begonnen met het maken van de share. Nadat de share is gemaakt met de opgegeven instellingen, wordt de sectie **Share** bijgewerkt met de nieuwe sharegegevens. 
    
    ![Bijgewerkte lijst met shares](./media/data-box-edge-deploy-add-shares/updated-list-of-shares.png) 

## <a name="connect-to-the-share"></a>Verbinding maken met de share

U kunt nu verbinding maken met een of meer shares die u hebt gemaakt in de laatste stap. Afhankelijk van of u een SMB- of een NFS-share hebt, kunnen de stappen variëren. 

### <a name="connect-to-an-smb-share"></a>Verbinding maken met een SMB-share

Maak op de Windows Server-client die is verbonden met uw Data Box Edge-apparaat, verbinding met een SMB-share door deze opdrachten in te voeren:


1. Typ in een opdrachtvenster:

    `net use \\<IP address of the device>\<share name>  /u:<user name for the share>`

1. Voer het wachtwoord voor de share in als u hierom wordt gevraagd.  
   De voorbeelduitvoer van deze opdracht wordt hier gepresenteerd.

    ```powershell
    Microsoft Windows [Version 10.0.16299.192) 
    (c) 2017 Microsoft Corporation. All rights reserved. 
    
    C: \Users\DataBoxEdgeUser>net use \\10.10.10.60\newtestuser /u:Tota11yNewUser 
    Enter the password for 'TotallyNewUser' to connect to '10.10.10.60': 
    The command completed successfully. 
    
    C: \Users\DataBoxEdgeUser>
    ```   


1. Selecteer Windows + R op het toetsenbord. 

1. Geef in het venster **Uitvoeren** de `\\<device IP address>` op en selecteer vervolgens **OK**.  
   Verkenner wordt geopend. Als het goed is, ziet u nu de shares die u hebt gemaakt, als mappen. Dubbelklik in Verkenner op een share (map) om de inhoud te bekijken.
 
    ![Verbinding maken met een SMB-share](./media/data-box-edge-deploy-add-shares/connect-to-share2.png)

    De gegevens worden naar deze shares geschreven terwijl ze worden gegenereerd en het apparaat pusht de gegevens naar de cloud.

### <a name="connect-to-an-nfs-share"></a>Verbinding maken met een NFS-share

Ga als volgt te werk in de Linux-client die is verbonden met het Data Box Edge-apparaat:

1. Zorg ervoor dat de NFSv4-client is geïnstalleerd op de client. Als u de NFS-client wilt installeren, gebruikt u de volgende opdracht:

   `sudo apt-get install nfs-common`

    Ga naar [Install NFSv4 client](https://help.ubuntu.com/community/SettingUpNFSHowTo#NFSv4_client) (NFSv4-client installeren) voor meer informatie.

1. Nadat de NFS-client is geïnstalleerd, koppelt u de NFS-share die u hebt gemaakt, aan het Data Box Edge-apparaat met behulp van de volgende opdracht:

   `sudo mount -t nfs -o sec=sys,resvport <device IP>:/<NFS shares on device> /home/username/<Folder on local Linux computer>`

    > [!IMPORTANT]
    > Voordat u de share koppelt, controleert u of de mappen die zullen fungeren als koppelpunten op de lokale computer, al zijn gemaakt. Deze mappen mogen geen bestanden of submappen bevatten.

    Het volgende voorbeeld toont hoe u via NFS verbinding maakt met een share op een Data Box Edge-apparaat. Het IP-adres van het apparaat is `10.10.10.60`. De share `mylinuxshare2` is gekoppeld aan de ubuntuVM. Het koppelpunt van de share is `/home/databoxubuntuhost/edge`.

    `sudo mount -t nfs -o sec=sys,resvport 10.10.10.60:/mylinuxshare2 /home/databoxubuntuhost/Edge`

> [!NOTE] 
> De volgende beperkingen zijn van toepassing op de preview-versie:
> - Nadat een bestand in de share is gemaakt, kan de naam van het bestand niet meer worden gewijzigd. 
> - Als een bestand uit een share wordt verwijderd, wordt de vermelding in het opslagaccount niet verwijderd.

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie bent u meer te weten gekomen over de volgende Data Box Edge-onderwerpen:

> [!div class="checklist"]
> * Een share toevoegen
> * Verbinding maken met een share

Als u wilt leren hoe u gegevens kunt transformeren met behulp van Data Box Edge, gaat u verder met de volgende zelfstudie:

> [!div class="nextstepaction"]
> [Gegevens transformeren met Data Box Edge](./data-box-edge-deploy-configure-compute.md)


