---
author: hesicong
comments: true
date: 2008-03-12 13:49:46+00:00
layout: post
slug: dx10-dxut-way-with-nvidia-perfhud-5-7
title: DX10 DXUT用Nvidia PerfHUD 5.7的方法
wordpress_id: 336
categories:
- 其他
tags:
- directx
---

研究了好半天，结果在网上搜索到一个结果，试了试，非常ＯＫ，方法是在DXUT.CPP 3568行插入如下代码：

```
D3D10_DRIVER_TYPE driver_type = D3D10_DRIVER_TYPE_HARDWARE;
while(pDXGIFactory->EnumAdapters(adapter_index, &pAdapter) !=
DXGI_ERROR_NOT_FOUND)
{
if(pAdapter)
{
DXGI_ADAPTER_DESC adapter_desc;
if(SUCCEEDED(pAdapter->GetDesc(&adapter_desc)))
{
const bool is_perf_hud = (wcscmp(adapter_desc.Description, L"NVIDIA PerfHUD") == 0);
if(is_perf_hud)
{
driver_type = D3D10_DRIVER_TYPE_REFERENCE;
break;
}
else
{
pAdapter->Release();
}
}
else
{
pAdapter->Release();
}
}
++adapter_index;
}
pNewDeviceSettings->d3d10.DriverType = driver_type;
```