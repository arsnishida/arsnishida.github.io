---
layout: post
title:  "22-12-05, Local Installs - Initial"
date:   2022-12-05
parent: ravnica
nav_order: 2
---

Install cmake
```sh
tar xfz cmake-3.24.3.tar.gz
cd cmake-3.24.3/
./bootstrap --prefix=/home/groups/ravnica/src/cmake/cmake-3.24.3
make
make install
```

Install boost
```sh
tar xfz boost_1_80_0.tar.gz
cd boost_1_80_0
./bootstrap.sh --prefix=/home/groups/ravnica/src/boost/boost_1_80_0
./b2 install
```

Install bedtools
```sh
wget https://github.com/arq5x/bedtools2/releases/download/v2.29.1/bedtools-2.29.1.tar.gz
tar -zxvf bedtools-2.29.1.tar.gz
mv bedtools2/ bedtools2.29.1
cd bedtools2.29.1/
make
```

Install bedops
```sh
bzip2 -dk bedops_linux_x86_64-v2.4.41.tar.bz2 
tar xf bedops_linux_x86_64-v2.4.41.tar
rm bedops_linux_x86_64-v2.4.41.tar
mkdir 2.4.41
mv bin 2.4.41
```

Install samtools
```sh
bzip2 -dk samtools-1.16.1.tar.bz2
tar xf samtools-1.16.1.tar
rm samtools-1.16.1.tar
cd samtools-1.16.1
./configure --prefix=/home/groups/ravnica/src/samtools/samtools-1.16.1
make
make install
```

Install htslib
```sh
bzip2 -dk htslib-1.16.tar.bz2 
tar xf htslib-1.16.tar
rm htslib-1.16.tar
cd htslib-1.16
./configure --prefix=/home/groups/ravnica/src/htslib/htslib-1.16/
make
make install
```

Install bcftools
```sh
bzip2 -dk bcftools-1.16.tar.bz2
tar xf bcftools-1.16.tar
rm bcftools-1.16.tar
cd bcftools-1.16
./configure --prefix=/home/groups/ravnica/src/bcftools/bcftools-1.16
make
make install
```

Install install sratoolkit
```sh
tar xfz sratoolkit.3.0.0-ubuntu64.tar.gz
```

Install subread
```sh
tar xfz subread-2.0.3-source.tar.gz
cd subread-2.0.3-source/src
make -f Makefile.Linux
```

Install STAR
```sh
wget https://github.com/alexdobin/STAR/archive/2.7.10b.tar.gz
tar -xzf 2.7.10b.tar.gz
cd STAR-2.7.10b/source
make STAR
```

Install bwa, pulled nov14 from master branch 139f68f on Sep 22. There's a bug with gcc versions that the tagged release cannot handle.
```sh
git clone https://github.com/lh3/bwa.git
mv bwa bwa-139f68f-0.7.17
cd bwa-139f68f-0.7.17
make
```

Install BLAST
```sh
tar xvf ncbi-blast-2.13.0+-src.tar.gz
cd ncbi-blast-2.13.0+-src/c++
./configure --prefix=/home/groups/ravnica/src/blast/ncbi-blast-2.13.0+-src
make
make install
```

Install kraken2
```sh
tar xvf kraken2-2.1.2.tar.gz
module load blast
./install_kraken2.sh /home/groups/ravnica/src/kraken/kraken2-2.1.2
```

Install GMP
```sh
tar xf gmp-6.1.0.tar
make
make install
```

Install MPFR
```sh
./configure --prefix=/home/groups/ravnica/src/mpfr/mpfr-3.1.4 --with-gmp-build=/home/groups/ravnica/src/gmp/gmp-6.1.0
make
make install
```

Install MPC
```sh
tar xf mpc-1.0.3.tar
./configure --prefix=/home/groups/ravnica/src/mpc/mpc-1.0.3 --with-gmp=/home/groups/ravnica/src/gmp/gmp-6.1.0 --with-mpfr=/home/groups/ravnica/src/mpfr/mpfr-3.1.4
make
make install
```

Install this older version of gcc for bcl2fastq.
```sh
tar xfz gcc-8.1.0.tar.gz
./configure --prefix=/home/groups/ravnica/src/gcc/gcc-8.1.0 --with-gmp=/home/groups/ravnica/src/gmp/gmp-6.1.0 --with-mpfr=/home/groups/ravnica/src/mpfr/mpfr-3.1.4 --with-mpc=/home/groups/ravnica/src/mpc/mpc-1.0.3 --disable-multilib --disable-libsanitizer
make
make install
```

Separetely install older cmake also for bcl2fastq. There’s a minor issue with the order in which the module should be loaded. I think it has to be done after bootstrapping, not before like on my notes.
```sh
tar xfz cmake-2.8.9.tar.gz
cd cmake-2.8.9/
module load gcc/8.1.0
export LD_LIBRARY_PATH=/usr/lib/x86_64-linux-gnu:/home/groups/ravnica/src/gmp/gmp-6.1.0/lib:/home/groups/ravnica/src/mpfr/mpfr-3.1.4/lib:/home/groups/ravnica/src/mpc/mpc-1.0.3/lib:/home/groups/ravnica/src/gcc/gcc-8.1.0/lib64
./bootstrap
make
make install
```

Install bcl2fastq. bcl2fastq will try to install its specific versions of cmake and boost. cmake-2.8.9 needs an older version of gcc so install separately. But let it install its own version of boost. Add an explicit path to C libraries since cmake-2.8.9 is needed but all its C libraries won't work.
```sh
tar xfz bcl2fastq2-v2.20.0.422-Source.tar.gz
mv bcl2fastq bcl2fastq-v2.20.0.422
module load cmake/2.8.9
export C_INCLUDE_PATH=/usr/include/x86_64-linux-gnu
./src/configure --prefix=/home/groups/ravnica/src/bcl2fastq2/bcl2fastq2-v2.20.0.422
make
make install
```

Install older autoconf
```sh
curl -O http://ftp.gnu.org/gnu/autoconf/autoconf-2.69.tar.gz
tar zxvf autoconf-2.69.tar.gz
cd autoconf-2.69
./configure && make && sudo make install
```

Install pcre2
```sh
tar xfz pcre2-10.40.tar.gz 
./configure --prefix=""
make
make install
```

Install Java
```sh
tar xfz openjdk-11.0.2_linux-x64_bin.tar.gz 
# that’s it
```

Install R, 4.2.2
```sh
tar xfz R-4.2.2.tar.gz
cd R-4.2.2/
module load pcre2/10.40
./configure --prefix="/home/groups/ravnica/src/R/R-4.2.2" --with-cairo --with-libpng --with-libtiff --with-jpeglib
make
make install
```

Install R, 4.2.1
```sh
tar xfz R-4.2.1.tar.gz
cd R-4.2.1/
module load pcre2/10.40
./configure --prefix="/home/groups/ravnica/src/R/R-4.2.1" --with-cairo --with-libpng --with-libtiff --with-jpeglib
make
make install
```

Install JAGS
```sh
tar xfz JAGS-4.3.1.tar.gz
cd JAGS-4.3.1
./configure --prefix="/home/groups/ravnica/src/jags/JAGS-4.3.1"
make
make install
```

Install UCSC Genome Browser Kent tools
```sh
$ rsync -azvP rsync://hgdownload.soe.ucsc.edu/genome/admin/exe/linux.x86_64/ ./
$ date
Mon Dec  5 09:19:31 PM PST 2022
```

Install older python
```sh
tar xfz Python-3.9.16.tgz
cd Python-3.9.16/
./configure --prefix=/home/groups/ravnica/src/python/Python-3.9.16
make
make install
```

Install FastQC
```
unzip fastqc_v0.11.9.zip
mv FastQC FastQC-0.11.9
```
