---
title: Azure Data Box oplossen van problemen voor het gebruik van de REST-interface | Microsoft Docs
description: Beschrijft hoe u problemen in Azure Data Box worden gezien bij het kopiëren van gegevens is via de REST-interface.
services: databox
author: alkohli
ms.service: databox
ms.subservice: disk
ms.topic: article
ms.date: 04/19/2019
ms.author: alkohli
ms.openlocfilehash: c5ceeb2e6419cab7945454087edd4c821db28343
ms.sourcegitcommit: 0568c7aefd67185fd8e1400aed84c5af4f1597f9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/06/2019
ms.locfileid: "65204224"
---
# <a name="troubleshoot-issues-related-to-azure-data-box-blob-storage"></a>Oplossen van problemen met betrekking tot Azure Data Box-Blob-opslag

Deze artikelinformatie meer informatie over het oplossen van problemen mogelijk ziet u bij het gebruik van de gegevens in Blob-opslag via de REST-interface op uw Data Box om gegevens te kopiëren. Deze problemen surface wanneer u gegevens in Blob-opslag met andere toepassingen of -clientbibliotheken zoals Azure Storage Explorer, AzCopy of Azure Storage-bibliotheek voor Python.

## <a name="errors-seen-in-azure-storage-explorer"></a>Fouten bekijken in Azure Storage Explorer

In deze sectie worden enkele van de problemen bij het gebruik van Azure Storage Explorer met de gegevens in Blob-opslag.

|Foutbericht  |Aanbevolen actie |
|---------|---------|
|Kan geen onderliggende resources ophalen. De waarde voor een van de HTTP-headers is niet de juiste indeling.|Uit de **bewerken** in het menu **doel Azure Stack-API's**. <br>Start opnieuw op Azure Storage Explorer.|
|`getaddrinfo ENOTFOUND <accountname>.blob.<serialnumber>.microsoftdatabox.com` |Controleer of de naam van het eindpunt `<accountname>.blob.<serialnumber>.microsoftdatabox.com` wordt toegevoegd aan het hosts-bestand in dit pad: <li>`C:\Windows\System32\drivers\etc\hosts` op Windows, of </li><li> `/etc/hosts` op Linux.</li>|
|Kan geen onderliggende resources ophalen. <br>Details: zelf-ondertekend certificaat |Importeer het SSL-certificaat voor uw apparaat in Azure Storage Explorer: <li>Download het certificaat van de Azure-portal. Ga voor meer informatie naar [downloaden van het certificaat](data-box-deploy-copy-data-via-rest.md#download-certificate).</li><li>Uit de **bewerken** in het menu **SSL-certificaten** en selecteer vervolgens **certificaten importeren**.</li>|

## <a name="errors-seen-in-azcopy-for-windows"></a>Fouten bekijken in de AzCopy voor Windows

In deze sectie worden enkele van de problemen bij het gebruik van AzCopy voor Windows met gegevens in Blob-opslag.

|Foutbericht  |Aanbevolen actie |
|---------|---------|
|AzCopy-opdracht weergegeven vastlopen gedurende één minuut voordat deze fout wordt weergegeven: <br>Fout bij het inventariseren van directory https://... De externe naam kan niet worden omgezet. `<accountname>.blob.<serialnumber>.microsoftdatabox.com`|Controleer of de naam van het eindpunt `<accountname>.blob.<serialnumber>.microsoftdatabox.com` wordt toegevoegd aan het hosts-bestand op: `C:\Windows\System32\drivers\etc\hosts`.|
|AzCopy-opdracht weergegeven vastlopen gedurende één minuut voordat deze fout wordt weergegeven: <br>Fout bij het parseren van de bronlocatie. De onderliggende verbinding is gesloten: Kan geen vertrouwensrelatie voor het beveiligde SSL/TLS-kanaal.|Importeer het SSL-certificaat voor uw apparaat in het certificaatarchief van het systeem. Ga voor meer informatie naar [downloaden van het certificaat](data-box-deploy-copy-data-via-rest.md#download-certificate).|


## <a name="errors-seen-in-azcopy-for-linux"></a>Fouten bekijken in de AzCopy voor Linux

In deze sectie worden enkele van de problemen bij het gebruik van AzCopy voor Linux met gegevens in Blob-opslag.

|Foutbericht  |Aanbevolen actie |
|---------|---------|
|AzCopy-opdracht weergegeven vastlopen voor 20 minuten voordat deze fout wordt weergegeven: <br>Fout bij het parseren van de locatie van `https://<accountname>.blob.<serialnumber>.microsoftdatabox.com/<cntnr>`. Onbekend apparaat of -adres|Controleer of de naam van het eindpunt `<accountname>.blob.<serialnumber>.microsoftdatabox.com` wordt toegevoegd aan het hosts-bestand op: `/etc/hosts`.|
|AzCopy-opdracht weergegeven vastlopen voor 20 minuten voordat deze fout wordt weergegeven: <br>Fout bij het parseren van de bronlocatie... De SSL-verbinding kan niet worden gemaakt.|Importeer het SSL-certificaat voor uw apparaat in het certificaatarchief van het systeem. Ga voor meer informatie naar [downloaden van het certificaat](data-box-deploy-copy-data-via-rest.md#download-certificate).|

## <a name="errors-seen-in-azure-storage-library-for-python"></a>Fouten bekijken in Azure Storage-bibliotheek voor Python

In deze sectie worden enkele van de meest voorkomende problemen tijdens de implementatie van de Data Box-schijf met het gebruik van een Linux-client voor het kopiëren van gegevens.

|Foutbericht  |Aanbevolen actie |
|---------|---------|
|De waarde voor een van de HTTP-headers is niet de juiste indeling. |De geïnstalleerde versie van de Microsoft Azure Storage-bibliotheek voor Python wordt niet ondersteund door Data Box. Zie vereisten voor Azure Data Box-Blob-opslag voor ondersteunde versies.|
|… [SSL: CERTIFICATE_VERIFY_FAILED]...|Voordat u Python uitvoert, stelt u de omgevingsvariabele requests_ca_bundle naar het pad van de met Base64 gecodeerde SSL-certificaatbestand (Zie hoe u [downloaden van het certificaat](data-box-deploy-copy-data-via-rest.md#download-certificate)). <br>Bijvoorbeeld:<br>`export REQUESTS_CA_BUNDLE=/tmp/mycert.cer` <br>`python` <br>U kunt ook het certificaat toevoegen aan het certificaatarchief van het systeem en vervolgens deze omgevingsvariabele instellen op het pad van dat archief. <br> Bijvoorbeeld op Ubuntu: <br>`export REQUESTS_CA_BUNDLE=/etc/ssl/certs/ca-certificates.crt` <br>`python`|


## <a name="common-errors"></a>Algemene fouten

Deze fouten zijn niet specifiek zijn voor elke toepassing.

|Foutbericht  |Aanbevolen actie |
|---------|---------|
|Er is een time-out opgetreden voor de verbinding. |Meld u aan bij de Data Box-apparaat en controleer of deze ontgrendeld is. Telkens wanneer u het apparaat opnieuw is opgestart en blijft het vergrendeld totdat iemand zich aanmeldt.|

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over de [gegevens in Blob storage-systeemvereisten](data-box-system-requirements-rest.md).
