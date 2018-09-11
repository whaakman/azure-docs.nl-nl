---
title: De extensie voor toegang tot Azure-Configuratiescherm probleemoplossing voor Internet Explorer | Microsoft Docs
description: Het gebruik van Groepsbeleid voor het implementeren van de Internet Explorer-invoegtoepassing voor de portal mijn Apps.
services: active-directory
documentationcenter: ''
author: barbkess
manager: mtillman
ms.service: active-directory
ms.component: app-mgmt
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/30/2018
ms.author: barbkess
ms.reviewer: asteen
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: b13b5a7d5366a0bce97d8e735dbf3822186d21cb
ms.sourcegitcommit: af9cb4c4d9aaa1fbe4901af4fc3e49ef2c4e8d5e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/11/2018
ms.locfileid: "44356750"
---
# <a name="troubleshooting-the-access-panel-extension-for-internet-explorer"></a>Het oplossen van het Configuratiescherm-extensie voor toegang voor Internet Explorer
Dit artikel helpt u de volgende problemen op te lossen:

* U kunt geen toegang krijgt tot uw apps via de portal mijn Apps tijdens het gebruik van Internet Explorer.
* U ziet het bericht 'Software installeren', zelfs als u de software al hebt geïnstalleerd.

Als u een beheerder bent, Zie ook: [over het implementeren van het Configuratiescherm-extensie voor toegang voor Internet Explorer met behulp van Groepsbeleid](deploy-access-panel-browser-extension.md)

## <a name="run-the-diagnostic-tool"></a>Het diagnostisch hulpprogramma uitvoeren
Installatieproblemen met de extensie van het Configuratiescherm toegang kunt u onderzoeken door te downloaden en uitvoeren van het toegangsvenster diagnostische hulpprogramma:

1. [Klik hier om de diagnostische hulpprogramma te downloaden.](https://account.activedirectory.windowsazure.com/applications/AccessPanelExtensionDiagnosticTool/AccessPanelExtensionDiagnosticTool.zip)
2. Open het bestand, en druk op **alle uitpakken** knop.
   
    ![Druk op alle uitpakken](./media/manage-access-panel-browser-extension/extract1.png)
3. Druk vervolgens op de **extraheren** om door te gaan.
   
    ![Druk op extraheren](./media/manage-access-panel-browser-extension/extract2.png)
4. Als u wilt het hulpprogramma uitvoert, met de rechtermuisknop op het bestand met de naam **AccessPanelExtensionDiagnosticTool**en selecteer vervolgens **openen met > Microsoft Windows Script Host die is gebaseerd**.
   
    ![Openen met > Microsoft Windows scripthost gebaseerd](./media/manage-access-panel-browser-extension/open_tool.png)
5. Vervolgens ziet u de volgende diagnostische venster, waarin wordt beschreven wat mis zijn met de installatie.
   
    ![Een voorbeeld van het venster diagnostische](./media/manage-access-panel-browser-extension/tool_preview.png)
6. Klik op '**Ja**"zodat het programma los de problemen die zijn gevonden.
7. Om deze wijzigingen hebt opgeslagen, sluit elke Internet Explorer-venster en opent u Internet Explorer opnieuw.<br />Als u nog steeds geen toegang uw apps tot, probeert u de onderstaande stappen.

## <a name="check-that-the-access-panel-extension-is-enabled"></a>Controleer of de uitbreiding van het Configuratiescherm toegang is ingeschakeld
Om te controleren dat de uitbreiding van het Configuratiescherm toegang is ingeschakeld in Internet Explorer:

1. Klik in Internet Explorer, op de **tandwielpictogram** in de rechterbovenhoek van het venster. Selecteer vervolgens **Internetopties**.<br />(In oudere versies van Internet Explorer kunt u vinden onder **Extra > Internetopties**.
   
    ![Ga naar Extra > Internetopties](./media/manage-access-panel-browser-extension/internetoptions.png)
2. Klik op de **programma's** tabblad en klik vervolgens op de **invoegtoepassingen beheren** knop.
   
    ![Klik op Invoegtoepassingen beheren](./media/manage-access-panel-browser-extension/internetoptions_programs.png)
3. In dit dialoogvenster, selecteer **Configuratiescherm-extensie voor toegang** en klik vervolgens op de **inschakelen** knop.
   
    ![Klik op inschakelen](./media/manage-access-panel-browser-extension/enableaddon.png)
4. Om deze wijzigingen hebt opgeslagen, sluit elke Internet Explorer-venster en opent u Internet Explorer opnieuw.

## <a name="enable-extensions-for-inprivate-browsing"></a>Uitbreidingen inschakelen voor InPrivate-Browsing
Als u de modus InPrivate-Browsing:

1. Klik in Internet Explorer, op de **tandwielpictogram** in de rechterbovenhoek van het venster. Selecteer vervolgens **Internetopties**.<br />(In oudere versies van Internet Explorer kunt u vinden onder **Extra > Internetopties**.
   
    ![Een voorbeeld van het venster diagnostische](./media/manage-access-panel-browser-extension/inprivateoptions.png)
2. Ga naar de **Privacy** tabblad vervolgens **Schakel** het selectievakje **werkbalken en extensies uitschakelen wanneer InPrivate-Browsing wordt gestart**</p>
   
    ![Schakel Disable werkbalken en extensies wanneer InPrivate-Browsing wordt gestart](./media/manage-access-panel-browser-extension/enabletoolbars.png)
3. Om deze wijzigingen hebt opgeslagen, sluit elke Internet Explorer-venster en opent u Internet Explorer opnieuw.

## <a name="uninstall-the-access-panel-extension"></a>De extensie van het Configuratiescherm toegang verwijderen
De extensie Toegangsvenster van uw computer verwijderen:

1. Op het toetsenbord, drukt u op de **Windows-toets** naar het menu Start openen. Wanneer het menu geopend is, kunt u alles wat u doet een zoekopdracht typen. Typ 'Configuratiescherm' en open vervolgens de **Configuratiescherm** wanneer deze wordt weergegeven in de lijst met zoekresultaten.
   
    ![Zoeken naar het Configuratiescherm](./media/manage-access-panel-browser-extension/search_sm.png)
2. Wijzig in de rechterbovenhoek van het Configuratiescherm van de **weergeven door** optie naar **grote pictogrammen**. Vervolgens zoeken en klik op de **programma's en onderdelen** knop.
   
    ![De weergave Chang grote pictogrammen weer te geven](./media/manage-access-panel-browser-extension/control_panel.png)
3. Selecteer in de lijst **Configuratiescherm-extensie voor toegang**, en klik op de **verwijderen** knop.
   
    ![Klik op verwijderen](./media/manage-access-panel-browser-extension/uninstall.png)
4. U kunt vervolgens voor het installeren van de extensie opnieuw uit om te zien als het probleem is opgelost.

Als er problemen zijn met de extensie verwijderen is, kunt u ook verwijderen met behulp van de [Microsoft Fix It](https://go.microsoft.com/?linkid=9779673) hulpprogramma.

## <a name="related-articles"></a>Gerelateerde artikelen
* [Article Index for Application Management in Azure Active Directory](../active-directory-apps-index.md) (Artikelindex voor toepassingsbeheer in Azure Active Directory)
* [Toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](what-is-single-sign-on.md)
* [Over het implementeren van het Configuratiescherm-extensie voor toegang voor Internet Explorer met behulp van Groepsbeleid](deploy-access-panel-browser-extension.md)

