## Настройка удаленного доступа к серверу по SSH

## Linux

### 1. Генерация ключей

```bash
ssh-keygen -t ed25519 -C "custom_name"
```

custom_name это просто имя по которому вы потом сможете понять что это за ключ в куче других, но по факту оно ни на что не влияет

он спросит ввести **passphrase**. просто нажмите enter.

passphrase это просто дополнительный уровень защиты вашего private key. даже если кто-то им завладеет, то он ничего не сможет сделать без пароля, который его зашифровал, но лично я нигде до этого не встречал подобного. обычно никто это не использует.

```console
Generating public/private ed25519 key pair.
Enter file in which to save the key (/home/dimweb/.ssh/id_ed25519): ~/.ssh/lesson_ssh
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
Your identification has been saved in /home/dimweb/.ssh/lesson_ssh
Your public key has been saved in /home/dimweb/.ssh/lesson_ssh.pub
The key fingerprint is:
SHA256:loA5ehe2tngsotRTN8m2Wl2Haur3pP8wLOAWISL9bWU custom_name
The key's randomart image is:
+--[ED25519 256]--+
|                 |
|  .  o           |
| . o+.+. E       |
|  ..oo++=.  .    |
|  . .o+@S  o .   |
|  ...==o* + .    |
| ..oo += = =     |
|.. ..o+ o.+ o    |
|.    ..o..oo..   |
+----[SHA256]-----+
```

### 2. Копирование публичного ключа на удаленный сервер

```bash
ssh-copy-id -i ~/.ssh/lesson_ssh.pub root@94.198.216.62
```

```console
usr/bin/ssh-copy-id: INFO: Source of key(s) to be installed: "/home/dimweb/.ssh/lesson_ssh.pub"
The authenticity of host '94.198.216.62 (94.198.216.62)' can't be established.
ECDSA key fingerprint is SHA256:SDLD0Yic330w7K1jqXcvh4eNCVFT4A9WcTd5iFbpoho.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
/usr/bin/ssh-copy-id: INFO: attempting to log in with the new key(s), to filter out any that are already installed
/usr/bin/ssh-copy-id: INFO: 1 key(s) remain to be installed -- if you are prompted now it is to install the new keys
root@94.198.216.62's password:

Number of key(s) added: 1

Now try logging into the machine, with:   "ssh 'root@94.198.216.62'"
and check to make sure that only the key(s) you wanted were added.
```

### 3. Подключение к серверу при помощи ssh

- аутпут из консоли подсказывает нам что мы можем подключиться простой командой ssh 'root@94.198.216.62', но это было бы правдой, если бы мы назвали файл по стандарту, но это не так. поэтому нам нужно указать свой путь к приватному ключу

```bash
ssh root@94.198.216.62 -i ~/.ssh/lesson_ssh
```

```console
Welcome to Ubuntu 18.04.6 LTS (GNU/Linux 4.15.0-202-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

 * Introducing Expanded Security Maintenance for Applications.
   Receive updates to over 25,000 software packages with your
   Ubuntu Pro subscription. Free for personal use.

     https://ubuntu.com/pro
New release '20.04.5 LTS' available.
Run 'do-release-upgrade' to upgrade to it.

Last login: Sun Feb 12 01:53:21 2023 from 195.14.48.196
root@1251457-cr63564:~#
```

Ссылки:

- https://docs.gitlab.com/ee/user/ssh.html#generate-an-ssh-key-pair
- https://phoenixnap.com/kb/setup-passwordless-ssh

## Windows

**все команды вводятся в powershell**

### 1. Генерация ключей

- также на вопросе passphrase просто жмем enter
- в моем случае я назвал свой ключ **lesson_key_1**, важно вводить путь целиком, а не только название

```bash
ssh-keygen
```

```console
Enter file in which to save the key (C:\Users\dimweb/.ssh/id_rsa): C:\Users\dimweb/.ssh/lesson_key_1
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
Your identification has been saved in C:\Users\dimweb/.ssh/lesson_key_1.
Your public key has been saved in C:\Users\dimweb/.ssh/lesson_key_1.pub.
The key fingerprint is:
SHA256:MZbUBWuBuNvIahThuBeHg1pqcm8dtM8ojtvyOVjCAEc dimweb\dimweb@dimweb
The key's randomart image is:
+---[RSA 3072]----+
| .E    ..ooo.    |
|. . . .....o     |
|.. + o .= o      |
|. + * +. +       |
| * . B =S        |
|+.= + * .        |
|o. B o =         |
|  ooB.o o        |
|  oB=o           |
+----[SHA256]-----+
```

### 2. Копирование публичного ключа на удаленный сервер

- scp программа на виндовс, которая позволяет копировать файлы с одной машины на другую
- самая простая сигнатура программы выглядит так: scp FILEPATH USER@IP_ADDRES:MOUNT_FOLDER

```bash
scp C:\Users\dimweb\.ssh\lesson_key_1.pub root@94.198.216.62:~/.ssh/authorized_keys
```

```console
root@94.198.216.62's password:
lesson_key_1.pub                                                                                         100%  574    14.4KB/s   00:00
```

### 3. Подключение к серверу при помощи ssh

```bash
ssh -i C:\Users\dimweb/.ssh/lesson_key_1 root@94.198.216.62
```

```console
Welcome to Ubuntu 18.04.6 LTS (GNU/Linux 4.15.0-202-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

 * Introducing Expanded Security Maintenance for Applications.
   Receive updates to over 25,000 software packages with your
   Ubuntu Pro subscription. Free for personal use.

     https://ubuntu.com/pro
New release '20.04.5 LTS' available.
Run 'do-release-upgrade' to upgrade to it.

Last login: Sun Feb 12 02:41:20 2023 from 195.14.48.196
```

Ссылки:

- https://chrisjhart.com/Windows-10-ssh-copy-id/
- https://success.tanaza.com/s/article/How-to-use-SCP-command-on-Windows
- https://www.strongdm.com/blog/ssh-passwordless-login

## VS code

**Данный метод сработает только после выполнения одного из гайдов выше**

### 1. Устанавливаем расширение

1.1 переходим по ссылке и устанавливаем [REMOTE SSH EXTENSION](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-ssh)

1.2 после установки должнен был появится значок Remote Explorer, нажимаем на него

### 2. Настройка конфига подключения

2.1 Выбираем раздел REMOTE рядом с разделом REMOTE EXPLORER

2.2 Наводим на область рядом с SSH и нажимаем на +

2.3 Должен был появится инпут, вводим туда следующую команду

```bash
ssh -i C:/Users/dimweb/.ssh/lesson_key_1 root@94.198.216.62
```

2.4 Далее он спросит куда мы хотим сохранить данное значение, я по умолчанию выбираю С:/Users/dimweb/.ssh/config

2.5 Наводим курсор на область с буквами REMOTE, видим там значок Refresh (такая вращающаяся стрелка 🔄)

2.6 В списке должен был появится новый хост

2.7 теперь мы можем открыть редактор vs code без пароля в нужной папке на удаленном сервере, особенно полезно если вы работаете с моделями, что в основном возможно только на крупных серверах

2.8 Если при подключении у вас снова спрашивает пароль, очень вероятно что неправильно указан путь. Попробуйте перепроверить путь который указан для этого сервера в месте IdentityFile в конфиге по адресу С:/Users/dimweb/.ssh/config, он должен быть абсолютным.
