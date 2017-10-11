---
title: Starten van de taken uitvoeren in Azure-Cloudservices | Microsoft Docs
description: Starten van de taken helpen bij het voorbereiden van uw cloud service-omgeving voor uw app. Dit leert u hoe starten van de taken werken en hoe u ze
services: cloud-services
documentationcenter: 
author: Thraka
manager: timlt
editor: 
ms.assetid: 886939be-4b5b-49cc-9a6e-2172e3c133e9
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/05/2017
ms.author: adegeo
ms.openlocfilehash: 1c1b3aa86dc8211de0c07c9fb68da5685c86f551
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/03/2017
---
# <a name="how-to-configure-and-run-startup-tasks-for-a-cloud-service"></a>Het configureren en starten van de taken voor een cloudservice uitvoeren
Starten van de taken kunt u bewerkingen uitvoeren voordat een rol wordt gestart. Bewerkingen die u wilt uitvoeren bevatten voor het installeren van een onderdeel, registreren van COM-onderdelen, registersleutels instellen of starten van een langdurige proces.

> [!NOTE]
> Starten van de taken zijn niet van toepassing op virtuele Machines, alleen op Cloud Service-Web- en werkrollen.
> 
> 

## <a name="how-startup-tasks-work"></a>De werking van de taken starten
Starten van de taken zijn acties die worden uitgevoerd voordat uw rollen beginnen en zijn gedefinieerd in de [ServiceDefinition.csdef] bestand met behulp van de [taak] element in de [opstarten] element. Starten van de taken zijn vaak batch-bestanden, maar ze kunnen ook worden consoletoepassingen of batch-bestanden die PowerShell-scripts starten.

Omgevingsvariabelen informatie doorgeven in een taak starten en lokale opslag kan worden gebruikt voor het doorgeven van gegevens uit een taak starten. Bijvoorbeeld een omgevingsvariabele kan het pad opgeven naar een programma dat u wilt installeren, en bestanden kunnen worden geschreven naar de lokale opslag, die vervolgens door uw rollen later kan worden gelezen.

Uw taak starten van de informatie en fouten kan vastleggen in de map die is opgegeven door de **TEMP** omgevingsvariabele. Tijdens het starten van de taak de **TEMP** omgevingsvariabele wordt omgezet naar de *C:\\Resources\\temp\\[guid]. [ rolename]\\RoleTemp* directory wanneer u gebruikmaakt van de cloud.

Starten van de taken kunnen ook worden uitgevoerd meerdere keren opnieuw wordt gestart. Bijvoorbeeld, het starten van de taak wordt uitgevoerd telkens wanneer die de functie wordt gerecycled en rol recyclet mogelijk opnieuw opstarten geen altijd opgenomen. Starten van de taken worden op een manier die ze meerdere keren uitvoert zonder problemen kan geschreven.

Starten van de taken moeten eindigen met een **errorlevel** (of afsluitcode) van nul voor het opstartproces te voltooien. Als een taak starten met een niet-nul eindigt **errorlevel**, de rol kan niet worden gestart.

## <a name="role-startup-order"></a>De opstartvolgorde rol
Hieronder vindt u de procedure voor het opstarten van rollen in Azure:

1. Het exemplaar is gemarkeerd als **starten** en ontvangt geen verkeer.
2. Alle starten van de taken worden uitgevoerd volgens hun **taskType** kenmerk.
   
   * De **eenvoudige** taken worden uitgevoerd, synchroon, één voor één.
   * De **achtergrond** en **voorgrond** taken voor de taak starten asynchroon, parallel worden gestart.  
     
     > [!WARNING]
     > IIS mogelijk niet volledig geconfigureerd tijdens de fase van de taak starten in het opstartproces zodat rolspecifieke gegevens mogelijk niet beschikbaar. Starten van de taken waarvoor rolspecifieke gegevens moeten gebruiken [Microsoft.WindowsAzure.ServiceRuntime.RoleEntryPoint.OnStart](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.onstart.aspx).
     > 
     > 
3. Het hostproces van de rol is gestart en de site is gemaakt in IIS.
4. De [Microsoft.WindowsAzure.ServiceRuntime.RoleEntryPoint.OnStart](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.onstart.aspx) methode wordt aangeroepen.
5. Het exemplaar is gemarkeerd als **gereed** en verkeer wordt doorgestuurd naar het exemplaar.
6. De [Microsoft.WindowsAzure.ServiceRuntime.RoleEntryPoint.Run](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleentrypoint.run.aspx) methode wordt aangeroepen.

## <a name="example-of-a-startup-task"></a>Voorbeeld van een taak starten
Starten van de taken zijn gedefinieerd in de [ServiceDefinition.csdef] bestand in de **taak** element. De **commandLine** kenmerk geeft de naam en parameters van de batch-bestand of console startopdracht, de **executionContext** kenmerk geeft u de bevoegdheden van de taak starten en de **taskType** kenmerk geeft aan hoe de taak wordt uitgevoerd.

In dit voorbeeld wordt een omgevingsvariabele **MyVersionNumber**, is gemaakt voor het starten van de taak en wordt ingesteld op de waarde '**1.0.0.0**'.

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
> In Visual Studio de **naar uitvoermap kopiëren** eigenschap voor het starten van de batch-bestand moet worden ingesteld op **kopie altijd** om ervoor te zorgen dat uw batchbestand opstarten goed aan uw project op Azure (wordtgeïmplementeerd**approot\\bin** voor Web-rollen en **approot** voor werkrollen).
> 
> 

## <a name="description-of-task-attributes"></a>Beschrijving van de kenmerken van de taak
De volgende beschrijft de kenmerken van de **taak** -element in de [ServiceDefinition.csdef] bestand:

**commandLine** -Hiermee wordt de opdrachtregel voor de taak starten:

* De opdracht met optionele opdrachtregelparameters, het starten van de taak wordt gestart.
* Dit is vaak de bestandsnaam van een batchbestand .cmd of .bat.
* De taak is ten opzichte van de AppRoot\\Bin-map voor de implementatie. Omgevingsvariabelen worden niet aangevuld bij het bepalen van het pad en de bestandsnaam van de taak. Als uitbreiding van de omgeving vereist is, kunt u een kleine CMD-script waarmee het starten van de taak wordt aangeroepen.
* Kan dit een consoletoepassing of een batchbestand dat begint een [PowerShell-script](cloud-services-startup-tasks-common.md#create-a-powershell-startup-task).

**executionContext** -Hiermee geeft u op het niveau van bevoegdheden voor het starten van de taak. Het niveau van bevoegdheden kan worden beperkt of met verhoogde bevoegdheden:

* **beperkt**  
  Het starten van de taak wordt uitgevoerd met dezelfde bevoegdheden als de rol. Wanneer de **executionContext** kenmerk voor de [Runtime] element is ook **beperkt**, en vervolgens de gebruiker heeft bevoegdheden die worden gebruikt.
* **verhoogde**  
  Het starten van de taak wordt uitgevoerd met administrator-bevoegdheden. Dit kan taken starten van de programma's installeren, IIS-configuratie wijzigen, voert u wijzigingen in het register en andere taken van de administrator-niveau zonder te verhogen van de bevoegdheden van de rol zelf.  

> [!NOTE]
> De bevoegdheden van een taak starten hoeft niet dezelfde zijn als de rol zelf.
> 
> 

**taskType** -geeft aan hoe een starten van de taak wordt uitgevoerd.

* **eenvoudige**  
  Taken worden uitgevoerd, synchroon, één voor één, in de volgorde die is opgegeven in de [ServiceDefinition.csdef] bestand. Wanneer een **eenvoudige** opstarttaak eindigt met een **errorlevel** is aan nul, de volgende **eenvoudige** starten van de taak wordt uitgevoerd. Als er niet meer **eenvoudige** starten van de taken worden uitgevoerd, en vervolgens de rol zelf wordt gestart.   
  
  > [!NOTE]
  > Als de **eenvoudige** taak eindigt met een niet-nul **errorlevel**, het exemplaar wordt geblokkeerd. Daaropvolgende **eenvoudige** starten van de taken en de rol zelf, kunnen niet worden gestart.
  > 
  > 
  
    Om ervoor te zorgen dat uw batchbestand met eindigt een **errorlevel** is aan nul, geef dan de opdracht `EXIT /B 0` aan het einde van uw batch-bestand.
* **achtergrond**  
  Taken worden in combinatie met het starten van de rol van asynchroon uitgevoerd.
* **voorgrond**  
  Taken worden in combinatie met het starten van de rol van asynchroon uitgevoerd. Het belangrijkste verschil tussen een **voorgrond** en een **achtergrond** taak is dat een **voorgrond** taak voorkomt u dat de rol van recycling of afgesloten totdat de taak is beëindigd. De **achtergrond** taken beschikt niet over deze beperking.

## <a name="environment-variables"></a>Omgevingsvariabelen
Omgevingsvariabelen zijn een manier om informatie doorgeven aan een taak starten. Bijvoorbeeld, kunt u het pad naar een blob met een programma te installeren, of poortnummers die door uw rol wordt gebruikt of instellingen voor de functies van de taak starten van de plaatsen.

Er zijn twee soorten van omgevingsvariabelen voor starten van de taken. statische omgevingsvariabelen en omgevingsvariabelen op basis van de leden van de [ RoleEnvironment] klasse. Beide zijn de [omgeving] sectie van de [ServiceDefinition.csdef] bestands- en beide gebruik de [variabele] element en **naam** kenmerk.

Maakt gebruik van statische omgevingsvariabelen de **waarde** kenmerk van de [variabele] element. Het bovenstaande voorbeeld maakt de omgevingsvariabele **MyVersionNumber** die een vaste waarde heeft van '**1.0.0.0**'. Een ander voorbeeld is voor het maken een **StagingOrProduction** omgevingsvariabele die u handmatig op waarden van instellen kunt '**fasering**"of"**productie**' uit te voeren starten van de verschillende acties op basis van de waarde van de **StagingOrProduction** omgevingsvariabele.

Gebruik geen omgevingsvariabelen op basis van de leden van de klasse RoleEnvironment de **waarde** kenmerk van de [variabele] element. In plaats daarvan de [RoleInstanceValue] onderliggend element met de juiste **XPath** kenmerkwaarde, worden gebruikt om het maken van een omgevingsvariabele op basis van een specifiek lid van de [ RoleEnvironment] klasse. Waarden voor de **XPath** kenmerk voor toegang tot verschillende [ RoleEnvironment] waarden kunt u vinden [hier](cloud-services-role-config-xpath.md).

Om bijvoorbeeld te maken van een omgevingsvariabele die is '**true**' wanneer het exemplaar wordt uitgevoerd in de rekenemulator en '**false**' wanneer in de cloud wordt uitgevoerd, gebruikt u de volgende [variabele] en [RoleInstanceValue] elementen:

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
Meer informatie over het uitvoeren van sommige [algemene beheertaken voor opstarten](cloud-services-startup-tasks-common.md) met de Cloudservice.

[Pakket](cloud-services-model-and-package.md) uw Cloudservice.  

[ServiceDefinition.csdef]: cloud-services-model-and-package.md#csdef
[taak]: https://msdn.microsoft.com/library/azure/gg557552.aspx#Task
[opstarten]: https://msdn.microsoft.com/library/azure/gg557552.aspx#Startup
[Runtime]: https://msdn.microsoft.com/library/azure/gg557552.aspx#Runtime
[omgeving]: https://msdn.microsoft.com/library/azure/gg557552.aspx#Environment
[variabele]: https://msdn.microsoft.com/library/azure/gg557552.aspx#Variable
[RoleInstanceValue]: https://msdn.microsoft.com/library/azure/gg557552.aspx#RoleInstanceValue
[ RoleEnvironment]: https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleenvironment.aspx
