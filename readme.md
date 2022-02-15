# How to disable ASIC offloading

there are 2 step necessary

## STEP 1 : Set it on all policies = initial run

### use script "set_offload_off.py"

1. edit global parameter in code
2. you can set "sim = yes" to test without changes
3. run set_offload_off.py

## STEP 2 : install backend service

this service receives any change on FMG via SYSLOG
and if you add or change a policy,
the service will immediately disable ASIC offloading of this policy

### use script "fmg_syslog.py"

and take care
all policy packages need to have differnet names, since the path is not sent in syslog msg!


tasks around the script
1. edit parameter in code "fmg_syslog.py"
2. edit "fmg_syslog.service"
3. cp fmg_syslog.service /etc/systemd/system/
4. sudo service fmg_syslog start

it is also possible to run the script standalone, especially for testing

in FMG CLI:

```
config system syslog
  edit "fmg-syslog"
      set ip <ip-off-service>
      set port 10514
  next
end
config system locallog syslogd setting
  set severity information
  set status enable
  set syslog-name "fmg-syslog"
end
```
