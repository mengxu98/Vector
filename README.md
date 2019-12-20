<img src="https://github.com/jumphone/BEER/blob/master/DATA/Vector_LOGO.png" width="266">

# Developing Vector Inference

#### Environment: R (3.6.1)

#### Please install following R packages before using VECTOR:

    install.packages('circlize')
    install.packages('gatepoints')
    install.packages('stringr')
    install.packages('igraph')

## Usage:

Please prepare a Seurat object with 150 PCs

Users can follow https://satijalab.org/seurat/ to generate Seurat object.
    
    # pbmc: a Seurat object

    VEC=pbmc@reductions$umap@cell.embeddings
    rownames(VEC)=colnames(pbmc)
    PCA= pbmc@reductions$pca@cell.embeddings

    # Define pixel
    OUT=vector.buildGrid(VEC, N=30,SHOW=TRUE)
    
    # Build network
    OUT=vector.buildNet(OUT, CUT=1, SHOW=TRUE)
    
    # Calculate Margin Score (MS)
    OUT=vector.getValue(OUT, PCA, SHOW=TRUE)
    
    # Get pixel's MS
    OUT=vector.gridValue(OUT,SHOW=TRUE)
    
    # Find summit
    OUT=vector.autoCenter(OUT,UP=0.9,SHOW=TRUE)
    
    # Infer vector
    OUT=vector.drawArrow(OUT,P=0.9,SHOW=TRUE, COL=OUT$COL, SHOW.SUMMIT=TRUE)




#### Change MS to NES gene expression:

    NES.EXP = pbmc@assays$RNA@data[which(rownames(pbmc) =='Nes'),]
    OUT=vector.buildGrid(VEC, N=40,SHOW=TRUE)
    OUT=vector.buildNet(OUT, CUT=1, SHOW=TRUE)
    OUT=vector.getValue(OUT, PCA, SHOW=TRUE)

    OUT$VALUE=NES.EXP

    OUT=vector.showValue(OUT)
    OUT=vector.gridValue(OUT, SHOW=TRUE)
    OUT=vector.autoCenter(OUT,UP=0.9,SHOW=TRUE)
    OUT=vector.drawArrow(OUT,P=0.9,SHOW=TRUE, COL=OUT$COL,OL=2,AL=50)

    
#### Select starting point:

    OUT=vector.buildGrid(VEC, N=40,SHOW=TRUE)
    OUT=vector.buildNet(OUT, CUT=1, SHOW=TRUE)
    OUT=vector.getValue(OUT, PCA, SHOW=TRUE)
    OUT=vector.gridValue(OUT,SHOW=TRUE)

    OUT=vector.selectCenter(OUT)

    OUT=vector.drawArrow(OUT,P=0.9,SHOW=TRUE, COL=OUT$COL)

#### Select region of interest:

    OUT=vector.buildGrid(VEC, N=40,SHOW=TRUE)
    OUT=vector.buildNet(OUT, CUT=1, SHOW=TRUE)
    OUT=vector.getValue(OUT, PCA, SHOW=TRUE)
    OUT=vector.gridValue(OUT,SHOW=TRUE)
    OUT=vector.autoCenter(OUT,UP=0.9,SHOW=TRUE)
    OUT=vector.drawArrow(OUT,P=0.9,SHOW=TRUE, COL=OUT$COL)

    OUT=vector.reDrawArrow(OUT, COL=OUT$COL)
    OUT=vector.selectRegion(OUT)

    SELECT_SCORE=OUT$SELECT_SCORE
    SELECT_INDEX=OUT$SELECT_INDEX


