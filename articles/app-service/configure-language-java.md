---
title: Configureren van Windows Java-apps - Azure App Service | Microsoft Docs
description: Informatie over het configureren van Java-apps uit te voeren op de standaard Windows-instanties in Azure App Service.
keywords: Azure appservice, web-app, windows, oss, java
services: app-service
author: jasonfreeberg
manager: jeconnock
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: java
ms.topic: article
ms.date: 04/12/2019
ms.author: jafreebe;cephalin
ms.custom: seodec18
ms.openlocfilehash: efeab014c7d92a6c19d516a121fbc4436925be75
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2019
ms.locfileid: "60850985"
---
# <a name="configure-a-windows-java-app-for-azure-app-service"></a>Configureren van een Windows Java-app voor Azure App Service

Azure App Service kunt u snel bouwen, implementeren en schalen van hun Tomcat Java-ontwikkelaars of Java Standard Edition (SE) verpakt in een volledig beheerde service voor het Windows-gebaseerde web-apps. Implementeer toepassingen in Maven-invoegtoepassingen vanaf de opdrachtregel of in een editor, zoals IntelliJ, Eclipse of Visual Studio Code.

Deze handleiding bevat de belangrijkste concepten en instructies voor het Java-ontwikkelaars met behulp van in App Service. Als u Azure App Service nog nooit hebt gebruikt, dient u te lezen via de [Java-quickstart](app-service-web-get-started-java.md) eerste. Algemene vragen over het gebruik van App Service die niet specifiek zijn voor de Java-ontwikkeling worden beantwoord het [Veelgestelde vragen over App Service Windows](faq-configuration-and-management.md).

> [!NOTE]
> Kunt u niet vinden wat u zoekt? Raadpleeg de [OSS Veelgestelde vragen over Windows](faq-configuration-and-management.md) of de [configuratiehandleiding voor Java Linux](containers/app-service-linux-java.md) voor informatie over het implementeren en beveiligen van uw Java-app.

## <a name="configuring-tomcat"></a>Tomcat configureren

Om te bewerken van Tomcat `server.xml` of andere configuratiebestanden, eerst een notitie van uw primaire versie van Tomcat uitvoeren in de portal.

1. De basismap van Tomcat vinden voor uw versie door het uitvoeren van de `env` opdracht. Zoek naar de omgevingsvariabele die met begint `AZURE_TOMCAT`en komt overeen met uw primaire versie. Bijvoorbeeld, `AZURE_TOMCAT85_HOME` verwijst naar de map Tomcat voor Tomcat 8.5.
1. Als u hebt de basismap van Tomcat vastgesteld voor uw versie, kopieert u de configuratiemap op `D:\home`. Bijvoorbeeld, als `AZURE_TOMCAT85_HOME` heeft een waarde van `D:\Program Files (x86)\apache-tomcat-8.5.37`, zou het volledige pad van de gekopieerde configuratiemap `D:\home\tomcat\conf`.

Start ten slotte App Service opnieuw. Uw implementaties moeten gaan `D:\home\site\wwwroot\webapps` net als voorheen.

## <a name="java-runtime-statement-of-support"></a>Ondersteuning voor statusverklaring van Java-runtime

### <a name="jdk-versions-and-maintenance"></a>JDK versies en onderhoud

Azure ondersteunde Java Development Kit (JDK) is [Zulu](https://www.azul.com/downloads/azure-only/zulu/) geleverd via [Azul Systems](https://www.azul.com/).

Belangrijke versieupdates aangeboden via de nieuwe runtime-opties in Azure App Service voor Windows. Klanten hebben bijgewerkt naar deze nieuwere versies van Java door het configureren van de App Service-implementatie en zijn verantwoordelijk voor het testen en ervoor te zorgen dat de grote update aan hun eisen voldoen.

Ondersteunde JDK worden automatisch gevuld op basis van een kwartaal in januari, April, juli en oktober van elk jaar.

### <a name="security-updates"></a>Beveiligingsupdates

Patches en oplossingen voor bekende beveiligingsproblemen wordt uitgebracht zodra deze beschikbaar van Azul Systems. Een beveiligingslek in de 'belangrijke' is gedefinieerd door een basis score van 9.0 of hoger op de [NIST algemene beveiligingsproblemen Scoring-systeem, versie 2](https://nvd.nist.gov/cvss.cfm).

### <a name="deprecation-and-retirement"></a>Afschaffing en buiten gebruik stellen

Als een ondersteunde Java-runtime wordt beëindigd, Azure-ontwikkelaars met behulp van de betrokken runtime krijgt een kennisgeving over afschaffing ten minste zes maanden voordat de runtime is buiten gebruik gesteld.

### <a name="local-development"></a>Lokale ontwikkeling

Ontwikkelaars kunnen de productie-editie van Azul Zulu Enterprise JDK voor lokale ontwikkeling van downloaden [site voor het downloaden van Azul](https://www.azul.com/downloads/azure-only/zulu/).

### <a name="development-support"></a>Ontwikkelingsondersteuning voor

Ondersteuning voor het product voor de [Azure wordt ondersteund Azul Zulu JDK](https://www.azul.com/downloads/azure-only/zulu/) is beschikbaar via Microsoft bij het ontwikkelen voor Azure of [Azure Stack](https://azure.microsoft.com/overview/azure-stack/) met een [gekwalificeerde Azure-ondersteuningsplan](https://azure.microsoft.com/support/plans/).

### <a name="runtime-support"></a>Runtime-ondersteuning

Ontwikkelaars kunnen [opent u een probleem](/azure/azure-supportability/how-to-create-azure-support-request) met de JDK Azul Zulu via ondersteuning voor Azure als ze beschikken over een [gekwalificeerde ondersteuningsplan](https://azure.microsoft.com/support/plans/).

## <a name="next-steps"></a>Volgende stappen

Dit onderwerp vindt u de Java-Runtime-instructie van ondersteuning voor Azure App Service op Windows.

- Voor meer informatie over het hosten van webtoepassingen met Azure App Service gaat [App Service-overzicht](overview.md).
- Zie voor meer informatie over Java op Azure-ontwikkeling [Azure voor Java-ontwikkelaarscentrum](https://docs.microsoft.com/java/azure/?view=azure-java-stable).
