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
ms.openlocfilehash: 61d427c22f2ac57627ac04a91748e4e6cf8c4e55
ms.sourcegitcommit: c282021dbc3815aac9f46b6b89c7131659461e49
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/12/2018
ms.locfileid: "49165353"
---
# <a name="tutorial-transfer-data-with-azure-data-box-edge-preview"></a>Zelfstudie: Gegevens overdragen met Azure Data Box Edge (preview)

In deze zelfstudie wordt beschreven hoe u shares toevoegt aan Data Box Edge en er verbinding mee maakt. Zodra de shares zijn toegevoegd, kan het Data Box Edge-apparaat gegevens overdragen naar Azure.

Deze procedure neemt in totaal ongeveer tien minuten in beslag. 

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Een share toevoegen
> * Verbinding maken met een share

> [!IMPORTANT]
> Data Box Edge is in de preview-fase. Lees de [Gebruiksvoorwaarden voor de preview](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) voordat u deze oplossing bestelt en implementeert. 
 
## <a name="prerequisites"></a>Vereisten

Voordat u shares aan Data Box Edge gaat toevoegen, controleert u het volgende:

* U hebt uw fysieke apparaat geïnstalleerd zoals beschreven in [Een Data Box Edge installeren](data-box-edge-deploy-install.md). 

    Het fysieke apparaat is geactiveerd zoals wordt beschreven in [Uw Azure Data Box-Edge aansluiten en activeren](data-box-edge-deploy-connect-setup-activate.md). Het apparaat is klaar om bestandsshares te maken en gegevens over te dragen.


## <a name="add-a-share"></a>Een share toevoegen

Voer in de [Azure-portal](https://portal.azure.com/) de volgende stappen uit om een share te maken.

1. Ga terug naar de Azure-portal. Ga naar **Alle resources** en zoek de Data Box Edge-resource op.
    
2. In de gefilterde lijst met resources selecteert u de Data Box Edge-resource en gaat u vervolgens naar **Overzicht**. Klik op de opdrachtbalk van het apparaat op **+ Share toevoegen**.
   
   ![Een share toevoegen](./media/data-box-edge-deploy-add-shares/click-add-share.png)

3. Geef in **Share toevoegen** de instellingen voor de share op. Geef een unieke naam voor de share op. 

   Namen van shares mogen alleen cijfers, kleine letters en afbreekstreepjes bevatten. De naam van de share moet tussen de 3 en 63 tekens bevatten en met een cijfer of letter beginnen. Elk afbreekstreepje moet worden voorafgegaan en gevolgd door een cijfer of letter.
    
    1. Selecteer een **Type** voor de share. Het type kan SMB of NFS zijn, waarbij SMB het standaardtype is. SMB is het standaardtype voor Windows-clients; NFS wordt gebruikt voor Linux-clients. 

    2. De opties wijken enigszins af, afhankelijk van welk type u kiest. 

    3. U dient een opslagaccount op te geven waaronder de share wordt opgeslagen. In het opslagaccount met de naam van de share wordt een container gemaakt, als deze nog niet bestaat. Als de container al bestaat, wordt de bestaande container gebruikt. 
    
    4. Kies de **Opslagservice** vanuit blok-blob, pagina-blob of bestanden. Het type service dat u kiest, is afhankelijk van de indeling waarin u de gegevens in Azure wilt opslaan. In dit geval kiezen we ervoor de gegevens als blok-blobs in Azure op te slaan, dus we selecteren Blok-blob. Als u Pagina-blob kiest, dient u ervoor te zorgen dat uw gegevens op 512 bytes zijn uitgelijnd. VHDX is bijvoorbeeld altijd op 512 bytes uitgelijnd.
   
    5. Deze stap hangt af van of u een SMB- of een NFS-share gaat maken. 
     
        - **Als u een SMB-share maakt**: maak in het veld voor lokale gebruikers met alle bevoegdheden een keuze uit **Nieuwe maken** of **Bestaande gebruiken**. Als u een nieuwe lokale gebruiker maakt, geeft u de **gebruikersnaam** en het **wachtwoord** op en **bevestigt u het wachtwoord**. Hiermee worden de bevoegdheden aan de lokale gebruiker toegewezen. Als u de bevoegdheden hier hebt toegewezen, kunt u Verkenner gebruiken om de bevoegdheden te wijzigen.

            Als u het selectievakje **allow only read operations** inschakelt voor deze share-gegevens, kunt u gebruikers opgeven die alleen kunnen lezen.

            ![SMB-share toevoegen](./media/data-box-edge-deploy-add-shares/add-share-smb-1.png)
   
        - **Als u een NFS-share maakt**: u dient de IP-adressen op te geven van de clients die toegang hebben tot de share.

            ![NFS-share toevoegen](./media/data-box-edge-deploy-add-shares/add-share-nfs-1.png)
   
4. Klik op **Maken** om de share te maken. 
    
    U ontvangt een melding als wordt begonnen met het maken van de share. Als de share met de opgegeven instellingen is gemaakt, wordt de blade **Shares** bijgewerkt in overeenstemming met de nieuwe share. 
    
    ![Bijgewerkte lijst met shares](./media/data-box-edge-deploy-add-shares/updated-list-of-shares.png) 

## <a name="connect-to-the-share"></a>Verbinding maken met de share

U kunt nu verbinding maken met een of meer shares die u in de vorige stap hebt gemaakt. Afhankelijk van of u een SMB- of een NFS-share hebt, kunnen de stappen verschillen. 

### <a name="connect-to-an-smb-share"></a>Verbinding maken met een SMB-share

Voer deze stappen uit op de Windows Server-client die is verbonden met Data Box Edge om verbinding te maken met de shares.


1. Open een opdrachtvenster. Typ het volgende na de opdrachtprompt:

    `net use \\<IP address of the device>\<share name>  /u:<user name for the share>`

    Voer het wachtwoord voor de share in wanneer hierom wordt gevraagd. De voorbeelduitvoer van deze opdracht wordt hier gepresenteerd.

    ```powershell
    Microsoft Windows [Version 10.0.16299.192) 
    (c) 2017 microsoft Corporation. All rights reserved . 
    
    C: \Users\DataBoxEdgeUser>net use \\10.10.10.60\newtestuser /u:Tota11yNewUser 
    Enter the password for 'TotallyNewUser' to connect to '10.10.10.60': 
    The command completed successfully. 
    
    C: \Users\DataBoxEdgeUser>
    ```   


2. Druk op Windows-toets+R. Geef in het venster **Uitvoeren** het `\\<device IP address>` op. Klik op **OK**. Verkenner wordt nu geopend. U moet nu de shares kunnen zien die u als mappen hebt gemaakt. Selecteer een share (map) en dubbelklik erop om de inhoud te bekijken.
 
    ![Verbinding maken met een SMB-share](./media/data-box-edge-deploy-add-shares/connect-to-share2.png)

    De gegevens worden naar deze shares geschreven terwijl ze worden gegenereerd en het apparaat pusht de gegevens naar de cloud.

### <a name="connect-to-an-nfs-share"></a>Verbinding maken met een NFS-share

Voer deze stappen uit op uw Linux-client die in verbinding staat met Data Box Edge.

1. Controleer of op de client de NFSv4-client is geïnstalleerd. Als u de NFS-client wilt installeren, gebruikt u de volgende opdracht:

   `sudo apt-get install nfs-common`

    Ga naar [Install NFSv4 client](https://help.ubuntu.com/community/SettingUpNFSHowTo#NFSv4_client) (NFSv4-client installeren) voor meer informatie.

2. Als de NFS-client is geïnstalleerd, gebruikt u de volgende opdracht om de NFS-share te koppelen die u op uw Data Box Edge-apparaat hebt gemaakt:

   `sudo mount <device IP>:/<NFS share on device> /home/username/<Folder on local Linux computer>`

    Voordat u de shares koppelt, controleert u of de mappen die zullen fungeren als koppelpunten op de lokale computer al zijn gemaakt. Deze mappen mogen geen bestanden of submappen bevatten.

    Het volgende voorbeeld toont hoe u via NFS verbinding maakt met een share op een Data Box Edge-apparaat. Het IP-adres van het apparaat is `10.10.10.60`. De share `mylinuxshare2` is gekoppeld aan de ubuntuVM. Het koppelpunt van de share is `/home/databoxubuntuhost/edge`.

    `sudo mount -t nfs 10.10.10.60:/mylinuxshare2 /home/databoxubuntuhost/Edge`

> [!NOTE] 
> De volgende beperkingen zijn van toepassing op de preview-versie:
> - Nadat een bestand in de shares is gemaakt, kan de naam van het bestand niet meer worden gewijzigd. 
> - Als een bestand uit een share wordt verwijderd, wordt de vermelding in het opslagaccount niet verwijderd.

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie bent u meer te weten gekomen over verschillende onderwerpen met betrekking tot Data Box Edge, zoals:

> [!div class="checklist"]
> * Een share toevoegen
> * Verbinding maken met een share


Ga naar de volgende zelfstudie om te lezen hoe u uw gegevens transformeert met Data Box Edge.

> [!div class="nextstepaction"]
> [Gegevens transformeren met Data Box Edge](./data-box-edge-deploy-configure-compute.md)


