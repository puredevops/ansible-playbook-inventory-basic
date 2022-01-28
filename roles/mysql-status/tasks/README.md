### Add multiple ssh keys 

```
Host *
  IdentityFile ~/.ssh/your_ssh_key
  IdentityFile ~/.ssh/your_ssh_key2
  AddKeysToAgent yes

```


output to file

```
- name: "Gather lsof on B"
  delegate_to: B
  command: lsof
  register: lsof_command
  run_once: true
- name: "Save lsof log"
  local_action:
    copy content="{{ lsof_command.stdout }}" dest="/root/lsof.log"
  run_once: true
```

```
ansible-playbook -i hosts.yml mc-play.yml --extra-vars "remote_user=ec2-user"
```

### List EC2 Ip's

aws ec2 describe-instances \
  --filter "Name=instance-state-name,Values=running" \
  --query "Reservations[*].Instances[*].[PublicIpAddress, Tags[?Key=='Name'].Value|[0]]" \
  --output json --profile parthi-ycn