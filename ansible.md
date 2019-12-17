
#### init
- set ssh key
public key -> authorized_keys, share to other server
- ansible hosts
/etc/ansible/hosts

- command
ansible [host_cluses] option args
```
ansible all -m ping # -m module_name
ansible all -a "echo 'dddd'"
```

- dictionary
inventory: 商品明細表
playbooks: シナリオ


#### ansible hosts
##### config
group crults群れ

```
[mysql]
mysql01.com
mysql02.com

[database:children]
mysql
oracle

[database:vars]
ansible_ssh_port: 22
```

ansible_ssh_port, ansible_ssh_user, ansible_ssh_host, ansible_ssh_pass
ansible_sudo_pass,
ansible_sudo_exe: sudo command path(/bin)

ansible_ssh_private_key_file
ansible_shell_type, ansible_python_interpreter




##### select

username, groupname,



#### playbooks

```
tasks:
  - name: xxx
    debug:
	  msg: xxxx
  - name: xxx
    debug:
	
```
hosts: host list

tasks: task list
vars: variavles dict
vars_files: file list

any_errors_fatal: boolean, 一つ錯誤があるとすぐに他のタスクを防ぐ
gather_facts: boolean, タスク実行する前にinventionを収集しておく

max_fail_percentage: number, すべてのタスクを終わった前に許した最大な錯誤数

no_log, port, sudo, su


##### commands

``` yaml
copy:
  src, dest
  owner, group, mode
  attribute,decrypt, force
  directory_mode: set the mode for the directories when do recursively
file:  # set a file
  owner, group, mode, attribute
  src: path of the file to link
  path==dest==name
  recurse
  state:
    directory: create sub directory
    file: will be create if not exist
	link: create a symbol link
	hard: hard link
	absent: delete a directory with recursively
fetch:
  src, dest, flat
command: #execute a command, but like "$HOME" and "|", "&"... will not work
 args:
   chdir: path
   stdin

archive: # tar, zip or other
  path, dest, group, owner, mode, exclude_path
  format: gz, bz2, zip
  remove: remove source file after archived
unarchive: # unzip
  list_file
  src, dest
with_fileglob: file, *

{{lookup("fileglob", value)}}}


```


##### global variable

inventory_hostname




















