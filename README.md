# Manganese

A memory tester for the modern age. Manganese utilizes modern CPU features to
run significantly faster than traditional memory tests, letting you run more
passes in less time.

## Features

- Supports as many CPU cores as OpenMP does, and automatically uses all of them.
- Supports pure AVX2 and AVX-512 codepaths with runtime feature detection
- Uses non-temporal stores to bypass the CPU cache without additional performance penalties
- Prints per-error warnings, total error counts, and average bandwidth used after each loop

## Performance

All benchmarks conducted on an i5 12600k paired with dual-channel DDR5 at 5400MT/s.

| ISA     | Threading | Avg. Bandwidth |
| ------- | --------- | -------------- |
| AVX2    | 1c/1T     | 5640MB/s       |
| AVX2    | 1C/1T     | 8600MB/s       |
| AVX-512 | 1C/1T     | 9400MB/s       |
| AVX2    | 6C+4c/16T | 53000MB/s      |
| AVX-512 | 6C/12T    | 62000MB/s      |

## Requirements
- Linux 5.x or newer
- C11 Compiler w/ GNU extensions
- OpenMP
- A CPU with AVX2 (slower) or AVX-512F and AVX-512BW (faster)
  
## Installation & Usage

Install the prerequisites, initialize the submodules, and run `make`.
You can then run `./manganese 10%` to test 10% of your total RAM.

e.g. on Ubuntu 20.04:
```shell
# install compiler and OpenMP header
sudo apt install make gcc-10 libomp-dev
# initialize the submodules (OpenBLAS and SIMDxorshift)
git submodule update --init --recursive

# compiling the OpenBLAS library will take a while for the first compilation
CC="gcc-10" make

# start a memory test
./manganese 10%
# as memory must be locked, running might require ulimit adjustments or running as root
sudo ./manganese 10%

# different core counts can be tested with taskset, e.g. for cores 0-7 (first four real cores including hyperthreads)
sudo taskset -c 0-7 ./manganese 10%
```

## Example

Tested at 80mV below the threshold of stability

![Console output for detected instability](run-example.png)

## Disclaimer

Do not mount important filesystems with a potentially unstable computer. Only
use this program and other stability tests from a Live CD or a separate
operating system on which all connected devices and mounted filesystems are
disposable.

Tests in this program may not detect certain memory faults. This program cannot
test memory reserved by the Linux kernel and other running programs. If this
program gives you the all-clear and then your million-dollar Bitcoin wallet
becomes corrupt, that's on you. If this program convinces you to throw away a
perfectly good memory module, that's also on you. See the LICENSE file, as well
as the LICENSE file within the SIMDxorshift directory for more information.

Thanks to Daniel Lemire for his AVX2 and AVX-512 random number generator
