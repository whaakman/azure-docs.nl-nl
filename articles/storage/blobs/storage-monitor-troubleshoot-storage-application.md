---
title: Controleren en problemen oplossen van een cloud-storage-toepassing in Azure | Microsoft Docs
description: Gebruik diagnostische hulpprogramma's, metrische gegevens en waarschuwingen oplossen en bewaken van een cloudtoepassing.
services: storage
documentationcenter: 
author: georgewallace
manager: timlt
editor: 
ms.service: storage
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: csharp
ms.topic: tutorial
ms.date: 09/19/2017
ms.author: gwallace
ms.custom: mvc
ms.openlocfilehash: db88c331f79d83e0124519f8b6dbb34514b456dd
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="monitor-and-troubleshoot-a-cloud-storage-application"></a>Controleren en problemen oplossen van een cloud-storage-toepassing

Deze zelfstudie wordt vier en het laatste gedeelte van een serie. U informatie over het controleren en problemen oplossen van een cloud-storage-toepassing.

In de vierde deel van de reeks leert u hoe:

> [!div class="checklist"]
> * Inschakelen van logboekregistratie en metrische gegevens
> * Waarschuwingen voor autorisatie fouten inschakelen
> * Verkeer van de test uitgevoerd met onjuiste SAS-tokens
> * Download en analyseren van Logboeken

[Azure storage analytics](../common/storage-analytics.md) biedt registreren en metrische gegevens voor een opslagaccount. Deze gegevens biedt inzicht in de status van uw opslagaccount. Voordat u de zichtbaarheid in uw storage-account zijn kunt, moet u het verzamelen van gegevens instellen. Dit proces omvat het inschakelen van logboekregistratie, metrische gegevens te configureren en inschakelen van waarschuwingen.

Logboekregistratie en metrische gegevens van de storage-accounts worden ingeschakeld vanuit de **Diagnostics** tabblad in de Azure-portal. Er zijn twee soorten metrische gegevens. **Cumulatieve** metrische gegevens verzamelen percentages inkomend en uitgaand, beschikbaarheid, latentie en geslaagd. Deze metrische gegevens worden voor de blob, queue, table en Bestandsservices samengevoegd. **Per API** dezelfde set van metrische gegevens voor elke opslagbewerking in de API van Azure Storage-service worden verzameld. Opslag logboekregistratie kunt u de details van de records voor geslaagde en mislukte aanvragen in uw opslagaccount. Deze logboeken kunnen u de details van lezen, schrijven en verwijderen van bewerkingen op basis van uw Azure-tabellen, wachtrijen en blobs. Ze ook mogelijk om te zien van de redenen voor mislukte aanvragen zoals time-outs, beperking en autorisatie-fouten.

## <a name="log-in-to-the-azure-portal"></a>Aanmelden bij Azure Portal

Meld u aan bij [Azure Portal](https://portal.azure.com).

## <a name="turn-on-logging-and-metrics"></a>Inschakelen van logboekregistratie en metrische gegevens

Selecteer in het menu links **resourcegroepen**, selecteer **myResourceGroup**, en selecteer vervolgens uw storage-account in de lijst met resources.

Onder **Diagnostics** ingesteld **Status** naar **op**. Zorg ervoor dat **Blob cumulatieve metrische gegevens**, **Blob per API metrische gegevens**, en **Blob-logboeken** zijn ingeschakeld.

Wanneer voltooid, klikt u op **opslaan**

![Deelvenster diagnostische gegevens](media/storage-monitor-troubleshoot-storage-application/figure1.png)

## <a name="enable-alerts"></a>Waarschuwingen inschakelen

Waarschuwingen bieden een manier om e-beheerders of een webhook op basis van een inbreuk op een drempelwaarde metrische trigger. In dit voorbeeld, schakelt u een waarschuwing voor de `SASClientOtherError` metriek.

### <a name="navigate-to-the-storage-account-in-the-azure-portal"></a>Navigeer naar het opslagaccount in de Azure-portal

Selecteer in het menu links **resourcegroepen**, selecteer **myResourceGroup**, en selecteer vervolgens uw storage-account in de lijst met resources.

Onder de **bewaking** sectie **waarschuwing regels**.

Selecteer **+ waarschuwing toevoegen**onder **een waarschuwingsregel toevoegen**, vul de vereiste informatie. Kies `SASClientOtherError` van de **metriek** vervolgkeuzelijst.

![Deelvenster diagnostische gegevens](media/storage-monitor-troubleshoot-storage-application/figure2.png)

## <a name="simulate-an-error"></a>Een fout simuleren

U kunt proberen om te simuleren een geldige waarschuwing, voor het aanvragen van een niet-bestaande blob uit uw storage-account. Om dit te doen, vervang de `<incorrect-blob-name>` waarde met een waarde die niet bestaat. Mislukte aanvragen voor blob het volgende codevoorbeeld een paar keer uitvoeren om te simuleren.

```azurecli-interactive
sasToken=$(az storage blob generate-sas \
    --account-name <storage-account-name> \
    --account-key <storage-account-key> \
    --container-name <container> \
    --name <incorrect-blob-name> \
    --permissions r \
    --expiry `date --date="next day" +%Y-%m-%d` \
    --output tsv)

curl https://<storage-account-name>.blob.core.windows.net/<container>/<incorrect-blob-name>?$sasToken
```

De volgende afbeelding is een voorbeeld van de waarschuwing die is gebaseerd op de gesimuleerde fout die is uitgevoerd met het voorgaande voorbeeld.

 ![Voorbeeld van de waarschuwing](media/storage-monitor-troubleshoot-storage-application/alert.png)

## <a name="download-and-view-logs"></a>Logboeken downloaden en weergeven

Opslag Logboeken gegevens opslaan in een reeks blobs in een blobcontainer met de naam **$logs** in uw opslagaccount. Deze container wordt niet weergegeven als u alle blob-containers in uw account weergeven, maar u de inhoud zien kunt als u rechtstreeks toegang.

In dit scenario gebruikt u [Microsoft Message Analyzer](http://technet.microsoft.com/library/jj649776.aspx) om te communiceren met uw Azure storage-account.

### <a name="download-microsoft-message-analyzer"></a>Downloaden van Microsoft Message Analyzer

Download [Microsoft Message Analyzer](https://www.microsoft.com/download/details.aspx?id=44226) en installeer de toepassing.

Start de toepassing en kies **bestand** > **Open** > **van andere bestandsbronnen**.

In de **bestand Selector** dialoogvenster Selecteer **+ Azure-verbinding toevoegen**. Voer in uw **opslagaccountnaam** en **accountsleutel** en klik op **OK**.

![Microsoft Message Analyzer - dialoogvenster voor Azure Storage-verbinding toevoegen](media/storage-monitor-troubleshoot-storage-application/figure3.png)

Nadat u verbonden bent, vouw de containers in de structuur van de opslag te bekijken de logboek-blobs. Selecteer de meest recente aanmelden en klik op **OK**.

![Microsoft Message Analyzer - dialoogvenster voor Azure Storage-verbinding toevoegen](media/storage-monitor-troubleshoot-storage-application/figure4.png)

Op de **nieuwe sessie** dialoogvenster, klikt u op **Start** om uw logboek weer te geven.

Als het logboek wordt geopend, kunt u de opslag-gebeurtenissen bekijken. Als u in de volgende afbeelding zien kunt, er is een `SASClientOtherError` geactiveerd op het opslagaccount. Voor meer informatie over logboekregistratie van opslag, gaat u naar [Opslaganalyse](../common/storage-analytics.md).

![Microsoft Message Analyzer - gebeurtenissen weergeven](media/storage-monitor-troubleshoot-storage-application/figure5.png)

[Opslagverkenner](https://azure.microsoft.com/features/storage-explorer/) is een ander hulpmiddel dat kan worden gebruikt om te communiceren met uw storage-accounts, met inbegrip van de **$logs** container en de logboeken die zijn opgenomen in deze.

## <a name="next-steps"></a>Volgende stappen

In deel vier en het laatste gedeelte van de reeks, hebt u geleerd hoe u kunt controleren en problemen oplossen van uw opslagaccount, zoals het:

> [!div class="checklist"]
> * Inschakelen van logboekregistratie en metrische gegevens
> * Waarschuwingen voor autorisatie fouten inschakelen
> * Verkeer van de test uitgevoerd met onjuiste SAS-tokens
> * Download en analyseren van Logboeken

Volg deze link om te zien van vooraf samengestelde opslag voorbeelden.

> [!div class="nextstepaction"]
> [Voorbeelden van Azure-opslag-script](storage-samples-blobs-cli.md)