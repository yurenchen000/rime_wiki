# Squirrel Release Manual

TODO: automate this.

1.  Update the version of `squirrel/data/squirrel.yaml` if there are changes since last release.

1.  Run `package/bump_version` to update app version; push the commit and version tag to GitHub.
    Pushing the version tag will trigger a release build on Travis CI.

1.  In Bintray package [`rime/squirrel/release`](https://bintray.com/rime/squirrel/release), publish the new version uploaded by Travis CI, then download the zip archive.

1.  Copy/move the downloaded file to local source tree, under `squirrel/package/archives/`.

1.  Put private key file in `squirrel/package/sign/`;
    The private key for signing auto-update Squirrel releases is owned by Rime Keyholder.

1.  In Bintray package [`rime/squirrel/release`](https://bintray.com/rime/squirrel/release), take down the sha256 checksum of the downloaded archive, for example:

    Downloads (ver: 0.14.0)
    Squirrel-0.14.0.zip
    Size: 19.52 MB
    sha256: e7bc74ed1f0447a48b8c86f3685d8c6d0640d39895e6e4550488beeb4f870926 <-- take this down

1.  Run
    ```shell
    checksum=e7bc74ed1f0447a48b8c86f3685d8c6d0640d39895e6e4550488beeb4f870926 make sign-archive
    ```
    The script generates `package/*appcast.xml`.

1.  Prepare to update project website.
    Assume GitHub repo `rime/<project>` is cloned to local directory `~/code/rime/<project>`; do
    ```shell
    cp ~/code/rime/squirrel/package/appcast.xml ~/code/rime/home/blog/source/release/squirrel/appcast.xml
    cp ~/code/rime/squirrel/package/testing-appcast.xml ~/code/rime/home/blog/source/testing/squirrel/appcast.xml

    cat <<EOF > ~/code/rime/home/blog/source/release/squirrel/index.md
    title: 【鼠鬚管】更新日誌
    comments: false
    $(date '+date: %Y-%m-%d %H:%M:%S')
    ---

    EOF
    cat ~/code/rime/squirrel/CHANGELOG.md >> ~/code/rime/home/blog/source/release/squirrel/index.md
    cp ~/code/rime/home/blog/source/release/squirrel/index.md  ~/code/rime/home/blog/source/testing/squirrel/index.md
    ```

1.  Also update latest Squirrel version and download URL in
    - `~/code/rime/home/blog/source/_data/downloads.yaml`
    - `~/code/rime/home/blog/source/download/index.md`

1.  Test the website locally with Hexo:
    ```shell
    npm install -g hexo-cli
    cd ~/code/rime/home/blog
    npm install
    hexo server
    open http://0.0.0.0:4000/
    ```
    Verify the updated links are valid.

1.  Commit changes to [`rime/home`](https://github.com/rime/home), with commit message `chore(release): squirrel ${version} :tada:`
    This will trigger automated update to https://rime.im by Travis CI.

