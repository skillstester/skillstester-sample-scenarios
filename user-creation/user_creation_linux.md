# Scenario
## User creation - Linux Skills
### Description:
Today we are going to focus on the user creation within unix. The two command to do this are `useradd` and `groupadd`.

The affected files are `/etc/passwd` and `/etc/group`

A user typically consists of a:

- username:
- uid:
- homedirectory:
- primary group:

_Hints:_
Use `man useradd` and `man groupadd`.

### Exercise:
@set:
- username: 'ubuntu'
- homedir: '/home/ubuntu'
- group: admins
- uid: 3000
- gid : 3000

Let's see if you can create a user ubuntu with:

- username: {{{ scenario.username }}}
- uid: {{{ scenario.uid }}}
- homedirectory: {{{ scenario.homedir }}}
- primary group: {{{ scenario.group }}}
- gid: {{{ scenario.gid }}}

@check: ubuntu.user.created
- type: exec
- command: @codeblock
- user: {{{ scenario.username }}}

```
# Any error will cause this script to fail
set -x

# Check that our username is correct
id -un  | grep -w {{{ scenario.username }}}

# Check uid
id -u  | grep -w {{{ scenario.uid }}}

# Check primary group
id -gn | grep -w  {{{ scenario.group }}}

# Check primary gid
id -g | grep -w {{{ scenario.gid }}}

# Check homedirectory
getent passwd {{{ scenario.username }}} | cut -d ':' -f 6 | grep -w {{{ scenario.homedir }}}
```

### Solution:

@action: ubuntu.user.created
- type: exec
- user: root
- command: @codeblock

```
groupadd admins --gid 1000
useradd -m ubuntu -d /home/ubuntu --gid 1000
```

# Actions
# Checks
