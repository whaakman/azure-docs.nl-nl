---
title: Windows java-apps configureren-Azure App Service | Microsoft Docs
description: Meer informatie over het configureren van Java-Apps voor uitvoering op de standaard Windows-exemplaren in Azure App Service.
keywords: Azure app service, Web-app, Windows, OSS, java
services: app-service
author: jasonfreeberg
manager: jeconnock
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: java
ms.topic: article
ms.date: 04/12/2019
ms.author: jafreebe
ms.reviewer: cephalin
ms.custom: seodec18
ms.openlocfilehash: c1ea306d8a6b5c1876ac6a9288820e1592dbfda6
ms.sourcegitcommit: a0b37e18b8823025e64427c26fae9fb7a3fe355a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/25/2019
ms.locfileid: "68498514"
---
# <a name="configure-a-windows-java-app-for-azure-app-service"></a>Een Windows java-app configureren voor Azure App Service

Met Azure App Service kunnen Java-Ontwikkel aars snel hun Tomcat of Java Standard Edition (SE) verpakte webtoepassingen bouwen, implementeren en schalen op een volledig beheerde, op Windows gebaseerde service. Implementeer toepassingen met maven plugins vanaf de opdracht regel of in editors zoals IntelliJ, eclips of Visual Studio code.

Deze hand leiding bevat belang rijke concepten en instructies voor Java-Ontwikkel aars die in App Service worden gebruikt. Als u Azure App Service nog nooit hebt gebruikt, lees dan eerst de [Java-Snelstartgids](app-service-web-get-started-java.md) . Algemene vragen over het gebruik van App Service die niet specifiek zijn voor de Java-ontwikkeling, worden beantwoord in de [Veelgestelde vragen over app service Windows](faq-configuration-and-management.md).

> [!NOTE]
> Kunt u niet vinden wat u zoekt? Raadpleeg de [Veelgestelde vragen over Windows OSS](faq-configuration-and-management.md) of de [configuratie handleiding voor Java Linux](containers/configure-language-java.md) voor informatie over het implementeren en beveiligen van uw Java-app.

## <a name="configuring-tomcat"></a>Tomcat configureren

Als u de Tomcat `server.xml` of andere configuratie bestanden wilt bewerken, moet u eerst een notitie maken van de primaire versie van Tomcat in de portal.

1. Zoek de Tomcat Home Directory voor uw versie door de `env` opdracht uit te voeren. Zoek naar de omgevings variabele die begint `AZURE_TOMCAT`met en overeenkomt met uw primaire versie. Bijvoorbeeld: verwijst `AZURE_TOMCAT85_HOME` naar de Tomcat-map voor tomcat 8,5.
1. Wanneer u de Tomcat-basismap voor uw versie hebt geïdentificeerd, kopieert u de configuratiemap naar `D:\home`. Als `AZURE_TOMCAT85_HOME` er bijvoorbeeld een waarde van `D:\Program Files (x86)\apache-tomcat-8.5.37`is, is `D:\home\apache-tomcat-8.5.37`het nieuwe pad van de gekopieerde map.

Start ten slotte App Service opnieuw. Uw implementaties moeten naar `D:\home\site\wwwroot\webapps` net zo eerder gaan.

## <a name="java-runtime-statement-of-support"></a>Java runtime-instructie van ondersteuning

### <a name="jdk-versions-and-maintenance"></a>JDK-versies en onderhoud

De ondersteunde Java Development Kit (JDK) van Azure wordt [Zulu](https://www.azul.com/downloads/azure-only/zulu/) geboden via [Azul-systemen](https://www.azul.com/).

Primaire versie-updates worden verschaft via nieuwe runtime-opties in Azure App Service voor Windows. Klanten werken bij naar deze nieuwere versies van Java door hun App Service-implementatie te configureren en verantwoordelijk te zijn voor het testen en ervoor te zorgen dat de belang rijke update aan hun behoeften voldoet.

Ondersteunde JDKs worden automatisch op een driemaandelijkse patch uitgevoerd in januari, april, juli en oktober van elk jaar.

### <a name="security-updates"></a>Beveiligingsupdates

Patches en oplossingen voor belang rijke beveiligings problemen worden vrijgegeven zodra deze beschikbaar worden gesteld via Azul-systemen. Een ' belang rijk ' beveiligingslek is gedefinieerd met een basis Score van 9,0 of hoger in het [gemeen schappelijke beveiligings risico van het NIST-systeem versie 2](https://nvd.nist.gov/cvss.cfm).

### <a name="deprecation-and-retirement"></a>Afschaffing en buiten gebruik stellen

Als een ondersteunde Java-runtime wordt ingetrokken, krijgen Azure-ontwikkel aars die de betrokken runtime gebruiken, een afschaffing die ten minste zes maanden duurt voordat de runtime wordt afgetrokken.

### <a name="local-development"></a>Lokale ontwikkeling

Ontwikkel aars kunnen de productie-editie van Azul Zulu Enter prise JDK downloaden voor lokale ontwikkeling vanaf [de download site van Azul](https://www.azul.com/downloads/azure-only/zulu/).

### <a name="development-support"></a>Ontwikkelings ondersteuning

Product ondersteuning voor de door [Azure ondersteunde Azul ZULU jdk](https://www.azul.com/downloads/azure-only/zulu/) is beschikbaar via micro soft bij het ontwikkelen voor azure of [Azure stack](https://azure.microsoft.com/overview/azure-stack/) met een gekwalificeerd ondersteunings [abonnement voor Azure](https://azure.microsoft.com/support/plans/).

### <a name="runtime-support"></a>Runtime-ondersteuning

Ontwikkel aars kunnen [een probleem](/azure/azure-supportability/how-to-create-azure-support-request) met de Azul Zulu JDKs via Azure-ondersteuning openen als ze een ondersteunings [plan](https://azure.microsoft.com/support/plans/)hebben.

## <a name="next-steps"></a>Volgende stappen

Dit onderwerp bevat de Java runtime-instructie voor de ondersteuning van Azure App Service in Windows.

- Zie [app Service Overview](overview.md)voor meer informatie over het hosten van webtoepassingen met Azure app service.
- Zie [Azure voor Java-ontwikkelaars centrum](https://docs.microsoft.com/java/azure/?view=azure-java-stable)voor meer informatie over java in azure Development.
