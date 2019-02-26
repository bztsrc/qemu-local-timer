QEMU ARM Control Local Timer Periodic IRQ support
=================================================

This patch adds the ARM Control Local Timer (see [BCM2836 QA7](https://www.raspberrypi.org/documentation/hardware/raspberrypi/bcm2836/QA7_rev3.4.pdf)
section 4.11 "Local timer" page 17) periodic IRQ feature to qemu. This patch is provided as-is, without any kind of warranty in the hope that it will be useful.

The features are very limited; because 0x4000001C Core timer access is not implemented yet, there's not much point in enabling the timer
without the interrupt enable bit. You can route the interrupt to one of the cores' IRQ or FIQ line by writing 0x40000024, and you can
set up a periodic interrupt with 38.4MHz frequency by writing the divider into 0x40000034. Prescaler are not taken into account.
When the IRQ fired, you'll see it in 0x40000040+4*N bit 11, and you can acknowledge it by writing 1<<31 to 0x40000038. Pretty much
that's all that this patch does.

Usage
-----

1. download latest [qemu source](https://github.com/qemu/qemu)
2. overwrite files from this repo (or if you prefer patches, the [diff is here](https://github.com/bztsrc/qemu-local-timer/tree/patches))
3. compile qemu

```sh
./configure --target-list=aarch64-softmmu --enable-modules --enable-tcg-interpreter --enable-debug-tcg
make -j4
```

4. install qemu

```sh
sudo make install
```

Enjoy!

bzt
