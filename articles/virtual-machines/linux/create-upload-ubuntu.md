---
title: Azure에서 Ubuntu Linux VHD 만들기 및 업로드
description: Ubuntu Linux 운영 체제가 포함된 Azure VHD(가상 하드 디스크)를 만들고 업로드하는 방법에 대해 알아봅니다.
author: gbowerman
ms.service: virtual-machines-linux
ms.topic: article
ms.date: 06/24/2019
ms.author: guybo
ms.openlocfilehash: 5fa3415d8663f358bf0ae48be46ac52b8f8b4b06
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80066722"
---
# <a name="prepare-an-ubuntu-virtual-machine-for-azure"></a>Azure용 Ubuntu 가상 머신 준비


우분투는 이제 에서 [https://cloud-images.ubuntu.com/](https://cloud-images.ubuntu.com/)다운로드 할 공식 Azure VHD를 게시합니다. Azure에 대해 특수한 사용자 고유의 Ubuntu 이미지를 빌드해야 하는 경우 아래 수동 절차 대신 이러한 알려진 작업 VHD를 시작하고 필요에 따라 사용자 지정하는 것이 좋습니다. 최신 이미지 릴리스는 항상 다음 위치에서 제공됩니다.

* Ubuntu 12.04/Precise: [ubuntu-12.04-server-cloudimg-amd64-disk1.vhd.zip](https://cloud-images.ubuntu.com/precise/current/precise-server-cloudimg-amd64-disk1.vhd.zip)
* Ubuntu 14.04/Trusty: [ubuntu-14.04-server-cloudimg-amd64-disk1.vhd.zip](https://cloud-images.ubuntu.com/releases/trusty/release/ubuntu-14.04-server-cloudimg-amd64-disk1.vhd.zip)
* 우분투 16.04/제니얼: [우분투-16.04-서버-흐cloudimg-amd64-disk1.vmdk](https://cloud-images.ubuntu.com/releases/xenial/release/ubuntu-16.04-server-cloudimg-amd64-disk1.vmdk)
* 우분투 18.04/바이오닉: [바이오닉-서버-흐린-amd64.vmdk](https://cloud-images.ubuntu.com/bionic/current/bionic-server-cloudimg-amd64.vmdk)
* Ubuntu 18.10/Cosmic: [cosmic-server-cloudimg-amd64.vhd.zip](http://cloud-images.ubuntu.com/releases/cosmic/release/ubuntu-18.10-server-cloudimg-amd64.vhd.zip)

## <a name="prerequisites"></a>사전 요구 사항
이 문서에서는 가상 하드 디스크에 Ubuntu Linux 운영 체제를 이미 설치했다고 가정합니다. .vhd 파일을 만드는 여러 도구가 있습니다(예: Hyper-V와 같은 가상화 솔루션). 지침은 [Hyper-V 역할 설치 및 가상 컴퓨터 구성을](https://technet.microsoft.com/library/hh846766.aspx)참조하십시오.

**Ubuntu 설치 참고 사항**

* Azure용 Linux를 준비하는 방법에 대한 추가 팁은 [일반 Linux 설치 참고 사항](create-upload-generic.md#general-linux-installation-notes) 을 참조하세요.
* VHDX 형식은 Azure에서 지원되지 않습니다. **고정된 VHD**만 지원됩니다.  Hyper-V 관리자 또는 convert-vhd cmdlet을 사용하여 디스크를 VHD 형식으로 변환할 수 있습니다.
* Linux 시스템 설치 시에는 LVM(설치 기본값인 경우가 많음)이 아닌 표준 파티션을 사용하는 것이 좋습니다. 이렇게 하면 특히 문제 해결을 위해 OS 디스크를 다른 VM에 연결해야 하는 경우 복제된 VM과 LVM 이름이 충돌하지 않도록 방지합니다. 원하는 경우 데이터 디스크에 [LVM](configure-lvm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) 또는 [RAID를](configure-raid.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) 사용할 수 있습니다.
* OS 디스크에 스왑 파티션을 구성하지 마세요. 임시 리소스 디스크에서 스왑 파일을 만들도록 Linux 에이전트를 구성할 수 있습니다.  여기에 대한 자세한 내용은 아래 단계에서 확인할 수 있습니다.
* Azure의 모든 VHD는 가상 크기가 1MB 단위로 조정되어야 합니다. 원시 디스크에서 VHD로 변환할 때 변환하기 전에 원시 디스크 크기가 1MB의 배수인지 확인해야 합니다. 자세한 내용은 [Linux 설치 참고 사항](create-upload-generic.md#general-linux-installation-notes)을 참조하세요.

## <a name="manual-steps"></a>수동 단계
> [!NOTE]
> Azure에 대 한 사용자 지정 Ubuntu 이미지를 만들기 전에 [https://cloud-images.ubuntu.com/](https://cloud-images.ubuntu.com/) 대신 미리 빌드 하 고 테스트 된 이미지를 사용 하 여 고려 하십시오.
> 
> 

1. Hyper-V 관리자의 가운데 창에서 가상 머신을 선택합니다.

2. **연결**을 클릭하여 가상 컴퓨터 창을 엽니다.

3. 이미지의 현재 리포지토리를 대체하여 우분투의 Azure 리포지토리를 사용합니다. 단계는 Ubuntu 버전에 따라 약간씩 다릅니다.
   
    `/etc/apt/sources.list`를 편집하기 전에 백업을 만드는 것이 좋습니다.
   
        # sudo cp /etc/apt/sources.list /etc/apt/sources.list.bak

    Ubuntu 12.04:
   
        # sudo sed -i 's/[a-z][a-z].archive.ubuntu.com/azure.archive.ubuntu.com/g' /etc/apt/sources.list
        # sudo apt-get update

    Ubuntu 14.04:
   
        # sudo sed -i 's/[a-z][a-z].archive.ubuntu.com/azure.archive.ubuntu.com/g' /etc/apt/sources.list
        # sudo apt-get update

    Ubuntu 16.04:
   
        # sudo sed -i 's/[a-z][a-z].archive.ubuntu.com/azure.archive.ubuntu.com/g' /etc/apt/sources.list
        # sudo apt-get update

4. 이제 Ubuntu Azure 이미지는 *하드웨어 지원* (HWE) 커널을 따릅니다. 다음 명령을 실행하여 운영 체제를 최신 커널로 업데이트합니다.

    Ubuntu 12.04:
   
        # sudo apt-get update
        # sudo apt-get install linux-image-generic-lts-trusty linux-cloud-tools-generic-lts-trusty
        # sudo apt-get install hv-kvp-daemon-init
        (recommended) sudo apt-get dist-upgrade
   
        # sudo reboot
   
    Ubuntu 14.04:
   
        # sudo apt-get update
        # sudo apt-get install linux-image-virtual-lts-vivid linux-lts-vivid-tools-common
        # sudo apt-get install hv-kvp-daemon-init
        (recommended) sudo apt-get dist-upgrade
   
        # sudo reboot

    Ubuntu 16.04:
   
        # sudo apt-get update
        # sudo apt-get install linux-generic-hwe-16.04 linux-cloud-tools-generic-hwe-16.04
        (recommended) sudo apt-get dist-upgrade

        # sudo reboot
    
    우분투 18.04.04:
    
        # sudo apt-get update
        # sudo apt-get install --install-recommends linux-generic-hwe-18.04 xserver-xorg-hwe-18.04
        # sudo apt-get install --install-recommends linux-cloud-tools-generic-hwe-18.04
        (recommended) sudo apt-get dist-upgrade

        # sudo reboot
    
    **다음 을 참조하십시오.**
    - [https://wiki.ubuntu.com/Kernel/LTSEnablementStack](https://wiki.ubuntu.com/Kernel/LTSEnablementStack)
    - [https://wiki.ubuntu.com/Kernel/RollingLTSEnablementStack](https://wiki.ubuntu.com/Kernel/RollingLTSEnablementStack)


5. Azure용 커널 매개 변수를 추가로 포함하려면 Grub의 커널 부팅 줄을 수정합니다. 이 작업을 수행하려면 `/etc/default/grub`을 텍스트 편집기에서 열고 `GRUB_CMDLINE_LINUX_DEFAULT` 변수를 찾거나 필요한 경우 추가하여 다음 매개 변수가 포함되도록 편집합니다.
   
        GRUB_CMDLINE_LINUX_DEFAULT="console=tty1 console=ttyS0,115200n8 earlyprintk=ttyS0,115200 rootdelay=300"

    이 파일을 저장하고 닫은 다음 `sudo update-grub`을 실행합니다. 이렇게 하면 모든 콘솔 메시지가 첫 번째 직렬 포트로 전송되므로 Azure 기술 지원에서 문제를 디버깅하는 데 도움이 될 수 있습니다.

6. SSH 서버가 설치되어 부팅 시 시작되도록 구성되어 있는지 확인합니다.  보통 SSH 서버는 기본적으로 이와 같이 구성되어 있습니다.

7. Azure Linux 에이전트를 설치합니다.
   
        # sudo apt-get update
        # sudo apt-get install walinuxagent

   > [!Note]
   >  `walinuxagent` 패키지는 `NetworkManager` 및 `NetworkManager-gnome` 패키지가 설치되어 있는 경우 이러한 패키지를 제거할 수 있습니다.


1. 다음 명령을 실행하여 가상 머신의 프로비전을 해제하고 Azure에서 프로비전할 준비를 합니다.
   
        # sudo waagent -force -deprovision
        # export HISTSIZE=0
        # logout

1. 하이퍼 V 관리자에서 **작업 -> 종료를** 클릭합니다. 이제 Linux VHD를 Azure에 업로드할 수 있습니다.

## <a name="references"></a>참조
[Ubuntu 하드웨어 지원(HWE) 커널](https://wiki.ubuntu.com/Kernel/LTSEnablementStack)

## <a name="next-steps"></a>다음 단계
이제 Ubuntu Linux 가상 하드 디스크를 사용하여 Azure에서 새 가상 머신을 만들 준비가 되었습니다. .vhd 파일을 Azure에 처음 업로드하는 경우 [사용자 지정 디스크에서 Linux VM 만들기](upload-vhd.md#option-1-upload-a-vhd)를 참조하세요.

