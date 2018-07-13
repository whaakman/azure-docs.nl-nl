---
title: Opstarttaken uitvoeren in Azure Cloudservices | Microsoft Docs
description: Opstarttaken kunnen voorbereiden op de cloud service-omgeving voor uw app. Hiermee leert u hoe u de werking van opstarttaken en hoe u ze
services: cloud-services
documentationcenter: ''
author: jpconnock
manager: timlt
editor: ''
ms.assetid: 886939be-4b5b-49cc-9a6e-2172e3c133e9
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/05/2017
ms.author: jeconnoc
ms.openlocfilehash: 6601eba90f3c3644d418ddd0a74746e1a12bcbd3
ms.sourcegitcommit: e0a678acb0dc928e5c5edde3ca04e6854eb05ea6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/13/2018
ms.locfileid: "39007776"
---
# <a name="how-to-configure-and-run-startup-tasks-for-a-cloud-service"></a>Over het configureren en uitvoeren van opstarttaken voor een cloudservice
U kunt opstarttaken gebruiken bewerkingen uit te voeren voordat een rol wordt gestart. Bewerkingen die u wilt uitvoeren zijn onder meer een onderdeel installeren, het registreren van COM-onderdelen, registersleutels instellen of starten van een langlopende proces.

> [!NOTE]
> Opstarttaken zijn niet van toepassing op virtuele Machines, alleen op Cloud Service-Web- en werkrollen.
> 
> 

## <a name="how-startup-tasks-work"></a>De werking van opstarttaken
Opstarttaken zijn acties die worden uitgevoerd voordat de rollen starten die zijn gedefinieerd in de [ServiceDefinition.csdef] bestand met behulp van de [taak] element in de [opstarten] element. Opstarttaken zijn vaak batch-bestanden, maar ze kunnen ook worden consoletoepassingen of batch-bestanden die PowerShell-scripts te starten.

Omgevingsvariabelen gegevens doorgeven naar een opstarttaak en lokale opslag kan worden gebruikt om informatie uit een opstarttaak doorgeven. Bijvoorbeeld een omgevingsvariabele kan het pad opgeven naar een programma dat u wilt installeren, en bestanden kunnen worden geschreven naar de lokale opslag, die vervolgens door uw rollen later kan worden gelezen.

De opstarttaak kan zich aanmelden informatie en fouten naar de map die is opgegeven door de **TEMP** omgevingsvariabele. Tijdens de opstarttaak de **TEMP** omgevingsvariabele wordt omgezet naar de *C:\\Resources\\temp\\[guid]. [ rolename]\\RoleTemp* directory wanneer u gebruikmaakt van de cloud.

Opstarttaken kunnen ook verschillende keren worden uitgevoerd tussen het opnieuw opstarten. De opstarttaak wordt bijvoorbeeld uitgevoerd telkens wanneer de rol opnieuw wordt gebruikt, en bij het opnieuw gebruiken van rollen wordt niet altijd opnieuw opgestart. Opstarttaken moeten worden geschreven in een manier waarmee ze meerdere keren zonder problemen uitgevoerd.

Opstarttaken moeten eindigen met een **errorlevel** (of afsluitcode) van nul om het opstartproces te voltooien. Als een opstarttaak met een niet-nul eindigt **errorlevel**, de rol kan niet worden gestart.

## <a name="role-startup-order"></a>Rol opstartvolgorde te wijzigen
Hieronder vindt u de procedure voor het opstarten van rollen in Azure:

1. Het exemplaar is gemarkeerd als **vanaf** en ontvangt geen verkeer.
2. Alle opstarttaken worden uitgevoerd volgens hun **taskType** kenmerk.
   
   * De **eenvoudige** taken worden uitgevoerd, synchroon, één voor één.
   * De **achtergrond** en **voorgrond** taken aan de opstarttaak asynchroon, parallel worden gestart.  
     
     > [!WARNING]
     > IIS mogelijk niet volledig geconfigureerd tijdens de fase van de taak starten in het opstartproces zodat rolspecifieke gegevens mogelijk niet beschikbaar. Opstarttaken waarbij rolspecifieke gegevens moeten gebruiken [Microsoft.WindowsAzure.ServiceRuntime.RoleEntryPoint.OnStart](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.onstart.aspx).
     > 
     > 
3. Het hostproces van de rol wordt gestart en de site wordt gemaakt in IIS.
4. De [Microsoft.WindowsAzure.ServiceRuntime.RoleEntryPoint.OnStart](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.onstart.aspx) methode wordt aangeroepen.
5. Het exemplaar is gemarkeerd als **gereed** en verkeer wordt doorgestuurd naar het exemplaar.
6. De [Microsoft.WindowsAzure.ServiceRuntime.RoleEntryPoint.Run](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) methode wordt aangeroepen.

## <a name="example-of-a-startup-task"></a>Voorbeeld van een opstarttaak
Opstarttaken zijn gedefinieerd in de [ServiceDefinition.csdef] bestand, in de **taak** element. De **commandLine** kenmerk geeft de naam en de parameters van de batch-bestand of console opstartopdracht, de **executionContext** kenmerk Hiermee geeft u het niveau van de bevoegdheden van de taak starten en de **taskType** kenmerk geeft aan hoe de taak wordt uitgevoerd.

In dit voorbeeld wordt een omgevingsvariabele **MyVersionNumber**, is gemaakt voor de opstarttaak en ingesteld op de waarde '**1.0.0.0**'.

**ServiceDefinition.csdef**:

```xml
<Startup>
    <Task commandLine="Startup.cmd" executionContext="limited" taskType="simple" >
        <Environment>
            <Variable name="MyVersionNumber" value="1.0.0.0" />
        </Environment>
    </Task>
</Startup>
```

In het volgende voorbeeld wordt de **Startup.cmd** batchbestand schrijft de regel 'de huidige versie is 1.0.0.0' naar het bestand StartupLog.txt in de map die is opgegeven door de omgevingsvariabele TEMP. De `EXIT /B 0` regel zorgt ervoor dat de opstarttaak met eindigt een **errorlevel** gelijk is aan nul.

```cmd
ECHO The current version is %MyVersionNumber% >> "%TEMP%\StartupLog.txt" 2>&1
EXIT /B 0
```

> [!NOTE]
> In Visual Studio, de **naar uitvoermap kopiëren** eigenschap voor het opstarten van batch-bestand moet worden ingesteld op **kopie altijd** om ervoor te zorgen dat uw starten batch-bestand correct is geïmplementeerd aan uw project op Azure (**approot\\bin** voor webrollen en **approot** voor werkrollen).
> 
> 

## <a name="description-of-task-attributes"></a>Beschrijving van de taakkenmerken
De volgende beschrijft de kenmerken van de **taak** -element in de [ServiceDefinition.csdef] bestand:

**commandLine** -Hiermee geeft u op de opdrachtregel voor de opstarttaak:

* De opdracht, inclusief optionele opdrachtregelparameters, die de opstarttaak begint.
* Dit is vaak de bestandsnaam van een batchbestand cmd of bat.
* De taak is ten opzichte van de AppRoot\\Bin-map voor de implementatie. Omgevingsvariabelen zijn niet uitgebreid bij het bepalen van het pad en de bestandsnaam van de taak. Als de uitbreiding is vereist, kunt u een kleine CMD-script waarmee de opstarttaak wordt aangeroepen.
* Een toepassing of een batchbestand dat begint een [PowerShell-script](cloud-services-startup-tasks-common.md#create-a-powershell-startup-task).

**executionContext** -Hiermee geeft u het niveau van bevoegdheden voor de opstarttaak. Het niveau van bevoegdheden kan worden beperkt of met verhoogde bevoegdheden:

* **Beperkt**  
  De opstarttaak wordt uitgevoerd met de dezelfde bevoegdheden als de rol. Wanneer de **executionContext** kenmerk voor de [Runtime] element is ook **beperkt**, en vervolgens de gebruiker heeft bevoegdheden die worden gebruikt.
* **met verhoogde bevoegdheid**  
  De opstarttaak wordt uitgevoerd met beheerdersbevoegdheden. Hierdoor kan opstarttaken programma's te installeren, IIS-configuratie wijzigen, voert u wijzigingen in het register, en andere taken van de administrator-niveau, zonder dat de bevoegdheden van de rol zelf.  

> [!NOTE]
> De bevoegdheden van een opstarttaak hoeft niet hetzelfde als de rol zelf.
> 
> 

**taskType** -Hiermee geeft u de manier waarop een opstarttaak wordt uitgevoerd.

* **Eenvoudige**  
  Taken worden uitgevoerd, synchroon, één voor één, in de volgorde die is opgegeven in de [ServiceDefinition.csdef] bestand. Wanneer een **eenvoudige** opstarttaak eindigt met een **errorlevel** gelijk is aan nul, de volgende **eenvoudige** opstarttaak wordt uitgevoerd. Als er geen meer **eenvoudige** opstarttaken om uit te voeren, en vervolgens de rol zelf wordt gestart.   
  
  > [!NOTE]
  > Als de **eenvoudige** taak eindigt met een niet-nul **errorlevel**, het exemplaar wordt geblokkeerd. Volgende **eenvoudige** opstarttaken en de rol zelf, kunnen niet worden gestart.
  > 
  > 
  
    Om ervoor te zorgen dat uw batchbestand wordt beëindigd met een **errorlevel** gelijk is aan nul, geeft u de opdracht `EXIT /B 0` aan het einde van uw batch-bestand-proces.
* **Achtergrond**  
  Taken worden asynchroon uitgevoerd in combinatie met het starten van de rol.
* **voorgrond**  
  Taken worden asynchroon uitgevoerd in combinatie met het starten van de rol. Het belangrijkste verschil tussen een **voorgrond** en een **achtergrond** taak is die een **voorgrond** taak wordt voorkomen dat de rol van recycling of wordt uitgeschakeld totdat de taak is beëindigd. De **achtergrond** taken beschikt niet over deze beperking.

## <a name="environment-variables"></a>Omgevingsvariabelen
Omgevingsvariabelen zijn een manier om door te geven informatie op een opstarttaak. Bijvoorbeeld, kunt u het pad naar een blob met een programma te installeren, of poortnummers die uw functie wilt gebruiken of instellingen voor het beheren van functies van uw opstarttaak plaatsen.

Er zijn twee soorten omgevingsvariabelen voor opstarttaken; statische omgevingsvariabelen en omgevingsvariabelen op basis van de leden van de [RoleEnvironment] klasse. Beide zijn de [omgeving] sectie van de [ServiceDefinition.csdef] bestands- en beide gebruiken de [variabele] element en **naam** het kenmerk.

Maakt gebruik van statische omgevingsvariabelen de **waarde** kenmerk van de [variabele] element. Het bovenstaande voorbeeld wordt de omgevingsvariabele **MyVersionNumber** heeft een statische waarde van "**1.0.0.0**'. Een ander voorbeeld zou zijn om te maken van een **StagingOrProduction** omgevingsvariabele die u handmatig met waarden van instellen kunt '**staging**'of'**productie**' om uit te voeren opstarten van de verschillende acties op basis van de waarde van de **StagingOrProduction** omgevingsvariabele.

Omgevingsvariabelen op basis van de leden van de klasse RoleEnvironment gebruik niet de **waarde** kenmerk van de [variabele] element. In plaats daarvan de [RoleInstanceValue] onderliggend element met de juiste **XPath** waarde van het kenmerk, worden gebruikt voor het maken van een omgevingsvariabele op basis van een specifiek lid van de [RoleEnvironment] klasse. Waarden voor de **XPath** kenmerk voor toegang tot verschillende [RoleEnvironment] waarden vindt [hier](cloud-services-role-config-xpath.md).

Bijvoorbeeld, om te maken van een omgevingsvariabele die is '**waar**"wanneer het exemplaar wordt uitgevoerd in de rekenemulator en"**false**"bij het uitvoeren in de cloud, gebruikt u de volgende [Variabele] en [RoleInstanceValue] elementen:

```xml
<Startup>
    <Task commandLine="Startup.cmd" executionContext="limited" taskType="simple">
        <Environment>

            <!-- Create the environment variable that informs the startup task whether it is running
                in the Compute Emulator or in the cloud. "%ComputeEmulatorRunning%"=="true" when
                running in the Compute Emulator, "%ComputeEmulatorRunning%"=="false" when running
                in the cloud. -->

            <Variable name="ComputeEmulatorRunning">
                <RoleInstanceValue xpath="/RoleEnvironment/Deployment/@emulated" />
            </Variable>

        </Environment>
    </Task>
</Startup>
```

## <a name="next-steps"></a>Volgende stappen
Meer informatie over het uitvoeren van sommige [veelvoorkomende opstarttaken](cloud-services-startup-tasks-common.md) met uw Service in de Cloud.

[Pakket](cloud-services-model-and-package.md) uw Cloudservice.  

[ServiceDefinition.csdef]: cloud-services-model-and-package.md#csdef
[Taak]: https://msdn.microsoft.com/library/azure/gg557552.aspx#Task
[Opstarten]: https://msdn.microsoft.com/library/azure/gg557552.aspx#Startup
[Runtime]: https://msdn.microsoft.com/library/azure/gg557552.aspx#Runtime
[Omgeving]: https://msdn.microsoft.com/library/azure/gg557552.aspx#Environment
[Variabele]: https://msdn.microsoft.com/library/azure/gg557552.aspx#Variable
[RoleInstanceValue]: https://msdn.microsoft.com/library/azure/gg557552.aspx#RoleInstanceValue
[RoleEnvironment]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleenvironment.aspx
