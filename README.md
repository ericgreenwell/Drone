## Creating git repo

1. Create repo in GitHub
2. Copy SSH link
3. Create sshkey and copy public key to github account:
    ```
    ssh-keygen -t rsa -b 4096
    cat ~/.ssh/id_rsa.pub

    ```
4. To initialize (in the directory containing your code:
    ```
    # set remote urls
    git init
    git remote add orgin master
    git remote set-url origin <ssh-url>

    # initial commit
    git add .
    git commit -m "init commit"
    git push -u origin master


    # all subsequent pushed
    git add .
    git commit -m "your msg"
    git push
    ```
