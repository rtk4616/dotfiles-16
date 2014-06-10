A new beginning for what used to be my
[conf_dir](https://github.com/kejadlen/conf_dir) project, since `dotfiles`
appears to be the conventional name of these types of repos nowadays.

This uses [Ansible](https://github.com/ansible/ansible) to provision new
machines and [stow](http://www.gnu.org/software/stow/) for managing conf
files that can be symlinked.

# Requirements

- [Ansible](https://github.com/ansible/ansible)

# Usage

``` shell
ansible-playbook [playbook].yml
```
