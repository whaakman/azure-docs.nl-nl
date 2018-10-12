---
title: Een pagina toevoegen aan de oplossing voor externe controle UI - Azure | Microsoft Docs
description: In dit artikel wordt beschreven hoe u voor het toevoegen van een nieuwe pagina in de Remote Monitoring solution accelerator-Webgebruikersinterface.
author: dominicbetts
manager: timlt
ms.author: dobett
ms.service: iot-accelerators
services: iot-accelerators
ms.date: 10/02/2018
ms.topic: conceptual
ms.openlocfilehash: 95830cdffb232e16f9fbae51cfa11fbd18172c3c
ms.sourcegitcommit: 4047b262cf2a1441a7ae82f8ac7a80ec148c40c4
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2018
ms.locfileid: "49094562"
---
# <a name="add-a-custom-page-to-the-remote-monitoring-solution-accelerator-web-ui"></a>Een aangepaste pagina toevoegen aan de Remote Monitoring solution accelerator-Webgebruikersinterface

In dit artikel wordt beschreven hoe u voor het toevoegen van een nieuwe pagina in de Remote Monitoring solution accelerator-Webgebruikersinterface. Dit artikel wordt beschreven:

- Klik hier voor meer informatie over het voorbereiden van een lokale ontwikkelingsomgeving.
- Over het toevoegen van een nieuwe pagina aan de web-UI.

Andere handleidingen uitbreiden in dit scenario voor het toevoegen van meer functies aan de pagina die u toevoegt.

## <a name="prerequisites"></a>Vereisten

Als u wilt de stappen in deze handleiding hebt voltooid, moet u de volgende software is geïnstalleerd op uw lokale ontwikkelcomputer:

- [Git](https://git-scm.com/downloads)
- [Node.js](https://nodejs.org/download/)

## <a name="prepare-a-local-development-environment-for-the-ui"></a>Een lokale ontwikkelomgeving voorbereiden in de gebruikersinterface

De Remote Monitoring solution accelerator UI-code is geïmplementeerd met behulp van de [reageren](https://reactjs.org/) JavaScript-framework. U vindt de broncode in de [webinterface voor externe bewaking](https://github.com/Azure/pcs-remote-monitoring-webui) GitHub-opslagplaats.

Als u wilt maken en testen van wijzigingen in de gebruikersinterface, kunt u deze uitvoeren op uw lokale ontwikkelcomputer. (Optioneel) kunt de lokale kopie koppelen aan een geïmplementeerd exemplaar van de oplossingsversnellers zodat deze kan communiceren met uw apparaten echt of gesimuleerd.

Om voor te bereiden op uw lokale ontwikkelomgeving, gebruikt u Git kloon de [webinterface voor externe bewaking](https://github.com/Azure/pcs-remote-monitoring-webui) opslagplaats naar uw lokale computer:

```cmd/sh
git clone https://github.com/Azure/pcs-remote-monitoring-webui.git
```

## <a name="add-a-page"></a>Een pagina toevoegen

Als u wilt een pagina toevoegen aan de web-UI, moet u de bronbestanden die definiëren van de pagina toevoegen en wijzigen van bestaande bestanden zodat de web-UI op de hoogte van de nieuwe pagina.

### <a name="add-the-new-files-that-define-the-page"></a>De nieuwe bestanden die definiëren van de pagina toevoegen

Aan de slag te gaan, de **src/overzicht/onderdelen/pagina's / basicPage** map bevat vier bestanden die een eenvoudige pagina definiëren:

**basicPage.container.js**

[!code-javascript[Page container source](~/remote-monitoring-webui/src/walkthrough/components/pages/basicPage/basicPage.container.js?name=container "Page container source")]

**basicPage.js**

[!code-javascript[Basic page](~/remote-monitoring-webui/src/walkthrough/components/pages/basicPage/basicPage.js?name=page "Basic page")]

**basicPage.scss**

[!code-javascript[Page styling](~/remote-monitoring-webui/src/walkthrough/components/pages/basicPage/basicPage.scss?name=styles "Page styling")]

**basicPage.test.js**

[!code-javascript[Test code for basic page](~/remote-monitoring-webui/src/walkthrough/components/pages/basicPage/basicPage.test.js?name=test "Test code for basic page")]

Maak een nieuwe map **src/onderdelen/pagina's / voorbeeld** en kopieer deze vier bestanden naar het.

### <a name="add-the-new-page-to-the-web-ui"></a>De nieuwe pagina toevoegen aan de web-UI

Als u wilt toevoegen de nieuwe pagina in de web-UI, moet u de volgende wijzigingen aanbrengen in bestaande bestanden:

1. Toevoegen van de nieuwe pagina container naar de **src/components/pages/index.js** bestand:

    ```js
    export * from './example/basicPage.container';
    ```

1. (Optioneel)  Een SVG-pictogram voor de nieuwe pagina toevoegen. Zie voor meer informatie, [webui/src/utilities/README.md](https://github.com/Azure/pcs-remote-monitoring-webui/blob/master/src/utilities/README.md). U kunt een bestaand SVG-bestand.

1. Naam van de pagina toevoegen aan het bestand vertalingen **public/locales/en/translations.json**. De web UI gebruikt [i18next](https://www.i18next.com/) voor verschillende talen.

    ```json
    "tabs": {
      "basicPage": "Example",
    },
    ```

1. Open de **src/components/app.js** -bestand dat de pagina op het hoogste niveau toepassing definieert. De nieuwe pagina aan de lijst met imports toevoegen:

    ```javascript
    // Page Components
    import  {
      //...
      BasicPageContainer
    } from './pages';
    ```

1. In hetzelfde bestand, de nieuwe pagina toevoegen aan de `pagesConfig` matrix. Stel de `to` adres voor de route, verwijzen naar het SVG-pictogram en de vertalingen eerder toegevoegd en stel de `component` naar de container van de pagina:

    ```js
    const pagesConfig = [
      //...
      {
        to: '/basicpage',
        exact: true,
        svg: svgs.tabs.example,
        labelId: 'tabs.basicPage',
        component: BasicPageContainer
      },
      //...
    ];
    ```

1. Toevoegen van een nieuwe breadcrumbs om de `crumbsConfig` matrix:

    ```js
    const crumbsConfig = [
      //...
      {
        path: '/basicpage', crumbs: [
          { to: '/basicpage', labelId: 'tabs.basicPage' }
        ]
      },
      //...
    ];
    ```

    In dit voorbeeldpagina heeft slechts één breadcrumb, maar enkele pagina's mogelijk meer.

Sla al uw wijzigingen op. U bent gereed voor het uitvoeren van de web-UI met uw nieuwe pagina toegevoegd.

### <a name="test-the-new-page"></a>De nieuwe pagina testen

Ga naar de hoofdmap van de lokale kopie van de opslagplaats bij een opdrachtprompt en voer de volgende opdrachten op de vereiste bibliotheken installeren en lokaal uitvoeren van de web-UI:

```cmd/sh
npm install
npm start
```

De vorige opdracht wordt uitgevoerd de Webinterface lokaal op [ http://localhost:3000/dashboard ](http://localhost:3000/dashboard).

Zonder verbinding met het maken van uw lokale exemplaar van de web-UI met een geïmplementeerd exemplaar van de oplossingsversnellers, kunt u fouten ziet op het dashboard. Deze fouten hebben geen invloed op uw mogelijkheid voor het testen van uw nieuwe pagina.

Nu kunt u de code bewerken terwijl de site lokaal wordt uitgevoerd en ziet de web UI dynamisch bijwerken.

## <a name="optional-connect-to-deployed-instance"></a>[Optioneel] Verbinding maken met geïmplementeerd exemplaar

Desgewenst kunt u uw lokale actieve kopie van de web-UI verbinding maken met de oplossingsverbetering voor externe controle in de cloud:

1. Implementeer een **basic** exemplaar van de solution accelerator met behulp van de **pc's** CLI. Noteer de naam van uw implementatie en de referenties die u hebt opgegeven voor de virtuele machine. Zie voor meer informatie, [implementeren met behulp van de CLI](iot-accelerators-remote-monitoring-deploy-cli.md).

1. Gebruik de Azure-portal of de [az CLI](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest) voor SSH-toegang tot de virtuele machine die als host fungeert voor de microservices in uw oplossing. Bijvoorbeeld:

    ```sh
    az network nsg rule update --name SSH --nsg-name {your solution name}-nsg --resource-group {your solution name} --access Allow
    ```

    U moet alleen SSH-toegang inschakelen tijdens het testen en ontwikkeling. Als u SSH schakelt, [moet u dit opnieuw zo snel mogelijk uitschakelen](../security/azure-security-network-security-best-practices.md).

1. Gebruik de Azure-portal of de [az CLI](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest) om de naam en openbare IP-adres van uw virtuele machine te vinden. Bijvoorbeeld:

    ```sh
    az resource list --resource-group {your solution name} -o table
    az vm list-ip-addresses --name {your vm name from previous command} --resource-group {your solution name} -o table
    ```

1. SSH gebruiken om te verbinden met uw virtuele machine met behulp van het IP-adres uit de vorige stap en de referenties die u hebt opgegeven toen u uitvoerde **pc's** om de oplossing te implementeren.

1. Als u wilt toestaan dat de lokale UX om verbinding te maken, moet u de volgende opdrachten uitvoeren in de bash-shell in de virtuele machine:

    ```sh
    cd /app
    sudo ./start.sh --unsafe
    ```

1. Nadat u ziet de opdracht is voltooid en de website wordt gestart, kunt u de verbinding verbreken van de virtuele machine.

1. In de lokale kopie van de [webinterface voor externe bewaking](https://github.com/Azure/pcs-remote-monitoring-webui) opslagplaats, bewerken de **.env** -bestand naar de URL van uw geïmplementeerde oplossing toevoegen:

    ```config
    NODE_PATH = src/
    REACT_APP_BASE_SERVICE_URL=https://{your solution name}.azurewebsites.net/
    ```

## <a name="next-steps"></a>Volgende stappen

In dit artikel hebt u geleerd over de resources beschikbaar voor het aanpassen van de web-UI in de oplossingsverbetering voor externe controle.

Nu u een pagina hebt gedefinieerd, wordt de volgende stap is het [een aangepaste service toevoegen aan de Remote Monitoring solution accelerator-Webgebruikersinterface](iot-accelerators-remote-monitoring-customize-service.md) waarmee gegevens om weer te geven in de gebruikersinterface worden opgehaald.

Zie voor meer informatie over de oplossingsverbetering voor externe controle [architectuur voor externe controle](iot-accelerators-remote-monitoring-sample-walkthrough.md).
