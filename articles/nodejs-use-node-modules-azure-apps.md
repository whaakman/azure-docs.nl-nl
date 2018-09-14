---
title: Werken met Node.js-Modules
description: Meer informatie over het werken met Node.js-modules bij het gebruik van Azure App Service of Cloud Services.
services: ''
documentationcenter: nodejs
author: TomArcher
manager: routlaw
editor: ''
ms.assetid: c0e6cd3d-932d-433e-b72d-e513e23b4eb6
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: article
ms.date: 08/17/2016
ms.author: tarcher
ms.openlocfilehash: 045250f0b0f97cbefe05b36f1c8d4480244a172d
ms.sourcegitcommit: e2ea404126bdd990570b4417794d63367a417856
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/14/2018
ms.locfileid: "45575844"
---
# <a name="using-nodejs-modules-with-azure-applications"></a>Node.js-modules gebruiken met Azure-toepassingen
Dit document bevat richtlijnen over het gebruik van Node.js-modules met toepassingen die worden gehost op Azure. Er wordt uitgelegd hoe ervoor te zorgen dat uw toepassing een specifieke versie van een module gebruikt, evenals met behulp van systeemeigen modules met Azure.

Als u al bekend bent met behulp van Node.js-modules, **package.json** en **npm-shrinkwrap.json** bestanden, de volgende informatie geeft een kort overzicht van wat in dit artikel wordt besproken:

* Azure App Service begrijpt **package.json** en **npm-shrinkwrap.json** bestanden en modules op basis van gegevens in deze bestanden kunt installeren.

* Azure Cloud Services wordt verwacht dat alle modules worden geïnstalleerd op de ontwikkelomgeving en de **knooppunt\_modules** directory om te worden opgenomen als onderdeel van het implementatiepakket. Het is mogelijk om in te schakelen van ondersteuning voor het installeren van modules met behulp van **package.json** of **npm-shrinkwrap.json** bestanden op Cloud Services; deze configuratie is echter vereist voor aanpassing van de standaard scripts die worden gebruikt door Cloud Service-projecten. Zie voor een voorbeeld van het configureren van deze omgeving [Azure opstarttaak npm-installatie om te voorkomen dat de implementatie van node-modules uitvoeren](https://github.com/woloski/nodeonazure-blog/blob/master/articles/startup-task-to-run-npm-in-azure.markdown)

> [!NOTE]
> Virtuele Machines van Azure worden niet in dit artikel wordt besproken, als de implementatie-ervaring in een virtuele machine afhankelijk van het besturingssysteem die wordt gehost door de virtuele Machine is.
> 
> 

## <a name="nodejs-modules"></a>Node.js-Modules
-Modules zijn geladen JavaScript-pakketten die specifieke functionaliteit voor uw toepassing bieden. Modules worden meestal geïnstalleerd met behulp van de **npm** opdrachtregel-hulpprogramma, maar sommige modules (zoals de HTTP-module) worden geleverd als onderdeel van de core Node.js-pakket.

Wanneer de modules zijn geïnstalleerd, worden opgeslagen in de **knooppunt\_modules** Active directory in de hoofdmap van de directory-structuur van uw toepassing. Elke module binnen de **knooppunt\_modules** directory onderhoudt een eigen map waarin alle modules die afhankelijk is van, en dit gedrag wordt herhaald voor elke module helemaal naar beneden de afhankelijkheidsketen. Deze omgeving kunt elke module geïnstalleerd om een eigen versievereisten voor de modules die afhankelijk is van, maar dit in zeer grote directory-structuur resulteren kan.

Implementatie van de **knooppunt\_modules** map als onderdeel van uw toepassing neemt de omvang van de implementatie in vergelijking met een **package.json** of  **npm-shrinkwrap.json** bestand; echter het garandeert dat de versies van de modules die worden gebruikt in productie gelijk aan de modules die worden gebruikt in ontwikkeling zijn.

### <a name="native-modules"></a>Systeemeigen Modules
Hoewel de meeste modules zijn alleen tekst zonder opmaak JavaScript-bestanden, zijn sommige modules platformspecifieke binaire afbeeldingen. Deze modules worden gecompileerd tijdens de installatie, meestal met behulp van Python en knooppunt-gyp. Omdat Azure Cloud Services afhankelijk van zijn de **knooppunt\_modules** map wordt geïmplementeerd als onderdeel van de toepassing, een systeemeigen module opgenomen als onderdeel van de geïnstalleerde modules werkt in een cloudservice, zolang deze is geïnstalleerd en het gecompileerde op een Windows-ontwikkelsysteem.

Azure App Service biedt geen ondersteuning voor alle systeemeigen modules en mislukken tijdens het compileren van modules aan de specifieke vereisten. Hoewel sommige populaire modules, zoals MongoDB optionele systeemeigen afhankelijkheden hebben en goed zonder dat ze werken, twee oplossingen in de praktijk bleek successen te behalen met bijna alle systeemeigen modules beschikbaar vandaag:

* Voer **npm installeren** op een Windows-machine met alle systeemeigen van de module vereiste onderdelen zijn geïnstalleerd. Implementeer vervolgens de gemaakte **knooppunt\_modules** map als onderdeel van de toepassing in Azure App Service.

  * Voor het compileren, Controleer of overeenkomende architectuur heeft als uw lokale Node.js-installatie en de versie zo dicht mogelijk bij een gebruikt in Azure (de huidige waarden kunnen worden gecontroleerd op de runtime van eigenschappen **process.arch** en **process.version**).

* Azure App Service kan worden geconfigureerd voor het uitvoeren van aangepaste bash- of shell-scripts tijdens de implementatie, zodat u de mogelijkheid om aangepaste opdrachten uit te voeren en nauwkeurig configureren hoe **npm installeren** wordt uitgevoerd. Zie voor een video waarin wordt getoond hoe het configureren van deze omgeving en [Scripts voor de implementatie van aangepaste Website met Kudu](https://azure.microsoft.com/resources/videos/custom-web-site-deployment-scripts-with-kudu/).

### <a name="using-a-packagejson-file"></a>Met behulp van een package.json-bestand

De **package.json** bestand is een manier om op te geven van de afhankelijkheden op het hoogste niveau is uw toepassing vereist, zodat de hostingplatform installeren kunt de afhankelijkheden, in plaats van dat u hoeft te bevatten de **knooppunt\_ modules** map als onderdeel van de implementatie. Nadat de toepassing is geïmplementeerd, de **npm installeren** opdracht wordt gebruikt voor het parseren van de **package.json** bestands- en installeer alle afhankelijkheden die worden vermeld.

Tijdens het ontwikkelen, kunt u de **--opslaan**, **--opslaan dev**, of **--opslaan-optionele** parameters bij het installeren van modules voor een vermelding voor de module toevoegen aan uw **package.json** automatisch het bestand. Zie voor meer informatie, [npm-installatie](https://docs.npmjs.com/cli/install).

Een mogelijk probleem met de **package.json** bestand is het alleen geeft u de versie voor op het hoogste niveau afhankelijkheden. Elke module geïnstalleerd kan of kan niet opgeven voor de versie van de modules die afhankelijk van is, en dus is het mogelijk dat u een andere afhankelijkheidsketen dan is gebruikt bij de ontwikkeling mogelijk uiteindelijk.

> [!NOTE]
> Bij het implementeren in Azure App Service als uw <b>package.json</b> bestand verwijst naar een systeemeigen module die u ziet mogelijk een vergelijkbaar met het volgende voorbeeld fout bij het publiceren van de toepassing met Git:
> 
> npm ERR! module-name@0.6.0 installeren: 'build knooppunt gyp configureren'
> 
> npm ERR! "cmd"/ c"'build knooppunt gyp configureren' ' is mislukt met 1
> 
> 

### <a name="using-a-npm-shrinkwrapjson-file"></a>Via een npm-shrinkwrap.json-bestand
De **npm-shrinkwrap.json** bestand is een poging om de beperkingen van de module versiebeheer van de **package.json** bestand. Terwijl de **package.json** bestand alleen-versies voor de modules op het hoogste niveau, bevat de **npm-shrinkwrap.json** -bestand bevat de versievereisten voor de volledige module afhankelijkheidsketen.

Wanneer uw toepassing gereed voor productie is, kunt u vereisten voor de versie vergrendelen en maken een **npm-shrinkwrap.json** bestand met behulp van de **npm verpakking** opdracht. Met deze opdracht gebruikt de versies die momenteel zijn geïnstalleerd de **knooppunt\_modules** map, en noteer deze versies naar de **npm-shrinkwrap.json** bestand. Nadat de toepassing is geïmplementeerd voor de hosting-omgeving, de **npm installeren** opdracht wordt gebruikt voor het parseren van de **npm-shrinkwrap.json** bestands- en installeer alle afhankelijkheden die worden vermeld. Zie voor meer informatie, [npm-verpakking](https://docs.npmjs.com/cli/shrinkwrap).

> [!NOTE]
> Bij het implementeren in Azure App Service als uw <b>npm-shrinkwrap.json</b> bestand verwijst naar een systeemeigen module die u ziet mogelijk een vergelijkbaar met het volgende voorbeeld fout bij het publiceren van de toepassing met Git:
> 
> npm ERR! module-name@0.6.0 installeren: 'build knooppunt gyp configureren'
> 
> npm ERR! "cmd"/ c"'build knooppunt gyp configureren' ' is mislukt met 1
> 
> 

## <a name="next-steps"></a>Volgende stappen
Nu u begrijpt hoe u Node.js-modules gebruiken met Azure, kunt u leren hoe u [opgeven van de Node.js-versie](https://github.com/squillace/staging/blob/master/articles/nodejs-specify-node-version-azure-apps.md), [bouwen en implementeren van een Node.js-web-app](app-service/app-service-web-get-started-nodejs.md), en [over het gebruik van de Azure-opdrachtregelinterface Interface voor Mac en Linux](https://azure.microsoft.com/blog/using-windows-azure-with-the-command-line-tools-for-mac-and-linux/).

Zie het [Node.js Developer Center](/nodejs/azure/) voor meer informatie.

[specify the Node.js version]: nodejs-specify-node-version-azure-apps.md
[How to use the Azure Command-Line Interface for Mac and Linux]:cli-install-nodejs.md
[Custom Website Deployment Scripts with Kudu]: https://channel9.msdn.com/Shows/Azure-Friday/Custom-Web-Site-Deployment-Scripts-with-Kudu-with-David-Ebbo
