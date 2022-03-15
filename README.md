# dcf-on-ubuntu

setup a docker-compose envirenment on ubuntu with vagrant

## prepare

- virtualbox
- vagrant

## setup

```bash
# speed up git clone in china
GC_PROXY="https://ghproxy.com/"
GC_URL="https://github.com/YMC-GitHub/vagrant-dcf-on-ubuntu.git"
GC_URL="${GC_PROXY}${GC_URL}"
git clone -b main "$GC_URL"
```

## start

```bash
vagrant up
```

## usage
```bash
vagrant ssh
```

## Author

yemiancheng <ymc.github@gmail.com>

## License

MIT
