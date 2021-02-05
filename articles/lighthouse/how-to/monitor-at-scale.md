---
title: Övervaka delegerade resurser i stor skala
description: Lär dig hur du effektivt använder Azure Monitor loggar på ett skalbart sätt över de kund innehavare som du hanterar.
ms.date: 02/02/2021
ms.topic: how-to
ms.openlocfilehash: 8847c2e5ee4986d35ad676440720b150794003e8
ms.sourcegitcommit: 1f1d29378424057338b246af1975643c2875e64d
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 02/05/2021
ms.locfileid: "99575914"
---
# <a name="monitor-delegated-resources-at-scale"></a>Övervaka delegerade resurser i stor skala

Som tjänst leverantör kan du ha registrerat flera kund klienter i [Azure-Lighthouse](../overview.md). Med Azure Lighthouse kan tjänst leverantörer utföra åtgärder i skala över flera klienter samtidigt, vilket gör hanterings uppgifter mer effektiva.

Det här avsnittet visar hur du använder [Azure Monitor loggar](../../azure-monitor/platform/data-platform-logs.md) på ett skalbart sätt över de kund innehavare som du hanterar. Även om vi refererar till tjänst leverantörer och kunder i det här avsnittet gäller den här vägledningen även för [företag som använder Azure-Lighthouse för att hantera flera klienter](../concepts/enterprise.md).

> [!NOTE]
> Se till att användarna i hanterings klienter har beviljats de [nödvändiga rollerna för att hantera Log Analytics arbets ytor](../../azure-monitor/platform/manage-access.md#manage-access-using-azure-permissions) på dina delegerade kund prenumerationer.

## <a name="create-log-analytics-workspaces"></a>Skapa Log Analytics arbets ytor

För att kunna samla in data måste du skapa Log Analytics arbets ytor. Dessa Log Analytics arbets ytor är unika miljöer för data som samlas in av Azure Monitor. Varje arbets yta har sin egen data lagrings plats och konfiguration, och data källor och lösningar har kon figurer ATS för att lagra data i en viss arbets yta.

Vi rekommenderar att du skapar dessa arbets ytor direkt i kundens klienter. På så sätt förblir deras data i sina klienter i stället för att exporteras till dina. Det möjliggör också centraliserad övervakning av resurser eller tjänster som stöds av Log Analytics, vilket ger dig mer flexibilitet för vilka typer av data du övervakar.

> [!TIP]
> Alla Automation-konton som används för att komma åt data från en Log Analytics-arbetsyta måste skapas i samma klient organisation som arbets ytan.

Du kan skapa en Log Analytics arbets yta genom att använda [Azure Portal](../../azure-monitor/learn/quick-create-workspace.md), med hjälp av [Azure CLI](../../azure-monitor/learn/quick-create-workspace-cli.md)eller genom att använda [Azure PowerShell](../../azure-monitor/platform/powershell-workspace-configuration.md).

> [!IMPORTANT]
> Även om alla arbets ytor skapas i kund klienten, måste Microsoft. Insights Resource Provider också registreras på en prenumeration i hanterings klienten.

## <a name="deploy-policies-that-log-data"></a>Distribuera principer som loggar data

När du har skapat dina Log Analytics arbets ytor kan du distribuera [Azure policy](../../governance/policy/index.yml) över dina kundhierarkier så att diagnostikdata skickas till lämplig arbets yta i varje klient. De exakta principer som du distribuerar kan variera beroende på de resurs typer som du vill övervaka.

Mer information om hur du skapar principer finns i [Självstudier: skapa och hantera principer för att genomdriva efterlevnad](../../governance/policy/tutorials/create-and-manage.md). Det här [Community-verktyget](https://github.com/Azure/Azure-Lighthouse-samples/tree/master/tools/azure-diagnostics-policy-generator) ger ett skript som hjälper dig att skapa principer för att övervaka de resurs typer som du väljer.

När du har bestämt vilka principer som ska distribueras kan du [distribuera dem till dina delegerade prenumerationer i stor skala](policy-at-scale.md).

## <a name="analyze-the-gathered-data"></a>Analysera insamlade data

När du har distribuerat dina principer kommer data att loggas i Log Analytics arbets ytor som du har skapat i varje kund klient organisation. För att få insikter över alla hanterade kunder kan du använda verktyg som [Azure Monitor arbets böcker](../../azure-monitor/platform/workbooks-overview.md) för att samla in och analysera information från flera data källor.

## <a name="view-alerts-across-customers"></a>Visa aviseringar över kunder

Du kan visa [aviseringar](../../azure-monitor/platform/alerts-overview.md) för de delegerade prenumerationerna på kund klienter som din hantering.

Du kan [skapa, Visa och hantera aktivitets logg aviseringar](../../azure-monitor/platform/alerts-activity-log.md) i Azure Portal eller via API: er och hanterings verktyg från din hanterings klient.

Om du vill uppdatera aviseringar automatiskt över flera kunder använder du en [Azure Resource Graph](../../governance/resource-graph/overview.md) -fråga för att filtrera efter aviseringar. Du kan fästa frågan på din instrument panel och välja alla lämpliga kunder och prenumerationer. Frågan nedan visar till exempel allvarlighets grad 0 och 1 aviseringar, som uppdateras var 60: e minut.

```kusto
alertsmanagementresources
| where type == "microsoft.alertsmanagement/alerts"
| where properties.essentials.severity =~ "Sev0" or properties.essentials.severity =~ "Sev1"
| where properties.essentials.monitorCondition == "Fired"
| where properties.essentials.startDateTime > ago(60m)
| project StartTime=properties.essentials.startDateTime,name,Description=properties.essentials.description, Severity=properties.essentials.severity, subscriptionId
| sort by tostring(StartTime)
```

## <a name="next-steps"></a>Nästa steg

- Utforska den här [MVP-skapade exempel arbets boken](https://github.com/scautomation/Azure-Automation-Update-Management-Workbooks), som spårar patch-rapportering genom att [skicka frågor till uppdateringshantering loggar](../../automation/update-management/query-logs.md) över flera Log Analytics arbets ytor. 
- Läs mer om [Azure Monitor](../../azure-monitor/index.yml).
- Lär dig mer om [Azure Monitor loggar](../../azure-monitor/platform/data-platform-logs.md).
- Lär dig mer om [hanterings upplevelser mellan flera innehavare](../concepts/cross-tenant-management-experience.md).