# SELinux

## Notes

> May a <subject> do <action> to <object>

### Modes

- Disabled
- Enforcing
- Permissive

When switching between **permissive** and **enforced** modes, there is no need to restart the server

```bash
/etc/sysconfig/selinux  # configuration file # to desactivate it, it is necessary to restart the server
setenforce <0|1>
getenforce
```

### Context

SELinux **context** defines what can be done to a file or process

_Roles_ define what a _user_ can do a _type_
> User:ObjectRole:Type:SecurityLevel ->  system_u:object_r:admin_home_t:s0

- User: wich roles can be used in the policy
- Object Role: grant a user permissions on a domain or type
- Type: actions that can be performed over files or process
- Security Level: level security from 0 to 15

```bash
ls -Z   # list context files
ps -eZ
```

## Booleans

Let us change SELinux policy without knowledge of policy

## Policy

### Commands

````bash
sestatus
seinfo    # yum provides seinfo/apt-file list seinfo
  seinfo -u   # availables users
  seinfo -r   # available role contexts
  seinfo -t   # list types
semanage
  semanage login -l     # conected users
  semanage user -l      # assigned roles for user
  semanage boolean -l   # list booleans
  semanage port -l
  semanage fcontext -l  # context used, including type, object and user
  semanage fcontext --<add|delete|deleteall> -context-flag <value> <file> # permanet
    semanage port -<a|d|m|D|l> -t <name_type> -p <tcp|udp> <port_number>
    getsebool -a        # used boolean values
      getsetbool <type>
    setsebool [-P] <bool> <0|1>  # set value bool. [-P] to persist the change
useradd -Z <selinux_user> <username>    # Add linux user with non-default context
  semanage login -m -s <selinux_user> -r s0 __default__   # 

# security Level
chcon -<u|r|t> <value> <file>  # temporary
restorecon <file>       # restore the context file if we use "chcon" command
#---

## Generate a policy for a service
ps -efZ
sepolicy generate --init <process location>   # generate a policy
  # run "nameProces.sh" generated and restart de service
ausearch -m AVC -Ts recent | grep <nameProcess>   # search error over a policy
ausearch -m AVC -Ts recent | audit2allow -R       # function suggestions to fix errors
  grep -r "suggestionName" /usr/share/selinux/devel/include/ | grep -if   # to check what do the suggested types
  # if necessary add the configurations (suggested functions) to processName.te file created by "generate init"
#---
```

## References
