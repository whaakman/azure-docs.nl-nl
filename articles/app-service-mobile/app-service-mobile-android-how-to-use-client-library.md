---
title: Het gebruik van de Azure Mobile Apps SDK voor Android | Microsoft Docs
description: Het gebruik van de Azure Mobile Apps SDK voor Android
services: app-service\mobile
documentationcenter: android
author: conceptdev
manager: crdun
ms.assetid: 5352d1e4-7685-4a11-aaf4-10bd2fa9f9fc
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-android
ms.devlang: java
ms.topic: article
ms.date: 11/16/2017
ms.author: crdun
ms.openlocfilehash: f04f3fc7d2ff2e01baa78571b2ba267f8e4905c6
ms.sourcegitcommit: c4cc4d76932b059f8c2657081577412e8f405478
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/11/2018
---
# <a name="how-to-use-the-azure-mobile-apps-sdk-for-android"></a>Het gebruik van de Azure Mobile Apps SDK voor Android

Deze handleiding wordt beschreven hoe u de Android SDK voor Mobile Apps-client gebruiken voor het implementeren van algemene scenario's, zoals:

* Opvragen van gegevens (invoegen, bijwerken en verwijderen).
* -Verificatie.
* Foutafhandeling.
* Aanpassing van de client.

Deze handleiding is gericht op de client-side Android SDK.  Zie voor meer informatie over de serverzijde SDK's voor mobiele Apps, [werken met .NET back-end SDK] [ 10] of [het gebruik van de SDK back-end Node.js][11].

## <a name="reference-documentation"></a>Naslagdocumentatie

U vindt de [Javadocs API-referentiemateriaal] [ 12] voor de Android-clientbibliotheek op GitHub.

## <a name="supported-platforms"></a>Ondersteunde Platforms

De Azure Mobile Apps SDK voor Android ondersteunt API niveaus 19 tot en met 24 (KitKat via deze) voor de telefoon en tablet vormfactoren.  Verificatie, met name maakt gebruik van een benadering van de algemene framework voor het verzamelen van referenties.  Server-flow-verificatie werkt niet met kleine formulier factor apparaten zoals kijkt naar.

## <a name="setup-and-prerequisites"></a>Het installatieprogramma en vereisten

Voltooi de [Mobile Apps Quick Start](app-service-mobile-android-get-started.md) zelfstudie.  Deze taak zorgt ervoor dat alle vereisten voor het ontwikkelen van Azure Mobile Apps is voldaan.  De Quick Start helpt u bij het configureren van uw account en uw eerste back-end voor mobiele apps te maken.

Als u besluit niet op de Quick Start-zelfstudie hebt voltooid, kunt u de volgende taken uitvoeren:

* [Maak een back-end voor de mobiele App] [ 13] voor gebruik met uw Android-app.
* In Android Studio [update de Gradle bouwen bestanden](#gradle-build).
* [Internet-machtiging wordt ingeschakeld](#enable-internet).

### <a name="gradle-build"></a>Het bestand van de build Gradle bijwerken

Beide wijzigen **build.gradle** bestanden:

1. Deze code toevoegen aan de *Project* niveau **build.gradle** bestand binnen de *buildscript* tag:

    ```text
    buildscript {
        repositories {
            jcenter()
        }
    }
    ```

2. Deze code toevoegen aan de *Module app* niveau **build.gradle** bestand binnen de *afhankelijkheden* tag:

    ```text
    compile 'com.microsoft.azure:azure-mobile-android:3.4.0@aar'
    ```

    De meest recente versie is momenteel 3.4.0. De ondersteunde versies zijn vermeld [op bintray][14].

### <a name="enable-internet"></a>Internet-machtiging wordt ingeschakeld

Voor toegang tot Azure, moet uw app de INTERNET-machtiging ingeschakeld hebben. Als deze nog niet is ingeschakeld, voeg de volgende regel code naar uw **AndroidManifest.xml** bestand:

```xml
<uses-permission android:name="android.permission.INTERNET" />
```

## <a name="create-a-client-connection"></a>Een clientverbinding maken

Mobiele Apps van Azure biedt vier functies voor uw mobiele App:

* Toegang tot gegevens en Offline synchronisatie met een Azure Mobile Apps-Service.
* Aangepaste API's die zijn geschreven met de Azure Mobile Apps Server SDK-aanroep.
* Verificatie met Azure App Service-verificatie en autorisatie.
* Registratie bij Notification Hubs voor pushmeldingen.

Elk van deze functies, moet u eerst dat u maakt een `MobileServiceClient` object.  Slechts één `MobileServiceClient` object moet worden gemaakt binnen uw mobiele clients (dat wil zeggen, deze moet een Singleton-patroon).  Maken van een `MobileServiceClient` object:

```java
MobileServiceClient mClient = new MobileServiceClient(
    "<MobileAppUrl>",       // Replace with the Site URL
    this);                  // Your application Context
```

De `<MobileAppUrl>` is een tekenreeks of een URL-object dat naar uw mobiele back-end verwijst.  Als u van Azure App Service gebruikmaakt voor het hosten van uw mobiele back-end, klikt u vervolgens te zorgen dat u de beveiligde `https://` versie van de URL.

De client vereist ook toegang tot de activiteit of Context - de `this` parameter in het voorbeeld.  De constructie MobileServiceClient moet gebeuren binnen de `onCreate()` methode van de activiteit waarnaar wordt verwezen in de `AndroidManifest.xml` bestand.

Als een best practice moet u communicatie tussen de server in een eigen (singleton-pattern)-klasse als abstract.  In dit geval moet u de activiteit in de constructor voor het configureren van de service op de juiste wijze doorgeven.  Bijvoorbeeld:

```java
package com.example.appname.services;

import android.content.Context;
import com.microsoft.windowsazure.mobileservices.*;

public class AzureServiceAdapter {
    private String mMobileBackendUrl = "https://myappname.azurewebsites.net";
    private Context mContext;
    private MobileServiceClient mClient;
    private static AzureServiceAdapter mInstance = null;

    private AzureServiceAdapter(Context context) {
        mContext = context;
        mClient = new MobileServiceClient(mMobileBackendUrl, mContext);
    }

    public static void Initialize(Context context) {
        if (mInstance == null) {
            mInstance = new AzureServiceAdapter(context);
        } else {
            throw new IllegalStateException("AzureServiceAdapter is already initialized");
        }
    }

    public static AzureServiceAdapter getInstance() {
        if (mInstance == null) {
            throw new IllegalStateException("AzureServiceAdapter is not initialized");
        }
        return mInstance;
    }

    public MobileServiceClient getClient() {
        return mClient;
    }

    // Place any public methods that operate on mClient here.
}
```

U kunt nu aanroepen `AzureServiceAdapter.Initialize(this);` in de `onCreate()` methode van uw belangrijkste activiteit.  Alle andere methoden die behoefte hebben toegang tot het gebruik van de client `AzureServiceAdapter.getInstance();` verkrijgen van een verwijzing naar de service-adapter.

## <a name="data-operations"></a>Gegevensbewerkingen

De kern van de Azure Mobile Apps SDK is voor toegang tot gegevens die in SQL Azure worden opgeslagen op de back-end voor de mobiele App.  U kunt toegang tot deze gegevens met behulp van sterk getypeerde klassen (aanbevolen) of getypeerde query's (niet aanbevolen).  Het merendeel van deze sectie behandelt sterk getypeerde klassen gebruiken.

### <a name="define-client-data-classes"></a>Definiëren van gegevensklassen client

Toegang tot gegevens uit SQL Azure-tabellen definiëren van client gegevensklassen die overeenkomen met de tabellen in de back-end voor de mobiele App. Voorbeelden in dit onderwerp wordt ervan uitgegaan dat een tabel met de naam **MyDataTable**, heeft de volgende kolommen:

* id
* Tekst
* Voltooien

Het bijbehorende getypte client-side '-object bevindt zich in een bestand met de naam **MyDataTable.java**:

```java
public class ToDoItem {
    private String id;
    private String text;
    private Boolean complete;
}
```

Voeg de getter en setter methoden voor elk veld dat u toevoegt.  Als uw Azure SQL-tabel meer kolommen bevat, zou u de overeenkomende velden toevoegen aan deze klasse.  Bijvoorbeeld, als de DTO (object data transfer) was een kolom met gehele getallen prioriteit en u kunt dit veld samen met de methoden getter en setter toevoegen:

```java
private Integer priority;

/**
* Returns the item priority
*/
public Integer getPriority() {
    return mPriority;
}

/**
* Sets the item priority
*
* @param priority
*            priority to set
*/
public final void setPriority(Integer priority) {
    mPriority = priority;
}
```

Zie voor meer informatie over het maken van aanvullende tabellen in uw back-end van Mobile Apps, [hoe: definiëren van een domeincontroller tabel] [ 15] (.NET-backend) of [definiëren tabellen met behulp van een dynamische Schema] [ 16] (Node.js back-end).

Een tabel van de back-end van Azure Mobile Apps definieert vijf speciale velden vier die beschikbaar voor clients zijn:

* `String id`: De globaal unieke ID voor de record.  Als een best practice, maakt u de-id de tekenreeksweergave van een [UUID] [ 17] object.
* `DateTimeOffset updatedAt`: De datum/tijd van de laatste update.  Het veld updatedAt is ingesteld door de server en nooit door uw clientcode moet worden ingesteld.
* `DateTimeOffset createdAt`: De datum/tijd die het object is gemaakt.  Het veld createdAt is ingesteld door de server en nooit door uw clientcode moet worden ingesteld.
* `byte[] version`: Normaal weergegeven als een tekenreeks, is de versie ook ingesteld door de server.
* `boolean deleted`: Dit geeft aan dat de record is verwijderd, maar nog niet is verwijderd.  Gebruik geen `deleted` als in uw klasse-eigenschap.

De `id` veld is vereist.  De `updatedAt` veld en `version` veld worden gebruikt voor offlinesynchronisatie (voor incrementele synchronisatie en conflict respectievelijk).  De `createdAt` veld is een verwijzingsveld en wordt niet gebruikt door de client.  De namen zijn namen van de eigenschappen 'in-the-wire' en niet aanpasbaar.  U kunt echter een toewijzing maken tussen uw object en de namen van de 'in-the-wire' met behulp van de [gson] [ 3] bibliotheek.  Bijvoorbeeld:

```java
package com.example.zumoappname;

import com.microsoft.windowsazure.mobileservices.table.DateTimeOffset;

public class ToDoItem
{
    @com.google.gson.annotations.SerializedName("id")
    private String mId;
    public String getId() { return mId; }
    public final void setId(String id) { mId = id; }

    @com.google.gson.annotations.SerializedName("complete")
    private boolean mComplete;
    public boolean isComplete() { return mComplete; }
    public void setComplete(boolean complete) { mComplete = complete; }

    @com.google.gson.annotations.SerializedName("text")
    private String mText;
    public String getText() { return mText; }
    public final void setText(String text) { mText = text; }

    @com.google.gson.annotations.SerializedName("createdAt")
    private DateTimeOffset mCreatedAt;
    public DateTimeOffset getUpdatedAt() { return mCreatedAt; }
    protected DateTimeOffset setUpdatedAt(DateTimeOffset createdAt) { mCreatedAt = createdAt; }

    @com.google.gson.annotations.SerializedName("updatedAt")
    private DateTimeOffset mUpdatedAt;
    public DateTimeOffset getUpdatedAt() { return mUpdatedAt; }
    protected DateTimeOffset setUpdatedAt(DateTimeOffset updatedAt) { mUpdatedAt = updatedAt; }

    @com.google.gson.annotations.SerializedName("version")
    private String mVersion;
    public String getVersion() { return mVersion; }
    public final void setVersion(String version) { mVersion = version; }

    public ToDoItem() { }

    public ToDoItem(String id, String text) {
        this.setId(id);
        this.setText(text);
    }

    @Override
    public boolean equals(Object o) {
        return o instanceof ToDoItem && ((ToDoItem) o).mId == mId;
    }

    @Override
    public String toString() {
        return getText();
    }
}
```

### <a name="create-a-table-reference"></a>De tabelverwijzing van een maken

Als u een tabel, maakt u eerst een [MobileServiceTable] [ 8] object door het aanroepen van de **getTable** methode op de [MobileServiceClient][9].  Deze methode heeft twee overloads:

```java
public class MobileServiceClient {
    public <E> MobileServiceTable<E> getTable(Class<E> clazz);
    public <E> MobileServiceTable<E> getTable(String name, Class<E> clazz);
}
```

In de volgende code **mClient** is een verwijzing naar uw MobileServiceClient-object.  De eerste overbelasting wordt gebruikt voor naam van de klasse en de naam van de tabel hetzelfde zijn waarbij de in de Snelstartgids wordt gebruikt:

```java
MobileServiceTable<ToDoItem> mToDoTable = mClient.getTable(ToDoItem.class);
```

De tweede overbelasting wordt gebruikt wanneer de tabelnaam die van de naam van de klasse afwijkt: de eerste parameter is de naam van de tabel.

```java
MobileServiceTable<ToDoItem> mToDoTable = mClient.getTable("ToDoItemBackup", ToDoItem.class);
```

## <a name="query"></a>Een tabel met back-end

Eerst moet u een tabelverwijzing.  Vervolgens wordt een query uitgevoerd op de tabelverwijzing.  Een query is een combinatie van:

* Een `.where()` [filter-component](#filtering).
* Een `.orderBy()` [ordening component](#sorting).
* Een `.select()` [veld selectiecomponent](#selection).
* Een `.skip()` en `.top()` voor [wisselbare resultaten](#paging).

De componenten moeten worden opgenomen in de voorgaande bewerkingsvolgorde.

### <a name="filter"></a>Filteren van resultaten

De algemene vorm van een query is:

```java
List<MyDataTable> results = mDataTable
    // More filters here
    .execute()          // Returns a ListenableFuture<E>
    .get()              // Converts the async into a sync result
```

Het vorige voorbeeld retourneert alle resultaten (maximaal de maximale paginagrootte ingesteld door de server).  De `.execute()` methode voert u de query op de back-end.  De query wordt geconverteerd naar een [OData v3] [ 19] query voor verzending naar de back-end van Mobile Apps.  Hebben ontvangen converteert de back-end voor mobiele Apps van de query naar een SQL-instructie voordat deze wordt uitgevoerd op de SQL Azure-instantie.  Aangezien netwerkactiviteit enige tijd neemt de `.execute()` methode retourneert een [ `ListenableFuture<E>` ] [ 18].

### <a name="filtering"></a>Geretourneerde gegevens filteren

De uitvoering van de volgende query retourneert alle items uit de **ToDoItem** tabel waar **voltooid** gelijk is aan **false**.

```java
List<ToDoItem> result = mToDoTable
    .where()
    .field("complete").eq(false)
    .execute()
    .get();
```

**mToDoTable** is de verwijzing naar de tabel van de mobiele service die we eerder hebben gemaakt.

Definieer een filter met de **waar** methodeaanroep op de tabelverwijzing. De **waar** methode wordt gevolgd door een **veld** methode gevolgd door een methode die het predicaat logische aangeeft. Mogelijke predikaat methoden omvatten **eq** (is gelijk aan), **ne** (niet gelijk) **gt** (groter dan) **ge** (groter dan of gelijk zijn aan), **lt** (minder dan,) **RP** (kleiner dan of gelijk aan). Deze methoden kunnen u velden en de tekenreeks op specifieke waarden te vergelijken.

U kunt filteren op datums. De volgende methoden kunnen u de volledige datumveld of delen van de datum vergelijken: **jaar**, **maand**, **dag**, **uur**, **minuut**, en **tweede**. Het volgende voorbeeld wordt een filter voor artikelen waarvan *vervaldatum* gelijk is aan 2013.

```java
List<ToDoItem> results = MToDoTable
    .where()
    .year("due").eq(2013)
    .execute()
    .get();
```

De volgende methoden ondersteuning voor complexe filters op tekenreeksvelden: **startsWith**, **endsWith**, **concat**, **subtekenreeks**, **indexOf**, **vervangen**, **toLower**, **toUpper**, **trim**, en **lengte**. Het volgende voorbeeld-filters voor de tabel rijen waar de *tekst* kolom begint met "PRI0."

```java
List<ToDoItem> results = mToDoTable
    .where()
    .startsWith("text", "PRI0")
    .execute()
    .get();
```

De volgende methoden van de operator worden ondersteund op numerieke velden: **toevoegen**, **sub**, **mul**, **div**, **mod**, **floor**, **maximum**, en **afronden**. Het volgende voorbeeld-filters voor de tabel rijen waar de **duur** een even getal is.

```java
List<ToDoItem> results = mToDoTable
    .where()
    .field("duration").mod(2).eq(0)
    .execute()
    .get();
```

U kunt predikaten met deze logische methoden combineren: **en**, **of** en **niet**. Twee van de voorgaande voorbeelden worden gecombineerd voor het volgende voorbeeld.

```java
List<ToDoItem> results = mToDoTable
    .where()
    .year("due").eq(2013).and().startsWith("text", "PRI0")
    .execute()
    .get();
```

Groep en het nesten van logische operators:

```java
List<ToDoItem> results = mToDoTable
    .where()
    .year("due").eq(2013)
    .and(
        startsWith("text", "PRI0")
        .or()
        .field("duration").gt(10)
    )
    .execute().get();
```

Zie voor meer gedetailleerde discussie en voorbeelden van filteren, [verkennen van de uitgebreide functionaliteit van het model van de query Android client][20].

### <a name="sorting"></a>Geretourneerde gegevens sorteren

De volgende code retourneert alle items uit een tabel met **taken** gesorteerd oplopende door de *tekst* veld. *mToDoTable* is de verwijzing naar de back-end-tabel die u eerder hebt gemaakt:

```java
List<ToDoItem> results = mToDoTable
    .orderBy("text", QueryOrder.Ascending)
    .execute()
    .get();
```

De eerste parameter van de **orderBy** methode is een tekenreeks die gelijk is aan de naam van het veld op waarop u wilt sorteren. De tweede parameter gebruikt de **QueryOrder** opsomming om op te geven of u wilt sorteren oplopende of aflopende.  Als u filtert met behulp van de ***waar*** methode, de ***waar*** methode moet worden aangeroepen voordat de ***orderBy*** methode.

### <a name="selection"></a>Selecteer specifieke kolommen

De volgende code laat zien hoe het retourneren van alle items van een tabel met **taken**, maar worden alleen de **voltooid** en **tekst** velden. **mToDoTable** is de verwijzing naar de back-end-tabel die we eerder hebben gemaakt.

```java
List<ToDoItemNarrow> result = mToDoTable
    .select("complete", "text")
    .execute()
    .get();
```

De parameters voor de functie select zijn de tekenreeksnamen van de van de tabel kolommen die u wilt retourneren.  De **Selecteer** methode moet volgen methoden zoals **waar** en **orderBy**. Het kan worden gevolgd door methoden zoals paginering **overslaan** en **boven**.

### <a name="paging"></a>Gegevens weergeven op pagina 's

Gegevens **altijd** geretourneerd op pagina's.  Het maximum aantal records dat wordt geretourneerd is door de server ingesteld.  Als de client meer records vraagt, retourneert de server het maximum aantal records.  De maximale grootte op de server is standaard 50 records.

Het eerste voorbeeld laat zien hoe de bovenste vijf items selecteren uit een tabel. De query retourneert de items van een tabel met **taken**. **mToDoTable** is de verwijzing naar de back-end-tabel die u eerder hebt gemaakt:

```java
List<ToDoItem> result = mToDoTable
    .top(5)
    .execute()
    .get();
```

Hier volgt een query die de eerste vijf items wordt overgeslagen en retourneert vervolgens de volgende vijf:

```java
List<ToDoItem> result = mToDoTable
    .skip(5).top(5)
    .execute()
    .get();
```

Als u ophalen van alle records in een tabel wilt, implementeert u code worden herhaald voor alle pagina's:

```java
List<MyDataModel> results = new List<MyDataModel>();
int nResults;
do {
    int currentCount = results.size();
    List<MyDataModel> pagedResults = mDataTable
        .skip(currentCount).top(500)
        .execute().get();
    nResults = pagedResults.size();
    if (nResults > 0) {
        results.addAll(pagedResults);
    }
} while (nResults > 0);
```

Een aanvraag voor alle records met deze methode maakt een minimum van twee aanvragen voor de back-end van Mobile Apps.

> [!TIP]
> Het formaat van de juiste kiezen een evenwicht tussen geheugengebruik terwijl de aanvraag er gebeurt, bandbreedtegebruik en vertraging bij het ontvangen van de gegevens volledig is.  De standaardwaarde (50 records) is geschikt voor alle apparaten.  Als u alleen op geheugenapparaten met grotere werkt, verhoogt u maximaal 500.  We hebben vastgesteld dat de pagina vergroten dan 500 registreert resultaten in onaanvaardbaar vertragingen en grote geheugenproblemen.

### <a name="chaining"></a>Hoe: querymethoden samenvoegen

De methoden die worden gebruikt in het opvragen van de back-end-tabellen kunnen worden samengevoegd. Methoden voor het query-koppeling kunt u specifieke kolommen met gefilterde rijen die worden gesorteerd en wisselbare selecteren. U kunt complexe logische filters maken.  Elke querymethode retourneert een Query-object. Aanroepen om te beëindigen van de reeks methoden en daadwerkelijk uitvoeren van de query, het **uitvoeren** methode. Bijvoorbeeld:

```java
List<ToDoItem> results = mToDoTable
        .where()
        .year("due").eq(2013)
        .and(
            startsWith("text", "PRI0").or().field("duration").gt(10)
        )
        .orderBy(duration, QueryOrder.Ascending)
        .select("id", "complete", "text", "duration")
        .skip(200).top(100)
        .execute()
        .get();
```

De keten querymethoden moeten als volgt worden gerangschikt:

1. Filteren (**waar**) methoden.
2. Sorteren (**orderBy**) methoden.
3. Selectie (**Selecteer**) methoden.
4. wisselgeheugengebruik (**overslaan** en **boven**) methoden.

## <a name="binding"></a>Gegevens binden aan de gebruikersinterface

Gegevensbinding omvat drie onderdelen:

* De gegevensbron
* De indeling startscherm
* De adapter die de twee elkaar verbindt.

In onze voorbeeldcode we de gegevens retourneert uit de tabel Mobile Apps SQL Azure **ToDoItem** in een matrix. Deze activiteit is een algemene patroon voor toepassingen van gegevens.  Databasequery's retourneren vaak een verzameling rijen die de client in een lijst of matrix opgehaald. De matrix is in dit voorbeeld wordt de gegevensbron.  De code wordt een schermindeling die de weergave van de gegevens die wordt weergegeven op het apparaat wordt gedefinieerd.  Afhankelijk van de twee samen met een netwerkadapter, die in deze code een uitbreiding is van de **ArrayAdapter&lt;ToDoItem&gt;**  klasse.

#### <a name="layout"></a>De indeling definiëren

De indeling wordt gedefinieerd door verschillende codefragmenten van XML-code. Uitgaande van een bestaande indeling, het volgende codevoorbeeld geeft de **ListView** wilt vullen met onze gegevens van de server.

```xml
    <ListView
        android:id="@+id/listViewToDo"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        tools:listitem="@layout/row_list_to_do" >
    </ListView>
```

In de vorige code de *listitem* kenmerk geeft de id van de indeling voor een afzonderlijke rij in de lijst. Deze code geeft een selectievakje en de bijbehorende tekst en eenmaal voor elk item in de lijst wordt geïnstantieerd. Deze indeling niet wordt weergegeven de **id** veld en een complexere indeling aanvullende velden in de weergave wilt opgeven. Deze code is in de **row_list_to_do.xml** bestand.

```java
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="horizontal">
    <CheckBox
        android:id="@+id/checkToDoItem"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="@string/checkbox_text" />
</LinearLayout>
```

#### <a name="adapter"></a>De adapter definiëren
Omdat de gegevensbron van onze weergave een matrix met is **ToDoItem**, we subklasse onze-adapter van een **ArrayAdapter&lt;ToDoItem&gt;**  klasse. Deze subklasse produceert een weergave voor elke **ToDoItem** met behulp van de **row_list_to_do** lay-out.  In onze code definiëren we de volgende klasse die is een uitbreiding van de **ArrayAdapter&lt;E&gt;**  klasse:

```java
public class ToDoItemAdapter extends ArrayAdapter<ToDoItem> {
}
```

Onderdrukking van de adapters **getView** methode. Bijvoorbeeld:

```
    @Override
    public View getView(int position, View convertView, ViewGroup parent) {
        View row = convertView;

        final ToDoItem currentItem = getItem(position);

        if (row == null) {
            LayoutInflater inflater = ((Activity) mContext).getLayoutInflater();
            row = inflater.inflate(R.layout.row_list_to_do, parent, false);
        }
        row.setTag(currentItem);

        final CheckBox checkBox = (CheckBox) row.findViewById(R.id.checkToDoItem);
        checkBox.setText(currentItem.getText());
        checkBox.setChecked(false);
        checkBox.setEnabled(true);

        checkBox.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View arg0) {
                if (checkBox.isChecked()) {
                    checkBox.setEnabled(false);
                    if (mContext instanceof ToDoActivity) {
                        ToDoActivity activity = (ToDoActivity) mContext;
                        activity.checkItem(currentItem);
                    }
                }
            }
        });
        return row;
    }
```

We geen exemplaar maken van deze klasse in onze activiteit als volgt:

```java
    ToDoItemAdapter mAdapter;
    mAdapter = new ToDoItemAdapter(this, R.layout.row_list_to_do);
```

De tweede parameter voor de constructor ToDoItemAdapter is een verwijzing naar de indeling. We kunnen nu exemplaar maken van de **ListView** en toewijzen van de adapter in de **ListView**.

```java
    ListView listViewToDo = (ListView) findViewById(R.id.listViewToDo);
    listViewToDo.setAdapter(mAdapter);
```

#### <a name="use-adapter"></a>Gebruik de Adapter moet worden gebonden aan de gebruikersinterface

U bent nu klaar voor gebruik van de gegevensbinding. De volgende code laat zien hoe het ophalen van items in de tabel en wordt de lokale adapter gevuld met de geretourneerde artikelen.

```java
    public void showAll(View view) {
        AsyncTask<Void, Void, Void> task = new AsyncTask<Void, Void, Void>(){
            @Override
            protected Void doInBackground(Void... params) {
                try {
                    final List<ToDoItem> results = mToDoTable.execute().get();
                    runOnUiThread(new Runnable() {

                        @Override
                        public void run() {
                            mAdapter.clear();
                            for (ToDoItem item : results) {
                                mAdapter.add(item);
                            }
                        }
                    });
                } catch (Exception exception) {
                    createAndShowDialog(exception, "Error");
                }
                return null;
            }
        };
        runAsyncTask(task);
    }
```

Aanroepen van de adapter elk gewenst moment u wijzigt de **ToDoItem** tabel. Omdat wijzigingen op basis van door een andere record klaar bent, kunt u een enkele rij in plaats van een verzameling verwerken. Wanneer u een item invoegt, roept de **toevoegen** methode op de adapter; wanneer te verwijderen, roept de **verwijderen** methode.

U vindt een compleet voorbeeld in de [Android Quick Start-Project][21].

## <a name="inserting"></a>Gegevens invoegen in de back-end

Een instantie van de *ToDoItem* klasse en stel de eigenschappen.

```java
ToDoItem item = new ToDoItem();
item.text = "Test Program";
item.complete = false;
```

Gebruik vervolgens **insert()** een object in te voegen:

```java
ToDoItem entity = mToDoTable
    .insert(item)       // Returns a ListenableFuture<ToDoItem>
    .get();
```

Het geretourneerde entiteit overeenkomt met de gegevens ingevoegd in de tabel back-end van de ID en andere waarden opgenomen (zoals de `createdAt`, `updatedAt`, en `version` velden) ingesteld voor de back-end.

Mobile Apps-tabellen vereisen een primaire sleutelkolom met de naam **id**. In deze kolom moet een tekenreeks zijn. De standaardwaarde van de kolom-ID is een GUID.  U kunt andere unieke waarden, zoals e-mailadressen of gebruikersnamen op te geven. Wanneer de waarde van een tekenreeks-ID niet voor een ingevoegde record opgegeven is, genereert de back-end een nieuwe GUID.

ID van tekenreekswaarden bieden de volgende voordelen:

* Id's kunnen worden gegenereerd zonder dat een retouren naar de database.
* Records zijn gemakkelijker te samenvoegen uit verschillende tabellen of databases.
* Id-waarden worden geïntegreerd beter met een toepassing logica.

Tekenreeks-ID-waarden zijn **REQUIRED** voor offlinesynchronisatie ondersteuning.  U kunt een Id niet wijzigen zodra deze is opgeslagen in de back-end-database.

## <a name="updating"></a>Gegevens bijwerken in een mobiele app

Voor het bijwerken van gegevens in een tabel, geeft u het nieuwe object wilt de **update()** methode.

```java
mToDoTable
    .update(item)   // Returns a ListenableFuture<ToDoItem>
    .get();
```

In dit voorbeeld *item* is een verwijzing naar een rij in de *ToDoItem* tabel een aantal wijzigingen aangebracht heeft.  De rij met dezelfde **id** wordt bijgewerkt.

## <a name="deleting"></a>Gegevens in een mobiele app verwijderen

De volgende code laat zien hoe om gegevens te verwijderen uit een tabel door te geven van het gegevensobject.

```java
mToDoTable
    .delete(item);
```

U kunt ook een item verwijderen door op te geven de **id** veld van de rij te verwijderen.

```java
String myRowId = "2FA404AB-E458-44CD-BC1B-3BC847EF0902";
mToDoTable
    .delete(myRowId);
```

## <a name="lookup"></a>Een specifiek item door-Id niet opzoeken

Opzoeken van een item met een specifieke **id** veld met de **lookUp()** methode:

```java
ToDoItem result = mToDoTable
    .lookUp("0380BAFB-BCFF-443C-B7D5-30199F730335")
    .get();
```

## <a name="untyped"></a>Procedure: werken met niet-getypeerde gegevens

De niet-getypeerde programmeermodel biedt u de precieze controle over de JSON-serialisatie.  Er zijn enkele algemene scenario's waar u kunt desgewenst een niet-getypeerde programmeermodel gebruiken. Als bijvoorbeeld uw back-end-tabel veel kolommen bevat en u hoeft alleen te verwijzen naar een subset van de kolommen.  De getypte model, moet u de kolommen die zijn gedefinieerd in de back-end voor mobiele Apps in uw gegevensklasse definiëren.  De meeste van de API-aanroepen voor toegang tot gegevens zijn vergelijkbaar met de getypte programming aanroepen. Het belangrijkste verschil is dat in het model niet-getypeerde u methoden aanroepen op de **MobileServiceJsonTable** object, in plaats van de **MobileServiceTable** object.

### <a name="json_instance"></a>Geen exemplaar maken van een niet-getypeerde tabel

Net als bij de getypte model kunt u beginnen met het ophalen van een tabelverwijzing, maar in dit geval is een **MobileServicesJsonTable** object. De verwijzing verkrijgen door het aanroepen van de **getTable** methode op een exemplaar van de client:

```java
private MobileServiceJsonTable mJsonToDoTable;
//...
mJsonToDoTable = mClient.getTable("ToDoItem");
```

Nadat u hebt gemaakt dat een exemplaar van de **MobileServiceJsonTable**, heeft bijna de dezelfde API beschikbaar als met de getypte programmeermodel. In sommige gevallen kan de methoden maken gebruik van een niet-getypeerde parameter in plaats van een type parameter.

### <a name="json_insert"></a>Invoegen in een niet-getypeerde tabel
De volgende code laat zien hoe een INSERT-bewerking uitvoeren. De eerste stap is het maken van een [JsonObject][1], die deel uitmaakt van de [gson] [ 3] bibliotheek.

```java
JsonObject jsonItem = new JsonObject();
jsonItem.addProperty("text", "Wake up");
jsonItem.addProperty("complete", false);
```

Vervolgens gebruikt u **insert()** het niet-getypeerde object in de tabel invoegen.

```java
JsonObject insertedItem = mJsonToDoTable
    .insert(jsonItem)
    .get();
```

Als u de ID van het ingevoegde object niet ophalen moet, gebruikt u de **getAsJsonPrimitive()** methode.

```java
String id = insertedItem.getAsJsonPrimitive("id").getAsString();
```
### <a name="json_delete"></a>Verwijderen uit een niet-getypeerde tabel
De volgende code toont hoe u verwijdert een exemplaar, in dit geval hetzelfde exemplaar van een **JsonObject** die is gemaakt in de voorafgaande *invoegen* voorbeeld. De code is hetzelfde als bij de getypte letters, maar de methode heeft een andere handtekening omdat deze verwijst naar een **JsonObject**.

```java
mToDoTable
    .delete(insertedItem);
```

U kunt ook een exemplaar rechtstreeks door met de bijbehorende ID verwijderen:

```java
mToDoTable.delete(ID);
```

### <a name="json_get"></a>Retourneert alle rijen uit een niet-getypeerde tabel
De volgende code laat zien hoe een hele tabel ophalen. Omdat u een tabel JSON gebruikt, kunt u slechts een deel van de tabelkolommen selectief ophalen.

```java
public void showAllUntyped(View view) {
    new AsyncTask<Void, Void, Void>() {
        @Override
        protected Void doInBackground(Void... params) {
            try {
                final JsonElement result = mJsonToDoTable.execute().get();
                final JsonArray results = result.getAsJsonArray();
                runOnUiThread(new Runnable() {

                    @Override
                    public void run() {
                        mAdapter.clear();
                        for (JsonElement item : results) {
                            String ID = item.getAsJsonObject().getAsJsonPrimitive("id").getAsString();
                            String mText = item.getAsJsonObject().getAsJsonPrimitive("text").getAsString();
                            Boolean mComplete = item.getAsJsonObject().getAsJsonPrimitive("complete").getAsBoolean();
                            ToDoItem mToDoItem = new ToDoItem();
                            mToDoItem.setId(ID);
                            mToDoItem.setText(mText);
                            mToDoItem.setComplete(mComplete);
                            mAdapter.add(mToDoItem);
                        }
                    }
                });
            } catch (Exception exception) {
                createAndShowDialog(exception, "Error");
            }
            return null;
        }
    }.execute();
}
```

Dezelfde set voor het filteren van filteren en methoden die beschikbaar voor het model getypte zijn paging beschikbaar zijn voor de niet-getypeerde model.

## <a name="offline-sync"></a>Offlinesynchronisatie implementeren

Offline synchronisatie van gegevens van implementeert de Client-SDK van Azure Mobile Apps ook met behulp van een SQLite-database voor het opslaan van een kopie van de servergegevens lokaal.  Bewerkingen die worden uitgevoerd op een offline tabel vereisen geen mobiele connectiviteit te werken.  Offline synchronisatie helpt prestaties ten koste van de complexere logica voor conflictoplossing en herstelmogelijkheden.  De Client-SDK van Azure Mobile Apps implementeert de volgende functies:

* Incrementele synchronisatie: Alleen bijgewerkt en nieuwe records worden gedownload verbruik van bandbreedte en geheugen.
* Optimistische gelijktijdigheid: Operations wordt aangenomen dat slagen.  Conflictoplossing wordt uitgesteld totdat de updates worden uitgevoerd op de server.
* Conflictoplossing: De SDK detecteert wanneer een conflicterende wijziging heeft aangebracht op de server en hooks biedt Waarschuw de gebruiker.
* Voorlopig verwijderen: Verwijderde records zijn gemarkeerd als verwijderd, zodat andere apparaten hun offline configuratiecache bij te werken.

### <a name="initialize-offline-sync"></a>Offlinesynchronisatie initialiseren

Elke tabel offline moet worden gedefinieerd in de cache offline vóór gebruik.  Normaal gesproken tabeldefinitie gebeurt onmiddellijk na het maken van de client:

```java
AsyncTask<Void, Void, Void> initializeStore(MobileServiceClient mClient)
    throws MobileServiceLocalStoreException, ExecutionException, InterruptedException
{
    AsyncTask<Void, Void, Void> task = new AsyncTask<Void, Void, Void>() {
        @Override
        protected void doInBackground(Void... params) {
            try {
                MobileServiceSyncContext syncContext = mClient.getSyncContext();
                if (syncContext.isInitialized()) {
                    return null;
                }
                SQLiteLocalStore localStore = new SQLiteLocalStore(mClient.getContext(), "offlineStore", null, 1);

                // Create a table definition.  As a best practice, store this with the model definition and return it via
                // a static method
                Map<String, ColumnDataType> toDoItemDefinition = new HashMap<String, ColumnDataType>();
                toDoItemDefinition.put("id", ColumnDataType.String);
                toDoItemDefinition.put("complete", ColumnDataType.Boolean);
                toDoItemDefinition.put("text", ColumnDataType.String);
                toDoItemDefinition.put("version", ColumnDataType.String);
                toDoItemDefinition.put("updatedAt", ColumnDataType.DateTimeOffset);

                // Now define the table in the local store
                localStore.defineTable("ToDoItem", toDoItemDefinition);

                // Specify a sync handler for conflict resolution
                SimpleSyncHandler handler = new SimpleSyncHandler();

                // Initialize the local store
                syncContext.initialize(localStore, handler).get();
            } catch (final Exception e) {
                createAndShowDialogFromTask(e, "Error");
            }
            return null;
        }
    };
    return runAsyncTask(task);
}
```

### <a name="obtain-a-reference-to-the-offline-cache-table"></a>Een verwijzing naar de tabel Offline Cache

Voor een online tabel, gebruikt u `.getTable()`.  Gebruik voor een offline tabel `.getSyncTable()`:

```java
MobileServiceSyncTable<ToDoItem> mToDoTable = mClient.getSyncTable("ToDoItem", ToDoItem.class);
```

De methoden die beschikbaar voor online-tabellen zijn (inclusief filteren, sorteren, paging, gegevens invoegen, bijwerken van gegevens en verwijderen van gegevens) werkt goed in online als offline tabellen.

### <a name="synchronize-the-local-offline-cache"></a>De lokale Cache voor het Offline synchroniseren

Synchronisatie is binnen het besturingselement van uw app.  Hier volgt een voorbeeld van de synchronisatie-methode:

```java
private AsyncTask<Void, Void, Void> sync(MobileServiceClient mClient) {
    AsyncTask<Void, Void, Void> task = new AsyncTask<Void, Void, Void>(){
        @Override
        protected Void doInBackground(Void... params) {
            try {
                MobileServiceSyncContext syncContext = mClient.getSyncContext();
                syncContext.push().get();
                mToDoTable.pull(null, "todoitem").get();
            } catch (final Exception e) {
                createAndShowDialogFromTask(e, "Error");
            }
            return null;
        }
    };
    return runAsyncTask(task);
}
```

Als de naam van de query is opgegeven voor de `.pull(query, queryname)` methode en vervolgens incrementele synchronisatie wordt gebruikt om terug te keren alleen de records die zijn gemaakt of gewijzigd sinds de laatste pull voltooid.

### <a name="handle-conflicts-during-offline-synchronization"></a>Afhandeling van conflicten tijdens het Offline synchroniseren

Als een conflict zich tijdens voordoet een `.push()` bewerking, een `MobileServiceConflictException` gegenereerd.   Het item server uitgegeven is ingesloten in de uitzondering en kan worden opgehaald door `.getItem()` op de uitzondering.  De push-bewerking door het aanroepen van de volgende items op het object MobileServiceSyncContext aanpassen:

*  `.cancelAndDiscardItem()`
*  `.cancelAndUpdateItem()`
*  `.updateOperationAndItem()`

Wanneer alle conflicten zijn gemarkeerd als u wilt, aanroepen `.push()` om opnieuw op te lossen alle conflicten.

## <a name="custom-api"></a>Een aangepaste API aanroepen

Een aangepaste API kunt u aangepaste eindpunten die serverfunctionaliteit weergeven die niet worden toegewezen aan een invoegen, bijwerken, verwijderen of leesbewerking definiëren. Met behulp van een aangepaste API gebruiken, kunt u meer controle over messaging, hebben waaronder lezen en HTTP-bericht headers instellen en definiëren van een berichttekst de indeling dan JSON.

U aanroepen in een Android-client de **invokeApi** methode voor het eindpunt van de aangepaste API aanroepen. Het volgende voorbeeld toont het aanroepen van een API-eindpunt met de naam **completeAll**, die de klasse van een verzameling met de naam retourneert **MarkAllResult**.

```java
public void completeItem(View view) {
    ListenableFuture<MarkAllResult> result = mClient.invokeApi("completeAll", MarkAllResult.class);
    Futures.addCallback(result, new FutureCallback<MarkAllResult>() {
        @Override
        public void onFailure(Throwable exc) {
            createAndShowDialog((Exception) exc, "Error");
        }

        @Override
        public void onSuccess(MarkAllResult result) {
            createAndShowDialog(result.getCount() + " item(s) marked as complete.", "Completed Items");
            refreshItemsFromTable();
        }
    });
}
```

De **invokeApi** methode wordt aangeroepen op de client, die een POST-aanvraag naar de nieuwe aangepaste API verzendt. Het resultaat geretourneerd door de aangepaste API wordt weergegeven in een dialoogvenster weergegeven als er fouten zijn. Andere versies van **invokeApi** kunt u eventueel het verzenden van een object in de aanvraagtekst, geeft u de HTTP-methode en queryparameters aan de aanvraag te verzenden. Versies van getypeerde **invokeApi** ook worden geleverd.

## <a name="authentication"></a>Verificatie toevoegen aan uw app

Het toevoegen van deze functies beschrijven zelfstudies al in detail.

App Service ondersteunt [verifiëren van gebruikers van de app](app-service-mobile-android-get-started-users.md) met behulp van verschillende externe id-providers: Facebook, Google, Microsoft-Account, Twitter en Azure Active Directory. U kunt machtigingen instellen voor tabellen toegang voor specifieke bewerkingen voor alleen geverifieerde gebruikers te beperken. U kunt ook de identiteit van de geverifieerde gebruikers gebruiken voor het implementeren van autorisatieregels in uw back-end.

Twee verificatie stromen worden ondersteund: een **server** stroom en een **client** stroom. De stroom van de server biedt de eenvoudigste verificatie-ervaring, is afhankelijk van de identiteit providers webinterface.  Er zijn geen extra SDK's zijn vereist voor het implementeren van verificatie van de stroom. Verificatie van de stroom biedt geen een diepe integratie in het mobiele apparaat en wordt alleen aanbevolen voor bewijs van concept scenario's.

De stroom kunt voor een betere integratie met apparaatspecifieke mogelijkheden, zoals het eenmalige aanmelding is afhankelijk van de SDK's die worden geleverd door de identiteitsprovider.  U kunt bijvoorbeeld de Facebook SDK integreren in uw mobiele App.  Mobiele clients worden omgewisseld in de Facebook-app en bevestigt de aanmelding voordat wisselen terug naar uw mobiele app.

Vier stappen zijn vereist voor verificatie in uw app inschakelen:

* Uw app registreren voor verificatie met een id-provider.
* Configureer uw App Service-back-end.
* Tabelmachtigingen beperken voor geverifieerde gebruikers alleen op de App Service-back-end.
* Verificatiecode toevoegen aan uw app.

U kunt machtigingen instellen voor tabellen toegang voor specifieke bewerkingen voor alleen geverifieerde gebruikers te beperken. U kunt ook de SID van een geverifieerde gebruiker te wijzigen van aanvragen.  Raadpleeg voor meer informatie [aan de slag met verificatie] en de procedure voor Server SDK-documentatie.

### <a name="caching"></a>: Verificatiestroom Server

De volgende code start een server stroom aanmelding met de Google-provider.  Aanvullende configuratie is vereist vanwege de beveiligingsvereisten voor de Google-provider:

```java
MobileServiceUser user = mClient.login(MobileServiceAuthenticationProvider.Google, "{url_scheme_of_your_app}", GOOGLE_LOGIN_REQUEST_CODE);
```

Voeg bovendien de volgende methode toe aan de hoofdklasse van de activiteit:

```java
// You can choose any unique number here to differentiate auth providers from each other. Note this is the same code at login() and onActivityResult().
public static final int GOOGLE_LOGIN_REQUEST_CODE = 1;

@Override
protected void onActivityResult(int requestCode, int resultCode, Intent data) {
    // When request completes
    if (resultCode == RESULT_OK) {
        // Check the request code matches the one we send in the login request
        if (requestCode == GOOGLE_LOGIN_REQUEST_CODE) {
            MobileServiceActivityResult result = mClient.onActivityResult(data);
            if (result.isLoggedIn()) {
                // login succeeded
                createAndShowDialog(String.format("You are now logged in - %1$2s", mClient.getCurrentUser().getUserId()), "Success");
                createTable();
            } else {
                // login failed, check the error message
                String errorMessage = result.getErrorMessage();
                createAndShowDialog(errorMessage, "Error");
            }
        }
    }
}
```

De `GOOGLE_LOGIN_REQUEST_CODE` gedefinieerd in uw belangrijkste activiteit wordt gebruikt voor de `login()` methode en binnen de `onActivityResult()` methode.  U kunt een uniek nummer, zolang de hetzelfde nummer wordt gebruikt binnen de `login()` methode en de `onActivityResult()` methode.  Als u de clientcode abstracte in een service-adapter (zoals eerder is weergegeven), moet u de juiste methoden aanroepen op de adapter van de service.

Ook moet u het project voor customtabs configureren.  Eerst een Omleidings-URL opgeven.  Voeg het volgende codefragment aan `AndroidManifest.xml`:

```xml
<activity android:name="com.microsoft.windowsazure.mobileservices.authentication.RedirectUrlActivity">
    <intent-filter>
        <action android:name="android.intent.action.VIEW" />
        <category android:name="android.intent.category.DEFAULT" />
        <category android:name="android.intent.category.BROWSABLE" />
        <data android:scheme="{url_scheme_of_your_app}" android:host="easyauth.callback"/>
    </intent-filter>
</activity>
```

Voeg de **redirectUriScheme** naar de `build.gradle` bestand voor uw toepassing:

```text
android {
    buildTypes {
        release {
            // … …
            manifestPlaceholders = ['redirectUriScheme': '{url_scheme_of_your_app}://easyauth.callback']
        }
        debug {
            // … …
            manifestPlaceholders = ['redirectUriScheme': '{url_scheme_of_your_app}://easyauth.callback']
        }
    }
}
```

Voeg `com.android.support:customtabs:23.0.1` aan de lijst met afhankelijkheden in de `build.gradle` bestand:

```text
dependencies {
    compile fileTree(dir: 'libs', include: ['*.jar'])
    compile 'com.google.code.gson:gson:2.3'
    compile 'com.google.guava:guava:18.0'
    compile 'com.android.support:customtabs:23.0.1'
    compile 'com.squareup.okhttp:okhttp:2.5.0'
    compile 'com.microsoft.azure:azure-mobile-android:3.4.0@aar'
    compile 'com.microsoft.azure:azure-notifications-handler:1.0.1@jar'
}
```

Verkrijgen van de ID van de aangemelde gebruiker van een **MobileServiceUser** met behulp van de **getUserId** methode. Zie voor een voorbeeld van het gebruik van Futures aanroepen van de asynchrone aanmelding API's [aan de slag met verificatie].

> [!WARNING]
> Het URL-schema vermeld is hoofdlettergevoelig.  Zorg ervoor dat alle instanties van `{url_scheme_of_you_app}` hoofdlettergevoelig.

### <a name="caching"></a>Verificatietokens cache

Verificatietokens caching, moet u de gebruikers-ID en -verificatietoken lokaal opslaan op het apparaat. De volgende keer dat de app wordt gestart, controleren van de cache, en als deze waarden aanwezig zijn, kunt u het logboek in procedure overslaan en de client bij deze gegevens rehydrate. Deze gegevens is echter gevoelige en moeten worden opgeslagen voor de veiligheid versleuteld als de telefoon wordt gestolen.  Ziet u een compleet voorbeeld van hoe u aan de cache verificatietokens in [verificatiesectie tokens in de Cache][7].

Wanneer u een verlopen token gebruiken probeert, krijgt u een *401-niet-geautoriseerde* antwoord. U kunt met behulp van filters verificatiefouten verwerken.  Filters onderscheppen aanvragen voor de App Service-back-end. De filtercode test van het antwoord op een 401, activeert het proces aanmelden en hervat vervolgens de aanvraag die de 401 gegenereerd.

### <a name="refresh"></a>Vernieuwen van Tokens gebruiken

Het token dat is geretourneerd door de Azure App Service-verificatie en autorisatie heeft een gedefinieerde levensduur van een uur.  Na deze periode, moet u de gebruiker te verifiëren.  Als u een lange levensduur token die u hebt ontvangen via client-flow-verificatie gebruikt, kunt u andere referenties met Azure App Service-verificatie en autorisatie met hetzelfde token.  Een andere Azure App Service-token wordt gegenereerd met een nieuwe levensduur.

U kunt ook de provider voor het gebruik van Tokens vernieuwen registreren.  Een Token voor vernieuwen is niet altijd beschikbaar.  Er is aanvullende configuratie vereist:

* Voor **Azure Active Directory**, een clientgeheim configureren voor Azure Active Directory-App.  Geef het clientgeheim in de Azure App Service bij het configureren van Azure Active Directory-verificatie.  Bij het aanroepen van `.login()`, doorgeven `response_type=code id_token` als een parameter:

    ```java
    HashMap<String, String> parameters = new HashMap<String, String>();
    parameters.put("response_type", "code id_token");
    MobileServiceUser user = mClient.login
        MobileServiceAuthenticationProvider.AzureActiveDirectory,
        "{url_scheme_of_your_app}",
        AAD_LOGIN_REQUEST_CODE,
        parameters);
    ```

* Voor **Google**, geven de `access_type=offline` als een parameter:

    ```java
    HashMap<String, String> parameters = new HashMap<String, String>();
    parameters.put("access_type", "offline");
    MobileServiceUser user = mClient.login
        MobileServiceAuthenticationProvider.Google,
        "{url_scheme_of_your_app}",
        GOOGLE_LOGIN_REQUEST_CODE,
        parameters);
    ```

* Voor **Microsoft-Account**, selecteer de `wl.offline_access` bereik.

Als u wilt vernieuwen van een token, aanroepen `.refreshUser()`:

```java
MobileServiceUser user = mClient
    .refreshUser()  // async - returns a ListenableFuture<MobileServiceUser>
    .get();
```

Als een best practice een filter maken waarmee een 401-respons van de server detecteert en probeert het gebruikerstoken te vernieuwen.

## <a name="log-in-with-client-flow-authentication"></a>Meld u aan met Client-flow-verificatie

Het algemene proces voor aangemeld met client-flow-verificatie is als volgt:

* Azure App Service-verificatie en autorisatie configureert als server-flow-verificatie.
* De verificatieprovider SDK voor verificatie voor het produceren van een toegangstoken integreren.
* Roep de `.login()` methode als volgt:

    ```java
    JSONObject payload = new JSONObject();
    payload.put("access_token", result.getAccessToken());
    ListenableFuture<MobileServiceUser> mLogin = mClient.login("{provider}", payload.toString());
    Futures.addCallback(mLogin, new FutureCallback<MobileServiceUser>() {
        @Override
        public void onFailure(Throwable exc) {
            exc.printStackTrace();
        }
        @Override
        public void onSuccess(MobileServiceUser user) {
            Log.d(TAG, "Login Complete");
        }
    });
    ```

Vervang de `onSuccess()` methode met wat u code wilt gebruiken op een geslaagde aanmelding.  De `{provider}` tekenreeks is een geldige provider: **aad** (Azure Active Directory), **facebook**, **google**, **microsoftaccount**, of **twitter**.  Als u aangepaste verificatie hebt geïmplementeerd, kunt u ook de code van de provider aangepaste verificatie gebruiken.

### <a name="adal"></a>Verificatie van gebruikers met de Active Directory Authentication Library (ADAL)

U kunt de Active Directory Authentication Library (ADAL) gebruiken voor het ondertekenen van gebruikers in uw toepassing met Azure Active Directory. Met behulp van de aanmelding van een client stroom is het handiger voor het gebruik van de `loginAsync()` methoden zoals deze biedt een meer systeemeigen UX idee en kunt u extra aanpassingen.

1. Uw mobiele app back-end voor aanmelding bij de AAD instellen door de [App Service configureren voor Active Directory-aanmelding] [ 22] zelfstudie. Zorg ervoor dat de optionele stap voor het registreren van een systeemeigen clienttoepassing van voltooien.
2. ADAL installeren door uw build.gradle-bestand om op te nemen van de volgende definities te wijzigen:

```
repositories {
    mavenCentral()
    flatDir {
        dirs 'libs'
    }
    maven {
        url "YourLocalMavenRepoPath\\.m2\\repository"
    }
}
packagingOptions {
    exclude 'META-INF/MSFTSIG.RSA'
    exclude 'META-INF/MSFTSIG.SF'
}
dependencies {
    compile fileTree(dir: 'libs', include: ['*.jar'])
    compile('com.microsoft.aad:adal:1.1.1') {
        exclude group: 'com.android.support'
    } // Recent version is 1.1.1
    compile 'com.android.support:support-v4:23.0.0'
}
```

1. De volgende code toevoegen aan uw toepassing, waardoor de volgende vervangingen:

* Vervang **INSERT-instantie-hier** met de naam van de tenant waarin u uw toepassing hebt ingericht. De indeling moet https://login.microsoftonline.com/contoso.onmicrosoft.com.
* Vervang **INSERT RESOURCE-ID hier** met de client-ID voor uw back-end voor de mobiele app. U vindt de client-ID van de **Geavanceerd** tabblad onder **Azure Active Directory-instellingen** in de portal.
* Vervang **INSERT-CLIENT-ID-hier** met de client-ID die u hebt gekopieerd uit de native client-toepassing.
* Vervang **INSERT-OMLEIDINGS-URI-hier** aan uw site */.auth/login/done* eindpunt, met behulp van het HTTPS-schema. Deze waarde moet er ongeveer als *https://contoso.azurewebsites.net/.auth/login/done*.

```java
private AuthenticationContext mContext;

private void authenticate() {
    String authority = "INSERT-AUTHORITY-HERE";
    String resourceId = "INSERT-RESOURCE-ID-HERE";
    String clientId = "INSERT-CLIENT-ID-HERE";
    String redirectUri = "INSERT-REDIRECT-URI-HERE";
    try {
        mContext = new AuthenticationContext(this, authority, true);
        mContext.acquireToken(this, resourceId, clientId, redirectUri, PromptBehavior.Auto, "", callback);
    } catch (Exception exc) {
        exc.printStackTrace();
    }
}

private AuthenticationCallback<AuthenticationResult> callback = new AuthenticationCallback<AuthenticationResult>() {
    @Override
    public void onError(Exception exc) {
        if (exc instanceof AuthenticationException) {
            Log.d(TAG, "Cancelled");
        } else {
            Log.d(TAG, "Authentication error:" + exc.getMessage());
        }
    }

    @Override
    public void onSuccess(AuthenticationResult result) {
        if (result == null || result.getAccessToken() == null
                || result.getAccessToken().isEmpty()) {
            Log.d(TAG, "Token is empty");
        } else {
            try {
                JSONObject payload = new JSONObject();
                payload.put("access_token", result.getAccessToken());
                ListenableFuture<MobileServiceUser> mLogin = mClient.login("aad", payload.toString());
                Futures.addCallback(mLogin, new FutureCallback<MobileServiceUser>() {
                    @Override
                    public void onFailure(Throwable exc) {
                        exc.printStackTrace();
                    }
                    @Override
                    public void onSuccess(MobileServiceUser user) {
                        Log.d(TAG, "Login Complete");
                    }
                });
            }
            catch (Exception exc){
                Log.d(TAG, "Authentication error:" + exc.getMessage());
            }
        }
    }
};

@Override
protected void onActivityResult(int requestCode, int resultCode, Intent data) {
    super.onActivityResult(requestCode, resultCode, data);
    if (mContext != null) {
        mContext.onActivityResult(requestCode, resultCode, data);
    }
}
```

## <a name="filters"></a>De Client-servercommunicatie aanpassen

De clientverbinding is normaal gesproken een eenvoudige HTTP-verbinding met de onderliggende HTTP-bibliotheek geleverd met de Android-SDK.  Er zijn diverse redenen waarom u deze instelling wijzigen zou willen:

* Wilt u een alternatieve HTTP-bibliotheek gebruiken om aan te passen time-outs.
* Wilt u een voortgangsbalk bieden.
* U wilt toevoegen van een aangepaste header ter ondersteuning van API management-functionaliteit.
* Wilt u een mislukte reactie intercept zodat u herauthenticatie kunt implementeren.
* Wilt u back-end-aanvragen met een analytics-service aanmelden.

### <a name="using-an-alternate-http-library"></a>Met behulp van een andere HTTP-bibliotheek

Roep de `.setAndroidHttpClientFactory()` methode onmiddellijk na het maken van uw client-verwijzing.  Als u bijvoorbeeld de verbindingstime-out ingesteld op 60 seconden (in plaats van de standaard 10 seconden):

```java
mClient = new MobileServiceClient("https://myappname.azurewebsites.net");
mClient.setAndroidHttpClientFactory(new OkHttpClientFactory() {
    @Override
    public OkHttpClient createOkHttpClient() {
        OkHttpClient client = new OkHttpClinet();
        client.setReadTimeout(60, TimeUnit.SECONDS);
        client.setWriteTimeout(60, TimeUnit.SECONDS);
        return client;
    }
});
```

### <a name="implement-a-progress-filter"></a>Een Filter voortgang implementeren

U kunt een intercept van elke aanvraag implementeren door het implementeren van een `ServiceFilter`.  Bijvoorbeeld updates de volgende van een vooraf gemaakte voortgangsbalk:

```java
private class ProgressFilter implements ServiceFilter {
    @Override
    public ListenableFuture<ServiceFilterResponse> handleRequest(ServiceFilterRequest request, NextServiceFilterCallback next) {
        final SettableFuture<ServiceFilterResponse> resultFuture = SettableFuture.create();
        runOnUiThread(new Runnable() {
            @Override
            public void run() {
                if (mProgressBar != null) mProgressBar.setVisibility(ProgressBar.VISIBLE);
            }
        });

        ListenableFuture<ServiceFilterResponse> future = next.onNext(request);
        Futures.addCallback(future, new FutureCallback<ServiceFilterResponse>() {
            @Override
            public void onFailure(Throwable e) {
                resultFuture.setException(e);
            }
            @Override
            public void onSuccess(ServiceFilterResponse response) {
                runOnUiThread(new Runnable() {
                    @Override
                    pubic void run() {
                        if (mProgressBar != null)
                            mProgressBar.setVisibility(ProgressBar.GONE);
                    }
                });
                resultFuture.set(response);
            }
        });
        return resultFuture;
    }
}
```

U kunt dit filter koppelen aan de client als volgt:

```java
mClient = new MobileServiceClient(applicationUrl).withFilter(new ProgressFilter());
```

### <a name="customize-request-headers"></a>Aanvraagheaders aanpassen

Gebruik de volgende `ServiceFilter` en koppelt u het filter op dezelfde manier als de `ProgressFilter`:

```java
private class CustomHeaderFilter implements ServiceFilter {
    @Override
    public ListenableFuture<ServiceFilterResponse> handleRequest(ServiceFilterRequest request, NextServiceFilterCallback next) {
        runOnUiThread(new Runnable() {
            @Override
            public void run() {
                request.addHeader("X-APIM-Router", "mobileBackend");
            }
        });
        SettableFuture<ServiceFilterResponse> result = SettableFuture.create();
        try {
            ServiceFilterResponse response = next.onNext(request).get();
            result.set(response);
        } catch (Exception exc) {
            result.setException(exc);
        }
    }
}
```

### <a name="conversions"></a>Automatische serialisatie configureren

U kunt opgeven dat een strategie voor een conversie die wordt toegepast op elke kolom met de [gson] [ 3] API. Maakt gebruik van de Android-clientbibliotheek [gson] [ 3] achter de schermen Java om objecten te serialiseren naar JSON-gegevens voordat de gegevens worden verzonden naar Azure App Service.  De volgende code gebruikt de **setFieldNamingStrategy()** methode voor het instellen van de strategie. In dit voorbeeld wordt het eerste teken (een ' m'), verwijderen en vervolgens kleine het volgende teken, voor elke veldnaam. Bijvoorbeeld, zou het veranderen 'mId' in 'id'.  Een strategie voor conversie zodat de noodzaak van implementeren `SerializedName()` aantekeningen in de meeste velden.

```java
FieldNamingStrategy namingStrategy = new FieldNamingStrategy() {
    public String translateName(File field) {
        String name = field.getName();
        return Character.toLowerCase(name.charAt(1)) + name.substring(2);
    }
}

client.setGsonBuilder(
    MobileServiceClient
        .createMobileServiceGsonBuilder()
        .setFieldNamingStrategy(namingStategy)
);
```

Deze code moet worden uitgevoerd voordat het maken van een mobiele clients verwijzing met de **MobileServiceClient**.

<!-- URLs. -->
[Get started with Azure Mobile Apps]: app-service-mobile-android-get-started.md
[ASCII control codes C0 and C1]: http://en.wikipedia.org/wiki/Data_link_escape_character#C1_set
[Mobile Services SDK for Android]: http://go.microsoft.com/fwlink/p/?LinkID=717033
[Azure portal]: https://portal.azure.com
[aan de slag met verificatie]: app-service-mobile-android-get-started-users.md
[1]: http://google-gson.googlecode.com/svn/trunk/gson/docs/javadocs/com/google/gson/JsonObject.html
[2]: http://hashtagfail.com/post/44606137082/mobile-services-android-serialization-gson
[3]: http://go.microsoft.com/fwlink/p/?LinkId=290801
[4]: http://go.microsoft.com/fwlink/p/?LinkId=296840
[5]: app-service-mobile-android-get-started-push.md
[6]: ../notification-hubs/notification-hubs-push-notification-overview.md#integration-with-app-service-mobile-apps
[7]: app-service-mobile-android-get-started-users.md#cache-tokens
[8]: http://azure.github.io/azure-mobile-apps-android-client/com/microsoft/windowsazure/mobileservices/table/MobileServiceTable.html
[9]: http://azure.github.io/azure-mobile-apps-android-client/com/microsoft/windowsazure/mobileservices/MobileServiceClient.html
[10]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
[11]: app-service-mobile-node-backend-how-to-use-server-sdk.md
[12]: http://azure.github.io/azure-mobile-apps-android-client/
[13]: app-service-mobile-android-get-started.md#create-a-new-azure-mobile-app-backend
[14]: http://go.microsoft.com/fwlink/p/?LinkID=717034
[15]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md#define-table-controller
[16]: app-service-mobile-node-backend-how-to-use-server-sdk.md#TableOperations
[17]: https://developer.android.com/reference/java/util/UUID.html
[18]: https://github.com/google/guava/wiki/ListenableFutureExplained
[19]: http://www.odata.org/documentation/odata-version-3-0/
[20]: http://hashtagfail.com/post/46493261719/mobile-services-android-querying
[21]: https://github.com/Azure-Samples/azure-mobile-apps-android-quickstart
[22]: ../app-service/app-service-mobile-how-to-configure-active-directory-authentication.md
[Future]: http://developer.android.com/reference/java/util/concurrent/Future.html
[AsyncTask]: http://developer.android.com/reference/android/os/AsyncTask.html
