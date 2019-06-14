---
layout: post
title: Run Profiler on Azure App Service!
---

Sometimes we face issues like high CPU usage in Production or in Azure environment, which we don't face usually in local environment. We can run profiler in azure environment.

## Step-by-step guide

1. Go to Azure Portal.
2. Go kudu for the application you want to run profiler on.
![_config.yml]({{ site.baseurl }}/images/azure-profiler/image1.webp)
3. Go to Process Explorer tab
![_config.yml]({{ site.baseurl }}/images/azure-profiler/image2.webp)
4. Now click on Start Profiling button on right for the app you want to. It might take few seconds to start.
![_config.yml]({{ site.baseurl }}/images/azure-profiler/image3.webp)
5. Wait for 90-120 seconds after starting the profiler. Then click on Stop Profiling button.
6. Now wait few minutes when it is generating the diagnostics session. After completion a file download will be prompted.
![_config.yml]({{ site.baseurl }}/images/azure-profiler/image4.webp)
7. A file (*.diagsession) will be downloaded. Open the file with Visual Studio.
![_config.yml]({{ site.baseurl }}/images/azure-profiler/image4.webp)
8. Analyze it. :)
