---
title: De extensie van de toegang tot Azure-Configuratiescherm probleemoplossing voor IE | Microsoft Docs
description: Het gebruik van Groepsbeleid voor het implementeren van de invoegtoepassing Internet Explorer voor de portal mijn Apps.
services: active-directory
documentationcenter: 
author: MarkusVi
manager: mtillman
editor: 
ms.assetid: f56b3230-26fd-42ec-9e3d-2c12daf15479
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 10/31/2017
ms.author: markvi
ms.reviewer: asteen
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: ff6d0b299c45d89ee8d3a79fa98ce4a542174a5b
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="troubleshooting-the-access-panel-extension-for-internet-explorer"></a>De uitbreiding van het deelvenster toegang tot het oplossen van problemen voor Internet Explorer
In dit artikel helpt u de volgende problemen:

* U kunt geen toegang krijgt tot uw apps via de portal mijn Apps tijdens het gebruik van Internet Explorer.
* U ziet het bericht 'Software installeren' zelfs als u de software al hebt geïnstalleerd.

Als u een beheerder bent, Zie ook: [het implementeren van de uitbreiding van het Configuratiescherm toegang voor Internet Explorer met behulp van Groepsbeleid](active-directory-saas-ie-group-policy.md)

## <a name="run-the-diagnostic-tool"></a>Het diagnostische hulpprogramma uitvoeren
U kunt problemen installatie met de extensie van het Configuratiescherm toegang door te downloaden en uitvoeren van het toegangsvenster diagnostische hulpprogramma:

1. [Klik hier om het diagnostisch hulpprogramma voor downloaden.](https://account.activedirectory.windowsazure.com/applications/AccessPanelExtensionDiagnosticTool/AccessPanelExtensionDiagnosticTool.zip)
2. Open het bestand en druk op **alle uitpakken** knop.
   
    ![Druk op alle uitpakken](./media/active-directory-saas-ie-troubleshooting/extract1.png)
3. Druk op de **extraheren** om door te gaan.
   
    ![Druk op uitpakken](./media/active-directory-saas-ie-troubleshooting/extract2.png)
4. Als u wilt het hulpprogramma uitvoert, met de rechtermuisknop op het bestand met de naam **AccessPanelExtensionDiagnosticTool**, selecteer daarna **openen met > Microsoft Windows Script Host die is gebaseerd**.
   
    ![Open met > Microsoft Windows scripthost gebaseerd](./media/active-directory-saas-ie-troubleshooting/open_tool.png)
5. Vervolgens ziet u de volgende diagnostische venster waarin wordt beschreven wat mis zijn met de installatie.
   
    ![Een voorbeeld van het venster diagnostische](./media/active-directory-saas-ie-troubleshooting/tool_preview.png)
6. Klik op '**Ja**' zodat het programma los de problemen die zijn gevonden.
7. Om deze wijzigingen opslaat, elke Internet Explorer-venster sluiten en open Internet Explorer opnieuw.<br />Als u nog steeds geen toegang uw apps tot, probeert u de volgende stappen uit.

## <a name="check-that-the-access-panel-extension-is-enabled"></a>Controleer of de uitbreiding van het Configuratiescherm toegang is ingeschakeld
Controleren dat de uitbreiding van het Configuratiescherm toegang is ingeschakeld in Internet Explorer:

1. Klik in Internet Explorer op de **Tandwielpictogram pictogram** in de rechterbovenhoek van het venster. Selecteer vervolgens **Internetopties**.<br />(In oudere versies van Internet Explorer kunt u dit onder vinden **Extra > Internetopties**.
   
    ![Ga naar Extra > Internetopties](./media/active-directory-saas-ie-troubleshooting/internetoptions.png)
2. Klik op de **programma's** tabblad en klik vervolgens op de **invoegtoepassingen beheren** knop.
   
    ![Klik op Invoegtoepassingen beheren](./media/active-directory-saas-ie-troubleshooting/internetoptions_programs.png)
3. Selecteer in dit dialoogvenster **Configuratiescherm-uitbreiding voor toegang tot** en klik vervolgens op de **inschakelen** knop.
   
    ![Klik op inschakelen](./media/active-directory-saas-ie-troubleshooting/enableaddon.png)
4. Om deze wijzigingen opslaat, elke Internet Explorer-venster sluiten en open Internet Explorer opnieuw.

## <a name="enable-extensions-for-inprivate-browsing"></a>Uitbreidingen inschakelen voor InPrivate-navigatie
Als u de modus InPrivate-navigatie:

1. Klik in Internet Explorer op de **Tandwielpictogram pictogram** in de rechterbovenhoek van het venster. Selecteer vervolgens **Internetopties**.<br />(In oudere versies van Internet Explorer kunt u dit onder vinden **Extra > Internetopties**.
   
    ![Een voorbeeld van het venster diagnostische](./media/active-directory-saas-ie-troubleshooting/inprivateoptions.png)
2. Ga naar de **Privacy** tabblad vervolgens **schakelt** het selectievakje **werkbalken en extensies uitschakelen wanneer InPrivate-navigatie wordt gestart**</p>
   
    ![Schakel het selectievakje uitschakelen werkbalken en extensies wanneer InPrivate-navigatie wordt gestart](./media/active-directory-saas-ie-troubleshooting/enabletoolbars.png)
3. Om deze wijzigingen opslaat, elke Internet Explorer-venster sluiten en open Internet Explorer opnieuw.

## <a name="uninstall-the-access-panel-extension"></a>De extensie van het Configuratiescherm toegang verwijderen
De extensie Toegangspaneel van uw computer verwijderen:

1. Op het toetsenbord, drukt u op de **Windows-toets** het menu Start openen. Wanneer het menu geopend is, typt u iets als u wilt zoeken. Typ 'Configuratiescherm' en open vervolgens de **Configuratiescherm** wanneer deze wordt weergegeven in de zoekresultaten.
   
    ![Zoeken naar het Configuratiescherm](./media/active-directory-saas-ie-troubleshooting/search_sm.png)
2. Wijzig in de rechterbovenhoek van het Configuratiescherm de **weergeven door** optie naar **grote pictogrammen**. Vervolgens zoeken en klik op de **programma's en onderdelen** knop.
   
    ![Chang de weergave grote pictogrammen weer te geven](./media/active-directory-saas-ie-troubleshooting/control_panel.png)
3. Selecteer in de lijst **Configuratiescherm-uitbreiding voor toegang tot**, en klik op de **verwijderen** knop.
   
    ![Klik op verwijderen](./media/active-directory-saas-ie-troubleshooting/uninstall.png)
4. U kunt vervolgens voor het installeren van de uitbreiding opnieuw uit om te zien als het probleem is opgelost.

Als u de uitbreiding verwijderen problemen ondervindt, kunt u ook verwijderen met behulp van de [Microsoft los It](https://go.microsoft.com/?linkid=9779673) hulpprogramma.

## <a name="related-articles"></a>Verwante artikelen
* [Article Index for Application Management in Azure Active Directory](active-directory-apps-index.md) (Artikelindex voor toepassingsbeheer in Azure Active Directory)
* [Toegang tot toepassingen en eenmalige aanmelding bij Azure Active Directory](active-directory-appssoaccess-whatis.md)
* [De extensie van het Configuratiescherm toegang voor Internet Explorer met behulp van Groepsbeleid implementeren](active-directory-saas-ie-group-policy.md)

