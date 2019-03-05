Bootstrap: docker
FROM: debian:9.3-slim
# Debian Stretch without manpages and other files
# usually not needed in containers.

%help

Contains R version 3.5.0

%post
apt-get update
apt-get install software-properties-common -y
add-apt-repository "deb http://ppa.launchpad.net/webupd8team/java/ubuntu xenial main"
apt-get update
apt-get install wget zlib1g-dev libbz2-dev liblzma-dev libpcre3-dev libcurl4-gnutls-dev gfortran g++ gcc make libcurl3-gnutls mcl bzip2 libxml2-dev libssl-dev libmariadbclient-dev libpq-dev ssh -y

mkdir -p /usr/share/man/man1

#Install java
echo debconf shared/accepted-oracle-license-v1-1 select true | debconf-set-selections
echo debconf shared/accepted-oracle-license-v1-1 seen true | debconf-set-selections
apt-get install oracle-java8-installer -y --allow-unauthenticated

#Install R/3.5.0
wget https://cran.r-project.org/src/base/R-3/R-3.5.0.tar.gz
tar -xf R-3.5.0.tar.gz
cd R-3.5.0
./configure --with-readline=no --with-x=no
make
make install
cd ../

#Install required R packages
R --slave -e 'install.packages("BiocManager",repos="https://cloud.r-project.org/")'
R --slave -e 'BiocManager::install(c("dplyr","tidyverse","magrittr","treeio","Biostrings","ape","yaml","seqinr"),dependencies=TRUE)'

sed -i 's/^R_LIBS_USER/#R_LIBS_USER/g' /usr/local/lib/R/etc/Renviron
echo "R_LIBS_USER='/usr/local/lib/R/library'" >> /usr/local/lib/R/etc/Renviron

#Install softwares of pipeline

wget https://github.com/davidemms/OrthoFinder/releases/download/v2.3.1-beta/OrthoFinder-2.3.1.tar.gz 
tar -xzvf OrthoFinder-2.3.1.tar.gz
cp -r OrthoFinder-2.3.1/* /usr/local/bin

wget https://github.com/bbuchfink/diamond/releases/download/v0.9.22/diamond-linux64.tar.gz
tar xzf diamond-linux64.tar.gz
mv diamond /usr/local/bin
  
wget https://bioweb.supagro.inra.fr/macse/releases/macse_v2.03.jar
mv macse_v2.03.jar /usr/local/bin
 
wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh
chmod +x Miniconda3-latest-Linux-x86_64.sh
bash Miniconda3-latest-Linux-x86_64.sh -b -p /usr/local/miniconda
export PATH=$PATH:/usr/local/miniconda/bin

conda install -c bioconda -c conda-forge snakemake
conda install -c biocore -y mafft 
conda install -c bioconda -y fastme
conda install -c bioconda -y fasttree
mv /usr/local/miniconda/bin/fasttree /usr/local/miniconda/bin/FastTree #name required by orthofinder
conda install -c bioconda -y treebest
conda install -c conda-forge openmpi
ln -s /usr/local/miniconda/lib/libmpi_cxx.so.40.10.0 /usr/local/miniconda/lib/libmpi_cxx.so.1 
ln -s /usr/local/miniconda/lib/libmpi.so.40.10.3 /usr/local/miniconda/lib/libmpi.so.1
ln -s /usr/local/lib/libpng15.so.15.13.0  /usr/local/miniconda/lib/libpng15.so.15

echo 'export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/usr/local/miniconda/lib' >>$SINGULARITY_ENVIRONMENT
echo 'export PATH=$PATH:/usr/local/miniconda/bin' >>$SINGULARITY_ENVIRONMENT

%files 
i-adhore /usr/local/bin
libpng15.so.15.13.0 /usr/local/lib

