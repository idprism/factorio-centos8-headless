# factorio-headless
**basic/minimal centos8 setup for a factorio server**

This ansible module is designed to use the tools created by some other people to set up and run a factorio headless server.

tl;dr: instructions
1. make sure you have sudo access on your server
2. get this role (git clone) to your server
3. install ansible on your server `(dnf|apt) install -y ansible`
4. set variables in `factorio_headless/vars/main.yml`
   1. factorio_account_username
   2. factorio_account_token
   3. IF your factorio user is not uid 1000, set uid/gid_number as well.
5. ```ansible-playbook -i inventory factorio-localhost.yml -K```

----

## Instructions & Notes
There is currently no additional glibc step in this module since it was designed primiarily for use on CentOS8, which has a glibc that is compatible with `factorio-headless 1.0.0`.

### Do this first
There are 2 variables that you will need to update ***BEFORE*** you run the role against your server. Also note that if your server is not using the 'factorio' user on your linux host as the first and only user, you'll need to update the `uid_number` and `gid_number` variables to something other than 1000 lest you introduce conflict.

### Additional security
Finally, I don't recommend storing your Wube factorio account's access token in plain text. If you do so, make sure the place you store your ansible role is somewhat secure, so nobody steals access to your account.

To use an encrypted/vaulted string in place of plaintext, run:

```bash
$ ansible-vault encrypt_string
```
This will ask you for the encryption password, and for the password string. Once it spits out the encrypted stuff, paste that into the variable for the `factorio_account_token`. After that you'll have to add an extra flag to your playbook runs.
```
$ ansible-playbook -i inventory --ask-vault-pass -K factorio-up.yml
```

### Running remotely
You can run this ansible playbook aginst a remote host.. just update your inventory file to include the host that you're trying to reach, and remove or comment the "connection: local" line. Everything else should be pretty much the same, unless you need to specify an ssh password, in which case, add -k.
```bash
$ ansible-playbook -i inventory factorio-edited.yml --ask-vault-pass -kK
```
That should get you going.
