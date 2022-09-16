# 1 Introduction

## 1.1 Author
| Author Name | E-mail | Date|
| :----------:| :---------------------:| :--------: |
| Nikolay Shergin | email@email.com | 16.09.2021 |

## 1.1.2 Change history
| Author Name | E-mail | Date| Comments | 
| :----------:| :---------------------:| :--------: | :----------: |
| Nikolay Shergin | email@email.com | 16.09.2021 | Initial Draft |

## 1.2 Purpose

The role applis Firmware Updates on HPE server using SPP ISO image mounted via iLO.

# 2 Dependencies
| ID | Description | 
| :----------:| :---------------------:| 
| D01 | Name of the SPP ISO Image has to be provided in EXTRA VAR |
| D02 | ISO image has to be available via http(s) |

# 3 Detailed role description

## 3.1 Playbook

| Playbook Name | Playbook role | Describtion|
| :----------:| :---------------------:| :--------: |
| hpe_spp_patching.yml | The role Launcher | Playbook starts tasks in the Role hpe_patching |

## 3.2 Tasks List
| Order nr. | Task Name | Task role | Describtion|
| :--: | :----------:| :---------------------:| :------------------------------: |
| 0. | mainHpePatching.yml | The Pipeline Launcher | Used only for pre-checks and include Tasks from the Role |
| 1. | getInfoOfVersion.yml| Task to backup current iLO version details | Captured version later compared with the version after installation to check is the patching was successful (Checks only BIOS version) |
| 2.| backupHpeIlo.yml | Task to backup current iLO config | Can be used to troubleshooting and rollback |
| 3. | insertVirtualMedia.yml | Task mounts SPP ISO image and boots HPE server from virtual CDROM | The server will be rebooted one time from mounted ISO. SPP patches wiil be installed automatically where it is required. The server will be rebooted normally after the installation  |
| 4. | compareVersions.yml | Task to compare BIOS version | The task captures BIOS version after the installation and compares it with the version captured before|

## 3.3 Variables

| Variable Name | Variable Source | Value | Describtion|
| :--: | :----------:| :---------------------:| :------------------------------: |
| basicPathToApi | defaults | /redfish/v1/ | path to redfish API |
| apiMethodGet | defaults | GET | API call method| 
| deployIloMedia | defaults | cdrom | boot source |
| deployIloState | defaults | boot_once | the state to boot only once from ISO|
| deployIloForce | defaults | yes | Forcing server reboot |
| delegateToLocalHost | defaults | localhost | perform the task from localhost |
| destinationOfHpeIloConfBackup | defaults | /tmp/ilocaonfbackups/ | path to storebackups |
| iloValidateCerts | defaults | no | ignore self-signed certificate warning | 
| iloForceBasicAuth | defaults | yes | authenticate via login and password |
| iloBodyFormat | defaults | json | the serialization of body format |
| pathForVirtualMedia2 | defaults | Managers/1/virtualmedia/2/ | path to redfish API for cdrom |
| pathForVersion | defaults | Systems/1/ | path to redfish API for versions |
| pathForBackupOfIloConf | defaults | Managers/1/BackupRestoreService |  path to redfish API for backup file |
| pathToImage | defaults | https://8.8.8.8:3000/SPP_Collection/ | URL for SPP ISO image | 
| pauseTime | defaults | 45 | Time to wait before compating the versions |