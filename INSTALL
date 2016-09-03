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
