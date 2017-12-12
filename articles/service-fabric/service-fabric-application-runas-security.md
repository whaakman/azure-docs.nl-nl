---
title: Meer informatie over Azure microservices beveiligingsbeleid | Microsoft Docs
description: Een overzicht van hoe u een Service Fabric-toepassing uitgevoerd onder system en lokale beveiligingsaccounts, met inbegrip van het SetupEntry punt waarin een toepassing moet een bevoorrechte actie uitgevoerd voordat deze wordt gestart
services: service-fabric
documentationcenter: .net
author: msfussell
manager: timlt
editor: 
ms.assetid: 4242a1eb-a237-459b-afbf-1e06cfa72732
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/30/2017
ms.author: mfussell
ms.openlocfilehash: b2ff715d8225bd0a9c7f6108f8804cdfa3189cc8
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="configure-security-policies-for-your-application"></a>Beveiligingsbeleid configureren voor uw toepassing
U kunt toepassingen die worden uitgevoerd in het cluster onder verschillende gebruikersaccounts beveiligen met behulp van Azure Service Fabric. Service Fabric helpt ook bij de resources die worden gebruikt door toepassingen op het moment van implementatie onder de gebruikersaccounts--bijvoorbeeld, bestanden, mappen en certificaten beveiligen. Hierdoor waarop toepassingen worden uitgevoerd, zelfs in een gedeelde gehoste omgeving veiliger van elkaar.

Service Fabric-toepassingen worden standaard uitgevoerd onder het account waaronder het Fabric.exe-proces wordt uitgevoerd onder. Service Fabric biedt tevens de mogelijkheid toepassingen uitvoeren in een lokale gebruikersaccount of het lokale systeemaccount gebruikt, is opgegeven in het manifest van de toepassing. Ondersteunde lokaal systeem accounttypen zijn **LocalUser**, **NetworkService**, **LocalService**, en **LocalSystem**.

 Als u Service Fabric op Windows Server in uw datacenter met behulp van het zelfstandige installatieprogramma, kunt u Active Directory-domeinaccounts, met inbegrip van beheerde serviceaccounts voor groepen.

U kunt definiëren en gebruikersgroepen maken, zodat een of meer gebruikers kunnen worden toegevoegd aan elke groep samen worden beheerd. Dit is handig wanneer er meerdere gebruikers voor verschillende toegangspunten en deze moeten bepaalde algemene rechten die beschikbaar op het groepeerniveau van de zijn.

## <a name="configure-the-policy-for-a-service-setup-entry-point"></a>Het beleid voor een toegangspunt voor service-instellingen configureren
Zoals beschreven in [toepassing en service manifesten](service-fabric-application-and-service-manifests.md), het ingangspunt setup **entrypoint**, is een bevoorrechte toegangspunt dat wordt uitgevoerd met dezelfde referenties als Service Fabric (meestal de *NetworkService* account) voordat andere toegangspunt. Het uitvoerbare bestand dat is opgegeven door **EntryPoint** is meestal de ServiceHost langlopende. Dus met een afzonderlijke installatie ingangspunt zo voorkomt u dat de ServiceHost uitvoerbare uitvoeren met hoge bevoegdheden voor langere tijd. Het uitvoerbare bestand dat **EntryPoint** geeft wordt uitgevoerd na **entrypoint** is afgesloten. Het resulterende proces wordt bewaakt en opnieuw opgestart, en opnieuw begint met **entrypoint** ooit wordt beëindigd als of als deze is vastgelopen.

Hier volgt een eenvoudige service manifest voorbeeld waarin de entrypoint en de belangrijkste EntryPoint voor de service.

```xml
<?xml version="1.0" encoding="utf-8" ?>
<ServiceManifest Name="MyServiceManifest" Version="SvcManifestVersion1" xmlns="http://schemas.microsoft.com/2011/01/fabric" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
  <Description>An example service manifest</Description>
  <ServiceTypes>
    <StatelessServiceType ServiceTypeName="MyServiceType" />
  </ServiceTypes>
  <CodePackage Name="Code" Version="1.0.0">
    <SetupEntryPoint>
      <ExeHost>
        <Program>MySetup.bat</Program>
        <WorkingFolder>CodePackage</WorkingFolder>
      </ExeHost>
    </SetupEntryPoint>
    <EntryPoint>
      <ExeHost>
        <Program>MyServiceHost.exe</Program>
      </ExeHost>
    </EntryPoint>
  </CodePackage>
  <ConfigPackage Name="Config" Version="1.0.0" />
</ServiceManifest>
```

### <a name="configure-the-policy-by-using-a-local-account"></a>Het beleid configureren met behulp van een lokaal account
Nadat u de service voor het beginpunt van een installatie hebt geconfigureerd, kunt u de beveiligingsmachtigingen die wordt uitgevoerd onder in het toepassingsmanifest. Het volgende voorbeeld laat zien hoe de service worden uitgevoerd onder gebruikersbevoegdheden administrator-account configureren.

```xml
<?xml version="1.0" encoding="utf-8"?>
<ApplicationManifest xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" ApplicationTypeName="MyApplicationType" ApplicationTypeVersion="1.0.0" xmlns="http://schemas.microsoft.com/2011/01/fabric">
   <ServiceManifestImport>
      <ServiceManifestRef ServiceManifestName="MyServiceTypePkg" ServiceManifestVersion="1.0.0" />
      <ConfigOverrides />
      <Policies>
         <RunAsPolicy CodePackageRef="Code" UserRef="SetupAdminUser" EntryPointType="Setup" />
      </Policies>
   </ServiceManifestImport>
   <Principals>
      <Users>
         <User Name="SetupAdminUser">
            <MemberOf>
               <SystemGroup Name="Administrators" />
            </MemberOf>
         </User>
      </Users>
   </Principals>
</ApplicationManifest>
```

Maak eerst een **Principals** sectie met een gebruikersnaam, zoals SetupAdminUser. Dit betekent dat de gebruiker lid is van de groep Administrators.

Vervolgens onder de **ServiceManifestImport** sectie, het configureren van een beleid om toe te passen deze principal die kan worden **entrypoint**. Dit Service Fabric vertelt dat wanneer de **MySetup.bat** -bestand wordt uitgevoerd, dit moet `RunAs` met administrator-bevoegdheden. Gezien het feit dat u hebt *niet* een beleid toegepast op de belangrijkste toegangspunt, de code in **MyServiceHost.exe** wordt uitgevoerd onder het systeem **NetworkService** account. Dit is het standaardaccount op dat alle toegangspunten van de service worden uitgevoerd.

Nu gaan we het bestand MySetup.bat toevoegen aan de Visual Studio-project voor het testen van de administrator-bevoegdheden. In Visual Studio met de rechtermuisknop op de service-project en voeg een nieuw bestand MySetup.bat aangeroepen.

Controleer vervolgens of het bestand MySetup.bat is opgenomen in het servicepakket. Standaard is dit niet. Selecteer het bestand, met de rechtermuisknop om het ophalen van het contextmenu en kiest u **eigenschappen**. Zorg ervoor dat in het dialoogvenster Eigenschappen **naar uitvoermap kopiëren** is ingesteld op **kopiëren indien nieuwer**. Zie de volgende schermafbeelding.

![Visual Studio CopyToOutput voor batchbestand entrypoint][image1]

Nu open het bestand MySetup.bat en voeg de volgende opdrachten:

```
REM Set a system environment variable. This requires administrator privilege
setx -m TestVariable "MyValue"
echo System TestVariable set to > out.txt
echo %TestVariable% >> out.txt

REM To delete this system variable us
REM REG delete "HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Session Manager\Environment" /v TestVariable /f
```

Vervolgens maken en implementeren van de oplossing voor een lokaal ontwikkelcluster. Nadat de service is gestart, zoals wordt weergegeven in Service Fabric Explorer, kunt u zien dat het bestand MySetup.bat geslaagd op een twee manieren. Open een PowerShell-opdrachtprompt en typ:

```
PS C:\ [Environment]::GetEnvironmentVariable("TestVariable","Machine")
MyValue
```

Noteer de naam van het knooppunt waar de service is geïmplementeerd en 2 is gestart in Service Fabric Explorer--bijvoorbeeld knooppunt. Vervolgens gaat u naar de map application exemplaar werk te zoeken naar de out.txt-bestand waarin de waarde van **TestVariable**. Bijvoorbeeld, als deze service is geïmplementeerd op knooppunt 2, klikt u vervolgens gaat u naar dit pad voor de **MyApplicationType**:

```
C:\SfDevCluster\Data\_App\Node.2\MyApplicationType_App\work\out.txt
```

### <a name="configure-the-policy-by-using-local-system-accounts"></a>Het beleid configureren met behulp van lokaal systeemaccounts
Vaak is het raadzaam het opstartscript uitvoeren met behulp van een lokale systeemaccount in plaats van een administrator-account. Het RunAs-beleid wordt uitgevoerd als een lid van de groep Administrators doorgaans werkt niet goed omdat machines gebruikers toegang Gebruikersaccountbeheer (UAC) is standaard ingeschakeld hebben. In dergelijke gevallen **de aanbeveling is het uitvoeren van de entrypoint als LocalSystem, in plaats van als een lokale gebruiker toegevoegd aan de groep Administrators**. Het volgende voorbeeld ziet u de entrypoint worden uitgevoerd als LocalSystem instellen:

```xml
<?xml version="1.0" encoding="utf-8"?>
<ApplicationManifest xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" ApplicationTypeName="MyApplicationType" ApplicationTypeVersion="1.0.0" xmlns="http://schemas.microsoft.com/2011/01/fabric">
   <ServiceManifestImport>
      <ServiceManifestRef ServiceManifestName="MyServiceTypePkg" ServiceManifestVersion="1.0.0" />
      <ConfigOverrides />
      <Policies>
         <RunAsPolicy CodePackageRef="Code" UserRef="SetupLocalSystem" EntryPointType="Setup" />
      </Policies>
   </ServiceManifestImport>
   <Principals>
      <Users>
         <User Name="SetupLocalSystem" AccountType="LocalSystem" />
      </Users>
   </Principals>
</ApplicationManifest>
```

Voor Linux-clusters, een service of de installatie uit te voeren ingangspunt als **hoofdmap**, kunt u de **AccountType** als **LocalSystem**.

## <a name="start-powershell-commands-from-a-setup-entry-point"></a>PowerShell-opdrachten starten vanuit een toegangspunt setup.
Om PowerShell uit de **entrypoint** punt, u kunt uitvoeren **PowerShell.exe** in een batchbestand dat naar een PowerShell-bestand verwijst. Eerst een PowerShell-bestand bijvoorbeeld aan de service-project toevoegen **MySetup.ps1**. Vergeet niet de *kopiëren indien nieuwer* eigenschap zodat het bestand ook in het servicepakket opgenomen is. Het volgende voorbeeld ziet u een batch voorbeeldbestand die een PowerShell-bestand aangeroepen MySetup.ps1 die Hiermee stelt u een systeemomgevingsvariabele aangeroepen begint **TestVariable**.

MySetup.bat een PowerShell-bestand te starten:

```
powershell.exe -ExecutionPolicy Bypass -Command ".\MySetup.ps1"
```

Voeg de volgende om in te stellen van een systeemomgevingsvariabele in het bestand PowerShell:

```
[Environment]::SetEnvironmentVariable("TestVariable", "MyValue", "Machine")
[Environment]::GetEnvironmentVariable("TestVariable","Machine") > out.txt
```

> [!NOTE]
> Standaard wanneer het batchbestand wordt uitgevoerd, wordt er gezocht op de map application **werken** voor bestanden. In dit geval wanneer MySetup.bat wordt uitgevoerd, willen we het MySetup.ps1-bestand niet vinden in dezelfde map, die de toepassing met deze **codepakket** map. Om deze map wijzigen, stelt u de map:
> 
> 

```xml
<SetupEntryPoint>
    <ExeHost>
    <Program>MySetup.bat</Program>
    <WorkingFolder>CodePackage</WorkingFolder>
    </ExeHost>
</SetupEntryPoint>
```

## <a name="use-console-redirection-for-local-debugging"></a>Console-omleiding gebruiken voor lokale foutopsporing
Soms is het handig om te zien van de console-uitvoer van het uitvoeren van een script voor foutopsporing. Om dit te doen, kunt u een console Omleidingsbeleid, schrijft de uitvoer naar een bestand instellen. De bestandsuitvoer is geschreven naar de map application aangeroepen **logboek** op het knooppunt waar de toepassing wordt geïmplementeerd en uitgevoerd. (Zie waar vind ik dit in het voorgaande voorbeeld).

> [!WARNING]
> Gebruik het beleid voor omleiding console nooit in een toepassing die wordt geïmplementeerd in productie, omdat dit de failover van de toepassing kan beïnvloeden. *Alleen* Gebruik deze optie voor lokale ontwikkeling en foutopsporing.  
> 
> 

Het volgende voorbeeld ziet u de consoleomleiding met een waarde FileRetentionCount instellen:

```xml
<SetupEntryPoint>
    <ExeHost>
    <Program>MySetup.bat</Program>
    <WorkingFolder>CodePackage</WorkingFolder>
    <ConsoleRedirection FileRetentionCount="10"/>
    </ExeHost>
</SetupEntryPoint>
```

Als u nu het bestand MySetup.ps1 schrijven wijzigt een **Echo** uitvoert, en dit wordt geschreven naar het uitvoerbestand voor foutopsporing:

```
Echo "Test console redirection which writes to the application log folder on the node that the application is deployed to"
```

**Nadat u fouten opsporen in uw script, onmiddellijk verwijderen van deze console Omleidingsbeleid**.

## <a name="configure-a-policy-for-service-code-packages"></a>Configureer een beleid voor servicepakketten code
In de voorgaande stappen hebt u gezien hoe u een Run as-beleid toepassen op entrypoint. We bekijken wat dieper in verschillende principals die kunnen worden toegepast als de service-beleid maken.

### <a name="create-local-user-groups"></a>Lokale gebruikersgroepen maken
U kunt definiëren en gebruikersgroepen die toestaat dat een of meer gebruikers worden toegevoegd aan een groep maken. Dit is handig als er meerdere gebruikers voor verschillende toegangspunten en deze moeten bepaalde algemene rechten die beschikbaar op het groepeerniveau van de zijn. Het volgende voorbeeld ziet u een lokale groep genaamd **LocalAdminGroup** die administrator-bevoegdheden heeft. Twee gebruikers, Customer1 en Customer2, worden leden van deze lokale groep gemaakt.

```xml
<Principals>
 <Groups>
   <Group Name="LocalAdminGroup">
     <Membership>
       <SystemGroup Name="Administrators"/>
     </Membership>
   </Group>
 </Groups>
  <Users>
     <User Name="Customer1">
        <MemberOf>
           <Group NameRef="LocalAdminGroup" />
        </MemberOf>
     </User>
    <User Name="Customer2">
      <MemberOf>
        <Group NameRef="LocalAdminGroup" />
      </MemberOf>
    </User>
  </Users>
</Principals>
```

### <a name="create-local-users"></a>Lokale gebruikers maken
U kunt een lokale gebruiker die kan worden gebruikt om te helpen beveiligen van een service in de toepassing maken. Wanneer een **LocalUser** accounttype is opgegeven in de sectie principals van het toepassingsmanifest Service Fabric lokale gebruikersaccounts maakt op computers waarop de toepassing wordt geïmplementeerd. Standaard hoeft deze accounts niet dezelfde naam als die zijn opgegeven in het toepassingsmanifest (bijvoorbeeld Customer3 in het volgende voorbeeld). In plaats daarvan dynamisch worden gegenereerd en willekeurige wachtwoorden hebben.

```xml
<Principals>
  <Users>
     <User Name="Customer3" AccountType="LocalUser" />
  </Users>
</Principals>
```

Als een toepassing vereist dat het gebruikersaccount en wachtwoord niet hetzelfde zijn op alle machines (bijvoorbeeld naar NTLM-verificatie inschakelen), moet het clustermanifest NTLMAuthenticationEnabled ingesteld op true. Het clustermanifest moet ook een NTLMAuthenticationPasswordSecret die wordt gebruikt voor het genereren van hetzelfde wachtwoord alle machines opgeven.

```xml
<Section Name="Hosting">
      <Parameter Name="EndpointProviderEnabled" Value="true"/>
      <Parameter Name="NTLMAuthenticationEnabled" Value="true"/>
      <Parameter Name="NTLMAuthenticationPassworkSecret" Value="******" IsEncrypted="true"/>
 </Section>
```

### <a name="assign-policies-to-the-service-code-packages"></a>Beleid toe te wijzen aan de service code pakketten
De **RunAsPolicy** sectie voor een **ServiceManifestImport** Hiermee geeft u het account van de sectie principals moet worden gebruikt voor het uitvoeren van een codepakket. Code-pakketten uit het servicemanifest wordt gekoppeld met een gebruikersaccount in de sectie principals. U kunt dit opgeven voor de installatie of de belangrijkste toegangspunten, of u kunt opgeven `All` toe te passen op beide. Het volgende voorbeeld ziet u verschillende soorten beleid wordt toegepast:

```xml
<Policies>
<RunAsPolicy CodePackageRef="Code" UserRef="LocalAdmin" EntryPointType="Setup"/>
<RunAsPolicy CodePackageRef="Code" UserRef="Customer3" EntryPointType="Main"/>
</Policies>
```

Als **EntryPointType** niet is opgegeven, de standaardwaarde is ingesteld op EntryPointType = 'Main'. Geven **entrypoint** is vooral nuttig wanneer u wilt uitvoeren van bepaalde installatiebewerkingen hoge privileges met een systeemaccount. De code van de werkelijke kunt uitvoeren onder een account met kleine-bevoegdheden.

### <a name="apply-a-default-policy-to-all-service-code-packages"></a>Een standaardbeleid toepassen op alle pakketten voor service-code
U gebruikt de **DefaultRunAsPolicy** sectie om op te geven van een standaardgebruikersaccount voor alle code pakketten die beschikt niet over een specifieke **RunAsPolicy** gedefinieerd. Als de meeste van de code-pakketten die zijn opgegeven in het servicemanifest van de die wordt gebruikt door een toepassing moet worden uitgevoerd met dezelfde gebruiker, kunt de toepassing alleen een standaardbeleid RunAs met dat gebruikersaccount definiëren. Het volgende voorbeeld geeft op dat als een codepakket geen heeft een **RunAsPolicy** opgegeven, moet het codepakket uitgevoerd onder de **MyDefaultAccount** opgegeven in de sectie principals.

```xml
<Policies>
  <DefaultRunAsPolicy UserRef="MyDefaultAccount"/>
</Policies>
```
### <a name="use-an-active-directory-domain-group-or-user"></a>Gebruik van een gebruiker of groep voor Active Directory-domein
U kunt de service onder de referenties voor een Active Directory-gebruiker of groepsaccount uitvoeren voor een Service Fabric die via het zelfstandige installatieprogramma is geïnstalleerd op Windows Server-exemplaar. Dit is van Active Directory on-premises binnen uw domein en is niet met Azure Active Directory (Azure AD). U kunt andere bronnen in het domein (bijvoorbeeld bestandsshares) die machtigingen hebben gekregen benaderen via een domeingebruiker of -groep.

Het volgende voorbeeld ziet u een Active Directory-gebruiker aangeroepen *testgebruiker* met hun domein wachtwoord dat wordt versleuteld met behulp van een certificaat genoemd *MyCert*. U kunt de `Invoke-ServiceFabricEncryptText` PowerShell-opdracht voor het maken van de geheime gecodeerde tekst. Zie [geheimen in Service Fabric-toepassingen beheren](service-fabric-application-secret-management.md) voor meer informatie.

U moet de persoonlijke sleutel van het certificaat voor ontsleuteling van het wachtwoord voor de lokale computer met behulp van een out-of-band-methode implementeren (in Azure, is dit via Azure Resource Manager). Wanneer het Service Fabric pakket met de service op de machine implementeert, wordt het ontsleutelen van het geheim en verifiëren met Active Directory worden uitgevoerd met deze referenties (samen met de naam van de gebruiker).

```xml
<Principals>
  <Users>
    <User Name="TestUser" AccountType="DomainUser" AccountName="Domain\User" Password="[Put encrypted password here using MyCert certificate]" PasswordEncrypted="true" />
  </Users>
</Principals>
<Policies>
  <DefaultRunAsPolicy UserRef="TestUser" />
  <SecurityAccessPolicies>
    <SecurityAccessPolicy ResourceRef="MyCert" PrincipalRef="TestUser" GrantRights="Full" ResourceType="Certificate" />
  </SecurityAccessPolicies>
</Policies>
<Certificates>
```
### <a name="use-a-group-managed-service-account"></a>Gebruik een groep beheerd serviceaccount.
Voor een instantie van Service Fabric die is geïnstalleerd op Windows Server met behulp van het zelfstandige installatieprogramma, kunt u de service uitvoeren als een groep beheerde serviceaccounts (gMSA). Active Directory wordt on-premises binnen uw domein en is niet met Azure Active Directory (Azure AD). Met behulp van een gMSA is geen wachtwoord of versleutelde wachtwoord opgeslagen in de `Application Manifest`.

Het volgende voorbeeld ziet u het maken van een beheerd serviceaccount voor aangeroepen *svc-Test$*; beheerde serviceaccount implementeren op de clusterknooppunten; en het configureren van de user principal.

##### <a name="prerequisites"></a>Vereisten.
- Het domein moet een KDS-hoofdsleutel.
- Het domein moet op een Windows Server 2012 of hoger functionaliteitsniveau.

##### <a name="example"></a>Voorbeeld
1. Hebt u een active directory-domein-beheerder maakt een groep beheerde service account met behulp van de `New-ADServiceAccount` commandlet en zorg ervoor dat de `PrincipalsAllowedToRetrieveManagedPassword` bevat alle knooppunten van het service fabric. Houd er rekening mee dat `AccountName`, `DnsHostName`, en `ServicePrincipalName` moeten uniek zijn.
```
New-ADServiceAccount -name svc-Test$ -DnsHostName svc-test.contoso.com  -ServicePrincipalNames http/svc-test.contoso.com -PrincipalsAllowedToRetrieveManagedPassword SfNode0$,SfNode1$,SfNode2$,SfNode3$,SfNode4$
```
2. Op alle knooppunten van het service fabric (bijvoorbeeld `SfNode0$,SfNode1$,SfNode2$,SfNode3$,SfNode4$`), installeren en testen van de gMSA.
```
Add-WindowsFeature RSAT-AD-PowerShell
Install-AdServiceAccount svc-Test$
Test-AdServiceAccount svc-Test$
```
3. De User principal configureren en configureer de RunAsPolicy om te verwijzen naar de gebruiker.
```xml
<?xml version="1.0" encoding="utf-8"?>
<ApplicationManifest xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" ApplicationTypeName="MyApplicationType" ApplicationTypeVersion="1.0.0" xmlns="http://schemas.microsoft.com/2011/01/fabric">
   <ServiceManifestImport>
      <ServiceManifestRef ServiceManifestName="MyServiceTypePkg" ServiceManifestVersion="1.0.0" />
      <ConfigOverrides />
      <Policies>
         <RunAsPolicy CodePackageRef="Code" UserRef="DomaingMSA"/>
      </Policies>
   </ServiceManifestImport>
  <Principals>
    <Users>
      <User Name="DomaingMSA" AccountType="ManagedServiceAccount" AccountName="domain\svc-Test$"/>
    </Users>
  </Principals>
</ApplicationManifest>
```

## <a name="assign-a-security-access-policy-for-http-and-https-endpoints"></a>Toewijzen van een beveiligingsbeleid voor toegang voor HTTP en HTTPS-eindpunten
Als u een Run as-beleid op een service toepassen en de servicemanifest eindpunt resources met de HTTP-protocol declareert, moet u een **SecurityAccessPolicy** om ervoor te zorgen dat de poorten die zijn toegewezen aan deze eindpunten correct zijn afgesloten toegangsbeheer voor het RunAs-gebruikersaccount dat de service wordt uitgevoerd onder weergegeven. Anders **http.sys** geen toegang tot de service en ophalen van fouten met aanroepen van de client. Het volgende voorbeeld wordt het account Customer1 naar een eindpunt aangeroepen **EndpointName**, waardoor het volledige toegangsrechten.

```xml
<Policies>
   <RunAsPolicy CodePackageRef="Code" UserRef="Customer1" />
   <!--SecurityAccessPolicy is needed if RunAsPolicy is defined and the Endpoint is http -->
   <SecurityAccessPolicy ResourceRef="EndpointName" PrincipalRef="Customer1" />
</Policies>
```

Voor het HTTPS-eindpunt hebt u ook de naam van het certificaat om terug te keren naar de client aangeeft. U kunt dit doen met behulp van **EndpointBindingPolicy**, met het certificaat dat is gedefinieerd in een sectie certificaten in het toepassingsmanifest.

```xml
<Policies>
   <RunAsPolicy CodePackageRef="Code" UserRef="Customer1" />
  <!--SecurityAccessPolicy is needed if RunAsPolicy is defined and the Endpoint is http -->
   <SecurityAccessPolicy ResourceRef="EndpointName" PrincipalRef="Customer1" />
  <!--EndpointBindingPolicy is needed if the EndpointName is secured with https -->
  <EndpointBindingPolicy EndpointRef="EndpointName" CertificateRef="Cert1" />
</Policies
```
## <a name="upgrading-multiple-applications-with-https-endpoints"></a>Bijwerken van meerdere toepassingen met https-eindpunten
Moet u ervoor dat u geen gebruik van de **dezelfde poort** voor verschillende exemplaren van dezelfde toepassing als u http gebruikt**s**. De reden is dat het certificaat voor een van de exemplaren van een toepassing bijwerken Service Fabric niet mogelijk. Als bijvoorbeeld toepassing 1 of een toepassing 2 beide hun certificaat 1 naar 2 cert upgrade wilt uitvoeren. Wanneer de upgrade gebeurt, Service Fabric mogelijk hebben opgeschoond de registratie van het certificaat 1 bij http.sys Hoewel nog steeds door de andere toepassing wordt gebruikt. Om dit te voorkomen, detecteert Service Fabric dat er al een ander exemplaar van de geregistreerd op de poort met het certificaat (als gevolg van http.sys), en de bewerking is mislukt.

Daarom Service Fabric biedt geen ondersteuning voor twee verschillende services met behulp van een upgrade **dezelfde poort** in exemplaren van een andere toepassing. U niet met andere woorden, hetzelfde certificaat gebruiken op verschillende services op dezelfde poort. Als u een gedeelde-certificaat hebben op dezelfde poort moet, moet u ervoor te zorgen dat de services op andere computers met plaatsingsbeperkingen zijn geplaatst. Of Overweeg het gebruik van dynamische poorten Service Fabric indien mogelijk voor elke service in elk toepassingsexemplaar. 

Als u een upgrade is mislukt met https, een foutbericht verschijnt spreken "Windows HTTP-Server API ondersteunt geen meerdere certificaten voor toepassingen die een poort delen."

## <a name="a-complete-application-manifest-example"></a>Een voorbeeld van een volledige toepassing manifest
Het manifest van de volgende toepassing ziet u een groot aantal verschillende instellingen:

```xml
<?xml version="1.0" encoding="utf-8"?>
<ApplicationManifest xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" ApplicationTypeName="Application3Type" ApplicationTypeVersion="1.0.0" xmlns="http://schemas.microsoft.com/2011/01/fabric">
   <Parameters>
      <Parameter Name="Stateless1_InstanceCount" DefaultValue="-1" />
   </Parameters>
   <ServiceManifestImport>
      <ServiceManifestRef ServiceManifestName="Stateless1Pkg" ServiceManifestVersion="1.0.0" />
      <ConfigOverrides />
      <Policies>
         <RunAsPolicy CodePackageRef="Code" UserRef="Customer1" />
         <RunAsPolicy CodePackageRef="Code" UserRef="LocalAdmin" EntryPointType="Setup" />
        <!--SecurityAccessPolicy is needed if RunAsPolicy is defined and the Endpoint is http -->
         <SecurityAccessPolicy ResourceRef="EndpointName" PrincipalRef="Customer1" />
        <!--EndpointBindingPolicy is needed the EndpointName is secured with https -->
        <EndpointBindingPolicy EndpointRef="EndpointName" CertificateRef="Cert1" />
     </Policies>
   </ServiceManifestImport>
   <DefaultServices>
      <Service Name="Stateless1">
         <StatelessService ServiceTypeName="Stateless1Type" InstanceCount="[Stateless1_InstanceCount]">
            <SingletonPartition />
         </StatelessService>
      </Service>
   </DefaultServices>
   <Principals>
      <Groups>
         <Group Name="LocalAdminGroup">
            <Membership>
               <SystemGroup Name="Administrators" />
            </Membership>
         </Group>
      </Groups>
      <Users>
         <User Name="LocalAdmin">
            <MemberOf>
               <Group NameRef="LocalAdminGroup" />
            </MemberOf>
         </User>
         <!--Customer1 below create a local account that this service runs under -->
         <User Name="Customer1" />
         <User Name="MyDefaultAccount" AccountType="NetworkService" />
      </Users>
   </Principals>
   <Policies>
      <DefaultRunAsPolicy UserRef="LocalAdmin" />
   </Policies>
   <Certificates>
     <EndpointCertificate Name="Cert1" X509FindValue="FF EE E0 TT JJ DD JJ EE EE XX 23 4T 66 "/>
  </Certificates>
</ApplicationManifest>
```


<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Volgende stappen
* [Inzicht in het toepassingsmodel](service-fabric-application-model.md)
* [Bronnen opgeven in een servicemanifest](service-fabric-service-manifest-resources.md)
* [Een toepassing implementeren](service-fabric-deploy-remove-applications.md)

[image1]: ./media/service-fabric-application-runas-security/copy-to-output.png
