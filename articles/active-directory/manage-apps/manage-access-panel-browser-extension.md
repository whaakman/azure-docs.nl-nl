---
title: Problemen met de extensie voor toegang tot Azure-Configuratiescherm voor Internet Explorer | Microsoft Docs
description: Het gebruik van Groepsbeleid voor het implementeren van de Internet Explorer-invoegtoepassing voor de portal mijn Apps.
services: active-directory
documentationcenter: ''
author: msmimart
manager: CelesteDG
ms.service: active-directory
ms.subservice: app-mgmt
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/11/2019
ms.author: mimart
ms.reviewer: asteen
ms.custom: H1Hack27Feb2017
ms.collection: M365-identity-device-management
ms.openlocfilehash: a0269c87572e2a9242a54491103ae0fcc3637518
ms.sourcegitcommit: 0ebc62257be0ab52f524235f8d8ef3353fdaf89e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/10/2019
ms.locfileid: "67723920"
---
# <a name="troubleshoot-the-access-panel-extension-for-internet-explorer"></a>De extensie van het Configuratiescherm toegang voor Internet Explorer oplossen

Dit artikel helpt u de volgende problemen op te lossen:

* U kunt geen toegang krijgt tot uw apps via de portal mijn Apps tijdens het gebruik van Internet Explorer.
* U ziet het bericht 'Software installeren', zelfs als u de software al hebt geïnstalleerd.

Als u een beheerder bent, Zie [over het implementeren van het Configuratiescherm-extensie voor toegang voor Internet Explorer met behulp van Groepsbeleid](deploy-access-panel-browser-extension.md).

## <a name="run-the-diagnostic-tool"></a>Het diagnostisch hulpprogramma uitvoeren

Installatieproblemen met de extensie van het Configuratiescherm toegang kunt u onderzoeken door te downloaden en uitvoeren van het toegangsvenster diagnostische hulpprogramma. 

Downloaden en installeren van het diagnostisch hulpprogramma van:

1. [Selecteer deze koppeling naar het diagnostisch hulpprogramma voor downloaden.](https://account.activedirectory.windowsazure.com/applications/AccessPanelExtensionDiagnosticTool/AccessPanelExtensionDiagnosticTool.zip)
1. Open het bestand en de inhoud uitpakken naar uw computer.
1. Als u wilt het hulpprogramma uitvoert, met de rechtermuisknop op het bestand met de naam *AccessPanelExtensionDiagnosticTool.js* en selecteer **openen met** > **Microsoft Windows Script Host die is gebaseerd** .

    ![Openen met > Microsoft Windows scripthost gebaseerd](./media/manage-access-panel-browser-extension/open-access-panel-extension-diagnostic-tool.png)

1. Bekijk de resultaten van de diagnostische die worden weergegeven en selecteer **Ja** de problemen op te lossen. De **resultaten controleren** in het dialoogvenster wordt weergegeven met informatie over wat er moet gebeuren als de extensie niet werkt.  
1. Lees het bericht en selecteer **OK**.

## <a name="check-that-the-access-panel-extension-is-enabled"></a>Controleer of de uitbreiding van het Configuratiescherm toegang is ingeschakeld

Om te controleren dat u de uitbreiding van het Configuratiescherm toegang in Internet Explorer hebt ingeschakeld:

1. Selecteer in Internet Explorer, de **tandwielpictogram** in de rechterbovenhoek van het venster en selecteer **Internetopties**.
1. Ga naar de **programma's** tabblad en selecteer **invoegtoepassingen beheren**.
1. Selecteer **Configuratiescherm-extensie voor toegang** in de **Microsoft Corporation** sectie en selecteer **inschakelen**.
1. U hebt de wijzigingen opslaan om alle te sluiten van de windows Internet Explorer-browser geopend. De wijziging wordt van kracht zodra die u Internet Explorer te openen.

## <a name="enable-extensions-for-inprivate-browsing"></a>Uitbreidingen inschakelen voor InPrivate-Browsing

Uitbreidingen inschakelen voor InPrivate-Browsing:

1. Selecteer in Internet Explorer, de **tandwielpictogram** in de rechterbovenhoek van het venster en selecteer **Internetopties**.
1. Ga naar de **Privacy** tabblad en controleer de **werkbalken en extensies uitschakelen wanneer InPrivate-Browsing Start** selectievakje is uitgeschakeld.
1. U hebt de wijzigingen opslaan om alle te sluiten van de windows Internet Explorer-browser geopend. De wijziging wordt van kracht zodra die u Internet Explorer te openen.

## <a name="uninstall-the-access-panel-extension"></a>De extensie van het Configuratiescherm toegang verwijderen

De extensie van het Configuratiescherm toegang vanaf uw computer verwijderen:

1. Zoek in het Configuratiescherm, *verwijderen*.
1. Selecteer in de lijst met zoekresultaten **een programma verwijderen**.

    ![Selecteer het verwijderen van een programma-optie in het Configuratiescherm](./media/manage-access-panel-browser-extension/uninstall-program-control-panel.png)

1. Selecteer in de lijst **Configuratiescherm-extensie voor toegang** en selecteer **verwijderen**.

    ![De extensie van het Configuratiescherm toegang verwijderen](./media/manage-access-panel-browser-extension/uninstall-access-panel-extension.png)

1. U kunt vervolgens voor het installeren van de extensie opnieuw uit om te zien als het probleem is opgelost.

Als u problemen met de extensie verwijderen ondervindt, kunt u ook verwijderen met behulp van de [Microsoft Fix It](https://go.microsoft.com/?linkid=9779673) hulpprogramma.

## <a name="related-articles"></a>Verwante artikelen:

* [Toegang tot toepassingen en eenmalige aanmelding met Azure Active Directory](what-is-single-sign-on.md)
* [Over het implementeren van het Configuratiescherm-extensie voor toegang voor Internet Explorer met behulp van Groepsbeleid](deploy-access-panel-browser-extension.md)
