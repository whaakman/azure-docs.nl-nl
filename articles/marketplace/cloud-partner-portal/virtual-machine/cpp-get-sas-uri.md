---
title: Handtekening voor gedeelde toegang URI voor uw Microsoft Azure op basis van VM-installatiekopie ophalen | Microsoft Docs
description: Wordt uitgelegd hoe u de shared access signature (SAS)-URI voor uw VM-installatiekopie.
services: Azure, Marketplace, Cloud Partner Portal,
documentationcenter: ''
author: pbutlerm
manager: Patrick.Butler
editor: ''
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: article
ms.date: 10/19/2018
ms.author: pbutlerm
ms.openlocfilehash: dcfe744cc8ca6f3b3cd201898a79fcce3f24f8d5
ms.sourcegitcommit: 17633e545a3d03018d3a218ae6a3e4338a92450d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/22/2018
ms.locfileid: "49639684"
---
# <a name="get-shared-access-signature-uri-for-your-vm-image"></a>Handtekening voor gedeelde toegang URI voor uw VM-installatiekopie ophalen

Tijdens het publicatieproces, moet u een uniform resource identifier (URI) opgeven voor elke virtuele harde schijf (VHD) die zijn gekoppeld aan uw SKU's. Tijdens het certificeringsproces moet Microsoft toegang hebben tot deze VHD's. In dit artikel wordt uitgelegd hoe u een shared access signature (SAS)-URI voor elke VHD genereren. Voert u deze URI in de **SKU's** tabblad in de Cloud Partner-Portal. 

Bij het genereren van SAS URI's voor uw VHD's, voldoen aan de volgende vereisten:

- Alleen niet-beheerde VHD's worden ondersteund.
- `List` en `Read` machtigingen zijn voldoende. Voer *niet* bieden `Write` of `Delete` toegang.
- De duur van de toegang (*vervaldatum*) moet minimaal drie weken wanneer de SAS-URI wordt gemaakt.
- Als u wilt beschermen tegen variaties van de UTC-tijd, stelt u de begindatum op een dag vóór de huidige datum. Als de huidige datum 6 oktober 2014 is, selecteert u bijvoorbeeld 10/5/2014.

## <a name="generate-the-sas-url"></a>De SAS-URL genereren

De SAS-URL kan worden gegenereerd in twee algemene manieren met behulp van de volgende hulpprogramma's:

-   Microsoft Storage Explorer - grafische hulpprogramma beschikbaar voor Windows, macOS en Linux
-   Microsoft Azure-CLI - aanbevolen voor niet - Windows-besturingssystemen en geautomatiseerde of continue integratie omgevingen


### <a name="azure-cli"></a>Azure-CLI

Gebruik de volgende stappen voor het genereren van een SAS-URI met Azure CLI.

1.  Download en installeer de [Microsoft Azure-CLI](https://azure.microsoft.com/documentation/articles/xplat-cli-install/).  Versies zijn beschikbaar voor Windows, macOS en verschillende distributies van Linux. 
2.  Maak een PowerShell-bestand (`.ps1` bestandsextensie), in de volgende code kopiëren en klik vervolgens lokaal opslaan.

    ``` powershell
    az storage container generate-sas --connection-string 'DefaultEndpointsProtocol=https;AccountName=<account-name>;AccountKey=<account-key>;EndpointSuffix=core.windows.net' --name <vhd-name> --permissions rl --start '<start-date>' --expiry '<expiry-date>'
    ```
    
3.  Bewerk het bestand dat u de volgende parameterwaarden kunt opgeven.  Datums moeten worden opgegeven in UTC-datum/tijd-notatie, bijvoorbeeld `10-25-2016T00:00:00Z`.
    - `<account-name>` -Uw Azure storage-accountnaam
    - `<account-key>` -Uw Azure storage-accountsleutel
    - `<vhd-name>` -De naam van de VHD
    - `<start-date>` -Machtiging begindatum voor toegang tot de VHD. Geef een datum één dag vóór de huidige datum. 
    - `<expiry-date>` -Vervaldatum machtiging voor toegang tot de VHD.  Geef een datum ten minste drie weken na de huidige datum. 
 
    Het volgende voorbeeld ziet de juiste parameterwaarden (op het moment van schrijven).

    ``` powershell
        az storage container generate-sas --connection-string 'DefaultEndpointsProtocol=https;AccountName=st00009;AccountKey=6L7OWFrlabs7Jn23OaR3rvY5RykpLCNHJhxsbn9ONc+bkCq9z/VNUPNYZRKoEV1FXSrvhqq3aMIDI7N3bSSvPg==;EndpointSuffix=core.windows.net' --name vhds --permissions rl --start '2017-11-06T00:00:00Z' --expiry '2018-08-20T00:00:00Z'
    ```
 
4. Sla de wijzigingen in dit PowerShell-script.
5. Dit script uitvoeren, met beheerdersbevoegdheden, voor het genereren van een *SAS-verbindingsreeks* voor toegang tot de container op.  U kunt twee basismethoden gebruiken:
    - Voer het script uit de console.  Bijvoorbeeld in Windows, schrijven klikt u op het script en selecteer **als administrator uitvoeren**.
    - Voer het script van een PowerShell-script-editor, zoals de [Windows PowerShell ISE](https://docs.microsoft.com/powershell/scripting/core-powershell/ise/introducing-the-windows-powershell-ise), met beheerdersbevoegdheden. 
  Het volgende voorbeeld toont een SAS-verbindingsreeks die wordt gegenereerd binnen deze editor. 

    ![Genereren van SAS-URI in PowerShell ISE](./media/publishvm_032.png)

6. Kopieer de resulterende SAS-verbindingsreeks en sla deze naar een tekstbestand op een veilige locatie.  U kunt deze tekenreeks om toe te voegen van de gekoppelde VHD-locatie-informatie voor het maken van de laatste SAS-URI wordt bewerken. 
7. Navigeer naar de blob-opslag met de VHD die is gekoppeld aan de zojuist gegenereerde-URI in de Azure-portal.
8. Kopieer de URL-waarde van de **eindpunt van Blob service**, zoals hieronder weergegeven.

    ![Eindpunt van BLOB service in Azure portal](./media/publishvm_033.png)

9. Bewerk het tekstbestand dat door de SAS-verbindingsreeks uit stap 6.  U kunt de volledige SAS-URI met de volgende indeling wordt maken:

    `<blob-service-endpoint-url>` + `/vhds/` + `<vhd-name>?` + `<sas-connection-string>`

    Bijvoorbeeld, als de naam van de VDH `TestRGVM2.vhd`, bedragen de resulterende SAS-URI:

    `https://catech123.blob.core.windows.net/vhds/TestRGVM2.vhd?st=2018-05-06T07%3A00%3A00Z&se=2019-08-02T07%3A00%3A00Z&sp=rl&sv=2017-04-17&sr=c&sig=wnEw9RfVKeSmVgqDfsDvC9IHhis4x0fc9Hu%2FW4yvBxk%3D`

Herhaal deze stappen voor elke VHD in de SKU's die u van plan bent om te publiceren.


### <a name="microsoft-storage-explorer"></a>Microsoft Storage Explorer

Gebruik de volgende stappen voor het genereren van een SAS-URI met de Microsoft Azure Storage Explorer.

1. De [Microsoft Azure Storage Explorer](https://azure.microsoft.com/features/storage-explorer/) downloaden en installeren.
2. Open de Verkenner en klik in de menubalk links op de **-Account toevoegen** pictogram.  De **verbinding maken met Azure Storage** in het dialoogvenster wordt weergegeven.
3. Selecteer **Een Azure-account toevoegen** en klik op **Aanmelden**.  Doorgaan met de vereiste stappen aan te melden bij uw Azure-account.
4. In de linkerbenedenhoek **Explorer** deelvenster, gaat u naar uw **Opslagaccounts** en vouw dit knooppunt.
5. Met de rechtermuisknop op uw VHD en selecteer **Share Access Signature ophalen** in het contextmenu. 

    ![SAS-item ophalen in Azure Explorer](./media/publishvm_034.png)

6. De **Shared Access Signature** dialoogvenster wordt weergegeven. Voer waarden in voor de volgende velden:
    - **Begintijd** -machtiging voor toegang tot de VHD de begindatum. Geef een datum die één dag vóór de huidige datum is verwijderd.
    - **Verlooptijd** -datum van afloop voor machtiging voor toegang tot de VHD.  Geef een datum ten minste drie weken na de huidige datum.
    - **Machtigingen** : Selecteer de `Read` en `List` machtigingen. 

    ![SAS-dialoogvenster in Azure Explorer](./media/publishvm_035.png)

7. Klik op **maken** te maken van de bijbehorende SAS-URI voor deze VHD.  Meer informatie over deze bewerking worden nu weergegeven in het dialoogvenster. 
8. Kopieer de **URL** waarde en sla deze op een tekstbestand op een veilige locatie. 

    ![SAS-URI maken in Azure Explorer](./media/publishvm_036.png)

    Deze gegenereerde SAS-URL is voor toegang tot de container-niveau.  Als u specifieke, moet de naam van de gekoppelde VHD worden toegevoegd.

9. Bewerk het tekstbestand. Invoegen van de naam van uw VHD na de `vhds` tekenreeks in de SAS-URL (inclusief een slash).  De laatste SAS-URI moet de indeling:

    `<blob-service-endpoint-url>` + `/vhds/` + `<vhd-name>?` + `<sas-connection-string>`

    Bijvoorbeeld, als de naam van de VDH `TestRGVM2.vhd`, bedragen de resulterende SAS-URI:

    `https://catech123.blob.core.windows.net/vhds/TestRGVM2.vhd?st=2018-05-06T07%3A00%3A00Z&se=2019-08-02T07%3A00%3A00Z&sp=rl&sv=2017-04-17&sr=c&sig=wnEw9RfVKeSmVgqDfsDvC9IHhis4x0fc9Hu%2FW4yvBxk%3D`

Herhaal deze stappen voor elke VHD in de SKU's die u van plan bent om te publiceren.


## <a name="verify-the-sas-uri"></a>Controleer of de SAS-URI

Bekijk en controleer of dat elk gegenereerde SAS-URI met behulp van de volgende controlelijst.  Controleren of:
- De URI is van het formulier:       `<blob-service-endpoint-url>` + `/vhds/` + `<vhd-name>?` + `<sas-connection-string>`
- De URI bevat het VHD-installatiekopie bestandsnaam, inclusief de extensie '.vhd'.
- Voor het midden van de URI, `sp=rl` wordt weergegeven. Deze tekenreeks geeft aan dat `Read` en `List` toegang is opgegeven.
- Daarna moet `sr=c` wordt ook weergegeven. Deze tekenreeks geeft aan dat toegang container-niveau is opgegeven.
- Kopieer en plak de URI in een browser om te beginnen met de bijbehorende blob te downloaden.  (U kunt de bewerking annuleert voordat het downloaden is voltooid.)


## <a name="next-steps"></a>Volgende stappen

Als u hebt met het genereren van een SAS-URI problemen, Zie [problemen met algemene SAS-URL](./cpp-common-sas-url-issues.md).  Anders de SAS URI(s) opslaan naar een veilige locatie voor later gebruik. Dit is vereist voor het [uw VM-aanbieding publiceren](./cpp-publish-offer.md) in de Cloud Partner-Portal.
