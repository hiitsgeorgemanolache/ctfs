Use the `s_client` command (or `openssl-s_client` as the terminal would find it) and its `-connect host:port` flag to specify the host and optional port to connect to (`man` page: [s_client(1) - Linux man page](https://linux.die.net/man/1/s_client))
```bash
openssl s_client -connect localhost:30001
```
