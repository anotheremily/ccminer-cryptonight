# ccminer-cryptonight
This is a CUDA accelerated mining application for use with Monero and other coins based on the Cryptonight algorithm. A modification of Christian Buchner's & Christian H.'s ccminer project by tsiv for Cryptonight mining.

THIS PROGRAM IS PROVIDED "AS-IS", USE IT AT YOUR OWN RISK!

## Installation
Requires CUDA SDK 6.0 which can be found here: https://developer.nvidia.com/linux-tegra-rel-19

Direct link: http://developer.download.nvidia.com/compute/cuda/6_0/rel/installers/cuda-repo-l4t-r19.2_6.0-42_armhf.deb

Make sure the system is up to date and the installer in `NVIDIA-INSTALLER` has been run.

Then download the 6.0 SDK and install using these instructions: http://elinux.org/Jetson/Installing_CUDA (included below.)

	# Install the CUDA repo metadata that you downloaded manually for L4T
	sudo dpkg -i cuda-repo-l4t-r19.2_6.0-42_armhf.deb
	# Download & install the actual CUDA Toolkit including the OpenGL toolkit from NVIDIA. (It only downloads around 15MB)
	sudo apt-get update
	# Install "cuda-toolkit-6-0" if you downloaded CUDA 6.0, or "cuda-toolkit-6-5" if you downloaded CUDA 6.5, etc.
	sudo apt-get install cuda-toolkit-6-0
	# Add yourself to the "video" group to allow access to the GPU
	sudo usermod -a -G video $USER

	# Add the 32-bit CUDA paths to your .bashrc login script, and start using it in your current console:
	echo "# Add CUDA bin & library paths:" >> ~/.bashrc
	echo "export PATH=/usr/local/cuda/bin:$PATH" >> ~/.bashrc
	echo "export LD_LIBRARY_PATH=/usr/local/cuda/lib:$LD_LIBRARY_PATH" >> ~/.bashrc
	source ~/.bashrc

The following instructions may be simplified later, but for now:

- Run `./autorun.sh`
- Run `./configure`
- Manually edit all `Makefile`, `Makefile.am`, and `Makefile.in` and remove the -msse2 flag from the gcc flags. It will appear once per file.
- Run `make`
- You should then be able to use the mining software.

So far it doesn't seem the Jetson performs well at all (worse than expected.) Looking into if this can be improved.

## Command Line Options
This code is based on the main ccminer branch, that in turn is based on the pooler cpuminer 2.3.2 release and inherits most of their command line interface and options.

    -d, --devices         Gives a comma separated list of CUDA device IDs
                          to operate on. Device IDs start counting from 0!
                          Alternatively give string names of your card like
                          gtx780ti or gt640#2 (matching 2nd gt640 in the PC).
    -l, --launch=CONFIG   Launch config for the Cryptonight kernel.
                          a comma separated list of values in form of
                          AxB where A is the number of threads to run in
                          each thread block and B is the number of thread
                          blocks to launch. If less values than devices in use
                          are provided, the last value will be used for
                          the remaining devices. If you don't need to vary the
                          value between devices, you can just enter a single
    	                    value and it will be used for all devices.
    	                    (default: 8x40)
    --bfactor=X           Enables running the Cryptonight kernel in smaller
                          pieces. The kernel will be run in 2^X parts according to bfactor, with a small pause between parts, specified by --bsleep.
                          This is a per-device setting like the launch config.
                          (default: 0 (no splitting) on Linux, 6 (64 parts) on Windows)
    --bsleep=X            Insert a delay of X microseconds between kernel
                          launches.
                          Use in combination with --bfactor to mitigate the lag
                          when running on your primary GPU.
                          This is a per-device setting like the launch config.
    -f, --diff            Divide difficulty by this factor (std is 1)
    -o, --url=URL         URL of mining server (default: " DEF_RPC_URL ")
    -O, --userpass=U:P    username:password pair for mining server
    -u, --user=USERNAME   username for mining server
    -p, --pass=PASSWORD   password for mining server
    --cert=FILE           certificate for mining server using SSL
    -x, --proxy=[PROTOCOL://]HOST[:PORT]  connect through a proxy
    -t, --threads=N       number of miner threads
                          (default: number of nVidia GPUs in your system)
    -r, --retries=N       number of times to retry if a network call fails
                          (default: retry indefinitely)
    -R, --retry-pause=N   time to pause between retries, in seconds
                          (default: 15)
    -T, --timeout=N       network timeout, in seconds (default: 270)
    -s, --scantime=N      upper bound on time spent scanning current work when
                          long polling is unavailable, in seconds (default: 5)
    --no-longpoll         disable X-Long-Polling support
    --no-stratum          disable X-Stratum support
    -q, --quiet           disable per-thread hashmeter output
    -D, --debug           enable debug output
    -P, --protocol-dump   verbose dump of protocol-level activities
    -B, --background      run the miner in the background
       --benchmark        run in offline benchmark mode
    -c, --config=FILE     load a JSON-format configuration file
    -V, --version         display version information and exit
    -h, --help            display this help text and exit

## Updates

### September 3rd 2016
Fork for Jetson TK1. See above for installation instructions.

### July 5th 2014
Massive improvement to interactivity on Windows, should also further help with TDR issues. Introducing the --bfactor and --bsleep command line parameters allows for control over execution of the biggest resource hog of the algorithm. Use bfactor to determine how many parts the kernel is split into and bsleep to insert a short delay between the kernel launches. The defaults are no splitting / no sleep for Linux and split into 64 (bfactor 6) parts / sleep 100 microseconds between launches for Windows. These defaults seem to work wonders on my 750 Ti on Windows 7, once again you may want to tweak according to your
environment.

### June 30th 2014
I've keep getting asked for donation addresses, here are some for wallets that I currently have up. Will set up other wallets on request, in case you feel like donating but don't hold any of the currencies I currently have addresses for.

* BTC: 1JHDKp59t1RhHFXsTw2UQpR3F9BBz3R3cs
* DRK: XrHp267JNTVdw5P3dsBpqYfgTpWnzoESPQ
* JPC: Jb9hFeBgakCXvM5u27rTZoYR9j13JGmuc2
* VTC: VwYsZFPb6KMeWuP4voiS9H1kqxcU9kGbsw
* XMR: 42uasNqYPnSaG3TwRtTeVbQ4aRY3n9jY6VXX3mfgerWt4ohDQLVaBPv3cYGKDXasTUVuLvhxetcuS16ynt85czQ48mbSrWX

In other news, I just yanked out the code for other alrogithms. This is now a cryptonight-only miner.

### June 24th 2014
Initial release, compiles and runs on Linux and Windows. Documentation is scarce and probably will be, see README.txt and ccminer --help for the basics.

Before you read further (and you should), I highly recommend running Linux. There are some issues with running on Windows that are

Do note that the cryptonight kernel on this release is FAT and SLOW, and pretty much makes your Windows computer unusable while running. If you plan on running it on your primary desktop with only a single GPU... Well, just don't think you'll be using the computer for anything. I haven't tested it, but I'd expect it'll be fine if you have multiple GPUs on the system and you run the miner only on the cards that don't have a display attached. You'll still have the TDR issue to deal with though:

The kernel also tends to turn just long enough for Windows to think the GPU crashed and trigger the driver timeout detection and recovery. This is where the kernel launch option (-l) hopefully comes in.

The default launch is 40 tread blocks of 8 threads each. Don't know why, but at least my 750 Ti seems to like 8 thread blocks best. 40 blocks of 8 is something that, once again on my 750 ti, manages to run fast enough to finish before the Windows default 2 second timeout. Basically enables you to run the damn thing without the driver crashing instantly, which is why I made it the default. Since I only have that one single 750 Ti to test on Windows, I haven't got the slightest clue how it works on other GPUs. Your mileage may vary.

I peaked out my hashrate with 60 blocks of 8 threads, you'll just have to experiment with it until (if) you find the sweet spot for your cards. Do keep in mind that cryptonight needs 2 MB of memory for each hash, that would mean about 960 MB of GPU memory for the 8x60 config. Keep that and the amount of memory on your card in mind while playing around with the numbers.


## Authors
Notable contributors to this application are:

- tsiv:
  - CUDA implementation for the Cryptonight algorithm.
- Christian Buchner, Christian H. (Germany):
  - modifying the original pooler-cpuminer for use with CUDA.
- Jeff Garzik, pooler + contributors:
  - The original pooler-cpuminer project
- LucasJones:
  - JSON-RPC 2.0 handling and the Cryptonight C-code comes from his cpuminer fork, cpuminer-multi

## Donations for original authors

### tsiv
If you find this tool useful and like to support its continued development, then consider a donation.

   BTC donation address: 1JHDKp59t1RhHFXsTw2UQpR3F9BBz3R3cs
   DRK donation address: XrHp267JNTVdw5P3dsBpqYfgTpWnzoESPQ
   JPC donation address: Jb9hFeBgakCXvM5u27rTZoYR9j13JGmuc2
   VTC donation address: VwYsZFPb6KMeWuP4voiS9H1kqxcU9kGbsw
   XMR donation address:
     (man these are long... single address, split on two lines)
     42uasNqYPnSaG3TwRtTeVbQ4aRY3n9jY6VXX3mfgerWt4ohD
     QLVaBPv3cYGKDXasTUVuLvhxetcuS16ynt85czQ48mbSrWX

### Christian Buchner and Christian H.
Don't forget to support the original ccminer authors Christian Buchner and Christian H. This mod would not be here without their work on ccminer:

   LTC donation address: LKS1WDKGED647msBQfLBHV3Ls8sveGncnm
   BTC donation address: 16hJF5mceSojnTD3ZTUDqdRhDyPJzoRakM
   YAC donation address: Y87sptDEcpLkLeAuex6qZioDbvy1qXZEj4
   VTC donation address: VrjeFzMgvteCGarLw85KivBzmsiH9fqp4a
   MAX donation address: mHrhQP9EFArechWxTFJ97s9D3jvcCvEEnt
  DOGE donation address: DT9ghsGmez6ojVdEZgvaZbT2Z3TruXG6yP
   HVC donation address: HNN3PyyTMkDo4RkEjkWSGMwqia1yD8mwJN
   GRS donation address: FmJKJAhvyHWPeEVeLQHefr2naqgWc9ABTM
   MYR donation address: MNHM7Q7HVfGpKDJgVJrY8ofwvmeugNewyf
   JPC donation address: JYFBypVDkk583yKWY4M46TG5vXG8hfgD2U
   SFR donation address: SR4b87aEnPfTs77bo9NnnaV21fiF6jQpAp
   MNC donation address: MShgNUSYwybEbXLvJUtdNg1a7rUeiNgooK
   BTQ donation address: 13GFwLiZL2DaA9XeE733PNrQX5QYLFsonS
