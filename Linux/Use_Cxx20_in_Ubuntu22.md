# Ubuntu 22 使用 C++20
## 1. 安装 build-essential
```bash
sudo apt install build-essential
```
安装完检查 /usr/bin/ 下是否有 gcc, g++, gcc-11, g++11.

## 2. 添加 ppa 源
```bash
sudo add-apt-repository ppa:ubuntu-toolchain-r/test
```

## 3. 安装 gcc-13 和 g++-13
```bash
sudo apt-get install gcc-13
sudo apt-get install g++-13
```

## 4. 设定优先级
```bash
sudo update-alternatives --install /usr/bin/gcc gcc /usr/bin/gcc-11 11
sudo update-alternatives --install /usr/bin/gcc gcc /usr/bin/gcc-13 13
```

```bash
sudo update-alternatives --install /usr/bin/gcc g++ /usr/bin/g++-11 11
sudo update-alternatives --install /usr/bin/gcc g++ /usr/bin/g++-13 13
```

## 5. 检查 gcc 和 g++ 的版本
```bash
gcc -v
g++ -v
```

