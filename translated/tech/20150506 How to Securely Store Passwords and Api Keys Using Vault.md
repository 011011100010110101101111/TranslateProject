���ʹ��Vault��ȫ�Ĵ洢�����API��Կ
=======================================================================
Vault�����ڰ�ȫ�Ļ�ȡ������Ϣ�Ĺ��ߡ������Ա������롢API��Կ��֤�����Ϣ��Vaultͨ��ǿ���ʿ��ƻ��ƺ͸���չ�Ե��¼���־�ṩ��һ��ͳһ�Ľӿ������ʱ�����Ϣ��

�Թؼ���Ϣ��׼�������һ�����ѵ����⣬�����ǵ�������û���ɫ���û�����ͬ�Ĺؼ���Ϣ������ʹ�ò�ͬ��Ȩ�޵�¼���ݿ��ϸ�ڣ���������API��Կ���������ļܹ���ͨ��֤��ȡ���������Ϣ�ɲ�ͬ��ƽ̨���й�������ʹ��һЩ�Զ��������ʱ�������ø��㣬��ˣ���ȫ�Ĵ洢�����������־�����ǲ����ܵġ���VaultΪ���ָ�������ṩ��һ�����������

### ͻ���ص� ###

���ݼ��ܣ�Vault�ܹ���֤�ڲ��洢���ݵ�����¶����ݽ��м��ܡ����ܡ������������ڱ���Դ洢���ܺ�����ݶ����迪�������Լ��ļ��ܼ�����Vault������ȫ�Ŷ��Զ��尲ȫ������

**��ȫ����洢**��Vault�ڽ�������Ϣ��API��Կ�����롢֤�飩�洢���־û��洢֮ǰ�����ݽ��м��ܡ���ˣ��������ż���õ��˻�ȡ�洢�����ݵ�Ȩ�ޣ���Ҳû���κ����壬�������ݱ����ܡ�

**��̬����**��VaultΪ����AWS��SQL���ݿ��ϵͳ����������롣���Ӧ����Ҫ���AWS S3��Ͱ�����磬����Vault����AWS��Կ�ԣ���������Ҫ�ı�����Ϣһ�������ڡ����������Ϣ������ʱ����ں󽫱�ò����á�

**���޺͸���**��Vault��������Ϊ�������豣����Ϣ��һ�������ڹ��ڣ����������ջر�����Ϣ�����Ӧ������Ҫ������Ϣ�������ͨ��API���������ڡ�

**����**���������ڵ���֮ǰ��Vault���Գ���һ��������Ϣ����һ��������Ϣ����

### ��װVault ###

�����ַ�ʽ����װʹ��Vault��

**1. Ԥ�����Vault������** ���������е�Linux���а棬���ص�ַ���£�һ��������ɣ���ѹ����������ϵͳPATH·���£��Ա㷽��ĵ��á�

- [Download Precompiled Vault Binary (32-bit)][1]
- [Download Precompiled Vault Binary (64-bit)][2]
- [Download Precompiled Vault Binary (ARM)][3]

������Ӧ��Ԥ�����Vault�����ư汾��

![wget binary](http://blog.linoxide.com/wp-content/uploads/2015/04/wget-binary.png)

��ѹ���ص��Ķ����ư汾��

![vault](http://blog.linoxide.com/wp-content/uploads/2015/04/unzip.png)

ף�أ������ڿ���ʹ��Vault�ˡ�

![](http://blog.linoxide.com/wp-content/uploads/2015/04/vault.png)

**2. ��Դ�������** ����һ����ϵͳ�а�װVault�ķ�ʽ���ڰ�װVault֮ǰ��Ҫ��װGO��GIT��

�� **Redhatϵͳ�а�װGO** ʹ�������ָ�

    sudo yum install go

�� **Debinϵͳ�а�װGO** ʹ�������ָ�

    sudo apt-get install golang

����

    sudo add-apt-repository ppa:gophers/go

    sudo apt-get update

    sudo apt-get install golang-stable

�� **Redhatϵͳ�а�װGIT** ʹ����������

    sudo yum install git

�� **Debianϵͳ�а�װGIT** ʹ����������

    sudo apt-get install git

һ��GO��GIT���ѱ���װ�ã����Ǳ���Կ�ʼ��Դ����밲װVault��

> �����е�Vault�ֿ⿽����GOPATH

    https://github.com/hashicorp/vault

> ����������ļ��Ƿ���ڣ�����������ڣ���ôVaultû�б���¡�����ʵ�·����

    $GOPATH/src/github.com/hashicorp/vault/main.go

> ִ�������ָ��������Vault�������������ļ��ŵ�ϵͳbinĿ¼�¡�

    make dev

![path](http://blog.linoxide.com/wp-content/uploads/2015/04/installation4.png)

### һ��Vault���Ž̳� ###

�����Ѿ�������Vault�Ĺٷ�����ʽ�̳̣��Լ�����SSH�ϵ����

**����**

��ݽ̳̰������в��裺

- ��ʼ������������Vault
- ��Vault�ж�����������Ȩ
- ��д������Ϣ
- �ܷ�����Vault

**��ʼ������Vault**

���ȣ�������ҪΪ����ʼ��һ��Vault�Ĺ���ʵ����
�ڳ�ʼ�������У�����������Vault���ܷ���Ϊ��
���ڳ�ʼ��Vault���������ʹ��һ�����ܷ���Կ

    vault init -key-shares=1 -key-threshold=1

����ע�⵽Vault�������ӡ����������Կ����Ҫ��������նˣ���Щ��Կ�ں���Ĳ����л�ʹ�õ���

![Initializing SSH](http://blog.linoxide.com/wp-content/uploads/2015/04/Initializing-SSH.png)

**��������Vault**

��һ��Vault����������ʱ�������ܷ��״̬��������״̬�£�Vault������Ϊ֪�����������δ�ȡ����洢������֪����ζ�����н��ܡ�
Vaultʹ�ü�����Կ���������ݡ������Կ��"����Կ"���ܣ�����Կ�����档��������Կ��Ҫһ����Ƭ����ֵ������������У�����ʹ��һ����Ƭ�������������Կ��

    vault unseal <key 1>

![Unsealing SSH](http://blog.linoxide.com/wp-content/uploads/2015/04/Unsealing-SSH.png)

**Ϊ����������Ȩ**

��ִ���κβ���֮ǰ�����ӵĿͻ���Ӧ�ñ���Ȩ����Ȩ�Ĺ����Ǽ���һ���˻��߻����ǲ�������˵��������������������ݡ�����������Vault��������ʱ��ʹ�á�
Ϊ����������ǽ�ʹ���ڲ���2�����ɵ�root���ơ�������Ӧ���Թ���ģʽ���֡�

    vault auth <root token>

![Authorize SSH](http://blog.linoxide.com/wp-content/uploads/2015/04/Authorize-SSH.png)

**��д������Ϣ**

����Vault�Ѿ��������׵������ǿ��Կ�ʼʹ��Ĭ�ϵ������˶�д������Ϣ�ˡ�д��Vault�еı�����Ϣ�����ܲ���д���˵Ĵ洢����˴洢���Ʋ���鿴δ���ܵ�ֵ������û���κ�����Vault���ɽ��ܵı�Ҫ��Ϣ��

    vault write secret/hello value=world

��Ȼ��������������Զ����������Ϣ�ˣ�

    vault read secret/hello

![RW_SSH](http://blog.linoxide.com/wp-content/uploads/2015/04/RW_SSH.png)

**�ܷ�����Vault**

��һ��API���ܷ�Vault����������������Կ����Ҫ�������ܹ������ָ������ܷ����Ҫһ��ӵ��rootȨ�޵ĵ�����������ͨ����һ�ֺ�����"���Ʋ�������"��һ���֡�
���ַ�ʽ�У������һ����⵽�����֣�Vault���ݽ������̱���ס���Ա���С����ʧ�����û�л�ȡ������Կ��Ƭ�������ᱻ�ٴλ�ȡ��

    vault seal

![Seal Vault SSH](http://blog.linoxide.com/wp-content/uploads/2015/04/Seal-Vault-SSH.png)

��������Ž̵̳Ľ�β��

### �ܽ� ###

Vault��һ���ǳ����õ�Ӧ�ã����ṩ��һ���ɿ��Ұ�ȫ�Ĵ洢�ؼ���Ϣ�ķ�ʽ�����⣬���ڴ洢ǰ���ܹؼ���Ϣ��ά�����������־�����������ڵķ�ʽ��ȡ������Ϣ����һ�������ڹ��ڣ����������ջر�����Ϣ��Vault��ƽ̨�����ģ����ҿ���������غͰ�װ��Ҫ����Vault�ĸ�����Ϣ������ʹٷ���վ��

--------------------------------------------------------------------------------

via: http://linoxide.com/how-tos/secure-secret-store-vault/

���ߣ�[Aun Raza][a]
���ߣ�[����ID](https://github.com/����ID)
У�ԣ�[У����ID](https://github.com/У����ID)

������ [LCTT](https://github.com/LCTT/TranslateProject) ԭ�����룬[Linux�й�](https://linux.cn/) �����Ƴ�

[a]:http://linoxide.com/author/arunrz/
[1]:https://dl.bintray.com/mitchellh/vault/vault_0.1.0_linux_386.zip
[2]:https://dl.bintray.com/mitchellh/vault/vault_0.1.0_linux_amd64.zip
[3]:https://dl.bintray.com/mitchellh/vault/vault_0.1.0_linux_arm.zip
