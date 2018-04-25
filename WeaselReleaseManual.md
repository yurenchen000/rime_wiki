# weasel 發佈手冊

1.  在 [Trello](https://trello.com/b/iUJWFnjb/rime-development) 或筆記本上闢一葉紙，記下主要更新內容，方便粘貼到更新日誌。
    例如 https://trello.com/c/WieadeZt
2.  對比上次發佈（git tag），檢查 `weasel.yaml` 是否需要更新版本號。
2.  代碼交齊後，用工具根據慣例格式生成變更記錄。
    以 clog-cli 爲例，爲新版本 0.11.0 生成變更記錄，做法是
    ``` sh
    # cargo install clog-cli
    cd weasel
    clog --from-latest-tag \
        --changelog CHANGELOG.md \
        --repository https://github.com/rime/weasel \
        --setversion 0.11.0
    ```
3.  把手寫的「主要更新」列在變更記錄文件中本次發佈的標題下，格式（包括空行）須與自動生成的段落相同：
    ``` markdown
    <a name="0.11.0"></a>
    ## 0.11.0 (2018-04-07)


    #### 主要更新

    * 新增 [Rime 配置管理器](https://github.com/rime/plum)，通過「輸入法設定／獲取更多輸入方案」調用
    * 在輸入法語言欄顯示狀態切換按鈕（TSF 模式）
    * 修復多個前端兼容性問題
    * 新增配色主題「現代藍」`metroblue`、「幽能」`psionics`

    #### Features
    ```
4.  提交對變更記錄的修改
    ``` sh
    git commit --all --message "doc(CHANGELOG.md): release 0.11.0"
    ```
5.  用腳本更新輸入法程序的版本號，並提交修改過的文件
    ``` sh
    update/bump-version.sh # print usage
    update/bump-version.sh 0.10.0 0.11.0

    git commit --all --message "chore(release): 0.11.0 :tada:"
    ```
6.  以上提交在 `master` 完成。如果在另一個分支發佈，如 `legacy-userdb-migration`，則將以上修改合併到分支。
7.  push 所有修改到 GitHub。檢驗 CI build：下載安裝程序，測試全新安裝以及從上一個推送版本升級。
8.  確認該版本可發佈後，在 `master` 或要做發佈的分支上打標籤。（可推遲到發佈完成）推送標籤到 GitHub。
    ``` sh
    git tag --annotate 0.11.0 --message "chore(release): 0.11.0 :tada:"
    git push --tags
    ```
9.  將（release tag 或最終 push 觸發的） CI build 生成的安裝包上傳到 bintray。
    在 https://bintray.com/rime/weasel/release 新建版本 (New version)，命名爲版本號 `0.11.0`，描述：`小狼毫 0.11.0`。
    爲新建的版本上傳文件 `weasel-0.11.0.0-installer.exe`。
10. 生成顯示在推送更新介面的變更記錄。只包含 `CHANGELOG.md` 的最新版本章節，格式爲樸素的 HTML 網頁。
    ``` sh
    # npm install --global marked
    update/write-release-notes.sh
    ```
    留存生成的 `release-notes.html` 文件備用。

（未完待續）
