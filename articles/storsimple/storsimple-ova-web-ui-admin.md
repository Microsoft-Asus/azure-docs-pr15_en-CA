<properties 
   pageTitle="StorSimple Virtual Array web UI administration | Microsoft Azure"
   description="Describes how to perform basic device administration tasks through the StorSimple Virtual Array web UI."
   services="storsimple"
   documentationCenter="NA"
   authors="alkohli"
   manager="carmonm"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="TBD"
   ms.date="04/07/2016"
   ms.author="alkohli" />

# <a name="use-the-web-ui-to-administer-your-storsimple-virtual-array"></a>Use the Web UI to administer your StorSimple Virtual Array

![setup process flow](./media/storsimple-ova-web-ui-admin/manage4.png)

## <a name="overview"></a>Overview

The tutorials in this article apply to the Microsoft Azure StorSimple Virtual Array (also known as the StorSimple on-premises virtual device) running March 2016 general availability (GA) release. This article describes some of the complex workflows and management tasks that can be performed on the StorSimple Virtual Array. You can manage the StorSimple Virtual Array using the StorSimple Manager service UI (referred to as the portal UI) and the local web UI for the device. This article focuses on the tasks that you can perform using the web UI.

This article includes the following tutorials:

- Get the service data encryption key
- Troubleshoot web UI setup errors
- Generate a log package
- Shut down or restart your device

## <a name="get-the-service-data-encryption-key"></a>Get the service data encryption key

A service data encryption key is generated when you register your first device with the StorSimple Manager service. This key is then required with the service registration key to register additional devices with the StorSimple Manager service.

If you have misplaced your service data encryption key and need to retrieve it, perform the following steps in the local web UI of the device registered with your service.

#### <a name="to-get-the-service-data-encryption-key"></a>To get the service data encryption key

1. Connect to the local web UI. Go to **Configuration** > **Cloud Settings**.
  

2. At the bottom of the page, click **Get service data encryption key**. A key will appear. Copy and save this key.
    
    ![get service data encryption key 1](./media/storsimple-ova-web-ui-admin/image27.png)
   


## <a name="troubleshoot-web-ui-setup-errors"></a>Troubleshoot web UI setup errors

In some instances when you configure the device through the local web UI, you might run into errors. To diagnose and troubleshoot such errors, you can run the diagnostics tests.

#### <a name="to-run-the-diagnostic-tests"></a>To run the diagnostic tests

1. In the local web UI, go to **Troubleshooting** > **Diagnostic tests**.

    ![run diagnostics 1](./media/storsimple-ova-web-ui-admin/image29.png)

2. At the bottom of the page, click **Run Diagnostic Tests**. This will initiate tests to diagnose any possible issues with your network, device, web proxy, time, or cloud settings. You will be notified that the device is running tests.

3. After the tests have completed, the results will be displayed. The following example shows the results of diagnostic tests. Note that the web proxy settings were not configured on this device, and therefore, the web proxy test was not run. All the other tests for network settings, DNS server, and time settings were successful.

    ![run diagnostics 2](./media/storsimple-ova-web-ui-admin/image30.png)

## <a name="generate-a-log-package"></a>Generate a log package

A log package is comprised of all the relevant logs that can assist Microsoft Support with troubleshooting any device issues. In this release, a log package can be generated via the local web UI.

#### <a name="to-generate-the-log-package"></a>To generate the log package

1. In the local web UI, go to **Troubleshooting** > **System logs**.

    ![generate log package 1](./media/storsimple-ova-web-ui-admin/image31.png)

2. At the bottom of the page, click **Create log package**. A package of the system logs will be created. This will take a couple of minutes.

    ![generate log package 2](./media/storsimple-ova-web-ui-admin/image32.png)

    You will be notified after the package is successfully created, and the page will be updated to indicate the time and date when the package was created.

    ![generate log package 3](./media/storsimple-ova-web-ui-admin/image33.png)

3. Click **Download log package**. A zipped package will be downloaded on your system.

    ![generate log package 4](./media/storsimple-ova-web-ui-admin/image34.png)

4. You can unzip the downloaded log package and view the system log files.

## <a name="shut-down-and-restart-your-device"></a>Shut down and restart your device

You can shut down or restart your virtual device using the local web UI. We recommend that before you restart, take the volumes or shares offline on the host and then the device. This will minimize any possibility of data corruption. 

#### <a name="to-shut-down-your-virtual-device"></a>To shut down your virtual device

1. In the local web UI, go to **Maintenance** > **Power settings**.

2. At the bottom of the page, click **Shutdown**.

    ![device shutdown 1](./media/storsimple-ova-web-ui-admin/image36.png)

3. A warning will appear stating that a shutdown of the device will interrupt any IO that were in progress, resulting in a downtime. Click the check icon ![check icon](./media/storsimple-ova-web-ui-admin/image3.png).

    ![device shutdown warning](./media/storsimple-ova-web-ui-admin/image37.png)

    You will be notified that the shutdown has been initiated.

    ![device shutdown started](./media/storsimple-ova-web-ui-admin/image38.png)

    The device will now shut down. If you want to start your device, you will need to do that through the Hyper-V Manager.

#### <a name="to-restart-your-virtual-device"></a>To restart your virtual device

1. In the local web UI, go to **Maintenance** > **Power settings**.

2. At the bottom of the page, click **Restart**.

    ![device restart](./media/storsimple-ova-web-ui-admin/image36.png)

3. A warning will appear stating that restarting the device will interrupt any IOs that were in progress, resulting in a downtime. Click the check icon ![check icon](./media/storsimple-ova-web-ui-admin/image3.png).

    ![restart warning](./media/storsimple-ova-web-ui-admin/image37.png)

    You will be notified that the restart has been initiated.

    ![restart initiated](./media/storsimple-ova-web-ui-admin/image39.png)

    While the restart is in progress, you will lose the connection to the UI. You can monitor the restart by refreshing the UI periodically. Alternatively, you can monitor the device restart status through the Hyper-V Manager.

## <a name="next-steps"></a>Next steps

Learn how to [use the StorSimple Manager service to manage your device](storsimple-manager-service-administration.md).
