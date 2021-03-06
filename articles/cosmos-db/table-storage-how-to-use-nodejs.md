---
title: Hur du använder Azure Table storage eller Azure Cosmos DB från Node.js | Microsoft Docs
description: Lagra strukturerade data i molnet med Azure Table storage eller Azure Cosmos DB.
services: cosmos-db
documentationcenter: nodejs
author: SnehaGunda
manager: kfile
ms.assetid: fc2e33d2-c5da-4861-8503-53fdc25750de
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: article
ms.date: 04/05/2018
ms.author: sngun
ms.openlocfilehash: 3f1908a6c2d129da44e0719b2cf69cf09baef356
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 04/16/2018
---
# <a name="how-to-use-azure-table-storage-from-nodejs"></a>Hur du använder Azure Table storage från Node.js
[!INCLUDE [storage-selector-table-include](../../includes/storage-selector-table-include.md)]
[!INCLUDE [storage-table-cosmos-db-tip-include](../../includes/storage-table-cosmos-db-tip-include.md)]

## <a name="overview"></a>Översikt
Den här artikeln visar hur du utför vanliga scenarier med tabelltjänsten för Azure Storage eller Azure Cosmos DB på ett Node.js-program.

## <a name="create-an-azure-service-account"></a>Skapa ett konto i Azure-tjänst

[!INCLUDE [cosmos-db-create-azure-service-account](../../includes/cosmos-db-create-azure-service-account.md)]

### <a name="create-an-azure-storage-account"></a>Skapa ett Azure-lagringskonto

[!INCLUDE [cosmos-db-create-storage-account](../../includes/cosmos-db-create-storage-account.md)]

### <a name="create-an-azure-cosmos-db-table-api-account"></a>Skapa ett Azure Cosmos DB tabell API-konto

[!INCLUDE [cosmos-db-create-tableapi-account](../../includes/cosmos-db-create-tableapi-account.md)]

## <a name="configure-your-application-to-access-azure-storage"></a>Konfigurera ditt program för att få åtkomst till Azure Storage
Om du vill använda Azure Storage eller Azure Cosmos DB, behöver du Azure Storage SDK: N för Node.js som innehåller en uppsättning bekvämlighet bibliotek som kommunicerar med Storage REST-tjänster.

### <a name="use-node-package-manager-npm-to-install-the-package"></a>Använd noden Package Manager (NPM) för att installera paketet
1. Använd ett kommandoradsgränssnitt som **PowerShell** (Windows), **Terminal** (Mac), eller **Bash** (Unix) och navigera till mappen där du skapade ditt program.
2. Typen **npm installera azure-lagring** i kommandofönstret. Utdata från kommandot liknar följande exempel.

       azure-storage@0.5.0 node_modules\azure-storage
       +-- extend@1.2.1
       +-- xmlbuilder@0.4.3
       +-- mime@1.2.11
       +-- node-uuid@1.4.3
       +-- validator@3.22.2
       +-- underscore@1.4.4
       +-- readable-stream@1.0.33 (string_decoder@0.10.31, isarray@0.0.1, inherits@2.0.1, core-util-is@1.0.1)
       +-- xml2js@0.2.7 (sax@0.5.2)
       +-- request@2.57.0 (caseless@0.10.0, aws-sign2@0.5.0, forever-agent@0.6.1, stringstream@0.0.4, oauth-sign@0.8.0, tunnel-agent@0.4.1, isstream@0.1.2, json-stringify-safe@5.0.1, bl@0.9.4, combined-stream@1.0.5, qs@3.1.0, mime-types@2.0.14, form-data@0.2.0, http-signature@0.11.0, tough-cookie@2.0.0, hawk@2.3.1, har-validator@1.8.0)
3. Du kan köra manuellt på **ls** kommando för att kontrollera att en **node_modules** mappen har skapats. I mappen hittar du den **azure storage** paket som innehåller de bibliotek som du behöver åtkomst till lagring.

### <a name="import-the-package"></a>Importera paketet
Lägg till följande kod högst upp i den **server.js** filen i ditt program:

```nodejs
var azure = require('azure-storage');
```

## <a name="add-an-azure-storage-connection"></a>Lägg till ett Azure Storage-anslutning
Azure-modulen läser miljövariablerna AZURE_STORAGE_ACCOUNT och AZURE_STORAGE_ACCESS_KEY eller AZURE_STORAGE_CONNECTION_STRING information som krävs för att ansluta till ditt Azure Storage-konto. Om de här miljövariablerna inte har angetts måste du ange kontoinformationen vid anrop av **TableService**. Till exempel följande kod skapar en **TableService** objekt:

```nodejs
var tableSvc = azure.createTableService('myaccount', 'myaccesskey');
```

## <a name="add-an-azure-comsos-db-connection"></a>Lägg till en Azure Comsos DB-anslutning
Om du vill lägga till en Azure Cosmos DB-anslutning, skapa en **TableService** objektet och ange ditt kontonamn, primärnyckel och slutpunkt. Du kan kopiera värdena från **inställningar** > **anslutningssträngen** i Azure-portalen för Cosmos-DB-konto. Exempel:

```nodejs
var tableSvc = azure.createTableService('myaccount', 'myprimarykey', 'myendpoint');
```  

## <a name="create-a-table"></a>Skapa en tabell
Följande kod skapar en **TableService** objekt och används för att skapa en ny tabell. 

```nodejs
var tableSvc = azure.createTableService();
```

Anropet till **createTableIfNotExists** skapar en ny tabell med det angivna namnet om det inte redan finns. I följande exempel skapas en ny tabell med namnet ”mytable” som prefix om det inte redan finns:

```nodejs
tableSvc.createTableIfNotExists('mytable', function(error, result, response){
  if(!error){
    // Table exists or created
  }
});
```

Den `result.created` är `true` om en ny tabell skapas och `false` om tabellen redan finns. Den `response` innehåller information om begäran.

### <a name="filters"></a>Filter
Du kan använda valfri filtrering till åtgärder som utförs med hjälp av **TableService**. Filtrering operations kan innehålla loggning, automatiska omförsök, osv. Filtren är objekt som implementerar en metod med signaturen:

```nodejs
function handle (requestOptions, next)
```

När du har gjort dess förbearbetning på begäran-alternativ måste anropa metoden **nästa**, skicka ett återanrop med följande signatur:

```nodejs
function (returnObject, finalCallback, next)
```

I den här motringning och efter bearbetning av **returnObject** (svaret från begäran till servern), återanropet måste antingen anropa **nästa** om den finns för att fortsätta att bearbeta filter eller helt enkelt anropa **finalCallback** på annat sätt att avsluta tjänsten-anrop.

Två filter som implementerar logik som medföljer Azure SDK för Node.js, **ExponentialRetryPolicyFilter** och **LinearRetryPolicyFilter**. Följande kod skapar en **TableService** objekt som använder den **ExponentialRetryPolicyFilter**:

```nodejs
var retryOperations = new azure.ExponentialRetryPolicyFilter();
var tableSvc = azure.createTableService().withFilter(retryOperations);
```

## <a name="add-an-entity-to-a-table"></a>Lägga till en entitet i en tabell
Om du vill lägga till en enhet måste du först skapa ett objekt som definierar egenskaper för enhet. Alla enheter måste innehålla en **PartitionKey** och **RowKey**, som är unika identifierare för entiteten.

* **PartitionKey** -avgör den partition som är lagrade i entiteten.
* **RowKey** - unikt identifierar entiteten i partitionen.

Båda **PartitionKey** och **RowKey** måste vara strängvärden. Mer information finns i [förstå den tabelltjänst-datamodellen](http://msdn.microsoft.com/library/azure/dd179338.aspx).

Följande är ett exempel för att definiera en entitet. Observera att **dueDate** definieras som en typ av **Edm.DateTime**. Anger vilken är valfri och typer är oavsiktliga om inget anges.

```nodejs
var task = {
  PartitionKey: {'_':'hometasks'},
  RowKey: {'_': '1'},
  description: {'_':'take out the trash'},
  dueDate: {'_':new Date(2015, 6, 20), '$':'Edm.DateTime'}
};
```

> [!NOTE]
> Det finns också en **tidsstämpel** för varje post som anges av Azure när en entitet infogas eller uppdateras.
>
>

Du kan också använda den **entityGenerator** att skapa entiteter. Följande exempel skapar samma aktivitet entitet med den **entityGenerator**.

```nodejs
var entGen = azure.TableUtilities.entityGenerator;
var task = {
  PartitionKey: entGen.String('hometasks'),
  RowKey: entGen.String('1'),
  description: entGen.String('take out the trash'),
  dueDate: entGen.DateTime(new Date(Date.UTC(2015, 6, 20))),
};
```

Om du vill lägga till en entitet i tabellen, skicka enhetsobjekt till den **insertEntity** metod.

```nodejs
tableSvc.insertEntity('mytable',task, function (error, result, response) {
  if(!error){
    // Entity inserted
  }
});
```

Om åtgärden lyckas `result` innehåller den [ETag](http://en.wikipedia.org/wiki/HTTP_ETag) infogade postens och `response` innehåller information om åtgärden.

Exempelsvar:

```nodejs
{ '.metadata': { etag: 'W/"datetime\'2015-02-25T01%3A22%3A22.5Z\'"' } }
```

> [!NOTE]
> Som standard **insertEntity** returnerar inte infogade entiteten som en del av den `response` information. Om du planerar att andra åtgärder pågår på den här entiteten eller vill cachelagra informationen, kan det vara praktiskt att ha den returneras som en del av den `result`. Du kan göra detta genom att aktivera **echoContent** på följande sätt:
>
> `tableSvc.insertEntity('mytable', task, {echoContent: true}, function (error, result, response) {...}`
>
>

## <a name="update-an-entity"></a>Uppdatera en entitet
Det finns flera metoder för att uppdatera en befintlig entitet:

* **replaceEntity** -uppdaterar en befintlig entitet genom att ersätta den.
* **mergeEntity** -uppdaterar en befintlig entitet genom att använda nya egenskapsvärden i befintliga entiteten.
* **insertOrReplaceEntity** -uppdaterar en befintlig entitet genom att ersätta den. Om inga entiteten finns infogas en ny.
* **insertOrMergeEntity** -uppdaterar en befintlig entitet genom att använda den nya egenskapsvärden i den befintliga. Om inga entiteten finns infogas en ny.

Exemplet nedan visar att uppdatera en entitet med **replaceEntity**:

```nodejs
tableSvc.replaceEntity('mytable', updatedTask, function(error, result, response){
  if(!error) {
    // Entity updated
  }
});
```

> [!NOTE]
> Som standard kontrollerar uppdaterar en entitet inte om du vill se om data uppdateras tidigare har ändrats av en annan process. Till stöd för samtidiga uppdateringar:
>
> 1. Hämta ETag i objektet som uppdateras. Det här felet returneras som en del av den `response` för varje entitet-relaterade åtgärden och kan hämtas via `response['.metadata'].etag`.
> 2. När du utför en update-åtgärd på en enhet, kan du lägga till ETag-information som tidigare har hämtats till den nya entiteten. Exempel:
>
>       entity2 [.metadata] .etag = currentEtag;
> 3. Utföra uppdateringen. Om entiteten har ändrats sedan du hämtade ETag-värde, till exempel en annan instans av programmet, en `error` returneras om villkoret uppdatering som anges i begäran att inte uppfylldes.
>
>

Med **replaceEntity** och **mergeEntity**om entiteten som ska uppdateras inte finns, misslyckas uppdateringsåtgärden; därför, om du vill spara en entitet oavsett om det redan finns, Använd **insertOrReplaceEntity** eller **insertOrMergeEntity**.

Den `result` lyckade uppdateringsåtgärder innehåller den **Etag** för entiteten uppdaterade.

## <a name="work-with-groups-of-entities"></a>Arbeta med grupper av entiteter
Ibland är det praktiskt att skicka flera åtgärder tillsammans i en grupp så atomiska bearbetning av servern. Om du vill göra det använder den **TableBatch** klassen om du vill skapa en grupp och sedan använda den **executeBatch** metod för **TableService** utföra gruppbaserad åtgärder.

 Exemplet nedan visar skickar två entiteter i en batch:

```nodejs
var task1 = {
  PartitionKey: {'_':'hometasks'},
  RowKey: {'_': '1'},
  description: {'_':'Take out the trash'},
  dueDate: {'_':new Date(2015, 6, 20)}
};
var task2 = {
  PartitionKey: {'_':'hometasks'},
  RowKey: {'_': '2'},
  description: {'_':'Wash the dishes'},
  dueDate: {'_':new Date(2015, 6, 20)}
};

var batch = new azure.TableBatch();

batch.insertEntity(task1, {echoContent: true});
batch.insertEntity(task2, {echoContent: true});

tableSvc.executeBatch('mytable', batch, function (error, result, response) {
  if(!error) {
    // Batch completed
  }
});
```

För lyckad batchåtgärder `result` innehåller information för varje åtgärd i batchen.

### <a name="work-with-batched-operations"></a>Arbeta med batch-åtgärder
Du kan inspektera åtgärder som lagts till i en batch med visning av `operations` egenskapen. Du kan också använda följande metoder för att arbeta med åtgärder:

* **Rensa** -rensar alla åtgärder från en batch.
* **getOperations** -hämtar en åtgärd från gruppen.
* **hasOperations** -returnerar true om batchen innehåller åtgärder.
* **removeOperations** -tar bort en åtgärd.
* **storlek** -returnerar antalet åtgärder i batchen.

## <a name="retrieve-an-entity-by-key"></a>Hämta en entitet med nyckeln
Returnera en specifik enhet baserat på de **PartitionKey** och **RowKey**, använda den **retrieveEntity** metod.

```nodejs
tableSvc.retrieveEntity('mytable', 'hometasks', '1', function(error, result, response){
  if(!error){
    // result contains the entity
  }
});
```

När den här åtgärden är klar `result` innehåller entiteten.

## <a name="query-a-set-of-entities"></a>Fråga en uppsättning enheter
Om du vill fråga en tabell, använder den **TableQuery** objekt att bygga upp ett frågeuttryck med hjälp av följande:

* **Välj** -fält som ska returneras från frågan.
* **där** -where satsen.

  * **och** – en `and` where-villkor.
  * **eller** – en `or` where-villkor.
* **TOP** -antal objekt som ska hämtas.

I följande exempel skapas en fråga som returnerar de översta fem posterna med en PartitionKey av 'hometasks'.

```nodejs
var query = new azure.TableQuery()
  .top(5)
  .where('PartitionKey eq ?', 'hometasks');
```

Eftersom **Välj** inte används, returneras alla fält. Använd för att utföra frågan mot en tabell **queryEntities**. I följande exempel används den här frågan för att returnera enheter från ”mytable” som prefix.

```nodejs
tableSvc.queryEntities('mytable',query, null, function(error, result, response) {
  if(!error) {
    // query was successful
  }
});
```

Om detta lyckas `result.entries` innehåller en matris med entiteter som matchar frågan. Om frågan inte kunde returnera alla entiteter `result.continuationToken` är icke -*null* och kan användas som den tredje parametern för **queryEntities** att hämta fler resultat. Den inledande frågan använder *null* för den tredje parametern.

### <a name="query-a-subset-of-entity-properties"></a>Fråga en deluppsättning entitetsegenskaper
En fråga till en tabell kan hämta bara några fält från en entitet.
Detta minskar bandbredden och kan förbättra frågeprestanda, särskilt för stora entiteter. Använd den **Välj** satsen och ange namnen på de fält som ska returneras. Följande fråga returnerar till exempel bara den **beskrivning** och **dueDate** fält.

```nodejs
var query = new azure.TableQuery()
  .select(['description', 'dueDate'])
  .top(5)
  .where('PartitionKey eq ?', 'hometasks');
```

## <a name="delete-an-entity"></a>Ta bort en entitet
Du kan ta bort en entitet med dess partition och raden nycklar. I det här exemplet i **task1** objektet innehåller de **RowKey** och **PartitionKey** värden för att ta bort entiteten. Sedan objektet skickas till den **deleteEntity** metod.

```nodejs
var task = {
  PartitionKey: {'_':'hometasks'},
  RowKey: {'_': '1'}
};

tableSvc.deleteEntity('mytable', task, function(error, response){
  if(!error) {
    // Entity deleted
  }
});
```

> [!NOTE]
> Överväg att använda ETags när du tar bort objekt för att se till att objektet inte har ändrats av en annan process. Se [uppdatera en entitet](#update-an-entity) information om hur du använder ETags.
>
>

## <a name="delete-a-table"></a>Ta bort en tabell
Följande kod tar bort en tabell från ett lagringskonto.

```nodejs
tableSvc.deleteTable('mytable', function(error, response){
    if(!error){
        // Table deleted
    }
});
```

Om du är osäker på om tabellen finns, Använd **deleteTableIfExists**.

## <a name="use-continuation-tokens"></a>Använd fortsättning token
När du frågar tabeller för stora mängder resultat, leta efter fortsättning token. Det kan finnas stora mängder data tillgängliga för din fråga som du inte kanske vet om du inte skapa känna igen när en fortsättningstoken finns.

Den **resultat** objektet som returnerades vid fråga entiteter anger en `continuationToken` egenskapen när dessa token finns. Du kan sedan använda den när du utför en fråga för att fortsätta att flytta mellan entiteterna partition och tabellen.

När du frågar, kan du ange en `continuationToken` parametern mellan objektinstansen frågan och Återanropsfunktionen:

```nodejs
var nextContinuationToken = null;
dc.table.queryEntities(tableName,
    query,
    nextContinuationToken,
    function (error, results) {
        if (error) throw error;

        // iterate through results.entries with results

        if (results.continuationToken) {
            nextContinuationToken = results.continuationToken;
        }

    });
```

Om du vill granska den `continuationToken` objekt, hittar du egenskaper som `nextPartitionKey`, `nextRowKey` och `targetLocation`, som kan användas för att söka igenom alla resultat.

## <a name="work-with-shared-access-signatures"></a>Arbeta med signaturer för delad åtkomst
Signaturer för delad åtkomst (SAS) är ett säkert sätt att tillhandahålla detaljerade åtkomst till tabeller utan att erbjuda dina lagringskontonamn eller nycklar. SAS används ofta för att ge begränsad åtkomst till dina data, till exempel att tillåta en mobil app om du vill söka efter poster.

En betrodda program, till exempel en molnbaserad tjänst genererar en SAS med hjälp av den **generateSharedAccessSignature** av den **TableService**, och som ger den till ett program som inte är betrodd eller delvis betrodd en mobil app. SAS genereras med hjälp av en princip som beskriver de då SAS är giltig start- och slutdatum samt den åtkomstnivå som beviljats till SAS-innehavare.

I följande exempel skapar en ny princip för delad åtkomst som låter innehavaren SAS att fråga (”r”) i tabell och upphör att gälla 100 minuter efter den tidpunkt som den har skapats.

```nodejs
var startDate = new Date();
var expiryDate = new Date(startDate);
expiryDate.setMinutes(startDate.getMinutes() + 100);
startDate.setMinutes(startDate.getMinutes() - 100);

var sharedAccessPolicy = {
  AccessPolicy: {
    Permissions: azure.TableUtilities.SharedAccessPermissions.QUERY,
    Start: startDate,
    Expiry: expiryDate
  },
};

var tableSAS = tableSvc.generateSharedAccessSignature('mytable', sharedAccessPolicy);
var host = tableSvc.host;
```

Observera att du måste också ange värdinformationen som det är nödvändigt när SAS-innehavaren försöker få åtkomst till tabellen.

Klientprogrammet sedan använder SAS med **TableServiceWithSAS** att utföra åtgärder mot tabellen. I följande exempel ansluter till tabellen och utför en fråga.

```nodejs
var sharedTableService = azure.createTableServiceWithSas(host, tableSAS);
var query = azure.TableQuery()
  .where('PartitionKey eq ?', 'hometasks');

sharedTableService.queryEntities(query, null, function(error, result, response) {
  if(!error) {
    // result contains the entities
  }
});
```

Eftersom SAS genererades med endast fråga åtkomst, returneras ett fel om du försöker infoga, uppdatera eller ta bort enheter.

### <a name="access-control-lists"></a>Åtkomstkontrollistor
Du kan också använda en åtkomstkontrollista (ACL) för att ange åtkomstprincipen för en SAS. Detta är användbart om du vill att flera klienter kan komma åt tabellen, men ger olika åtkomstprinciper för varje klient.

En ACL implementeras med hjälp av en matris med principer för åtkomst med ett ID som är associerade med varje princip. I följande exempel definierar två principer, en för 'Användare1' och en för 'användare2':

```nodejs
var sharedAccessPolicy = {
  user1: {
    Permissions: azure.TableUtilities.SharedAccessPermissions.QUERY,
    Start: startDate,
    Expiry: expiryDate
  },
  user2: {
    Permissions: azure.TableUtilities.SharedAccessPermissions.ADD,
    Start: startDate,
    Expiry: expiryDate
  }
};
```

I följande exempel hämtar den aktuella ACL för den **hometasks** tabell och lägger sedan till de nya principer med hjälp av **setTableAcl**. Den här metoden kan:

```nodejs
var extend = require('extend');
tableSvc.getTableAcl('hometasks', function(error, result, response) {
if(!error){
    var newSignedIdentifiers = extend(true, result.signedIdentifiers, sharedAccessPolicy);
    tableSvc.setTableAcl('hometasks', newSignedIdentifiers, function(error, result, response){
      if(!error){
        // ACL set
      }
    });
  }
});
```

Du kan sedan skapa en SAS baserat på en princip-ID när Åtkomstkontrollistan har angetts. I följande exempel skapas en ny SAS för 'användare2':

```nodejs
tableSAS = tableSvc.generateSharedAccessSignature('hometasks', { Id: 'user2' });
```

## <a name="next-steps"></a>Nästa steg
Mer information finns i följande resurser.

* [Microsoft Azure Storage Explorer](../vs-azure-tools-storage-manage-with-storage-explorer.md) är en kostnadsfri, fristående app från Microsoft som gör det möjligt att arbeta visuellt med Azure Storage-data i Windows, macOS och Linux.
* [Azure Storage-SDK för Node.js](https://github.com/Azure/azure-storage-node) databasen på GitHub.
* [Azure för Node.js-utvecklare](https://docs.microsoft.com/javascript/azure/?view=azure-node-latest)
* [Skapa en Node.js-webbapp i Azure](../app-service/app-service-web-get-started-nodejs.md)
* [Skapa och distribuera ett Node.js-program till en Azure-molntjänst](../cloud-services/cloud-services-nodejs-develop-deploy-app.md) (med Windows PowerShell)
