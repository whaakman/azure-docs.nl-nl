---
title: Werken met Node.js-Modules
description: Informatie over het werken met Node.js-modules bij gebruik van Azure App Service- of Cloudservices.
services: 
documentationcenter: nodejs
author: TomArcher
manager: routlaw
editor: 
ms.assetid: c0e6cd3d-932d-433e-b72d-e513e23b4eb6
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: article
ms.date: 08/17/2016
ms.author: tarcher
ms.openlocfilehash: 76679ea0ff2c1e88d1923488717a245351437165
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="using-nodejs-modules-with-azure-applications"></a>Node.js-modules gebruiken met Azure-toepassingen
Dit document biedt richtlijnen voor het gebruik van Node.js-modules met toepassingen die worden gehost op Azure. Er wordt uitgelegd hoe u ervoor zorgt dat uw toepassing een bepaalde versie van een module gebruikt en dat er systeemeigen modules voor Azure worden gebruikt.

Als u al bekend bent met behulp van Node.js-modules **package.json** en **npm shrinkwrap.json** bestanden, de volgende informatie bevat een kort overzicht van wat in dit artikel wordt beschreven:

* Azure App Service begrijpt **package.json** en **npm shrinkwrap.json** -bestanden en modules op basis van gegevens in deze bestanden kunt installeren.

* Azure Cloud Services verwachten alle modules worden geïnstalleerd op de ontwikkelomgeving, en de **knooppunt\_modules** directory om te worden opgenomen als onderdeel van het implementatiepakket. Het is mogelijk om in te schakelen van ondersteuning voor het installeren van modules met behulp van **package.json** of **npm shrinkwrap.json** bestanden op Cloudservices; deze configuratie is echter vereist voor aanpassing van de standaardscripts die worden gebruikt door de Cloud Service-projecten. Zie voor een voorbeeld van het configureren van deze omgeving [Azure opstarttaak npm installatie om te voorkomen dat knooppunt modules implementeren uitvoeren](https://github.com/woloski/nodeonazure-blog/blob/master/articles/startup-task-to-run-npm-in-azure.markdown)

> [!NOTE]
> Virtuele Machines in Azure worden niet in dit artikel wordt besproken, als de implementatie-ervaring in een virtuele machine afhankelijk van het besturingssysteem dat wordt gehost door de virtuele Machine is.
> 
> 

## <a name="nodejs-modules"></a>Node.js-Modules
Modules zijn geladen JavaScript-pakketten die specifieke functionaliteit voor uw toepassing bieden. Modules worden meestal geïnstalleerd met de **npm** opdrachtregelprogramma hulpprogramma, maar sommige modules (zoals de bewakingsmodule voor http) worden geleverd als onderdeel van de core Node.js-pakket.

Wanneer modules zijn geïnstalleerd, worden opgeslagen in de **knooppunt\_modules** map in de hoofdmap van de directory-structuur van uw toepassing. Elke module binnen de **knooppunt\_modules** directory onderhoudt een eigen **knooppunt\_modules** map waarin zich geen modules die afhankelijk is van en dit gedrag wordt herhaald voor elke module helemaal naar beneden de afhankelijkheidsketen. Deze omgeving kunt elke module geïnstalleerd om een eigen versievereisten voor de modules die afhankelijk is van, maar dit in zeer grote directory-structuur resulteren kan.

Implementatie van de **knooppunt\_modules** map als onderdeel van uw toepassing neemt de grootte van de implementatie in vergelijking met een **package.json** of **npm shrinkwrap.json** bestand; echter, wordt gegarandeerd dat de versies van de modules die wordt gebruikt in productie hetzelfde als de modules die wordt gebruikt in ontwikkeling zijn.

### <a name="native-modules"></a>Systeemeigen Modules
Hoewel de meeste modules gewoon JavaScript tekstbestanden zijn, zijn sommige modules platform-specifieke binaire installatiekopieën. Deze modules worden gecompileerd tijdens de installatie, meestal met behulp van Python en knooppunt gyp. Omdat Azure Cloud Services zijn afhankelijk van de **knooppunt\_modules** map wordt geïmplementeerd als onderdeel van de toepassing, een systeemeigen module opgenomen als onderdeel van de geïnstalleerde modules moet werken in een cloudservice, zolang het is geïnstalleerd en op een Windows-ontwikkelsysteem gecompileerde.

Azure App Service biedt geen ondersteuning voor alle systeemeigen modules en mislukken bij het compileren van modules met specifieke vereisten. Hoewel sommige populaire modules zoals MongoDB optionele systeemeigen afhankelijkheden hebben en zonder deze werken, twee oplossingen bewezen succesvol kunnen werken met bijna alle beschikbare systeemeigen modules vandaag:

* Voer **npm installeren** op een Windows-machine met alle systeemeigen van de module onderdelen zijn geïnstalleerd. Vervolgens implementeert het gemaakte **knooppunt\_modules** map als onderdeel van de toepassing in Azure App Service.

  * Controleer voordat u compileren, dat overeenkomende architectuur heeft als uw lokale Node.js-installatie en de versie zo dicht mogelijk bij een gebruikt in Azure is (de huidige waarden kunnen worden gecontroleerd op de runtime van eigenschappen **process.arch** en **process.version**).

* Azure App Service kan worden geconfigureerd voor het aangepaste bash of shell-scripts uitvoeren tijdens de implementatie, zodat u aangepaste opdrachten uit te voeren en nauwkeurig configureren hoe **npm installeren** wordt uitgevoerd. Zie voor een video waarin wordt getoond hoe deze omgeving configureren en [aangepaste Website Implementatiescripts met Kudu].

### <a name="using-a-packagejson-file"></a>Met behulp van een package.json-bestand

De **package.json** bestand is een manier om op te geven van het hoogste niveau afhankelijkheden uw toepassing vereist zodat de host-platform installeren kunt de afhankelijkheden in plaats van dat u wilt opnemen hoeft de **knooppunt\_ modules** map als onderdeel van de implementatie. Nadat de toepassing is geïmplementeerd, de **npm installeren** opdracht wordt gebruikt voor het parseren van de **package.json** bestands- en de vermelde afhankelijkheden te installeren.

Tijdens de ontwikkeling, kunt u de **--opslaan**, **--opslaan dev**, of **--opslaan optionele** parameters bij het installeren van modules voor het toevoegen van een vermelding voor de module die u wilt uw **package.json** bestand automatisch. Zie voor meer informatie [npm-Installeer](https://docs.npmjs.com/cli/install).

Een mogelijk probleem met de **package.json** bestand is het alleen geeft u de versie voor afhankelijkheden op het hoogste niveau. Elke module geïnstalleerd mogelijk of kan niet opgeven voor de versie van de modules die afhankelijk van is en is het mogelijk dat u met een andere afhankelijkheidsketen dan een gebruikt in ontwikkeling eindigen mogelijk.

> [!NOTE]
> Als bij het implementeren in Azure App Service, uw <b>package.json</b> bestand verwijst naar een systeemeigen module die u mogelijk een foutbericht weergegeven dat vergelijkbaar is met het volgende voorbeeld zien bij het publiceren van de toepassing met Git:
> 
> npm ERR! module-name@0.6.0installeren: 'build knooppunt gyp configureren'
> 
> npm ERR! ' cmd "/ c" 'knooppunt gyp configureren build' ' is mislukt: 1
> 
> 

### <a name="using-a-npm-shrinkwrapjson-file"></a>Met behulp van een bestand npm shrinkwrap.json
De **npm shrinkwrap.json** bestand is een poging om de module versioning beperkingen van de **package.json** bestand. Terwijl de **package.json** bestand bevat alleen versies voor de modules die op het hoogste niveau de **npm shrinkwrap.json** bestand bevat de versievereisten voor de volledige module afhankelijkheidsketen.

Wanneer uw toepassing gereed voor productie is, kunt u versievereisten vergrendelen en maakt een **npm shrinkwrap.json** bestand met behulp van de **npm verpakking** opdracht. Met deze opdracht gebruikt de versies die momenteel geïnstalleerd in de **knooppunt\_modules** map, en noteer deze versies naar de **npm shrinkwrap.json** bestand. Nadat de toepassing is geïmplementeerd voor de hosting-omgeving, de **npm installeren** opdracht wordt gebruikt voor het parseren van de **npm shrinkwrap.json** bestands- en de vermelde afhankelijkheden te installeren. Zie voor meer informatie [npm verpakking](https://docs.npmjs.com/cli/shrinkwrap).

> [!NOTE]
> Als bij het implementeren in Azure App Service, uw <b>npm shrinkwrap.json</b> bestand verwijst naar een systeemeigen module die u mogelijk een foutbericht weergegeven dat vergelijkbaar is met het volgende voorbeeld zien bij het publiceren van de toepassing met Git:
> 
> npm ERR! module-name@0.6.0installeren: 'build knooppunt gyp configureren'
> 
> npm ERR! ' cmd "/ c" 'knooppunt gyp configureren build' ' is mislukt: 1
> 
> 

## <a name="next-steps"></a>Volgende stappen
Nu u weet hoe Node.js-modules gebruiken met Azure, kunt u nagaan hoe [Geef de Node.js-versie], [bouwen en implementeren van een Node.js-web-app](app-service/app-service-web-get-started-nodejs.md), en [het gebruik van de Azure-opdrachtregelinterface voor Mac en Linux].

Zie het [Node.js Developer Center](/nodejs/azure/) voor meer informatie.

[Geef de Node.js-versie]: nodejs-specify-node-version-azure-apps.md
[het gebruik van de Azure-opdrachtregelinterface voor Mac en Linux]:cli-install-nodejs.md
[aangepaste Website Implementatiescripts met Kudu]: https://channel9.msdn.com/Shows/Azure-Friday/Custom-Web-Site-Deployment-Scripts-with-Kudu-with-David-Ebbo
