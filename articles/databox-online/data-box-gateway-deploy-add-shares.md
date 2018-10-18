---
title: Gegevens overdragen met Azure Data Box Gateway | Microsoft Docs
description: Informatie over het toevoegen van en verbinding maken met shares op een Data Box Gateway-apparaat.
services: databox
author: alkohli
ms.service: databox
ms.subservice: gateway
ms.topic: tutorial
ms.date: 09/24/2018
ms.author: alkohli
ms.openlocfilehash: 6220a794c10f43d562155adbad910694a233ec88
ms.sourcegitcommit: c282021dbc3815aac9f46b6b89c7131659461e49
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/12/2018
ms.locfileid: "49166014"
---
# <a name="tutorial-transfer-data-with-azure-data-box-gateway-preview"></a>Zelfstudie: Gegevens overdragen met Azure Data Box Gateway (preview-versie)


## <a name="introduction"></a>Inleiding

In dit artikel wordt beschreven hoe u shares toevoegt aan Data Box Gateway en er verbinding mee maakt. Zodra de shares zijn toegevoegd, kan het Data Box Gateway-apparaat gegevens overdragen naar Azure.

Deze procedure neemt in totaal ongeveer tien minuten in beslag. 

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Een share toevoegen
> * Verbinding maken met een share

> [!IMPORTANT]
> - Data Box Gateway verkeert in de preview-fase. Lees de [Gebruiksvoorwaarden voor de preview](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) voordat u deze oplossing bestelt en implementeert. 
 
## <a name="prerequisites"></a>Vereisten

Voordat u shares aan Data Box Gateway gaat toevoegen, controleert u het volgende:

* U hebt een virtueel apparaat ingericht en er verbinding mee gemaakt, zoals beschreven in [Provision a Data Box Gateway in Hyper-V](data-box-gateway-deploy-provision-hyperv.md) (Data Box Gateway inrichten in Hyper-V) of [Provision a Data Box Gateway in VMware](data-box-gateway-deploy-provision-vmware.md) (Data Box Gateway inrichten in VMware). 

    Het virtuele apparaat wordt geactiveerd, zoals beschreven in [Connect and activate your Azure Data Box Gateway](data-box-gateway-deploy-connect-setup-activate.md) (Verbinding maken met Azure Data Box Gateway en het activeren). U kunt het gebruiken om shares te maken en gegevens over te dragen.


## <a name="add-a-share"></a>Een share toevoegen

Voer in de [Azure-portal](https://portal.azure.com/) de volgende stappen uit om een share te maken.

1. Ga terug naar de Azure-portal. Ga naar **Alle resources** en zoek de Data Box Gateway-resource.
    
2. In de gefilterde lijst met resources selecteert u de Data Box Gateway-resource en gaat u vervolgens naar **Overzicht**. Klik op de opdrachtbalk van het apparaat op **+ Share toevoegen**.
   
   ![Een share toevoegen](./media/data-box-gateway-deploy-add-shares/click-add-share.png)

4. Geef in **Share toevoegen** de instellingen voor de share op. Geef een unieke naam voor de share op. 

   Namen van shares mogen alleen cijfers, kleine letters en afbreekstreepjes bevatten. De naam van de share moet tussen de 3 en 63 tekens bevatten en met een cijfer of letter beginnen. Elk afbreekstreepje moet worden voorafgegaan en gevolgd door een cijfer of letter.
    
5. Selecteer een **Type** voor de share. Het type kan SMB of NFS zijn, waarbij SMB het standaardtype is. SMB is het standaardtype voor Windows-clients; NFS wordt gebruikt voor Linux-clients. De opties wijken enigszins af, afhankelijk van welk type u kiest. 

6. U dient een opslagaccount op te geven waaronder de share wordt opgeslagen. In het opslagaccount met de naam van de share wordt een container gemaakt, als deze nog niet bestaat. Als de container al bestaat, wordt de bestaande container gebruikt. 
    
7. Kies de **Opslagservice** vanuit blok-blob, pagina-blob of bestanden. Het type service dat u kiest, is afhankelijk van de indeling waarin u de gegevens in Azure wilt opslaan. In dit geval kiezen we ervoor de gegevens als blok-blobs in Azure op te slaan, dus we selecteren Blok-blob. Als u Pagina-blob kiest, dient u ervoor te zorgen dat uw gegevens op 512 bytes zijn uitgelijnd. VHDX is altijd op 512 bytes uitgelijnd.
   
8. Deze stap hangt af van of u een SMB- of een NFS-share gaat maken. 
     
    - **Als u een SMB-share maakt**: maak in het veld voor lokale gebruikers met alle bevoegdheden een keuze uit **Nieuwe maken** of **Bestaande gebruiken**. Als u een nieuwe lokale gebruiker maakt, geeft u de **gebruikersnaam** en het **wachtwoord** op en **bevestigt u het wachtwoord**. Hiermee worden de bevoegdheden aan de lokale gebruiker toegewezen. Als u de bevoegdheden hier hebt toegewezen, kunt u Verkenner gebruiken om de bevoegdheden te wijzigen.
    
        ![SMB-share toevoegen](./media/data-box-gateway-deploy-add-shares/add-share-smb-1.png)
        
        Als u het selectievakje **allow only read operations** inschakelt voor deze share-gegevens, kunt u gebruikers opgeven die alleen kunnen lezen.
        
    - **Als u een NFS-share maakt**: u dient de IP-adressen op te geven van de clients die toegang hebben tot de share.

        ![NFS-share toevoegen](./media/data-box-gateway-deploy-add-shares/add-share-nfs-1.png)
   
9. Klik op **Maken** om de share te maken. 
    
    U ontvangt een melding als wordt begonnen met het maken van de share. Als de share met de opgegeven instellingen is gemaakt, wordt de blade **Shares** bijgewerkt in overeenstemming met de nieuwe share. 
    
    ![Bijgewerkte lijst met shares](./media/data-box-gateway-deploy-add-shares/updated-list-of-shares.png) 

## <a name="connect-to-the-share"></a>Verbinding maken met de share

Voer deze stappen uit op de Windows Server-client die is verbonden met Data Box Gateway om verbinding te maken met de shares.


1. Open een opdrachtvenster. Typ het volgende na de opdrachtprompt:

    `net use \\<IP address of the device>\<share name>  /u:<user name for the share>`

    Voer het wachtwoord voor de share in wanneer hierom wordt gevraagd. De voorbeelduitvoer van deze opdracht wordt hier gepresenteerd.

    ```powershell
    Microsoft Windows [Version 18.8.16299.192) 
    (c) 2817 microsoft Corporation. All rights reserved . 
    
    C: \Users\GatewayUser>net use \\10.10.10.60\newtestuser /u:Tota11yNewUser 
    Enter the password for 'TotallyNewUser' to connect to '10.10.10.60' • 
    The command completed successfully. 
    
    C: \Users\GatewayUser>
    ```   


2. Druk op Windows-toets+R. Geef in het venster **Uitvoeren** het `\\<device IP address>` op. Klik op **OK**. Verkenner wordt nu geopend. U moet nu de shares kunnen zien die u als mappen hebt gemaakt. Selecteer een share (map) en dubbelklik erop om de inhoud te bekijken.
 
    ![Verbinding maken met een SMB-share](./media/data-box-gateway-deploy-add-shares/connect-to-share2.png)-->

    De gegevens worden naar deze shares geschreven terwijl ze worden gegenereerd en het apparaat pusht de gegevens naar de cloud.

### <a name="connect-to-an-nfs-share"></a>Verbinding maken met een NFS-share

Voer deze stappen uit op uw Linux-client die in verbinding staat met Data Box Edge.

1. Controleer of op de client de NFSv4-client is geïnstalleerd. Als u de NFS-client wilt installeren, gebruikt u de volgende opdracht:

   `sudo apt-get install nfs-common`

    Ga naar [Install NFSv4 client](https://help.ubuntu.com/community/SettingUpNFSHowTo#NFSv4_client) (NFSv4-client installeren) voor meer informatie.

2. Als de NFS-client is geïnstalleerd, gebruikt u de volgende opdracht om de NFS-share te koppelen die u op uw Data Box Gateway-apparaat hebt gemaakt:

   `sudo mount <device IP>:/<NFS share on device> /home/username/<Folder on local Linux computer>`

    Voordat u de koppelingen gaat instellen, controleert u of de mappen die als koppelpunten op uw lokale computer fungeren, al zijn gemaakt en geen bestanden of submappen bevatten.

    Het volgende voorbeeld toont hoe u via NFS verbinding maakt met een share op een Gateway-apparaat. Het IP-adres van het virtuele apparaat is `10.10.10.60`, de share `mylinuxshare2` wordt gekoppeld op de Ubuntu-VM, het koppelpunt is `/home/databoxubuntuhost/gateway`.

    `sudo mount -t nfs 10.10.10.60:/mylinuxshare2 /home/databoxubuntuhost/gateway`

> [!NOTE] 
> De volgende beperkingen zijn van toepassing op de preview-versie:
> - Nadat een bestand in de shares is gemaakt, kan de naam van het bestand niet meer worden gewijzigd. 
> - Als een bestand uit een share wordt verwijderd, wordt de vermelding in het opslagaccount niet verwijderd.

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie bent u meer te weten gekomen over verschillende onderwerpen met betrekking tot Data Box Gateway, zoals:

> [!div class="checklist"]
> * Een share toevoegen
> * Verbinding maken met een share


Ga naar de volgende zelfstudie om te lezen hoe u Data Box Gateway kunt beheren.

> [!div class="nextstepaction"]
> [Use local web UI to administer a Data Box Gateway](http://aka.ms/dbg-docs) (Lokale webgebruikersinterface gebruiken om Data Box Gateway te beheren)


