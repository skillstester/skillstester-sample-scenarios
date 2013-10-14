# Scenario
## User creation - Puppet Skills
### Description:
Today we are going to focus on the user creation with puppet.

A user typically consists of a:

- username: used to login with
- uid: unique id that identifies the user
- homedirectory: the directory where the user lives
- primary group: integer that is the group id

### Exercise:
Let's see if you can create a user ubuntu with:

- uid: 1000
- gid: 1000
- username: ubuntu
- homedir: /home/ubuntu

- @set: [An ubuntu user](#settings-ubuntu-user)

Waiting until you finish the job ...

- @check: [Is user created?](#check-user-created)

### Solution:

- @action: [Create user with puppet](#action-user-create)

# Actions
## action-user-create
- type: exec
- command: @codeblock

```
group { '{{{ scenario.group }}}':
  gid => {{{ scenario.gid }}}
}

user { '{{{ scenario.username }}}':
  uid     => {{{ scenario.uid }}},
  homedir => '{{{ scenario.homedir }}}',
  gid     => {{{ scenario.gid }}},
  after   => [ Group['{{{ scenario.group }}}']]
}
```

# Checks
## check-user-created
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

# Settings
## settings-ubuntu-user:1
- username: 'ubuntu'
- homedir: '/home/ubuntu'
- group: admins
- uid: 3000
- gid : 3000
