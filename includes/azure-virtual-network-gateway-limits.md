---
title: ta med fil
description: ta med fil
services: networking
author: anzaman
ms.service: VPN Gateway
ms.topic: include
ms.date: 08/25/2020
ms.author: alzam
ms.custom: include file
ms.openlocfilehash: 9fe9ef5549ced3b73d18d553fa0b62ec019684fe
ms.sourcegitcommit: c95e2d89a5a3cf5e2983ffcc206f056a7992df7d
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 11/24/2020
ms.locfileid: "95557022"
---
| Resurs                                | Gräns        |
|-----------------------------------------|------------------------------|
| Prefix för VNet-adress                   | 600 per VPN-gateway          |
| Aggregera BGP-vägar                    | 4 000 per VPN-gateway        |
| Adressprefix för lokal nätverksgateway  | 1000 per lokal nätverksgateway               |
| S2S-anslutningar                         | [Beror på Gateway-SKU: n](../articles/vpn-gateway/vpn-gateway-about-vpngateways.md#gwsku)|
| P2S-anslutningar                         | [Beror på Gateway-SKU: n](../articles/vpn-gateway/vpn-gateway-about-vpngateways.md#gwsku) |
| P2S-flödes gräns – IKEv2                 | 256 för icke-Windows **/** 25 för Windows           |
| P2S-flödes gräns – OpenVPN               | 1000                         |
| Max. flows                              | 100 000 för VpnGw1/AZ  **/**  512 kB för VpnGw2-4/AZ|