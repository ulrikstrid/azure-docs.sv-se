---
title: ta med fil
description: ta med fil
services: virtual-machines-linux
author: dlepow
ms.service: virtual-machines-linux
ms.topic: include
ms.date: 04/16/2018
ms.author: danlep
ms.custom: include file
ms.openlocfilehash: 919582745d166cf8b3f3937f9bac4fc0dc1fe64f
ms.sourcegitcommit: 59914a06e1f337399e4db3c6f3bc15c573079832
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 04/20/2018
---
## <a name="overview-of-ssh-and-keys"></a>Översikt över SSH och nycklar

SSH är en krypterad anslutningsprotokoll som tillåter säker inloggning via oskyddat anslutningar. Det är standardprotokollet för anslutning för virtuella Linux-datorer finns i Azure. Även om SSH själva ger en krypterad anslutning, lämnar med hjälp av lösenord med SSH-anslutningar fortfarande den virtuella datorn sårbar för brute force-attacker eller att gissa lösenord. Det är en säkrare och önskade metod för att ansluta till en virtuell dator med hjälp av SSH med hjälp av en privat-offentligt nyckelpar, även kallat SSH-nycklar. 

* Den *offentliga nyckel* är placerad på ditt Linux-VM eller någon annan tjänst som du vill använda med offentliga nycklar.

* Den *privata nyckel* är vad du presentera för Linux-VM när du gör en SSH-anslutning, verifiera din identitet. Skydda den privata nyckeln. Dela den inte med andra.

Beroende på organisationens säkerhetsprinciper, kan du återanvända ett enstaka privat-offentligt nyckelpar för att komma åt flera virtuella Azure-datorer och tjänster. Du behöver inte en separat nyckelpar för varje virtuell dator eller tjänst som du vill använda. 

Din offentliga nyckel kan delas med alla, men bara du (eller din lokala säkerhetsinfrastruktur) har tillgång till den privata nyckeln.