---
title: Distribuera LEMP på en virtuell Linux-dator i Azure | Microsoft Docs
description: 'Självstudier: Installera LEMP-stacken på en virtuell Linux-dator i Azure'
services: virtual-machines-linux
documentationcenter: virtual-machines
author: dlepow
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: azurecli
ms.topic: tutorial
ms.date: 11/27/2017
ms.author: danlep
ms.openlocfilehash: f907b468a409135d4b45e76297fc7cd86eeead78
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: HT
ms.contentlocale: sv-SE
ms.lasthandoff: 04/06/2018
---
# <a name="install-a-lemp-web-server-on-an-azure-vm"></a>Installera en LEMP-webbserver på en virtuell Azure-dator
I den här artikeln får du veta hur du distribuerar en NGINX-webbserver, MySQL och PHP (LEMP-stacken) på en virtuell Ubuntu-dator i Azure. LEMP-stacken är ett alternativ till den populära [LAMP-stacken](tutorial-lamp-stack.md) som du också kan installera i Azure. Om du vill se LEMP-servern i praktiken kan du installera och konfigurera en WordPress-webbplats. I den här självstudiekursen får du lära du dig att:

> [!div class="checklist"]
> * Skapa en virtuell Ubuntu-dator (L:et i LEMP-stacken)
> * Öppna port 80 för webbtrafik
> * Installera NGINX, MySQL och PHP
> * Verifiera installation och konfiguration
> * Installera WordPress på LEMP-servern


Den här installationen är avsedd för snabbtester och konceptbevis.

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Om du väljer att installera och använda CLI lokalt kräver de här självstudierna att du kör Azure CLI version 2.0.4 eller senare. Kör `az --version` för att hitta versionen. Om du behöver installera eller uppgradera kan du läsa [Installera Azure CLI 2.0]( /cli/azure/install-azure-cli). 

[!INCLUDE [virtual-machines-linux-tutorial-stack-intro.md](../../../includes/virtual-machines-linux-tutorial-stack-intro.md)]

## <a name="install-nginx-mysql-and-php"></a>Installera NGINX, MySQL och PHP

Kör följande kommandon för att uppdatera Ubuntu-paketkällorna och installera NGINX, MySQL och PHP. 

```bash
sudo apt update && sudo apt install nginx mysql-server php-mysql php php-fpm
```

Du uppmanas att installera paketen och andra beroenden. När du uppmanas anger du ett rotlösenord för MySQL. Välj [Retur] för att fortsätta. Följ återstående anvisningar. Den här processen installerar lägsta nödvändiga PHP-tillägg för användning av PHP med MySQL. 

![MySQL-rotlösenordssida][1]

## <a name="verify-installation-and-configuration"></a>Verifiera installation och konfiguration


### <a name="nginx"></a>NGINX

Kontrollera version av NGINX med följande kommando:
```bash
nginx -v
```

När NGINX är installerat och port 80 är öppen för den virtuella datorn kan webbservern nås från internet. När du vill visa NGINX-startsidan öppnar du en webbläsare och anger den virtuella datorns offentliga IP-adress. Använd den offentliga IP-adressen som du använde för SSH till den virtuella datorn:

![NGINX-standardsida][3]


### <a name="mysql"></a>MySQL

Kontrollera versionen av MySQL med följande kommando (observera versal i parameter `V`):

```bash
mysql -V
```

Kör skriptet `mysql_secure_installation` för att skydda installationen av MySQL. Om du bara konfigurerar en tillfällig server kan du hoppa över det här steget. 

```bash
mysql_secure_installation
```

Ange ett rotlösenord för MySQL och konfigurera säkerhetsinställningarna för miljön.

Om du vill prova MySQL-funktioner (skapa en MySQL-databas, lägga till användare eller ändra konfigurationsinställningar) loggar du in på MySQL. Det här steget krävs inte för att genomföra kursen. 


```bash
mysql -u root -p
```

När du är klar lämnar du mysql-frågan genom att skriva `\q`.

### <a name="php"></a>PHP

Kontrollera versionen av PHP med följande kommando:

```bash
php -v
```

Konfigurera NGINX för att använda PHP FastCGI Process Manager (PHP-FPM). Kör följande kommandon för att säkerhetskopiera den ursprungliga NGINX-serverblockkonfigurationsfilen. Redigera sedan den ursprungliga filen i valfritt redigeringsprogram:

```bash
sudo cp /etc/nginx/sites-available/default /etc/nginx/sites-available/default_backup

sudo sensible-editor /etc/nginx/sites-available/default
```

I redigeringsprogrammet byter du ut innehållet i `/etc/nginx/sites-available/default` mot följande. Visa kommentarerna för att få en förklaring av inställningarna. Ersätt den offentliga IP-adressen för den virtuella datorn med *din offentliga IP-adress*, och låt de övriga inställningarna stå. Spara sedan filen.

```
server {
    listen 80 default_server;
    listen [::]:80 default_server;

    root /var/www/html;
    # Homepage of website is index.php
    index index.php;

    server_name yourPublicIPAddress;

    location / {
        try_files $uri $uri/ =404;
    }

    # Include FastCGI configuration for NGINX
    location ~ \.php$ {
        include snippets/fastcgi-php.conf;
        fastcgi_pass unix:/run/php/php7.0-fpm.sock;
    }
}
```

Sök i NGINX-konfigurationen efter syntaxfel:

```bash
sudo nginx -t
```

Om syntax stämmer startar du om NGINX med följande kommando:

```bash
sudo service nginx restart
```

Om du vill testa ytterligare skapar du en snabb PHP-informationssida som visas i en webbläsare. Följande kommando skapar PHP-informationssidan:

```bash
sudo sh -c 'echo "<?php phpinfo(); ?>" > /var/www/html/info.php'
```



Nu kan du kontrollera PHP-informationssidan som du har skapat. Öppna en webbläsare och navigera till `http://yourPublicIPAddress/info.php`. Ersätt den offentliga IP-adressen för den virtuella datorn. Det bör se ut ungefär som den här bilden:

![PHP-informationssida][2]


[!INCLUDE [virtual-machines-linux-tutorial-wordpress.md](../../../includes/virtual-machines-linux-tutorial-wordpress.md)]

## <a name="next-steps"></a>Nästa steg

Under den här kursen distribuerade du en LEMP-server i Azure. Du har lärt dig att:

> [!div class="checklist"]
> * Skapa en virtuell Ubuntu-dator
> * Öppna port 80 för webbtrafik
> * Installera NGINX, MySQL och PHP
> * Verifiera installation och konfiguration
> * Installera WordPress på LEMP-stacken

Gå vidare till nästa självstudiekurs där du får lära dig att skydda webbservrar med SSL-certifikat.

> [!div class="nextstepaction"]
> [Säker webbserver med SSL](tutorial-secure-web-server.md)

[1]: ./media/tutorial-lemp-stack/configmysqlpassword-small.png
[2]: ./media/tutorial-lemp-stack/phpsuccesspage.png
[3]: ./media/tutorial-lemp-stack/nginx.png
