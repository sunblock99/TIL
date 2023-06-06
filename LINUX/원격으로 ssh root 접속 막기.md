![](https://velog.velcdn.com/images/sunblock99/post/b9028f7f-7302-4276-9d38-7ba3f9746a30/image.png)

root계정으로 설정해야함 !!

vi /etc/ssh/sshd_config

#PermitRootLogin yes
→ root 로그인 허용값이 yes로 된 상태로, 주석처리되어 있다. 어쨌든 기본값은 no

service sshd restart
