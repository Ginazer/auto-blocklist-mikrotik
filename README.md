# auto-blocklist-mikrotik

This is your easy MikroTik auto blocklist, updated daily.
prevent your device from attacker have bad reputation public ip or your client access to malicious site public ip.

Plush Reference IP Blacklist .txt


# Setup MikroTik

First create script

Attention! check at delete-own-ippublic, please adjust your ip public.

`/system script`
`add name=gnzdef-rbl-download source="/tool fetch url=\"https://raw.githubusercontent.com/Ginazer/auto-blocklist-mikrotik/main/rbl.rsc " mode=https"`
`add name=gnzdef-rbl-replace source="/ip firewall address-list remove [find where list=\"gnzdef-blacklist\"]; /import file-name=rbl.rsc`
`add name=delete-own-ippublic source="/ip firewall address-list remove [find where address=\"your.ip.pub.lic\"]`

Schedule

Note: for scheduler please adjust your time before use command below.
`/system scheduler`
`add interval=1d name=update-rbl-gnzdef on-event=gnzdef-rbl-download start-date=dec/10/2021 start-time=23:30:00`
`add interval=1d name=install-rbl-gnzdef on-event=gnzdef-rbl-replace start-date=dec/10/2021 start-time=23:35:00`
`add interval=1d name=delete-own-ippublic on-event=delete-own-ippublic start-date=dec/10/2021 start-time=23:40:00`

Filter

Attention! check at interface, please adjust your wan interface.
`/ip firewall filter`
`add action=drop chain=input comment="Realtime Blocklist GNZDEF (https://raw.githubusercontent.com/Ginazer/auto-blocklist-mikrotik/main/rbl.rsc)" connection-state=new in-interface="your wan ehter" src-address-list=gnzdef-blacklist`
`add action=drop chain=forward connection-state=new dst-address-list=gnzdef-blacklist out-interface="your wan ether"`

Thank you...

