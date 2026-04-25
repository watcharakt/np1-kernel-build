name: NP1 Kernel Build

on:
  workflow_dispatch:

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
    - name: Checkout
      uses: actions/checkout@v4

    - name: Install deps
      run: |
        sudo apt update
        sudo apt install -y git bc bison flex libssl-dev make \
        libc6-dev libncurses5-dev clang lld llvm zip curl cpio

    - name: Clone Kernel
      run: |
        git clone --depth=1 https://github.com/NothingOSS/android_kernel_msm-5.4_nothing_sm7325 kernel

    - name: Setup KernelSU
      run: |
        cd kernel
        curl -LSs https://raw.githubusercontent.com/tiann/KernelSU/main/kernel/setup.sh | bash -

    - name: Build
      run: |
        cd kernel
        export ARCH=arm64
        export SUBARCH=arm64
        export CC=clang
        export CLANG_TRIPLE=aarch64-linux-gnu-

        make O=out vendor/lahaina-perf_defconfig
        make -j$(nproc) O=out

    - name: Pack AnyKernel3
      run: |
        git clone https://github.com/osm0sis/AnyKernel3 AnyKernel
        cp kernel/out/arch/arm64/boot/Image AnyKernel/

        cd AnyKernel
        zip -r kernel.zip *

    - name: Upload
      uses: actions/upload-artifact@v4
      with:
        name: kernel
        path: AnyKernel/kernel.zip
