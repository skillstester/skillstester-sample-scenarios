# Scenario
## User creation - Puppet Skills
### Description:
Today we are going to focus on the user creation with puppet.

A user typically consists of a:

- username:
- uid:
- homedirectory:
- primary group:

@set:
- username: 'ubuntu'
- homedir: '/home/ubuntu'
- group: admins
- uid: 3000
- gid : 3000


### Exercise:
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

# Actions
# Checks
