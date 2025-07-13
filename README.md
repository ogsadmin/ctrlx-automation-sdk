# Software Development Kit for ctrlX AUTOMATION

This is the software development kit (SDK) for [ctrlX AUTOMATION](https://www.ctrlx-automation.com). It can be used to program Apps for ctrlX CORE.

Browse through the manual via: [ctrlX AUTOMATION Software Development Kit](https://boschrexroth.github.io/ctrlx-automation-sdk)

> [!NOTE]  
> This is a fork with custom changes/fixes, see [https://github.com/boschrexroth/ctrlx-automation-sdk](https://github.com/boschrexroth/ctrlx-automation-sdk) for the original code!

> [!WARNING]
> The SDK seems to not easily work on `Ubuntu 24`, so only use it on `Ubuntu 22.04`. Even though the [official documentation here](https://boschrexroth.github.io/ctrlx-automation-sdk/latest/setup_windows_virtualbox_ubuntu.html) explicitely references `Ubuntu 22.04`, the instructions listed there only partially work. See the comments below for hints on how to fix.
>
> A good source of information is the [ctrlx-automation-sdk/blob/main/scripts/environment/cloud-config](https://github.com/ogsadmin/ctrlx-automation-sdk/blob/main/scripts/environment/cloud-config) file in the repo, check additionally (e.g. for the list of packages to install).

## Bugs and known issues

This section is relevant for SDK V3.6 and OS version V3.6.1 (automation core V3.6.1).

## Hints

The following might come in handy to debug your snaps on a physical device:

- Request root access to the CtrlX device: 
- Show the snap logs:
	- connect over ssh and run `snap logs <snapname> -f` to get realtime logs
	- configure syslog and point to your syslog collector
- Run an interactive shell inside the snap container:
	1. ssh into the device
	2. run `snap run --shell <snapname>` 
	
- App application data cleanup

  Uninstalling an application does not clean up the application data folder at `/var/snap/rexroth-solutions/common/solutions/DefaultSolution/configurations/appdata/<appname>`. If the layout of the app data changes (or some mistake happens during development), then this must be deleted manually. 

## Installation of the App Build Environment

__To develop ctrlX Apps we recommend to use a ctrlX App Build Environment.__ Otherwise a Ubuntu Server or Desktop system is needed.

How to create and start a ctrlX App Build Environment is described in [ctrlX WORKS App Build Environment](https://boschrexroth.github.io/ctrlx-automation-sdk/setup_qemu_ctrlx_works.html)

If your ctrlX App Build Environment is running, you can log in and install the ctrlX AUTOMATION SDK - see below.

## Installation of the ctrlX AUTOMATION SDK

__These installation steps are required on both an App Build Environment and an Ubuntu Server or Desktop System.__

> [!NOTE]  
> The following instructions seem to only only work, if the commands are executed from the users root folder (`/home/boschrexroth/`)! Do not install in a subfolder, else the datalayer .deb package install fails.

> [!NOTE]  
> Snapcraft uses LXC-containers managed through `lxd`. By default this requires root access (sudo) for building. To fix this, add your user to the `lxd` group as follows
>
>   sudo usermod -a -G lxd $USER
>
> Then you can remove the `sudo` calls in the samples build scripts.

### Clone and Install the ctrlX AUTOMATION SDK

Start a console session, change to your destination directory and enter:

	wget https://raw.githubusercontent.com/boschrexroth/ctrlx-automation-sdk/main/scripts/clone-install-sdk.sh && chmod a+x *.sh && ./clone-install-sdk.sh

As a result, your local copy of the github repo is stored within the directory __ctrlx-automation-sdk/__

> [!NOTE]  
> NOTE: The script will ask for the github tag to use for installing - enter the version as shown in the list, e.g. `2.6.7`, then hit enter (also for the next question)

### Install required Debian packages

Stay in the directory and enter:

	ctrlx-automation-sdk/scripts/install-required-packages.sh

### Install snapcraft

Stay in the directory and enter:

	ctrlx-automation-sdk/scripts/install-snapcraft.sh

### Install ctrlx-datalayer Debian package

Stay in the directory and enter:

	ctrlx-automation-sdk/scripts/install-ctrlx-datalayer.sh ctrlx-automation-sdk/deb

### Building Sample Projects

Now you are able to build the C++ and Python sample projects.

To build sample projects in other programming languages further installation steps are required.

Change to the directory `ctrlx-automation-sdk/scripts` and start the according install script.

For go samples:

	install-go.sh

> [!WARNING]  
> The `install-dotnet-sdk.sh` script seem to not work for Ubuntu 22.04, as the script downloads a debian package, which breaks the Ubuntu 22 provided packages. Here is how to officially install the `dotnet 8.0.411` framework and sdk: [Install DotNet 8 for Ubuntu 22](https://learn.microsoft.com/de-de/dotnet/core/install/linux-ubuntu-install?tabs=dotnet8&pivots=os-linux-ubuntu-2204).
> Use the following instead:
	
	sudo apt-get install -y dotnet-sdk-8.0 aspnetcore-runtime-8.0 dotnet-runtime-8.0

For .NET samples (**MIGHT NEED TO REINSTALL DOTNET AS SHOWN ABOVE**):

	install-dotnet-sdk.sh

> [!NOTE]
> To develop on windows and linux in parallel, make sure to delete the `nuget.config` file from the C# samples (this is not used by VS2022 and VSCode/dotnet correctly resolves the packages references (nowadays) correctly anyway). 
>
> To use the datalyer on windows, make sure to download/install the OpenSSH libraries.


> [!TIP]
> The [hard to find DotNET class library documentation](https://apps.boschrexroth.com/docs/ctrlx/csharp/html/files.html)
>
> The [official .NET Apps @ctrlX documentation](https://boschrexroth.github.io/ctrlx-automation-sdk/latest/dotnet.html)

For nodejs samples:

	install-nodejs-npm.sh

Overview of all scripts: [Description of the scripts](scripts/README.md)

## Important directions for use

### General Terms of Use

In order to download and use the binary packages of the ctrlX AUTOMATION Software Development Kit you have to accept the [Terms and Conditions for the Provision of Products of Bosch Rexroth AG Free of Charge](https://dc-corp.resource.bosch.com/media/xc/homepage/TC_for_provision_of_products_free_of_charge.pdf)

### Areas of use and application

The content (e.g. source code and related documents) of this repository is intended to be used for configuration, parameterization, programming or diagnostics in combination with selected Bosch Rexroth ctrlX AUTOMATION devices.
Additionally, the specifications given in the "Areas of Use and Application" for ctrlX AUTOMATION devices used with the content of this repository do also apply.

### Unintended use

Any use of the source code and related documents of this repository in applications other than those specified above or under operating conditions other than those described in the documentation and the technical specifications is considered as "unintended". Furthermore, this software must not be used in any application areas not expressly approved by Bosch Rexroth.

## License

SPDX-FileCopyrightText: Bosch Rexroth AG
SPDX-License-Identifier: MIT

## About

Please note that any trademarks, logos and pictures contained or linked to in this Software are owned by or copyright © Bosch Rexroth AG 2021-2024 and not licensed under the Software's license terms.

<https://www.boschrexroth.com>

Bosch Rexroth AG  
Bgm.-Dr.-Nebel-Str. 2  
97816 Lohr am Main  
GERMANY
