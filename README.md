# auto-blocklist-mikrotik

This is your easy MikroTik auto blocklist, updated daily.

prevent your device from attacker have bad reputation public ip or your client access to malicious site public ip.

Plush Reference IP Blacklist .txt


# Setup MikroTik

## Create Script
>/system script

download rsc
>add name=gnzdef-rbl-download source="/tool fetch url='https://raw.githubusercontent.com/Ginazer/auto-blocklist-mikrotik/main/rbl.rsc' mode=https"

update replace list 
>add name=gnzdef-rbl-replace source="/ip firewall address-list remove [find where list=\"gnzdef-blacklist\"]; /import file-name=rbl.rsc"

remove own ip public from list 
**Attention! check at delete-own-ippublic, please adjust your ip public**
>add name=delete-own-ippublic source="/ip firewall address-list remove [find where address=\"**your.ip.pub.lic**\"]"


## Schedule
Note: for scheduler please adjust your time before use command below.
>/system scheduler

Schedule Download
>add interval=1d name=update-rbl-gnzdef on-event=gnzdef-rbl-download start-date=**dec/10/2021** start-time=23:30:00

Schedule Install
>add interval=1d name=install-rbl-gnzdef on-event=gnzdef-rbl-replace start-date=**dec/10/2021** start-time=23:35:00

Schedule Delete Own Ip Public
>add interval=1d name=delete-own-ippublic on-event=delete-own-ippublic start-date=dec/10/2021 start-time=23:59:00


## Filter
**Attention! check the interface, please adjust your wan interface.**
>/ip firewall filter

Block attacker to MikroTik
>add action=drop chain=input comment="Realtime Blocklist GNZDEF (https://raw.githubusercontent.com/Ginazer/auto-blocklist-mikrotik/main/rbl.rsc)" connection-state=new in-interface="**your wan ether**" src-address-list=gnzdef-blacklist

Block client to access malecious site
>add action=drop chain=forward connection-state=new dst-address-list=gnzdef-blacklist out-interface="**your wan ether**"

Thank you...
