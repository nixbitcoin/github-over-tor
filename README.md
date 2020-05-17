# GitHub over Tor

So you're an pseudonymous developer like me. You don't want to leak your real identity through your IP address. You also don't want to rely on shoddy VPN services. What to do? Follow this guide to setup GitHub over Tor-tunneled ssh.

1. Install required software

     ```console
     $ sudo apt-get install netcat tor
     ```

2. Create an ssh key

     ```console
     $ ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
     ```

3. When you're prompted to "Enter a file in which to save the key," press Enter. This accepts the default file location.

4. At the prompt, type a secure passphrase.

5. [Add the key to your GitHub account](https://help.github.com/en/github/authenticating-to-github/adding-a-new-ssh-key-to-your-github-account)

6. Add this to your `~/.ssh/config`

     ```
     Host github.com
     IdentityFile ~/.ssh/github_rsa
     ProxyCommand nc -X 5 -x localhost:9050 %h %p
     ```

     This will forward all ssh commands to github.com through your Tor proxy

7. Make sure Tor is off

     ```console
     $ sudo systemctl stop tor
     ```

8. Try cloning a repository, ex.

     ```console
     $ git clone git@github.com:fort-nix/nix-bitcoin.git
     ```

     This should yield a

     ```
     Cloning into 'nix-bitcoin'...
     ssh_exchange_identification: Connection closed by remote host
     fatal: Could not read from remote repository.

     Please make sure you have the correct access rights
     and the repository exists.
     ```

9. Turn on Tor

     ```console
     $ sudo systemctl start tor
     ```

10. Clone a repository

     ```console
     $ git clone git@github.com:fort-nix/nix-bitcoin.git
     ```

     Make sure to always use the ssh variant of GitHub repositories.
