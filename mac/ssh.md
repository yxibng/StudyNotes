# github ssh & ubuntu ssh

参考文章

- [SSH Config 那些你所知道和不知道的事](https://deepzz.com/post/how-to-setup-ssh-config.html)
- [Connecting to GitHub with SSH](https://help.github.com/articles/connecting-to-github-with-ssh/)
- [https://www.ssh.com/ssh/](https://www.ssh.com/ssh/)
- [https://help.ubuntu.com/lts/serverguide/openssh-server.html.en](https://help.ubuntu.com/lts/serverguide/openssh-server.html.en)

## github ssh 
1. create a new ssh key

    ``` bash
    ➜  ~ ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
    ```

    默认生成到 `~/.ssh`

    ```
    id_rsa
    id_rsa.pub
    ```

    针对不同的场景，可以取不同的名字

    ```
    ➜  .ssh ls
    bwg_id_rsa        config            github_id_rsa.pub id_rsa.pub
    bwg_id_rsa.pub    github_id_rsa     id_rsa            known_hosts
    ```
2. add to ssh-agent

    start ssh-agent 
    ```
    ➜  ~ eval "$(ssh-agent -s)"
    Agent pid 73358

    ```

    modify `~/.ssh/config`

    ```
    Host *
     AddKeysToAgent yes
     UseKeychain yes
     IdentityFile ~/.ssh/id_rsa
    ```

    可以有不同的配置，参考[SSH Config 那些你所知道和不知道的事](https://deepzz.com/post/how-to-setup-ssh-config.html)

    我的配置如下
    ```
    ➜  .ssh cat config
    Host github.com
     AddKeysToAgent yes
     UseKeychain yes
     IdentityFile ~/.ssh/github_id_rsa
    Host 104.194.69.135
     AddKeysToAgent yes
     UseKeychain yes
     IdentityFile ~/.ssh/bwg_id_rsa
    ```

    add to ssh-agent， `-K Store passphrases in your keychain.`

    ``` bash
    ssh-add -K ~/.ssh/id_rsa
    ```


    ssh-add 命令

    ```
    ➜  .ssh ssh-add --help
    ssh-add -l
    ssh-add -D
    ssh-add -d
    ssh-add -K
    ```

    重启 ssh-agent
    
    ```
    killall ssh-agent; eval `ssh-agent`
    ```
3. [Add the SSH key to your GitHub account.](https://help.github.com/articles/adding-a-new-ssh-key-to-your-github-account)

    ```
    ➜  .ssh cat id_rsa.pub
    ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAACAQDFQhwpYZzOoUc+ztzG6QJmexDIQecw0eQIpX33GcmIbnbv18dO+Muz/zCLCf3TMRu02XgB1dfqRvo+3sMzkXgEffnsf0BdHaWUwOVO1XxXhf7bXz4CpXHNc1Le//tuAYx5Wv/JmOfm5oLQD+c9VaoXQfJMODXXi7psmk17BJGrMMUexmgu2hKuG9jeY5woBKbTF4mbkjHpQHxj3HMlTH01c8yHcZQCtsVFgvoA3NUTWGgrHPWd2SLpp589huKfd9xNsjTMHkFwyBsWOfIUp/tI6+EggvQl7e53qxYhXA4GuDA+IAs3FC56nuyHFq0fU6HDLkZGk+0HhvdEwlAk1ON7alx17XDWhf5DeRpeK34uJBb4DhwxI1o5g2iTxYN95b9w+tGZmcNrB39C+30OYQCAr2LMlmX4oUnhf6ukZOiGeVs9gu8iv1e3ijcYLNaUmjhjTNIZ0k9xUxcR7ndy7gbe+4DWa1SLP1yeWqA4oTu2J8AKFQ/PkCz1pAUV+jiiDGRcXKxBt2n9UOA5aUDY+B6Nk1VW4h20lst4e7aYXBXYM6qE/GRA6EPMh8bI95YD5MeB3nTYYe1MLF93+248SvtbykzzLKXx2CgRALszvS/dZRwV7q6e1fAWA7nhK+IfOp1ma743NpdmXCzsLhN5B/kxPlwH/g9wSfX61MxMQ+75RQ== your_email@example.com
    ```

    拷贝到剪切板命令
    ```
    pbcopy < ~/.ssh/id_rsa.pub
    # Copies the contents of the id_rsa.pub file to your clipboard
    ```

    测试ssh是否成功
    ```
    ➜  ~ ssh -T git@github.com
    Hi yxibng! You've successfully authenticated, but GitHub does not provide shell access.
    ```

## ubuntu ssh 远程登录

1. create ssh keys

    ```
    ssh-keygen -t rsa

    ```

    > bwg_id_rsa </br>
    > bwg_id_rsa.pub

2. add to ssh-agent， 参考上面的步骤
3. copy `xx.pub` to the remote host

    ```
    ssh-copy-id username@remotehost
    ssh-copy-id -i ~/.ssh/bwg_id_rsa.pub -p 10922 root@10.0.0.2
    ```
4. ssh login 

    ```
    ssh -p 10922 root@10.0.0.2
    ```


