---
title: bestand opnemen
description: bestand opnemen
services: active-directory
documentationcenter: dev-center-name
author: jmprieur
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.devlang: na
ms.topic: include
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/17/2018
ms.author: jmprieur
ms.custom: include file
ms.openlocfilehash: 220723988f349bf015d2de7633af78782bc03bac
ms.sourcegitcommit: dec7947393fc25c7a8247a35e562362e3600552f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/19/2019
ms.locfileid: "58203182"
---
## <a name="register-your-application"></a>Uw toepassing registreren

U kunt uw toepassing registreren in een van de twee manieren.

### <a name="option-1-express-mode"></a>Optie 1: De Express-modus

U kunt snel uw toepassing registreren met het volgende te doen:
1. Ga naar de [Microsoft-portal voor app-registratie](https://apps.dev.microsoft.com/portal/register-app?appType=mobileAndDesktopApp&appTech=windowsDesktop&step=configure).

2. Selecteer **een app toevoegen**.

3. Typ in het vak **Toepassingsnaam** een naam voor de toepassing.

4. Zorg ervoor dat de **begeleide installatie** selectievakje is geselecteerd en selecteer vervolgens **maken**.

5. Volg de instructies voor het verkrijgen van de toepassings-ID en plak deze in uw code.

### <a name="option-2-advanced-mode"></a>Optie 2: Geavanceerde modus

Ga als volgt te werk om de toepassing te registreren en de registratiegegevens van de toepassing toe te voegen aan uw oplossing:
1. Als u uw toepassing nog niet hebt geregistreerd, gaat u naar de [Portal voor Appregistratie Microsoft](https://apps.dev.microsoft.com/portal/register-app).

2. Selecteer **een app toevoegen**.

3. Typ in het vak **Toepassingsnaam** een naam voor de toepassing.

4. Zorg ervoor dat het selectievakje **Stapsgewijze instelling** is uitgeschakeld en selecteer vervolgens **Maken**.

5. Selecteer **Platform toevoegen**, selecteer **Systeemeigen toepassing** en selecteer vervolgens **Opslaan**.

6. In de **toepassings-ID** vak, kopieert u de GUID.

7. Ga naar Visual Studio, open de *App.xaml.cs* bestand en vervang `your_client_id_here` met toepassings-ID die u zojuist hebt geregistreerd en gekopieerd.

    ```csharp
    private static string ClientId = "your_application_id_here";
    ```
