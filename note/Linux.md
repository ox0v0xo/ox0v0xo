# Linux

- [Linux](#linux)
  - [Tools](#tools)
    - [Find](#find)
    - [Git](#git)
      - [.gitconfig](#gitconfig)
    - [Pacman](#pacman)
    - [Socket Statistics](#socket-statistics)

## Tools

### Find

### Git

```sh
$ git config --global user.name "ox0v0xo"
$ git config --global user.email "ox0v0xo@outlook.com"

# git 走代理 192.168.8.1:7897
$ git config --global http.proxy "http://192.168.8.1:7897"
$ git config --global https.proxy "http://192.168.8.1:7897"
```

#### .gitconfig

```.gitconfig
[user]
	name = ox0v0xo
	email = ox0v0xo@outlook.com
[http]
	proxy = http://192.168.8.1:7897
[https]
	proxy = http://192.168.8.1:7897
```

### Pacman

> `pacman`

### Socket Statistics

> `ss`

- l : Listening
- p : Processes
- t : TCP
- u : UDP
