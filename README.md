# zudo
Run any command as root


some random ideas:

File Hirachy

```
=./zudo main binary to execute by user  
=./db/<CVE><name> contains the exploit stubs  
=./lib shell script include files  
=./run a local directory where the exploit is run from  
-./bin/<CVE><name>/<OS><DISTRO><arch>.tar.gz contains the pre-compiled binary  
+./src/<CVE><name> contains the source for each exploit
+./packaging/ contains scripts to turn /src/* into binary packages for various platforms
```
  
The user only ever needs directories prefixed wth `+`. Directories with `-` are for remote downloading and directories with `+` are for developers (but the user never needs to build the exploits as we do this for him - pre compiled static binaries)

Most of our design work will be in `./db/<CVE><name>/'. This could be `./db/CVE-2021-4034-PwnKit-2/` as an example. Thereunder it's unified structure for the `zudo` script to include:

conf.sh - Contains list of distro's and architectures for which static binaries exists.
check.sh - Return 0 if local system is vulnerable.
run.sh - Download & run the exploit (CWD is ./run)

All shell scripts source ./lib/funcs. The lib contain bash functions for downloading (using any available tool) and other bash defines/declarations (like colors) and `zudo` initialized so that ARCH, HOME, UID, DISTRO, TMPDIR etc are all set correctly.

Things to consider:
1. If we can not determine DISTRO then we should try to run all distro's. 
