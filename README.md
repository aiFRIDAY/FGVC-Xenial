# FGVC-Xenial
[vt-vl-lab FGVC](https://github.com/vt-vl-lab/FGVC) implemented in Ubuntu Xenial
(Alert!!! With GPU limitations)


## Installation


1.Install python 3.8.x in (Xenial) using these [instructions](https://medium.com/analytics-vidhya/installing-python-3-8-3-66701d3db134)


2.Install anaconda in (Xenial) using these [instruction](https://medium.com/@menuram1126/how-to-install-anaconda-on-ubuntu-16-04-538009ca7936)


3.Follow the pre-requisites from the [FGVC](https://github.com/vt-vl-lab/FGVC) and only edit cudatoolkit according to what your system can install.

I installed CUDA 9.2 (after lot of hit and trials and errors) so instead of cudatoolkit=10.1 , you might go for cudatoolkit=9.2


Now running the python will give few errors , so going to error resolution 


## Error Resolution

1. CV2 Not found :

Now in my case it resulted in conflict due to ros kinetic , so I commented out the source bash files of ros in .bashrc file in home.


2. CUDA CPU only : torch.cuda.is_available() is false

open the python file and make all torch.load lines like : torch.load( something , map_location=torch.device('cpu'))

3.CUDA Runtime error 100:

Now this error is resulted as you don't have CUDA installed probably. So as I mentioned before I installed CUDA 9.2.

Make sure you have the PPAs or serach for it and then :

```bash
sudo apt-get install cuda-9-2
```

Now the trouble I faced here is even though it installs and works but the problem came with the NVIDIA Driver( which i discuss later)

4.CUDA Runtime error 30:

The best solution is to reboot , do make sure everything is updated and no dependencies left.


## NVIDIA Driver Trouble

There are lot things that could go wrong with driver installation

DO NOT prefer to download a run file from NVIDIA website.

Check your GPU , CUDA version and Driver versions compatability

Take my example : while installing CUDA 10.x or 9.x or even 8.x it took NVIDIA-460 as default driver along with it 

It resulted in repeated crash that took hours to finally find what should be changed.

So for me , 450 was the latest supported and compatible version , so I added the PPAs then:

```bash
sudo apt-get install nvidia-450
```

After it install (whatever suitable driver version) reboot and check if you don't face any crashes.

If you do go to ttyl console using ctlr+alt+F1 and proceed to remove those drivers and reboot.

So if driver is sucessful then proceed again with CUDA installation only this time

```bash
sudo aptitude install cuda-9-2
```

Now as aptitude works it will provide various schemes but none seemed suitable , but eventually after quiting and again running through proposed solutions 

Finally a solution appears which left nvidia-460 unresolved and installed cuda-9-2 at nvidia-450 dependencies.


## GPU Limitation

After running program I faced the limitation : RuntimeError: CUDA out of memory. Tried to allocate 226.00 MiB (GPU 0; 1.96 GiB total capacity; 916.15 MiB already allocated; 172.69 MiB free; 1.13 GiB reserved in total by PyTorch)



