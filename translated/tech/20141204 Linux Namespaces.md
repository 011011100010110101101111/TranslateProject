Linux �����ռ�
================================================================================
### ���� ###

��2.6.24����ں˿�ʼ��Linux ��֧��6�ֲ�ͬ���͵������ռ䡣���ǵĳ��֣�ʹ�û������Ľ����ܹ���ϵͳ����ø��ӳ��ף��Ӷ�����Ҫ����̫��ײ�����⻯������

- **CLONE_NEWIPC**: ���̼�ͨ��(IPC)�������ռ䣬���Խ� SystemV �� IPC �� POSIX ����Ϣ���ж���������
- **CLONE_NEWPID**: ���� ID �������ռ䣬���� ID ��������˼���������ռ��ڵĽ��� ID ���ܻ��������ռ���Ľ��� ID ��ͻ�����������ռ��ڵĽ��� ID ӳ�䵽�����ռ���ʱ��ʹ������һ������ ID������˵�������ռ��� ID Ϊ1�Ľ��̣��������ռ������ָ init ���̡�
- **CLONE_NEWNET**: ���������ռ䣬���ڸ���������Դ��/proc/net��IP ��ַ��������·�ɵȣ�����̨���̿��������ڲ�ͬ�����ռ��ڵ���ͬ�˿��ϣ��û������������һ��������
- **CLONE_NEWNS**: ���������ռ䣬��������ʱ���Խ����ص���ϵͳ���룬ʹ���������ʱ�����ǿ��Դﵽ chroot �Ĺ��ܣ����ڰ�ȫ�Է���� chroot ���ߡ�
- **CLONE_NEWUTS**: UTS �����ռ䣬��ҪĿ���Ƕ�������������������Ϣ����NIS����
- **CLONE_NEWUSER**: �û������ռ䣬ͬ���� ID һ�����û� ID ���� ID �������ռ������ǲ�һ���ģ������ڲ�ͬ�����ռ��ڿ��Դ�����ͬ�� ID��

������ C ���Խ������������Ϊ��ʾ���������ռ��ʱ����Ҫ�õ� C ���ԡ�����Ĳ��Թ����� Debian 6 �� Debian 7 ��ִ�С����ȣ���ջ�ڷ���һҳ�ڴ�ռ䣬����ָ��ָ���ڴ�ҳ��ĩβ����������ʹ�� **alloca()** �����������ڴ棬��Ҫ�� malloc() ������������ڴ�����ڶ��ϡ�

    void *mem = alloca(sysconf(_SC_PAGESIZE)) + sysconf(_SC_PAGESIZE);

Ȼ��ʹ�� **clone()** ���������ӽ��̣�����ջ�ռ�ĵ�ַ "mem"���Լ�ָ�������ռ�ı�ǡ�ͬʱ���ǻ�ָ����callee����Ϊ�ӽ������еĺ�����

    mypid = clone(callee, mem, SIGCHLD | CLONE_NEWIPC | CLONE_NEWPID | CLONE_NEWNS | CLONE_FILES, NULL);

**clone** ֮������Ҫ�ڸ������еȴ��ӽ������˳�������Ļ��������̻����������ȥ��ֱ�����̽����������ӽ��̱�ɹ¶����̣�

    while (waitpid(mypid, &r, 0) < 0 && errno == EINTR)
    {
    	continue;
    }

����ӽ����˳������ǻ�ص� shell ���档

    if (WIFEXITED(r))
    {
    	return WEXITSTATUS(r);
    }
    return EXIT_FAILURE;

���Ľ��ܵ� **callee** �����������£�

    static int callee()
    {
    	int ret;
    	mount("proc", "/proc", "proc", 0, "");
    	setgid(u);
    	setgroups(0, NULL);
    	setuid(u);
    	ret = execl("/bin/bash", "/bin/bash", NULL);
    	return ret;
    }

������� **/proc** �ļ�ϵͳ�������û� ID ���� ID��ֵ��Ϊ��u����Ȼ������ **/bin/bash** ����[LXC][1] �ǲ���ϵͳ�������⻯���ߣ�ʹ�� cgroups �������ռ��������Դ�ķ��롣�������ǰ����д������һ�𣬱�����u����ֵ��Ϊ65534���� Debian ϵͳ�У����ǡ�nobody���͡�nogroup����

    #define _GNU_SOURCE
    #include <unistd.h>
    #include <stdio.h>
    #include <stdlib.h>
    #include <sys/types.h>
    #include <sys/wait.h>
    #include <sys/mount.h>
    #include <grp.h>
    #include <alloca.h>
    #include <errno.h>
    #include <sched.h>
    static int callee();
    const int u = 65534;
    int main(int argc, char *argv[])
    {
    	int r;
    	pid_t mypid;
    	void *mem = alloca(sysconf(_SC_PAGESIZE)) + sysconf(_SC_PAGESIZE);
    	mypid = clone(callee, mem, SIGCHLD | CLONE_NEWIPC | CLONE_NEWPID | CLONE_NEWNS | CLONE_FILES, NULL);
    	while (waitpid(mypid, &r, 0) < 0 && errno == EINTR)
    	{
    		continue;
    	}
    	if (WIFEXITED(r))
    	{
    		return WEXITSTATUS(r);
    	}
    	return EXIT_FAILURE;
    }
    static int callee()
    {
    	int ret;
    	mount("proc", "/proc", "proc", 0, "");
    	setgid(u);
    	setgroups(0, NULL);
    	setuid(u);
    	ret = execl("/bin/bash", "/bin/bash", NULL);
    	return ret;
    }

ִ��������������������Ĵ��룺

    root@w:~/pen/tmp# gcc -O -o ns.c -Wall -Werror -ansi -c89 ns.c
    root@w:~/pen/tmp# ./ns
    nobody@w:~/pen/tmp$ id
    uid=65534(nobody) gid=65534(nogroup)
    nobody@w:~/pen/tmp$ ps auxw
    USER       PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
    nobody       1  0.0  0.0   4620  1816 pts/1    S    21:21   0:00 /bin/bash
    nobody       5  0.0  0.0   2784  1064 pts/1    R+   21:21   0:00 ps auxw
    nobody@w:~/pen/tmp$ 

ע������Ľ����UID �� GID �����ó� nobody �� nogroup �ˣ��ر��� ps ����ֻ����������̣����ǵ� ID �ֱ���1��5��LCTTע����������Ľ��� CLONE_NEWPID ʱ�ᵽ�Ĺ��ܣ����߳����ڵ������ռ��ڣ����� ID ����Ϊ1��ӳ�䵽�����ռ������65534���������ռ���� ID Ϊ1�Ľ���һֱ�� init�����������ֵ�ʹ�� ip netns ����������������ռ䡣��һ����ȷ����ǰϵͳû�������ռ䣺

    root@w:~# ip netns list
    Object "netns" is unknown, try "ip help".

��������£�����Ҫ�������ϵͳ�ںˣ��Լ� ip ���ߡ������������ں˰����2.6.24��ip ���߰汾Ҳ��࣬����2.6.24��LCTTע��ip ������ iproute ��װ���ṩ���˰�װ���汾���ں˰汾����������ºú�**ip netns list** ��û�������ռ���ڵ�����²������������Ϣ���Ӹ���Ϊ��ns1���������ռ俴����

    root@w:~# ip netns add ns1
    root@w:~# ip netns list
    ns1

�г�������

    root@w:~# ip link list
    1: lo:  mtu 65536 qdisc noqueue state UNKNOWN mode DEFAULT 
        link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    2: eth0:  mtu 1500 qdisc pfifo_fast state UNKNOWN mode DEFAULT qlen 1000
        link/ether 00:0c:29:65:25:9e brd ff:ff:ff:ff:ff:ff

�����µ������������ӵ������ռ䡣����������Ҫ�ɶԴ�������������������뽻����°ɣ�

    root@w:~# ip link add veth0 type veth peer name veth1
    root@w:~# ip link list
    1: lo:  mtu 65536 qdisc noqueue state UNKNOWN mode DEFAULT 
        link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    2: eth0:  mtu 1500 qdisc pfifo_fast state UNKNOWN mode DEFAULT qlen 1000
        link/ether 00:0c:29:65:25:9e brd ff:ff:ff:ff:ff:ff
    3: veth1:  mtu 1500 qdisc noop state DOWN mode DEFAULT qlen 1000
        link/ether d2:e9:52:18:19:ab brd ff:ff:ff:ff:ff:ff
    4: veth0:  mtu 1500 qdisc noop state DOWN mode DEFAULT qlen 1000
        link/ether f2:f7:5e:e2:22:ac brd ff:ff:ff:ff:ff:ff

���ʱ�� **ifconfig** -a ����Ҳ����ʾ����ӵ� veth0 �� veth1 ����������

�ܺã����ڽ������ݿ������ӵ������ռ���ȥ��ע��һ�£������ ip **netns exec** �������ڽ�����������������ռ���ִ�У�LCTTע������Ľ����ʾ���� ns1 ������������ռ��У�ֻ���� lo �� veth1 ������������

    root@w:~# ip link set veth1 netns ns1
    root@w:~# ip netns exec ns1 ip link list
    1: lo:  mtu 65536 qdisc noop state DOWN mode DEFAULT 
        link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    3: veth1:  mtu 1500 qdisc noop state DOWN mode DEFAULT qlen 1000
    link/ether d2:e9:52:18:19:ab brd ff:ff:ff:ff:ff:ff

���ʱ�� **ifconfig** -a ����ֻ����ʾ veth0��������ʾ veth1����Ϊ���������� ns1 �����ռ��С�

�����ɾ�� veth1������ִ����������

    ip netns exec ns1 ip link del veth1

Ϊ veth0 ���� IP ��ַ��

    ifconfig veth0 192.168.5.5/24

�������ռ���Ϊ veth1 ���� IP ��ַ��

    ip netns exec ns1 ifconfig veth1 192.168.5.10/24 up

�������ռ�����ִ�� ip addr **list** ���

    root@w:~# ip addr list
    1: lo:  mtu 65536 qdisc noqueue state UNKNOWN 
        link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
        inet 127.0.0.1/8 scope host lo
        inet6 ::1/128 scope host 
           valid_lft forever preferred_lft forever
    2: eth0:  mtu 1500 qdisc pfifo_fast state UNKNOWN qlen 1000
        link/ether 00:0c:29:65:25:9e brd ff:ff:ff:ff:ff:ff
        inet 192.168.3.122/24 brd 192.168.3.255 scope global eth0
        inet6 fe80::20c:29ff:fe65:259e/64 scope link 
           valid_lft forever preferred_lft forever
    6: veth0:  mtu 1500 qdisc pfifo_fast state UP qlen 1000
        link/ether 86:b2:c7:bd:c9:11 brd ff:ff:ff:ff:ff:ff
        inet 192.168.5.5/24 brd 192.168.5.255 scope global veth0
        inet6 fe80::84b2:c7ff:febd:c911/64 scope link 
           valid_lft forever preferred_lft forever
    root@w:~# ip netns exec ns1 ip addr list
    1: lo:  mtu 65536 qdisc noop state DOWN 
        link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    5: veth1:  mtu 1500 qdisc pfifo_fast state UP qlen 1000
        link/ether 12:bd:b6:76:a6:eb brd ff:ff:ff:ff:ff:ff
        inet 192.168.5.10/24 brd 192.168.5.255 scope global veth1
        inet6 fe80::10bd:b6ff:fe76:a6eb/64 scope link 
           valid_lft forever preferred_lft forever

�������ռ�����鿴·�ɱ�

    root@w:~# ip route list
    default via 192.168.3.1 dev eth0  proto static 
    192.168.3.0/24 dev eth0  proto kernel  scope link  src 192.168.3.122 
    192.168.5.0/24 dev veth0  proto kernel  scope link  src 192.168.5.5 
    root@w:~# ip netns exec ns1 ip route list
    192.168.5.0/24 dev veth1  proto kernel  scope link  src 192.168.5.10 

��󣬽����������������������ϣ�������Ҫ�õ��Žӡ����������ǽ� veth0 �Žӵ� eth0���� ns1 �����ռ�����ʹ�� DHCP �Զ���ȡ IP ��ַ��

    root@w:~# brctl addbr br0
    root@w:~# brctl addif br0 eth0
    root@w:~# brctl addif br0 veth0
    root@w:~# ifconfig eth0 0.0.0.0
    root@w:~# ifconfig veth0 0.0.0.0
    root@w:~# dhclient br0
    root@w:~# ip addr list br0
    7: br0:  mtu 1500 qdisc noqueue state UP 
        link/ether 00:0c:29:65:25:9e brd ff:ff:ff:ff:ff:ff
        inet 192.168.3.122/24 brd 192.168.3.255 scope global br0
        inet6 fe80::20c:29ff:fe65:259e/64 scope link 
           valid_lft forever preferred_lft forever

Ϊ���� br0 ����� IP ��ַΪ192.168.3.122/24��������Ϊ�����ռ�����ַ��

    root@w:~# ip netns exec ns1 dhclient veth1
    root@w:~# ip netns exec ns1 ip addr list
    1: lo:  mtu 65536 qdisc noop state DOWN 
        link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    5: veth1:  mtu 1500 qdisc pfifo_fast state UP qlen 1000
        link/ether 12:bd:b6:76:a6:eb brd ff:ff:ff:ff:ff:ff
        inet 192.168.3.248/24 brd 192.168.3.255 scope global veth1
        inet6 fe80::10bd:b6ff:fe76:a6eb/64 scope link 
           valid_lft forever preferred_lft forever

���ڣ� veth1 �� IP �����ó� 192.168.3.248/24 �ˡ�

--------------------------------------------------------------------------------

via: http://www.howtoforge.com/linux-namespaces

���ߣ�[aziods][a]
���ߣ�[bazz2](https://github.com/bazz2)
У�ԣ�[У����ID](https://github.com/У����ID)

������ [LCTT](https://github.com/LCTT/TranslateProject) ԭ�����룬[Linux�й�](http://linux.cn/) �����Ƴ�

[a]:http://www.howtoforge.com/forums/private.php?do=newpm&u=138952
[1]:http://en.wikipedia.org/wiki/LXC
