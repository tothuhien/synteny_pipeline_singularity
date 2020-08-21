Bootstrap: docker
FROM: debian:9.3-slim
# Debian Stretch without manpages and other files
# usually not needed in containers.

%help

This image contains required softwares to run the pipeline https://gitlab.com/sandve-lab/salmonid_synteny
First pull the image from shub: singularity pull shub://tothuhien/synteny_pipeline_singularity
Then cd to the folder of the pipeline and use the dowload image to run the snakefile of the pipeline: singularity exec synteny_pipeline_singularity.simg snakemake -s Snakefile.py --configfile config.yaml --cores 10

%post
apt-get update
apt-get install software-properties-common -y
add-apt-repository "deb http://ppa.launchpad.net/webupd8team/java/ubuntu xenial main"
apt-get install wget zlib1g-dev libbz2-dev liblzma-dev libpcre3-dev libcurl4-gnutls-dev gfortran g++ gcc make libcurl3-gnutls mcl bzip2 libxml2-dev libssl-dev libmariadbclient-dev libpq-dev ssh apt-utils gnupg locales slurm-client -y

echo "LC_ALL=en_US.UTF-8" >> /etc/environment
echo "en_US.UTF-8 UTF-8" >> /etc/locale.gen
echo "LANG=en_US.UTF-8" > /etc/locale.conf
locale-gen en_US.UTF-8

mkdir -p /usr/share/man/man1

#Install java
apt install default-jre -y

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

wget https://github.com/davidemms/OrthoFinder/releases/download/2.3.14/OrthoFinder.tar.gz
tar -xzvf OrthoFinder.tar.gz
cp -r OrthoFinder /usr/local/bin

  
wget https://bioweb.supagro.inra.fr/macse/releases/macse_v2.03.jar
mv macse_v2.03.jar /usr/local/bin
 
wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh
chmod +x Miniconda3-latest-Linux-x86_64.sh
bash Miniconda3-latest-Linux-x86_64.sh -b -p /usr/local/miniconda
export PATH=$PATH:/usr/local/miniconda/bin

conda install -c bioconda -c conda-forge snakemake
conda install -c biocore -y mafft 
conda install -c bioconda -y fasttree
mv /usr/local/miniconda/bin/fasttree /usr/local/miniconda/bin/FastTree #name required by orthofinder
conda install -c bioconda -y treebest
conda install -c conda-forge openmpi
ln -s /usr/local/miniconda/lib/libmpi_cxx.so.40.10.0 /usr/local/miniconda/lib/libmpi_cxx.so.1 
ln -s /usr/local/miniconda/lib/libmpi.so.40.10.3 /usr/local/miniconda/lib/libmpi.so.1
ln -s /usr/local/lib/libpng15.so.15.13.0  /usr/local/miniconda/lib/libpng15.so.15

echo 'export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/usr/local/miniconda/lib' >>$SINGULARITY_ENVIRONMENT
echo 'export PATH=$PATH:/usr/local/miniconda/bin:/usr/local/bin/OrthoFinder:/usr/local/bin/OrthoFinder/bin' >>$SINGULARITY_ENVIRONMENT


%files 
i-adhore /usr/local/bin
libpng15.so.15.13.0 /usr/local/lib

%runscript
exec "$@"`
