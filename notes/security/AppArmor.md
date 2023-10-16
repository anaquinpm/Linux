# AppArmor

## Notes

### Modes

> /etc/apparmor.d/

- Enforced/Confined
- Complaining/Learning

### Profiles

Profiles use file paths to define access

This contains -> commentes, includes, child/hat profiles, capabilty rules, rlimit rules, files rules, network rules, IPC rules, conditional rules, enforcement rules

### Commands

````bash
apt install apparmor-utils   # to have aa-enforce/complain command
aa-status
aa-enforce </path/to/bin>
  aa-enforce /etc/apparmor.d/<profile>
aa-complain </path/to/bin>
  aa-complain /etc/apparmor.d/<profile>

## Gereate a profile
apt install apparmor-easyprof apparmor-notifyy
aa-easyprof </path/binary/location> > /etc/apparmor.d/path.binary.location # generate a profile
apparmor_parser -r /etc/apparmor.d/<profile>    # load profile into kernel
systemctl restart <service> # check error logs
aa-complain </bin/command>  # to to check permitions
systemctl restart <service>
aa-notify -s 1 -v           # check AppArmor errors
aa-logprof                  # generate profiles rules based on logged errors
apparmor_parser -r /etc/apparmor.d/<profile>    # reload profile into kernel
aa-enforce <service>        # enforce the service again
systemctl restart <service>
#---
```

## References
