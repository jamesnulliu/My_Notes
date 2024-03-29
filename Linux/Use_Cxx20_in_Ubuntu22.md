# Use C++20 in Ubuntu 22

Install build essentials.

```bash
sudo apt install build-essential
```


Check which version of gcc and g++ is installed on your system:

```bash
gcc -v  # or g++ -v
```

**Suppose** that your currently installed gcc and g++ version is 11, you should be able to find gcc-11 and g++-11 under "/usr/bin/":

```bash
ls /usr/bin
# All files under "/usr/bin" would be listd.
```

Now, install gcc-13 and g++-13, while keeping older version existed:

```bash
# Install gcc-13 and g++-13
sudo add-apt-repository ppa:ubuntu-toolchain-r/test
sudo apt-get update
sudo apt-get install gcc-13 g++-13

# Register gcc-11 and g++-11 as one group of alternatives
sudo update-alternatives --install /usr/bin/gcc gcc /usr/bin/gcc-11 110 --slave /usr/bin/g++ g++ /usr/bin/g++-11

# Register gcc-13 and g++-13 as another group of alternatives
sudo update-alternatives --install /usr/bin/gcc gcc /usr/bin/gcc-13 130 --slave /usr/bin/g++ g++ /usr/bin/g++-13

# Pop a prompt to select the default version of gcc, and g++ would be updated automatically
sudo update-alternatives --config gcc
```

For general purpose of C++ programing, it is suggested that gcc-11 and g++-11 is installed on your system. Some softwares may have a strict rule for gcc version not larger than 12.
