can you explain step by step how to install pytorch on a PowerPC 64-bit little-endian architecture Red Hat Enterprise Linux 8.9 with a ppc64le system

We know this reference

Using Source Code to Install PyTorch
Installing PyTorch 1.8.1/1.11.0 by Compiling Source Code
The following procedure uses PyTorch 1.8.1 as an example.

Install the official torch package.
In the architecture, you can compile and install the official torch package.
Download the PyTorch v1.8.1 source package. For PyTorch 1.11.0, replace the version number with v1.11.0.
git clone -b v1.8.1 https://github.com/pytorch/pytorch.git --depth=1 pytorch_v1.8.1
Access the source package to obtain the passive dependency code.
cd pytorch_v1.8.1
git submodule sync
git submodule update --init --recursive
Configure environment variables.
export USE_XNNPACK=0
Perform compilation-based installation.
python3 setup.py install
Compile and generate the binary installation package of the PyTorch plugin.
# Download the branch code of the corresponding PyTorch version and go to the root directory of the plugin. Take v1.8.1-3.0.0 as an example. For the 1.11.0 version, replace v1.8.1-3.0.0 with v1.11.0-3.0.0..
git clone -b v1.8.1-3.0.0 https://gitee.com/ascend/pytorch.git 
cd pytorch    
# Specify the Python version packaging mode. Python 3.10 is used as an example. 
bash ci/build.sh --python=3.10
https://www.ibm.com/docs/en/wmlce/1.7.0?topic=installing-mldl-frameworks

https://leoleoasd.me/2023/11/26/compiling-pytorch-from-source-on-powerpc/

https://github.com/pedrodiamel/configurate-deeplearning-ibm-ppc64le

https://public.dhe.ibm.com/ibmdl/export/pub/software/server/ibm-ai/conda/#/

https://www.ibm.com/docs/en/wmlce/1.7.0?topic=installing-mldl-frameworks

https://gauravm.gitbook.io/about/blogs/installing-pytorch-and-transformers-on-ibm-powerpc-architecture
https://developer.ibm.com/articles/awb-ai-model-training-with-pytorch/?mhsrc=ibmsearch_a&mhq=power8%20pytorch

https://developer.ibm.com/tutorials/install-docker-on-linux-on-power/?mhsrc=ibmsearch_a&mhq=install%20pytorch%20power%208

https://developer.ibm.com/articles/pytorchcaffe2-with-onnx-on-aix/?mhsrc=ibmsearch_a&mhq=Pytorch%20installation

https://github.com/pytorch/pytorch/issues/4224

https://discuss.pytorch.org/t/installing-pytorch-on-ibm-power-9-architecture/135915

https://github.com/shanemcandrewai/pytorch-setup

https://support.huawei.com/enterprise/en/doc/EDOC1100289998/4bb2a561/using-source-code-to-install-pytorch

https://developer.nvidia.com/cuda-12-1-0-download-archive?target_os=Linux&target_arch=ppc64le&Distribution=RHEL&target_version=8&target_type=runfile_local

https://developer.nvidia.com/cuda-12-1-0-download-archive?target_os=Linux&target_arch=ppc64le&Distribution=RHEL&target_version=8&target_type=runfile_local

Run ./configure --enable-optimizations


https://cahyati2d.medium.com/ibm-visual-insights-1-2-installation-with-ubuntu-os-on-power-ac922-summery-6bd7ab05bbdc

https://github.ibm.com/Watson-IoT/iot-iva-docs-v300/blob/73835647e772920f791be23bad6b2e9f01050495/iva/InstallingSolution/install_dle_nvidia_driver.md\

https://leoleoasd.me/2023/11/26/compiling-pytorch-from-source-on-powerpc/

https://repo.anaconda.com/archive/

https://www.ibm.com/docs/en/wmlce/1.7.0?topic=setup-installing-anaconda

Installing the NVIDIA driver
After you install the Graphics Processing Unit (GPU), the next step is to obtain and install the NVIDIA driver that is compatible with CUDA (Combined Unified Device Architecture) 10.0.

About this task
Follow the procedure to install the NVIDIA driver.

Procedure
Go to http://www.nvidia.com/Download/index.aspx

Select the options that match your GPU, operating system and computer architecture. While the CUDA toolkit is not required to be installed, there may be a prompt referring to the toolkit during the NVIDIA driver installation. If prompted, select 10.0 for CUDA toolkit.

Install the driver.

Post-driver steps for POWER9 only. Skip to step 5 if you are not installing the DLE on a POWER9 system.

The following instructions can also be found in the NVIDIA CUDA Toolkit documentation:

a. The NVIDIA Persistence Daemon should be automatically started for POWER9 installations. Check that it is running with the following command:

  # systemctl status nvidia-persistenced
If it is not active, run the following command:

  # sudo systemctl enable nvidia-persistenced
b. Disable a udev rule installed by default in some Linux distributions that cause hot-pluggable memory to be automatically onlined when it is physically probed. {{site.data.keyword.product_name}} behavior prevents NVIDIA software from bringing NVIDIA device memory online with non-default settings. {{site.data.keyword.product_name}} udev rule must be disabled in order for the NVIDIA CUDA driver to function properly on POWER9 systems.

{{site.data.keyword.product_name}} rule must be disabled by copying the file to /etc/udev/rules.d and commenting out, removing, or changing the hot-pluggable memory rule in the /etc copy so that it does not apply to POWER9 NVIDIA systems.

https://github.ibm.com/Watson-IoT/iot-iva-docs-v300/blob/73835647e772920f791be23bad6b2e9f01050495/iva/InstallingSolution/install_dle_nvidia_driver.md

https://github.ibm.com/mandie-quartly/opf_gov/blob/ebc21007a2e4d61d6956106a2ad68f1babed20a9/openpower.md
https://github.ibm.com/Tetsuya-Kawano/Code-Tutorials-Retired/blob/77793ef6fa113fc3929dbfa739ade23092266fed/accelerate-random-forest-model-training-with-watson-ml-accelerator/index.md

https://docs.nvidia.com/cuda/cuda-installation-guide-linux/#prepare-rhel-8-rocky-8

## How to Install Pytorch and Transformers on IBM Power 9 architecture 

Hello everyone, today we are going to setup the server.
##   Installation Drivers
Here is a step-by-step guide to installing CUDA on a Power9 system with Red Hat 8 (RHEL 8):

**Step 1: Enable the EPEL repository**

To enable the EPEL repository, you need to create a new file called `epel.repo` in the `/etc/yum.repos.d/` directory.

Run the following command to open the file in the `vi` editor with superuser privileges:
```
sudo vi /etc/yum.repos.d/epel.repo
```
This will create a new file called `epel.repo` in the `/etc/yum.repos.d/` directory. Add the following contents to the file:
```markdown
[epel]
name=epel
baseurl=http://download.fedoraproject.org/pub/epel/8/Everything/ppc64le/
enabled=1
gpgcheck=0
```
Save and exit the file by pressing `:wq` and then `Enter`.

Then run the following command to update the repository cache:
```
sudo yum update
```
**Step 2: Enable the CUDA repository**

To enable the CUDA repository, you need to create a new file called `cuda-rhel8.repo` in the `/etc/yum.repos.d/` directory.

```
sudo vi /etc/yum.repos.d/cuda-rhel8.repo
```
This will create a new file called `cuda-rhel8.repo` in the `/etc/yum.repos.d/` directory. Add the following contents to the file:
```markdown
[cuda-rhel8]
name=cuda-rhel8
baseurl=https://developer.download.nvidia.com/compute/cuda/repos/rhel8/ppc64le/
enabled=1
gpgcheck=0
```
Save and exit the file by pressing `:wq` and then `Enter`.
Then run the following command to update the repository cache:
```
sudo yum update
```

You need to download the CUDA installer from the NVIDIA website. Go to the [NVIDIA CUDA download page](https://developer.nvidia.com/cuda-downloads) and select the correct version for your system (Red Hat Enterprise Linux 8.9 with a ppc64le system).

I'm not an expert in this field, but I can try to help you with the information you provided. Here's a possible solution to install the NVIDIA driver without rebooting the server on a Power9 with Red Hat 8:

1. Download the custom driver version:
```
wget https://us.download.nvidia.com/tesla/550.54.15/nvidia-driver-local-repo-rhel8-550.54.15-1.0-1.ppc64le.rpm
```

yum localinstall ./nvidia-driver-local-repo-rhel8-550.54.15-1.0-1.ppc64le.rpm

2. Install the downloaded RPM package:
```
sudo rpm -ivh nvidia-driver-local-repo-rhel8-550.54.15-1.0-1.ppc64le.rpm
```

3. Update the repository cache:
```
sudo dnf clean all
```

4. Install the NVIDIA driver:
```
sudo dnf install -y nvidia-driver
```

5. Verify that the driver has been installed correctly:
```
nvidia-smi
```

Since you are not rebooting the server, the new driver may not be loaded immediately. In that case, you can try unloading the existing NVIDIA driver module (if any) and loading the new one:

```
sudo rmmod nvidia
sudo modprobe nvidia
```

Please note that this may not work in all cases, and a reboot might still be required for the new driver to take effect.


```
sudo dnf config-manager --add-repo https://developer.download.nvidia.com/compute/cuda/repos/rhel8/ppc64le/cuda-rhel8.repo
```

```
sudo dnf clean all
```


```
sudo dnf -y install cuda-toolkit-12-1
```
![](assets/2024-05-01-09-52-47.png)



```
sudo dnf clean all
#sudo dnf -y install cudnn
To install for CUDA 12, perform the above configuration but install the CUDA 12 specific package:

```
sudo dnf -y install cudnn-cuda-12
```


16.1. Mandatory Actions
Some actions must be taken after the installation before the CUDA Toolkit and Driver can be used.
16.1.1. Environment Setup
The PATH variable needs to include export 

```
export PATH=/usr/local/cuda-12.1/bin${PATH:+:${PATH}}
```


 Nsight Compute has moved to ∕opt∕nvidia∕nsight-compute∕ only
in rpm/deb installation method. When using .run installer it is still located under ∕usr∕local∕
cuda-12.4∕.
To add this path to the PATH variable:
export PATH=∕usr∕local∕cuda-12.4∕bin${PATH:+:${PATH}}

##Anaconda


wget https://repo.continuum.io/archive/Anaconda3-2023.09-0-Linux-ppc64le.sh











```
chmod +x Anaconda3-2023.09-0-Linux-ppc64le.sh

./Anaconda3-2023.09-0-Linux-ppc64le.sh

**Configure Anaconda**

1. Once the installation is complete, add the Anaconda bin directory to your system's PATH environment variable:
```bash
echo ". /home/cecuser/anaconda3/etc/profile.d/conda.sh" >> ~/.bashrc
```
2. Reload your `~/.bashrc` file by running:
```bash
source ~/.bashrc
```
**Verify Anaconda installation**

1. Open a new terminal session or run `source ~/.bashrc` to apply the changes to your PATH.
2. Verify that Anaconda is installed by running:
```bash
conda --version
```
This should display the version of Anaconda installed on your system.


Step 2. Environment creation
The environments supported that I will consider is Python 3.10,

I will create an environment called pytorch, but you can put the name that you like.

conda create -n pytorch python==3.10
then we activate

conda activate pytorch
then in your terminal type the following commands:

conda install ipykernel
then
 ``` 
python -m ipykernel install --user --name pytorch --display-name "Python (pytorch)"
```
then we install the following libraries used to the data analysis
```
conda install ipython
conda install pytorch
```
pip install wget seaborn matplotlib requests tqdm


PowerPC is IBM’s architecture  IBM have their own build of PyTorch and TensorFlow to powerpc, which called the powerai bundle.

conda create -n powerai python==3.7
then we activate

conda activate powerai
then in your terminal type the following commands:

conda install ipykernel
then
 ``` 
python -m ipykernel install --user --name powerai --display-name "Python (powerai)"
```


conda config --prepend channels \
https://public.dhe.ibm.com/ibmdl/export/pub/software/server/ibm-ai/conda/


With conda, you can create environments that have different versions of Python or packages installed in them. Conda environments are optional but recommended. If not used, packages are installed in the default environment called base, which often has a higher risk of containing conflicting packages or dependencies. Switching between environments is called activating the environment.


conda install pytorch powerai-release=1.7.0


To install powerai GPU packages in the conda environment, run:
conda install powerai


Install pytorch and ipython.
Copy
conda install ipython
conda install pytorch



Installing Docker on CentOS 7
Perform the following steps to install Docker 19.03.8 on CentOS 7:

Remove the older version of Docker.

sudo yum remove docker \
  docker-client \
  docker-client-latest \
  docker-common \
  docker-latest \
  docker-latest-logrotate \
  docker-logrotate \
  docker-engine \
  docker-ce \
  docker-ce-cli

Show more
Install containerd.

Check if the epel repository is installed.
yum update
rpm -qa | grep epel
If it is not installed, install it:
yum install epel-release
Run the following command to install containerd:
sudo yum --enablerepo="epel" install containerd
Download the .rpm packages.

Docker Engine
CLI
Install these packages.
sudo rpm -i docker-ce-cli-19.03.8-3.el7.ppc64le.rpm docker-ce-19.03.8-3.el7.ppc64le.rpm