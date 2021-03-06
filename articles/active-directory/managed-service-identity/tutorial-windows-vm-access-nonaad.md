---
title: Använd en Windows VM-MSI för att komma åt Azure Key Vault
description: En självstudiekurs som vägleder dig genom processen med att använda en Windows VM hanterade tjänsten identitet (MSI) för att få åtkomst till Azure Key Vault.
services: active-directory
documentationcenter: ''
author: daveba
manager: mtillman
editor: daveba
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 11/20/2017
ms.author: daveba
ms.openlocfilehash: c65e2dc3d1b7a754bda54bb9127bbc777b514768
ms.sourcegitcommit: fa493b66552af11260db48d89e3ddfcdcb5e3152
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 04/23/2018
---
# <a name="use-a-windows-vm-managed-service-identity-msi-to-access-azure-key-vault"></a>Använd en Windows VM hanterade tjänsten identitet (MSI) för att komma åt Azure Key Vault 

[!INCLUDE[preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

Den här kursen visar hur du aktiverar hanterade tjänsten identitet (MSI) för en virtuell Windows-dator och sedan använda identitet för åtkomst till Azure Key Vault. Fungerar som en bootstrap gör Key Vault det möjligt för ditt klientprogram att använda hemligheten som ska få tillgång till resurser som inte skyddas av Azure Active Directory (AD). Hanterade Tjänsteidentiteter hanteras automatiskt av Azure och gör att du kan autentisera tjänster som stöder Azure AD-autentisering utan att behöva infoga autentiseringsuppgifter i din kod. 

Lär dig att:


> [!div class="checklist"]
> * Aktivera hanterade tjänstidentiteten på en virtuell dator för Windows 
> * Ge dina VM-åtkomst till en hemlighet som lagras i ett Nyckelvalv 
> * Hämta en åtkomst-token med VM-identitet och använda den för att hämta hemligheten från Nyckelvalvet 

## <a name="prerequisites"></a>Förutsättningar

[!INCLUDE [msi-qs-configure-prereqs](../../../includes/active-directory-msi-qs-configure-prereqs.md)]

[!INCLUDE [msi-tut-prereqs](../../../includes/active-directory-msi-tut-prereqs.md)]

## <a name="sign-in-to-azure"></a>Logga in på Azure

Logga in på Azure Portal på [https://portal.azure.com](https://portal.azure.com).

## <a name="create-a-windows-virtual-machine-in-a-new-resource-group"></a>Skapa en virtuell Windows-dator i en ny resursgrupp

Den här självstudiekursen skapar vi en ny Windows virtuell dator. Du kan också aktivera MSI på en befintlig virtuell dator.

1.  Klicka på knappen **Skapa en resurs** längst upp till vänster i Azure Portal.
2.  Välj **Compute**, och välj sedan **Windows Server 2016 Datacenter**. 
3.  Ange informationen för den virtuella datorn. Den **användarnamn** och **lösenord** skapade här är de autentiseringsuppgifter som du använder för att logga in på den virtuella datorn.
4.  Välj rätt **prenumeration** för den virtuella datorn i listrutan.
5.  Att välja en ny **resursgruppen** du skulle vilja virtuella datorn ska skapas i, Välj **Skapa nytt**. När du är klar klickar du på **OK**.
6.  Välj storlek för den virtuella datorn. Om du vill se fler storlekar väljer du **Visa alla** eller så ändrar du filtret för **disktyper som stöds**. Acceptera alla standardvärden på bladet Inställningar och klicka på **OK**.

    ![ALT bildtext](../media/msi-tutorial-windows-vm-access-arm/msi-windows-vm.png)

## <a name="enable-msi-on-your-vm"></a>Aktivera MSI på den virtuella datorn 

En virtuell dator MSI kan du få åtkomst-token från Azure AD utan att du behöver publicera autentiseringsuppgifter i koden. Aktivera MSI visar Azure för att skapa en hanterad identitet för den virtuella datorn. Under försättsbladen, aktivera MSI gör två saker: registrerar den virtuella datorn med Azure Active Directory för att skapa det hanterade identitet och konfigurerar identiteten på den virtuella datorn.

1.  Välj den **virtuella** som du vill aktivera MSI på.  
2.  Klicka på det vänstra navigeringsfältet **Configuration**. 
3.  Du ser **hanterade tjänstidentiteten**. För att registrera och aktivera MSI-filerna, Välj **Ja**, om du vill inaktivera det, väljer du Nej. 
4.  Se till att du klickar på **spara** att spara konfigurationen.  

    ![ALT bildtext](../media/msi-tutorial-linux-vm-access-arm/msi-linux-extension.png)

## <a name="grant-your-vm-access-to-a-secret-stored-in-a-key-vault"></a>Ge dina VM-åtkomst till en hemlighet som lagras i ett Nyckelvalv 
 
Med hjälp av MSI hämta koden åtkomsttoken att autentisera till resurser som stöder Azure AD-autentisering.  Dock stöder inte alla Azure-tjänster Azure AD-autentisering. Om du vill använda MSI med dessa tjänster lagra autentiseringsuppgifterna för tjänsten i Azure Key Vault och använda MSI åtkomst till Nyckelvalvet för att hämta autentiseringsuppgifterna. 

Vi behöver först skapa ett Nyckelvalv och ge våra VM identitet åtkomst till Nyckelvalvet.   

1. Längst upp i det vänstra navigeringsfältet, Välj **skapar du en resurs** > **säkerhet + identitet** > **Nyckelvalvet**.  
2. Ange en **namn** för nya Nyckelvalvet. 
3. Leta upp Nyckelvalvet i gruppen samma prenumeration och resurs som den virtuella datorn som du skapade tidigare. 
4. Välj **åtkomstprinciper** och på **Lägg till ny**. 
5. Välj i Konfigurera från mallen **hemlighet Management**. 
6. Välj **Välj huvudnamn**, och ange namnet på den virtuella datorn som du skapade tidigare i sökfältet.  Välj den virtuella datorn i resultatlistan och klicka på **Välj**. 
7. Klicka på **OK** till slutar att lägga till nya åtkomstprincipen och **OK** Slutför princip valet. 
8. Klicka på **skapa** skapa Nyckelvalvet. 

    ![ALT bildtext](../media/msi-tutorial-windows-vm-access-nonaad/msi-blade.png)


Lägg sedan till en hemlighet i nyckelvalvet, så att senare kan du hämta hemligheten med kod som körs i den virtuella datorn: 

1. Välj **alla resurser**, och leta upp och markera Nyckelvalv som du skapade. 
2. Välj **hemligheter**, och klicka på **Lägg till**. 
3. Välj **manuell**, från **överför alternativ**. 
4. Ange namn och värde för hemligheten.  Värdet kan vara vad du vill. 
5. Lämna aktiveringsdatumet och avmarkera förfallodatum och lämna **aktiverad** som **Ja**. 
6. Klicka på **skapa** att skapa hemligheten. 
 
## <a name="get-an-access-token-using-the-vm-identity-and-use-it-to-retrieve-the-secret-from-the-key-vault"></a>Hämta en åtkomst-token med VM-identitet och använda den för att hämta hemligheten från Nyckelvalvet  

Om du inte har PowerShell 4.3.1 eller högre har installerats, måste du [ladda ned och installera den senaste versionen](https://docs.microsoft.com/powershell/azure/overview).

Vi använder först MSI för den virtuella datorn för att få en åtkomsttoken att autentisera till Key Vault:
 
1. I portalen, går du till **virtuella datorer** och gå till din Windows-dator och i den **översikt**, klickar du på **Anslut**.
2. Ange i din **användarnamn** och **lösenord** för som du har lagt till när du skapade den **Windows VM**.  
3. Nu när du har skapat en **anslutning till fjärrskrivbord** med den virtuella datorn öppnar du PowerShell i fjärrsessionen.  
4. Anropa webbegäran för innehavaren att hämta token för den lokala värden i den specifika porten för den virtuella datorn i PowerShell.  

    PowerShell-begäran:
    
    ```powershell
    $response = Invoke-WebRequest -Uri 'http://169.254.169.254/metadata/identity/oauth2/token?api-version=2018-02-01&resource=https%3A%2F%2Fvault.azure.net' -Method GET -Headers @{Metadata="true"} 
    ```
    
    Därefter extraheras det fullständiga svaret som lagras som en JavaScript Object Notation (JSON) formaterad sträng i $response-objektet.  
    
    ```powershell
    $content = $response.Content | ConvertFrom-Json 
    ```
    
    Extrahera sedan den åtkomst-token från svaret.  
    
    ```powershell
    $KeyVaultToken = $content.access_token 
    ```
    
    Använd slutligen PowerShells Invoke-WebRequest kommando för att hämta den hemlighet som du skapade tidigare i Nyckelvalvet, skicka åtkomst-token i auktoriseringshuvudet.  Du behöver URL-Adressen till ditt Nyckelvalv som finns i den **Essentials** avsnitt i den **översikt** sida i Nyckelvalvet.  
    
    ```powershell
    (Invoke-WebRequest -Uri https://<your-key-vault-URL>/secrets/<secret-name>?api-version=2016-10-01 -Method GET -Headers @{Authorization="Bearer $KeyVaultToken"}).content 
    ```
    
    Svaret ser ut så här: 
    
    ```powershell
    {"value":"p@ssw0rd!","id":"https://mytestkeyvault.vault.azure.net/secrets/MyTestSecret/7c2204c6093c4d859bc5b9eff8f29050","attributes":{"enabled":true,"created":1505088747,"updated":1505088747,"recoveryLevel":"Purgeable"}} 
    ```
    
När du har hämtat hemligheten från Nyckelvalvet kan använda du den för att autentisera till en tjänst som kräver ett användarnamn och lösenord. 

## <a name="related-content"></a>Relaterat innehåll

- En översikt över MSI finns [hanterade tjänstidentiteten översikt](overview.md).

Använd följande avsnitt för kommentarer för att ge feedback och hjälp oss att förfina och utforma innehållet.
