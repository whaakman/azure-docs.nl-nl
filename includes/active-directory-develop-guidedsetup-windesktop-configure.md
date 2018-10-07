---
title: bestand opnemen
description: bestand opnemen
services: active-directory
documentationcenter: dev-center-name
author: jmprieur
manager: mtillman
editor: ''
ms.service: active-directory
ms.devlang: na
ms.topic: include
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/17/2018
ms.author: jmprieur
ms.custom: include file
ms.openlocfilehash: fb521fc9d4927a953cdd66948101969dfc102de4
ms.sourcegitcommit: 6f59cdc679924e7bfa53c25f820d33be242cea28
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/05/2018
ms.locfileid: "48843533"
---
## <a name="register-your-application"></a>Uw toepassing registreren
U kunt uw toepassing registreren in een van de twee manieren.

### <a name="option-1-express-mode"></a>Optie 1: Snelle modus
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
