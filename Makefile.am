AUTOMAKE_OPTIONS = subdir-objects

if WANT_JANSSON
JANSSON_INCLUDES= -I$(top_srcdir)/compat/jansson
else
JANSSON_INCLUDES=
endif

EXTRA_DIST		= autogen.sh README.txt LICENSE.txt \
			  cudaminer.sln cudaminer.vcxproj cudaminer.vcxproj.filters \
			  compat/gettimeofday.c compat/getopt/getopt_long.c cpuminer-config.h.in

SUBDIRS		= compat

bin_PROGRAMS	= ccminer

ccminer_SOURCES		= elist.h miner.h compat.h \
			  compat/inttypes.h compat/stdbool.h compat/unistd.h \
			  compat/sys/time.h compat/getopt/getopt.h \
			  cpu-miner.c util.c sha2.c \
			  cryptonight/cryptonight.cu cryptonight/cuda_cryptonight_core.cu cryptonight/cuda_cryptonight_extra.cu \
			  cryptonight.c \
			  crypto/oaes_lib.c \
			  crypto/c_keccak.c \
			  crypto/c_groestl.c \
			  crypto/c_blake256.c \
			  crypto/c_jh.c \
			  crypto/c_skein.c \
			  crypto/aesb.c


ccminer_LDFLAGS		= $(PTHREAD_FLAGS) @CUDA_LDFLAGS@
ccminer_LDADD		= @LIBCURL@ @JANSSON_LIBS@ @PTHREAD_LIBS@ @WS2_LIBS@ @CUDA_LIBS@ @OPENMP_CFLAGS@ @LIBS@
ccminer_CPPFLAGS	= @LIBCURL_CPPFLAGS@ @OPENMP_CFLAGS@ $(PTHREAD_FLAGS) -fno-strict-aliasing $(JANSSON_INCLUDES) -DSCRYPT_KECCAK512 -DSCRYPT_CHACHA -DSCRYPT_CHOOSE_COMPILETIME

NVCC_GENCODE		= --generate-code=arch=compute_32,code=\"sm_32,compute_32\"

# we're now targeting all major compute architectures within one binary.
.cu.o:
	$(NVCC) @CFLAGS@ -I . -Xptxas "-abi=no -v" $(NVCC_GENCODE) --gpu-architecture=sm_32 --maxrregcount=80 --ptxas-options=-v $(JANSSON_INCLUDES) -o $@ -c $<
