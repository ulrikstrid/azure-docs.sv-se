---
title: Läs in data till Azure Data Lake Store med hjälp av Azure Data Factory | Microsoft Docs
description: Använd Azure Data Factory för att kopiera data till Azure Data Lake Store
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.topic: article
ms.date: 01/17/2018
ms.author: jingwang
ms.openlocfilehash: bf0d607d63a68a222a1d44d9cb05253497d12591
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 03/23/2018
---
# <a name="load-data-into-azure-data-lake-store-by-using-azure-data-factory"></a>Läs in data till Azure Data Lake Store med hjälp av Azure Data Factory

[Azure Data Lake Store](../data-lake-store/data-lake-store-overview.md) är en företagsomfattande storskalig lagringsplats för analytiska arbetsbelastningar för stordata. Azure Data Lake kan du samla in data av valfri storlek, typ och hastighet för införandet. Data hämtas i en enda plats för drifts- och undersökande analyser.

Azure Data Factory är en helt hanterad molnbaserade integration datatjänst. Du kan använda tjänsten för att fylla i lake med data från ditt befintliga system och spara tid när du skapar din Analyslösningar.

Azure Data Factory har följande fördelar för att läsa in data i Azure Data Lake Store:

* **Enkelt att ställa in**: en intuitiv 5-steg-guiden med ingen skript som krävs.
* **Omfattande stöd för datalager**: inbyggt stöd för ett stort utbud av lokala och molnbaserade dataarkiv. En detaljerad lista finns i tabellen i [stöds datalager](copy-activity-overview.md#supported-data-stores-and-formats).
* **Skyddade och kompatibla**: Data överförs över HTTPS eller ExpressRoute. Tjänsten för global närvaro garanterar att dina data aldrig lämnar geografisk gräns.
* **Högpresterande**: upp till 1 GB/s-datainläsning hastighet i Azure Data Lake Store. Mer information finns i [kopiera aktivitet prestanda](copy-activity-performance.md).

Den här artikeln visar hur du använder verktyget Data Factory kopieringsdata till _Läs in data från Amazon S3 till Azure Data Lake Store_. Du kan följa liknande steg för att kopiera data från andra typer av datalager.

> [!NOTE]
> Mer information finns i [kopiera data till och från Azure Data Lake Store med hjälp av Azure Data Factory](connector-azure-data-lake-store.md).
>
> Den här artikeln gäller för version 2 av Azure Data Factory, som för närvarande är en förhandsversion. Om du använder version 1 av Data Factory-tjänsten, som är allmänt tillgänglig (GA), se [Kopieringsaktiviteten i Azure Data Factory version 1](v1/data-factory-data-movement-activities.md).

## <a name="prerequisites"></a>Förutsättningar

* Azure-prenumeration: Om du inte har en Azure-prenumeration kan du skapa en [kostnadsfritt konto](https://azure.microsoft.com/free/) innan du börjar.
* Azure Data Lake Store: Om du inte har ett Data Lake Store-konto, se anvisningarna i [skapa ett Data Lake Store-konto](../data-lake-store/data-lake-store-get-started-portal.md#create-an-azure-data-lake-store-account).
* Amazon S3: Den här artikeln visar hur du kopierar data från Amazon S3. Du kan använda andra dataarkiv genom att följa liknande steg.

## <a name="create-a-data-factory"></a>Skapa en datafabrik

1. På den vänstra menyn väljer **ny** > **Data + analys** > **Data Factory**:
   
   ![Skapa en ny datafabrik](./media/load-data-into-azure-data-lake-store/new-azure-data-factory-menu.png)
2. I den **nya data factory** anger värden för fälten som visas i följande bild: 
      
   ![Sida för ny datafabrik](./media/load-data-into-azure-data-lake-store//new-azure-data-factory.png)
 
    * **Namnet**: Ange ett globalt unikt namn för din Azure data factory. Om du får felet ”datafabriksnamnet \"LoadADLSDemo\" är inte tillgängligt” ange ett annat namn för data factory. Du kan till exempel använda namnet  _**dittnamn**_**ADFTutorialDataFactory**. Försök att skapa datafabriken igen. Se artikeln [Data Factory – namnregler](naming-rules.md) för namnregler för Data Factory-artefakter.
    * **Prenumerationen**: Välj din Azure-prenumeration att skapa datafabriken. 
    * **Resursgruppen**: Välj en befintlig resursgrupp från den nedrullningsbara listan eller Välj den **Skapa nytt** och ange namnet på en resursgrupp. Mer information om resursgrupper finns i [Använda resursgrupper till att hantera Azure-resurser](../azure-resource-manager/resource-group-overview.md).  
    * **Version**: Välj **V2 (förhandsgranskning)**.
    * **Plats**: Välj platsen för data factory. Endast platser som stöds visas i listrutan. Datalager som används av datafabriken kan vara på andra platser och regioner. Lagrar dessa data inkluderar Azure Data Lake Store, Azure Storage, Azure SQL Database och så vidare.

3. Välj **Skapa**.
4. När du har skapats, gå till din data factory. Du ser den **Data Factory** startsidan som visas i följande bild: 
   
   ![Datafabrikens startsida](./media/load-data-into-azure-data-lake-store/data-factory-home-page.png)

   Välj den **författare & övervakaren** rutan för att starta programmet på Integration Data i en separat flik.

## <a name="load-data-into-azure-data-lake-store"></a>Läs in data till Azure Data Lake Store

1. I den **Kom igång** väljer den **kopieringsdata** rutan för att starta verktyget kopieringsdata: 

   ![Panel för verktyget Kopiera data](./media/load-data-into-azure-data-lake-store/copy-data-tool-tile.png)
2. I den **egenskaper** anger **CopyFromAmazonS3ToADLS** för den **aktivitet** och markerar **nästa**:

    ![Sidan Egenskaper](./media/load-data-into-azure-data-lake-store/copy-data-tool-properties-page.png)
3. I den **källa datalagret** väljer **Amazon S3**, och välj **nästa**:

    ![Sidan Källdatalager](./media/load-data-into-azure-data-lake-store/source-data-store-page.png)
4. I den **ange Amazon S3 anslutning** gör du följande: 
   1. Ange den **åtkomst nyckel-ID** värde.
   2. Ange den **hemlighet åtkomstnyckel** värde.
   3. Välj **Nästa**.
   
   ![Ange Amazon S3 konto](./media/load-data-into-azure-data-lake-store/specify-amazon-s3-account.png)
5. I den **Välj inkommande filen eller mappen** sidan, bläddra till mappen och filen som du vill kopiera över. Välj mappen/filen **Välj**, och välj sedan **nästa**:

    ![Välj indatafil eller mapp](./media/load-data-into-azure-data-lake-store/choose-input-folder.png)

6. I den **data målarkiv** väljer **Azure Data Lake Store**, och välj **nästa**:

    ![Sidan Måldatalager](./media/load-data-into-azure-data-lake-store/destination-data-storage-page.png)

7. Välj Kopiera beteendet genom att välja den **kopiera filer rekursivt** och **binära kopiera** (kopiera filer som-är) alternativ. Välj **nästa**:

    ![Ange utdatamapp](./media/load-data-into-azure-data-lake-store/specify-binary-copy.png)

8. I den **ange Data Lake Store anslutning** gör du följande: 

   1. Välj din Data Lake Store för den **Data Lake Store-kontonamnet**.
   2. Ange tjänstens huvudnamn information: **klient**, **tjänsten ägar-ID**, och **Service principal key**.
   3. Välj **Nästa**.
   
   > [!IMPORTANT]
   > I den här genomgången ska du använda en _tjänstens huvudnamn_ att autentisera ditt Data Lake Store. Se till att ge tjänstens huvudnamn i Azure Data Lake Store rätt behörighet genom att följa [instruktionerna](connector-azure-data-lake-store.md#using-service-principal-authentication).
   
   ![Ange Azure Data Lake Store-konto](./media/load-data-into-azure-data-lake-store/specify-adls.png)
9. I den **välja filen eller mappen** anger **copyfroms3** som utdata mappnamn och välj **nästa**: 

    ![Ange utdatamapp](./media/load-data-into-azure-data-lake-store/specify-adls-path.png)

10. I den **inställningar** väljer **nästa**:

    ![Sidan Inställningar](./media/load-data-into-azure-data-lake-store/copy-settings.png)
11. I den **sammanfattning** , granskar du inställningarna och väljer **nästa**:

    ![Sammanfattningssida](./media/load-data-into-azure-data-lake-store/copy-summary.png)
12. I den **distribution sida**väljer **övervakaren** att övervaka pipeline (aktivitet):

    ![Distributionssida](./media/load-data-into-azure-data-lake-store/deployment-page.png)
13. Observera att fliken **Övervaka** till vänster väljs automatiskt. Den **åtgärder** kolumn innehåller länkar för att visa aktivitet kör information och att köra pipelinen:

    ![Övervaka pipelinekörningar](./media/load-data-into-azure-data-lake-store/monitor-pipeline-runs.png)
14. Om du vill visa aktivitetskörningar som är associerade med pipeline kör, Välj den **visa aktiviteten körs** länken i den **åtgärder** kolumn. Det finns bara en aktivitet (kopieringsaktiviteten) i pipelinen. Därför visas bara en post. Växla tillbaka till pipelinen körs visa, Välj den **Pipelines** längst upp. Om du vill uppdatera listan väljer du **Uppdatera**. 

    ![Övervaka aktivitetskörningar](./media/load-data-into-azure-data-lake-store/monitor-activity-runs.png)

15. För att övervaka körning av information för varje kopieringsaktiviteten, Välj den **information** länken under **åtgärder** i aktiviteten övervakningsvyn. Du kan övervaka information som mängden data som kopieras från källan till mottagare, överföring av data, utförande med motsvarande varaktighet och används konfigurationer:

    ![Övervakaraktiviteten kör information](./media/load-data-into-azure-data-lake-store/monitor-activity-run-details.png)

16. Kontrollera att data kopieras till din Azure Data Lake Store: 

    ![Kontrollera Data Lake Store-utdata](./media/load-data-into-azure-data-lake-store/adls-copy-result.png)

## <a name="next-steps"></a>Nästa steg

Gå till följande artikel för att lära dig om Azure Data Lake Store-stöd: 

> [!div class="nextstepaction"]
>[Azure Data Lake Store-koppling](connector-azure-data-lake-store.md)