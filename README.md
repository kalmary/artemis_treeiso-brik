
# Individual-tree isolation (treeiso) from terrestrial laser scanning

This repository is a fork of https://github.com/truebelief/artemis_treeiso.git. Many thanks for its authors' hard work and implementation of the tool. Main goal of this modified program is to adjust it for multiple-files processing pipeline by silencing printing information, modyfing computation parameters and simplifying the output produced by the tool.



### **Authors of the original repository:**

Zhouxin Xi (zhouxin.xi@nrcan-rncan.gc.ca) and Chris Hopkinson (c.hopkinson@uleth.ca)

The University of Lethbridge - Department of Geography & Environment - Artemis Lab

Please cite:

> Xi, Z.; Hopkinson, C. 3D Graph-Based Individual-Tree Isolation (_Treeiso_) from Terrestrial Laser Scanning Point Clouds. _Remote Sens_. **2022**, _14_, 6116. https://doi.org/10.3390/rs14236116


This tool relies on the cut-pursuit algorithm, please also consider citing:

> Landrieu, L.; Obozinski, G. Cut Pursuit: Fast Algorithms to Learn Piecewise Constant Functions on General Weighted Graphs. SIAM J. Imaging Sci. 2017, 10, 1724–1766. [hal link](https://hal.archives-ouvertes.fr/hal-01306779)

> Raguet, H.; Landrieu, L. Cut-pursuit algorithm for regularizing nonsmooth functionals with graph total variation. In Proceedings of the International Conference on Machine Learning, Stockholm, Sweden, 10–15 July 2018; Volume 80, pp. 4247–4256.

## Folder Structure

```
├── Python                                  # treeiso Python source code
│   ├── treeiso.py                          # Main Python program
│   ├── cut_pursuit_L0.py                   # Simplified Python version of cut-pursuit (L0 norm only)
│   └── cut_pursuit_L0_replica_cpp.py       # Python version closer to the original C++ (slower; for reference)
├── PythonCPP                               # Python source code accelerated by C++ (Recommended)
│   ├── treeiso.py                          # Main Python program
│   └── cut_pursuit_L0.py                   # Backup simplified cut-pursuit (L0 norm only)
├── cut_pursuit_py                          # C++ cut-pursuit Python binder (external link)
├── .gitmodules                             # External repository declarations
├── LICENSE
└── README.md
```
#### Installation

1. Code is tested for Python3.12.
2. Clone or download the repository in choosen directory:
   ```bash
   git clone https://github.com/kalmary/artemis_treeiso_brik.git
   ```
3. Install the dependencies:
   ```bash
   pip install -r requirements.txt
   git submodule update --init --recursive
   ```

#### Usage

Import ``from artemis_treeiso_brik.PythonCpp.treeiso_CPP import tree_isolator``. Once your point cloud is loaded and all non-vegetation segments are isolated, run: 
```python
tree_ids: ndarray = tree_isolator(vegetation_pcd)
```
Tree_ids are individual ids for each tree clustered based on predefined parameters.

### Important notes
- The program first checks for the presence of the C++-based `cut_pursuit_py` module. If it is not installed, it falls back to the pure Python version (`cut_pursuit_L0.py`). The C++ interface is provided via pre-built wheels for Ubuntu, macOS, and Windows, so you typically do not need a C++ compiler. The C++ version offers a speed-up of over 5× compared to the pure Python implementation, though the results may differ slightly.
- Computations are based on predefined global parameters in `treeiso_CPP`. Results may be different depending on point cloud density, trees' size and their variety.
- Flags `silent` and `cut_pursuit_silent` are meant to control information printing level.
- Flag `USE_CPP` is a choice of CPP/ pure Python tool's implementation. 
 

## Example isolated trees: 

|Raw TLS Example1| After treeiso isolation| Top view|
|:---:|:---:|:---:|
|<img width="635" alt="demo1_crop" src="https://user-images.githubusercontent.com/8785889/182312969-7c81949f-67fa-409b-bb24-73b094917c52.png">|<img width="635" alt="demo1_treeiso_crop" src="https://user-images.githubusercontent.com/8785889/182313130-7d6ba091-b2cb-4482-ae21-3ee2ec5bb34e.png">|<img width="700" alt="demo1_treeiso2_crop" src="https://user-images.githubusercontent.com/8785889/182313150-a14e7a3e-79b0-4d40-a400-ce89520e5e4d.png">|


|Raw TLS Example2| After treeiso isolation| Top view|
|:---:|:---:|:---:|
|<img width="1354" alt="TAspen1_crop" src="https://user-images.githubusercontent.com/8785889/182315836-78bdd338-2678-4d29-bcce-1442777e6918.png">|<img width="1354" alt="TAspen1_treeiso_crop" src="https://user-images.githubusercontent.com/8785889/182315950-f44937e2-0ec8-4b43-a980-0da9b7925534.png">|<img width="724" alt="TAspen1_treeiso2_crop" src="https://user-images.githubusercontent.com/8785889/182315979-d8e9c5ea-7a00-4490-8bab-c43fd6af9498.png">|


|Raw TLS Example3| After treeiso isolation| Top view|
|:---:|:---:|:---:|
|<img width="1228" alt="NPoplar2_crop" src="https://user-images.githubusercontent.com/8785889/182318128-ddeac093-f48d-4e69-a9b2-560f5a25335e.png">|<img width="1228" alt="NPoplar2_treeiso_crop" src="https://user-images.githubusercontent.com/8785889/182318163-290e0663-4e0a-455c-85d5-5160f00a8e33.png">|<img width="1026" alt="NPoplar2_treeiso2_crop" src="https://user-images.githubusercontent.com/8785889/182318398-fc428e17-1fd7-415c-9594-e0b9e3f0c340.png">|

## Visual pros and cons:

![image](https://github.com/user-attachments/assets/c10659a7-b64e-4e1e-83d6-eeed9968c7e3)


### Note

*treeiso* is not intended to be a perfect, all-in-one solution. Instead, it is designed as an interactive tool that allows users to re-run the algorithm on problematic areas or manually adjust the segmentation results within CloudCompare.










