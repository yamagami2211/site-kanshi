# v1.26.1から更新されない場合の対処法  
Upptimeが v1.26.1 に更新( [Commit 72a0b89](https://github.com/yamagami2211/site-kanshi/commit/72a0b89341b16453f76523278d23ab5d33928012#diff-87ee5504a3e25ac558b343724c905f2f7949e8cec3d92b9c4300bb922afa164f) )してから更新されないことが発生。
原因はバージョンアップにより `GH_PAT` から `GITHUB_TOKEN` に変更されたことが原因です。

update-template.ymlの内容を手動で書き換えて、Update Template CIを実行することでアップデートが行えます。

```YAML
    steps:
      - name: Checkout
        uses: actions/checkout@v2.3.3
        with:
          ref: ${{ github.head_ref }}
          token: ${{ secrets.GITHUB_TOKEN }}
      - name: Update template
        uses: upptime/uptime-monitor@master
        with:
          command: "update-template"
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
```
から以下のように書き換えます。

```YAML
    steps:
      - name: Checkout
        uses: actions/checkout@v2.3.3
        with:
          ref: ${{ github.head_ref }}
          token: ${{ secrets.GH_PAT }}
      - name: Update template
        uses: upptime/uptime-monitor@master
        with:
          command: "update-template"
        env:
          GH_PAT: ${{ secrets.GH_PAT }}
```
