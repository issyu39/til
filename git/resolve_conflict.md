## コンフリクト解消手順

- 対象リポジトリをチェックアウトします。

    ```git checkout branch2```

- 以下で rebase します。

    ```git pull --rebase origin master```

- コンフリクトが発生するので手動でコンフリクトを解消して git add します。

    ```git add a.txt```

- 以下で rebase を続行します。
    
    ```git rebase --continue```

- 以下でリモートリポジトリに強制的に push します。
    
    ```git push origin branch2 --force```
