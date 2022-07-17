You can change version of pritunl in Dockerfile variable.

To run pritunl you should run this command:
```
docker-compose up -d
```

after you should get password, enter to container:
```
docker exec -ti pritunl_pritunl_1 bash
```

and run command:
```
root@f8c91b8c82a9:/# pritunl default-password
```

you will get answer:
```
[undefined][2022-07-17 14:39:37,596][INFO] Getting default administrator password
Administrator default password:
  username: "pritunl"
  password: "RNQg3alBwWEM"
```

with this credential you can enter to pritunl web interface
