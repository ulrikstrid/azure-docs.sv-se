---
title: Vanliga frågor om Azure Container Service
description: Innehåller svar på några vanliga frågor om Azure Container Service.
services: container-service
author: neilpeterson
manager: timlt
ms.service: container-service
ms.topic: article
ms.date: 2/14/2018
ms.author: nepeters
ms.openlocfilehash: 5eb949e420d9b055d014b973d452dc95fc358f83
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 04/16/2018
---
# <a name="frequently-asked-questions-about-azure-container-service-aks"></a>Vanliga frågor och svar om Azure-behållaren (AKS)

Den här artikeln adresser vanliga frågor om Azure-behållaren (AKS).

> [!IMPORTANT]
> Azure Container Service (AKS) finns för närvarande i **förhandsversion**. Förhandsversioner görs tillgängliga för dig under förutsättning att du godkänner [kompletterande användningsvillkor](https://azure.microsoft.com/support/legal/preview-supplemental-terms/). Vissa aspekter av funktionen kan ändras innan den är allmänt tillgänglig (GA).
>

## <a name="which-azure-regions-provide-the-azure-container-service-aks-today"></a>Vilka regioner som Azure tillhandahåller Azure Container Service (AKS) idag?

- Centrala Kanada
- Östra Kanada
- Centrala USA
- Östra USA
- Sydostasien
- Västra Europa
- Västra USA 2

## <a name="when-will-additional-regions-be-added"></a>När läggs ytterligare regioner?

Ytterligare regioner läggs till som ökar.

## <a name="are-security-updates-applied-to-aks-agent-nodes"></a>Tillämpas säkerhetsuppdateringar AKS agent noder?

Azure tillämpas automatiskt säkerhetskorrigeringar på noderna i klustret enligt ett schema som automatiskt varje natt. Men du är ansvarig för att säkerställa att noderna startas om efter behov. Du har flera alternativ för att utföra omstarter av noden:

- Manuellt via Azure-portalen eller Azure CLI.
- Genom att uppgradera AKS klustret. Klustret uppgraderingar automatiskt [cordon och tömmer noder](https://kubernetes.io/docs/tasks/administer-cluster/safely-drain-node/), anpassa dem igen med senaste Ubuntu avbildningen. Uppdatera OS-avbildningen på noderna utan att ändra Kubernetes versioner genom att ange den aktuella versionen för klustret i `az aks upgrade`.
- Med hjälp av [Kured](https://github.com/weaveworks/kured), en öppen källkod omstart daemon för Kubernetes. Kured körs som en [DaemonSet](https://kubernetes.io/docs/concepts/workloads/controllers/daemonset/) och övervakar varje nod för förekomsten av en fil som anger att en omstart krävs. Den samordnar sedan dessa omstarter i klustret, följa samma cordon och tömning process som beskrivs ovan.

## <a name="do-you-recommend-customers-use-acs-or-aks"></a>Rekommenderar du kunder Använd ACS eller AKS?

Även om AKS finns kvar i preview, rekommenderar vi skapar produktion kluster med hjälp av ACS-Kubernetes eller [acs-motorn](https://github.com/azure/acs-engine). Använd AKS för proof of concept distributioner och miljöer för utveckling och testning.

## <a name="when-will-acs-be-deprecated"></a>När kommer ACS inaktuell?

ACS att bli inaktuell vid ungefär samma tidpunkt som AKS blir GA. Har du 12 månader efter det att migrera kluster till AKS. Du kan köra alla ACS-åtgärder under 12 månader.

## <a name="does-aks-support-node-autoscaling"></a>Stöder AKS nod autoskalning?

Noden autoskalning stöds inte men på Översikt. Du kanske vill titta på den här öppen källkod [autoskalning implementering][auto-scaler].

## <a name="does-aks-support-kubernetes-role-based-access-control-rbac"></a>Stöder AKS Kubernetes rollbaserad åtkomstkontroll (RBAC)?

Nej, RBAC stöds inte för närvarande i AKS men blir snart tillgänglig.

## <a name="can-i-deploy-aks-into-my-existing-virtual-network"></a>Kan jag distribuera AKS i Mina befintliga virtuella nätverk?

Nej, detta är inte tillgänglig ännu men blir snart tillgänglig.

## <a name="is-azure-key-vault-integrated-with-aks"></a>Azure Key Vault är integrerad med AKS?

Nej, det är inte men den här integreringen planeras. Under tiden kan prova att använda följande lösningar från [Hexadite][hexadite].

## <a name="can-i-run-windows-server-containers-on-aks"></a>Kan jag köra Windows Server-behållare på AKS?

Nej, AKS inte för närvarande tillhandahåller Windows Server-baserad agent noder, så att du kan köra Windows Server-behållare. Om du behöver köra Windows Server-behållare på Kubernetes i Azure finns i [dokumentationen för acs-motorn](https://github.com/Azure/acs-engine/blob/master/docs/kubernetes/windows.md).

## <a name="why-are-two-resource-groups-created-with-aks"></a>Varför skapas två resursgrupper med AKS?

Varje AKS distribution omfattar två resursgrupper. Först skapas av dig och innehåller AKS resursen. Resursprovidern AKS skapar automatiskt det andra under distributionen med namnet *MC_myResourceGRoup_myAKSCluster_eastus*. Andra resursgruppen som innehåller alla infrastrukturresurser som är associerade med klustret, till exempel virtuella datorer, nätverk och lagring. Den har skapats för att förenkla rensning av resursen.

Om du skapar resurser som ska användas med AKS klustret, till exempel storage-konton eller reserverade offentliga IP-adressen bör du placera dem i den automatiskt genererade resursgruppen.

<!-- LINKS - external -->
[auto-scaler]: https://github.com/kubernetes/autoscaler
[hexadite]: https://github.com/Hexadite/acs-keyvault-agent