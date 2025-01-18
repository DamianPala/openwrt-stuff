# Compiling custom packages

We are going to compile package usteer from custom repository for Cudy WR3000 router.

## Enironment

Ubuntu 24.04 (probably previous versions works)

Install essential packages

```bash
sudo apt update && \
sudo apt install -y build-essential git libncurses5-dev gawk gettext unzip zlib1g-dev file python3 wget swig
```

Prepare repository

Clone OpenWrt repository

```bash
git clone https://git.openwrt.org/openwrt/openwrt.git
cd openwrt
```

Checkout to the specified version

```bash
git checkout v24.10.0-rc4
```

Edit feed configuration

```bash
vi feeds.conf.default
```

And add custom usteer repository

```
src-git usteer https://github.com/NilsRo/usteer.git
```

Remove current usteer package from feeds

```bash
rm -rf feeds/packages/net/usteer
```

Update and install feeds

```bash
./scripts/feeds update -a
./scripts/feeds install -af
```

Open menuconfig 

```bash
make menuconfig
```

Set desired target system and subtarget (MediaTek ARM and MT798x), set target profile to Multiple devices.

Go to the Network and press Y when on usteer package.

Select `Save` and `Exit` from menuconfig 

## Compilation

First compile and install toolchains

```bash
make -j$(nproc) toolchain/install
```

Next compile package

```bash
make -j$(nproc) package/usteer/compile
```

## Cleaning packages

Use

```bash
make clean
```

## Cleaning everything

```bash
make dirclean
```

## Troubleshooting

Use verbose flag when compiling to get more info about problems.

```bash
make -j$(nproc) toolchain/install V=s
```
