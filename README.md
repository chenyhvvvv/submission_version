# Cell Segmentation Competition
## Background
## Dataset

The Stereo-seq dataset [[1]](#1) captures a whole adult mouse brain slice. The barcoded spots are arranged in a grid with a distance of 0.5 μm between spots. In total, this dataset profiled 26,177 genes in more than 42,000,000 spots with an average of 3.3 unique molecular identifier (UMI) counts per spot. The brain slice was imaged with nucleic acid staining, allowing for segmentation of the nucleus using image-based methods.

To get the raw complete data for this competition, you can download the transcriptomics data [Mouse_brain_Adult_GEM_bin1.tsv.gz](https://ftp.cngb.org/pub/SciRAID/stomics/STDS0000058/Bin1_matrix/Mouse_brain_Adult_GEM_bin1.tsv.gz) and the stain image data [Mouse_brain_Adult.tif](https://ftp.cngb.org/pub/SciRAID/stomics/STDS0000058/Image/Mouse_brain_Adult.tif) from [MOSTA](https://db.cngb.org/stomics/mosta/download/).

In this competition, we select only ten 1200*1200 patches from the original dataset for training and testing. These ten patches have the highest gene counts. The selected patches are as follows:
`'6000/9600/1200/1200', '7200/8400/1200/1200', '8400/3600/1200/1200', '4800/1200/1200/1200', '3600/1200/1200/1200', '8400/6000/1200/1200', '8400/4800/1200/1200', '4800/10800/1200/1200', '7200/1200/1200/1200', '6000/1200/1200/1200'`
(`'start row index/start column index/patchsize/patchsize'`).

<span style="color:red">The dataset of this competition can be download from</span> [here.](https://drive.google.com/file/d/1s5mNr_dLLB_VKmHV2_GyiVS9sXj-TjHp/view?usp=drive_linkhttps://drive.google.com/file/d/1s5mNr_dLLB_VKmHV2_GyiVS9sXj-TjHp/view?usp=drive_link)

The stain images are stored in `tiff` folder. They are 8-bit DNA Fluorescent stains. And the corresponding gene expressions are in `gene` folder. They follow the following format in a tab-delimited file:
```
geneID  row  column  counts
```
For example, a geneID `0610009B22Rik` lies in (426,1021) with gene counts 3. The corresponding line in the gene expression file is `0610009B22Rik  426 1021  3`.

The segmentation[[2]](#2) results by [SCS](https://doi.org/10.1038/s41592-023-01939-3) are stored in `seg` folder. For each patch, there are several files:

- mask_[patch_id].png: the segmentation mask of the patch
- spot2cell_SCS_[patch_id].txt: the mapping from spot coordinates to cell indexes of the segmentation by SCS. Each line has the following format: `row:column  cell_id`. <span style="color:red">Important: this is also the submission format of your result!</span>
- (For evaluation)spot2cell_cellpose_[patch_id].txt: the mapping from spot coordinates to cell indexes of the segmentation by Cellpose[[3]](#3) method.
- (For evaluation)spot2nucl_[patch_id].txt: the mapping from spot coordinates to cell indexes by nucleus segmentation[[4]](#4).

By simply modify the path, you can reproduce the segmentation by running the source code.

You can also refer to the source code of the paper to know how to [preprocess the data](https://github.com/chenhcs/SCS/blob/main/src/preprocessing.py) in details. A jupyter version of part of the preprocess code with additional note is available (Not exactly the same since SCS does downscale, and some code of multiprocessing is added).

## File structure
    .
    ├── dataset
    │   ├── gene                # Gene expression data
    │       ├── patch_tsv_6000/1200/1200/1200.tsv  # Gene expression data of patch 6000/1200/1200/1200
    │       ├── ... (Other 9 patches)
    │   ├── seg                 # Segmentation results
    │       ├── mask_6000_1200_1200_1200.png  # Segmentation mask fig by SCS of patch 6000/1200/1200/1200
    │       ├── spot2cell_SCS_6000_1200_1200_1200.txt  # Spot to cell mapping by SCS of patch 6000/1200/1200/1200
    │       ├── spot2cell_cellpose_6000_1200_1200_1200.txt # Spot to cell mapping by Cellpose of patch 6000/1200/1200/1200
    │       ├── spot2nucl_6000_1200_1200_1200.txt # Spot to cell mapping by nucleus segmentation of patch 6000/1200/1200/1200
    │       ├── ... (Other 9 patches)
    │   ├── tiff                # Stain images
    │       ├── raw_stain_6000/1200/1200/1200.tif  # 8-bit Stain image of patch 6000/1200/1200/1200
    │       ├── ... (Other 9 patches)
    ├── document
    │   ├── README.md       # Competition description
    │   ├── evaluation.ipynb    # Evaluation demo
    │   ├── preprocess_demo.ipynb # Preprocess demo
    │   ├── evaluation.py   # Evaluation code from SCS
    └── ...
## Preprocess demo
See the notebook `preprocess_demo.ipynb` for details. Since the patch data provided in this competition is already preprocessed, this notebook is only for demonstration purpose and show you how to deal with the other patches in the large scale mouse brain dataset. You can use it to get familiar with the data. The ten pairs of stain image and gene expression data can be used directly for your model.

## Evaluation
See the notebook `evaluation.ipynb` for details.

## References
<a id="1">[1]</a> 
Chen, A. et al. Spatiotemporal transcriptomic atlas of mouse organogenesis using DNA nanoball-patterned arrays. Cell 185, 1777–1792 (2022).

<a id="2">[2]</a> 
Chen, H., Li, D. & Bar-Joseph, Z. SCS: cell segmentation for high-resolution spatial transcriptomics.
Nat Methods (2023).

<a id="3">[3]</a> 
Stringer, C., Wang, T., Michaelos, M. & Pachitariu, M. Cellpose: a generalist algorithm for cellular segmentation. Nat. Methods 18, 100–106 (2021).

<a id="4">[4]</a> 
Beucher, S. Use of watersheds in contour detection. In Proc. International Workshop on Image Processing 17–21 (CCETT, 1979).