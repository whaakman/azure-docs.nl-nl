---
title: Het gebruik van de Engagement API op Universal Windows
description: Het gebruik van de Engagement API op Universal Windows
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: bb501fca-9cfe-4495-81df-b5efd6e0137b
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-windows-store
ms.devlang: dotnet
ms.topic: article
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: 75fc134a5535e6113331470cf61df9c06eb8e2ab
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-use-the-engagement-api-on-windows-universal"></a>Het gebruik van de Engagement API op Universal Windows
Dit document is een invoegtoepassing voor het document [hoe integreren Engagement voor universele Windows-](mobile-engagement-windows-store-integrate-engagement.md): biedt diepte meer informatie over het gebruik van de Engagement-API voor het rapporteren van de toepassingsstatistieken van uw.

Houd er rekening mee dat als u alleen Engagement voor het rapporteren van uw toepassing sessies, activiteiten, crashes en technische informatie wilt, klikt u vervolgens de eenvoudigste manier is om alle uw `Page` onderliggende klassen overnemen van de `EngagementPage` klasse.

Als u wilt meer doen, bijvoorbeeld als u wilt rapporteren toepassing specifieke gebeurtenissen, fouten en taken, of als u hebt voor het rapporteren van activiteiten van uw toepassing op een andere manier dan de geïmplementeerd in de `EngagementPage` klassen, moet u de Engagement-API gebruiken.

De Engagement-API wordt geleverd door de `EngagementAgent` klasse. U kunt toegang tot deze methoden via `EngagementAgent.Instance`.

Zelfs als de agent-module is niet geïnitialiseerd, wordt elke aanroep van de API wordt uitgesteld en weer worden uitgevoerd wanneer de agent beschikbaar is.

## <a name="engagement-concepts"></a>Engagement-concepten
De volgende onderdelen verfijnen de algemene [Mobile Engagement-concepten](mobile-engagement-concepts.md) voor het Universal Windows platform.

### <a name="session-and-activity"></a>`Session` en `Activity`
Een *activiteit* wordt meestal gekoppeld aan één pagina van de toepassing, dat wil zeggen de *activiteit* wordt gestart wanneer de pagina wordt weergegeven en stopt wanneer de pagina is gesloten: dit is het geval wanneer de Engagement SDK is geïntegreerd met behulp van de `EngagementPage` klasse.

Maar *activiteiten* kunnen ook handmatig met behulp van de Engagement-API worden beheerd. Hiermee kunt u het scheiden van een bepaalde pagina in verschillende sub-onderdelen voor meer informatie over het gebruik van deze pagina (bijvoorbeeld weten hoe vaak en hoe lang dialoogvensters worden gebruikt binnen deze pagina).

## <a name="reporting-activities"></a>Rapportage
### <a name="user-starts-a-new-activity"></a>Gebruiker start een nieuwe activiteit
#### <a name="reference"></a>Naslaginformatie
            void StartActivity(string name, Dictionary<object, object> extras = null)

U moet aan te roepen `StartActivity()` elke keer dat de wijzigingen van de gebruiker-activiteit. De eerste aanroep van deze functie wordt een nieuwe gebruikerssessie gestart.

> [!IMPORTANT]
> De SDK worden automatisch de EndActivity-methode aangeroepen wanneer de toepassing wordt gesloten. Dus is het raadzaam de StartActivity-methode niet aanroepen wanneer de activiteit van de wijzigingen van de gebruiker en op nooit de EndActivity-methode niet aanroepen omdat de huidige sessie moet worden beëindigd zorgt ervoor dat u deze methode aanroept.
> 
> 

#### <a name="example"></a>Voorbeeld
            EngagementAgent.Instance.StartActivity("main", new Dictionary<object, object>() {{"example", "data"}});

### <a name="user-ends-his-current-activity"></a>Gebruiker eindigt zijn huidige activiteit
#### <a name="reference"></a>Naslaginformatie
            void EndActivity()

Hiermee wordt de activiteit en de sessie beëindigd. U moet deze methode niet aanroepen, tenzij u zeker weet wat u doet.

#### <a name="example"></a>Voorbeeld
            EngagementAgent.Instance.EndActivity();

## <a name="reporting-jobs"></a>Rapportage van taken
### <a name="start-a-job"></a>Een taak starten
#### <a name="reference"></a>Naslaginformatie
            void StartJob(string name, Dictionary<object, object> extras = null)

De taak kunt u sommige taken gedurende een periode bijhouden.

#### <a name="example"></a>Voorbeeld
            // An upload begins...

            // Set the extras
            var extras = new Dictionary<object, object>();
            extras.Add("title", "avatar");
            extras.Add("type", "image");

            EngagementAgent.Instance.StartJob("uploadData", extras);

### <a name="end-a-job"></a>Een taak beëindigen
#### <a name="reference"></a>Naslaginformatie
            void EndJob(string name)

Zodra een taak die wordt gevolgd door een taak is beëindigd, moet u de methode EndJob aanroepen voor deze taak door de taaknaam opgeven.

#### <a name="example"></a>Voorbeeld
            // In the previous section, we started an upload tracking with a job
            // Then, the upload ends

            EngagementAgent.Instance.EndJob("uploadData");

## <a name="reporting-events"></a>Melden van gebeurtenissen
Er is drie soorten gebeurtenissen:

* Zelfstandige gebeurtenissen
* Sessiegebeurtenissen
* Gebeurtenissen van de taak

### <a name="standalone-events"></a>Zelfstandige gebeurtenissen
#### <a name="reference"></a>Naslaginformatie
            void SendEvent(string name, Dictionary<object, object> extras = null)

Er kunnen zelfstandige gebeurtenissen plaatsvinden buiten de context van een sessie.

#### <a name="example"></a>Voorbeeld
            EngagementAgent.Instance.SendEvent("event", extra);

### <a name="session-events"></a>Sessiegebeurtenissen
#### <a name="reference"></a>Naslaginformatie
            void SendSessionEvent(string name, Dictionary<object, object> extras = null)

Sessiegebeurtenissen worden meestal gebruikt voor het rapporteren van de acties die worden uitgevoerd door een gebruiker tijdens de sessie.

#### <a name="example"></a>Voorbeeld
**Zonder gegevens:**

            EngagementAgent.Instance.SendSessionEvent("sessionEvent");

            // or

            EngagementAgent.Instance.SendSessionEvent("sessionEvent", null);

**Met gegevens:**

            Dictionary<object, object> extras = new Dictionary<object,object>();
            extras.Add("name", "data");
            EngagementAgent.Instance.SendSessionEvent("sessionEvent", extras);

### <a name="job-events"></a>Taakgebeurtenissen
#### <a name="reference"></a>Naslaginformatie
            void SendJobEvent(string eventName, string jobName, Dictionary<object, object> extras = null)

Taakgebeurtenissen worden meestal gebruikt voor het rapporteren van de acties die door een gebruiker tijdens een taak wordt uitgevoerd.

#### <a name="example"></a>Voorbeeld
            EngagementAgent.Instance.SendJobEvent("eventName", "jobName", extras);

## <a name="reporting-errors"></a>Rapportage van fouten
Er zijn drie soorten fouten:

* Zelfstandige fouten
* Sessie-fouten
* Taakfouten

### <a name="standalone-errors"></a>Zelfstandige fouten
#### <a name="reference"></a>Naslaginformatie
            void SendError(string name, Dictionary<object, object> extras = null)

Zelfstandige fouten kunnen strijd sessie-fouten optreden buiten de context van een sessie.

#### <a name="example"></a>Voorbeeld
            EngagementAgent.Instance.SendError("errorName", extras);

### <a name="session-errors"></a>Sessie-fouten
#### <a name="reference"></a>Naslaginformatie
            void SendSessionError(string name, Dictionary<object, object> extras = null)

Sessie-fouten worden gewoonlijk gebruikt voor het rapporteren van de fouten die invloed hebben op de gebruiker tijdens de sessie.

#### <a name="example"></a>Voorbeeld
            EngagementAgent.Instance.SendSessionError("errorName", extra);

### <a name="job-errors"></a>Taakfouten
#### <a name="reference"></a>Naslaginformatie
            void SendJobError(string errorName, string jobName, Dictionary<object, object> extras = null)

Fouten kunnen zijn gerelateerd aan een actieve taak in plaats van te wijten aan de huidige gebruikerssessie.

#### <a name="example"></a>Voorbeeld
            EngagementAgent.Instance.SendJobError("errorName", "jobname", extra);

## <a name="reporting-crashes"></a>Reporting Crashes
De agent biedt twee methoden om te gaan met crashes.

### <a name="send-an-exception"></a>Een uitzondering verzenden
#### <a name="reference"></a>Naslaginformatie
            void SendCrash(Exception e, bool terminateSession = false)

#### <a name="example"></a>Voorbeeld
U kunt een uitzondering op elk gewenst moment kunt verzenden door aan te roepen:

            EngagementAgent.Instance.SendCrash(aCatchedException);

U kunt ook een optionele parameter gebruiken om de sessie engagement op hetzelfde moment dan het verzenden van de crash te beëindigen. Bellen om dit te doen:

            EngagementAgent.Instance.SendCrash(new Exception("example"), terminateSession: true);

Als u dat doet, wordt de sessie en taken worden gesloten na het verzenden van de crash.

### <a name="send-an-unhandled-exception"></a>Verzenden van een onverwerkte uitzondering
#### <a name="reference"></a>Naslaginformatie
            void SendCrash(Exception e)

Engagement biedt ook een methode voor het verzenden van niet-verwerkte uitzonderingen hebt u **UITGESCHAKELDE** Engagement automatisch **crash** reporting. Dit is vooral nuttig wanneer gebruikt in de toepassing UnhandledException gebeurtenis-handler.

Deze methode wordt **altijd** de engagement-sessie en taken te beëindigen nadat deze is aangeroepen.

#### <a name="example"></a>Voorbeeld
U kunt deze gebruiken voor het implementeren van uw eigen UnhandledExceptionEventArgs-handler. Bijvoorbeeld, voeg de `Current_UnhandledException` methode van de `App.xaml.cs` bestand:

            // In your App.xaml.cs file

            // Code to execute on Unhandled Exceptions
            void Current_UnhandledException(object sender, UnhandledExceptionEventArgs e)
            {
               EngagementAgent.Instance.SendCrash(e.Exception,false);
            }

Voeg in App.xaml.cs in "Openbare App() {}" toe:

            Application.Current.UnhandledException += Current_UnhandledException;

## <a name="device-id"></a>Apparaat-Id
            String EngagementAgent.Instance.GetDeviceId()

U kunt de engagement apparaat-id ophalen door deze methode aanroept.

## <a name="extras-parameters"></a>Parameters voor extra 's
Willekeurige gegevens kunnen worden gekoppeld aan een gebeurtenis, een fout, een activiteit of een taak. Deze gegevens kunnen worden onderverdeeld met behulp van een woordenboek. Sleutels en waarden kunnen van elk type zijn.

Extra gegevens worden geserialiseerd, dus als u wilt uw eigen type invoegen in extra's u moet een gegevenscontract voor dit type toevoegen.

### <a name="example"></a>Voorbeeld
We maken een nieuwe klasse 'Persoon'.

            using System.Runtime.Serialization;

            namespace Microsoft.Azure.Engagement
            {
              [DataContract]
              public class Person
              {
                public Person(string name, int age)
                {
                  Age = age;
                  Name = name;
                }

                // Properties

                [DataMember]
                public int Age
                {
                  get;
                  set;
                }

                [DataMember]
                public string Name
                {
                  get;
                  set; 
                }
              }
            }

Vervolgens voegen we toe een `Person` een extra exemplaar.

            Person person = new Person("Engagement Haddock", 51);
            var extras = new Dictionary<object, object>();
            extras.Add("people", person);

            EngagementAgent.Instance.SendEvent("Event", extras);

> [!WARNING]
> Als u andere typen objecten plaatst, zorg er dan voor dat hun methode ToString() wordt geïmplementeerd om een menselijke leesbare tekenreeks geretourneerd.
> 
> 

### <a name="limits"></a>Limieten
#### <a name="keys"></a>Sleutels
Elke sleutel in het object moet overeenkomen met de volgende reguliere expressie:

`^[a-zA-Z][a-zA-Z_0-9]*$`

Dit betekent dat sleutels met ten minste één letter beginnen moeten, gevolgd door letters, cijfers of onderstrepingstekens (\_).

#### <a name="size"></a>Grootte
Extra's zijn beperkt tot **1024** tekens per aanroep.

## <a name="reporting-application-information"></a>Rapportage-toepassingsinformatie
### <a name="reference"></a>Naslaginformatie
            void SendAppInfo(Dictionary<object, object> appInfos)

U kunt handmatig bijhouden informatie (of andere toepassing specifieke gegevens) met behulp van de SendAppInfo() functie rapporteren.

Merk op dat deze gegevens stapsgewijs kunnen worden verzonden: alleen de meest recente waarde voor een opgegeven sleutel worden behouden voor een bepaald apparaat. Als gebeurtenis extra's, een woordenlijst gebruiken\<-object, object\> gegevens koppelen.

### <a name="example"></a>Voorbeeld
            Dictionary<object, object> appInfo = new Dictionary<object, object>()
              {
                {"birthdate", "1983-12-07"},
                {"gender", "female"}
              };

            EngagementAgent.Instance.SendAppInfo(appInfo);

### <a name="limits"></a>Limieten
#### <a name="keys"></a>Sleutels
Elke sleutel in het object moet overeenkomen met de volgende reguliere expressie:

`^[a-zA-Z][a-zA-Z_0-9]*$`

Dit betekent dat sleutels met ten minste één letter beginnen moeten, gevolgd door letters, cijfers of onderstrepingstekens (\_).

#### <a name="size"></a>Grootte
Toepassingsinformatie is beperkt tot **1024** tekens per aanroep.

In het vorige voorbeeld is de JSON naar de server verzonden 44 tekens bevatten:

            {"birthdate":"1983-12-07","gender":"female"}

## <a name="logging"></a>Logboekregistratie
### <a name="enable-logging"></a>Logboekregistratie inschakelen
De SDK kan worden geconfigureerd voor het produceren van Logboeken van de test in de IDE-console.
Deze logboeken worden niet standaard geactiveerd. Werk de eigenschap voor het aanpassen van dit `EngagementAgent.Instance.TestLogEnabled` op een van de waarde in de `EngagementTestLogLevel` opsomming, bijvoorbeeld:

            EngagementAgent.Instance.TestLogLevel = EngagementTestLogLevel.Verbose;
            EngagementAgent.Instance.Init();

