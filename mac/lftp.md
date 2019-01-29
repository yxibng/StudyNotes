# [lftp man page](https://lftp.yar.ru/lftp-man.html)


1. 安装
    ```
    brew install lftp
    ```
2. 登录
    ```
    lftp [OPTS] <site>

    lftp -u <user>[,<pass>]  <site>
    ```
3. 上传

    ```
    Usage: put [OPTS] <lfile> [-o <rfile>]
    Upload <lfile> with remote name <rfile>.
    -o <rfile> specifies remote file name (default - basename of lfile)
    -c  continue, reput
        it requires permission to overwrite remote files
    -E  delete local files after successful transfer (dangerous)
    -a  use ascii mode (binary is the default)
    -O <base> specifies base directory or URL where files should be placed
    
    # example
    put -e -O /ios_inner_test/release/ $PRJ_ROOT_DIR/build/index.html -o release.html;
    ```