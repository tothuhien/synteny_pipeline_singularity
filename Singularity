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
apt-get install wget zlib1g-dev libbz2-dev liblzma-dev libpcre3-dev libcurl4-gnutls-dev gfortran g++ gcc make libcurl3-gnutls mcl bzip2 libxml2-dev libssl-dev libmariadbclient-dev libpq-dev -y

mkdir -p /usr/share/man/man1
apt-get install ssh openmpi-bin -y

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

#Install required R packages
R --slave -e 'install.packages("BiocManager",repos="https://cloud.r-project.org/")'
R --slave -e 'BiocManager::install(c("dplyr","tidyverse","magrittr","treeio","Biostrings","ape","yaml","seqinr"),dependencies=TRUE)'

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

echo 'export PATH=$PATH:/usr/local/miniconda/bin' >>$SINGULARITY_ENVIRONMENT

%files 
i-adhore /usr/local/bin
i-visualize /usr/local/bin

%test
java -version
R --version

[thut@elixir-dev thut]$ ll -h
total 89M
drwxr-xr-x. 2 thut 10065   45 Feb 27 13:02 Singularity
-rwxr-xr-x. 1 thut 10065 667K Mar  4 09:53 i-adhore
-rwxr-xr-x. 1 thut 10065 707K Mar  4 09:53 i-visualize
-rwxr-xr-x. 1 thut 10065  88M Mar  4 13:33 lolcow_latest.sif
-rw-r--r--. 1 thut 10065  732 Mar  4 09:27 recipe1
-rw-r--r--. 1 thut 10065 2.4K Mar  4 11:16 recipe_pipeline
[thut@elixir-dev thut]$ cat recipe_pipeline 
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
apt-get install wget zlib1g-dev libbz2-dev liblzma-dev libpcre3-dev libcurl4-gnutls-dev gfortran g++ gcc make libcurl3-gnutls mcl bzip2 libxml2-dev libssl-dev libmariadbclient-dev libpq-dev -y

mkdir -p /usr/share/man/man1
apt-get install ssh openmpi-bin -y

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

#Install required R packages
R --slave -e 'install.packages("BiocManager",repos="https://cloud.r-project.org/")'
R --slave -e 'BiocManager::install(c("dplyr","tidyverse","magrittr","treeio","Biostrings","ape","yaml","seqinr"),dependencies=TRUE)'

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

echo 'export PATH=$PATH:/usr/local/miniconda/bin' >>$SINGULARITY_ENVIRONMENT

%files 
i-adhore /usr/local/bin


