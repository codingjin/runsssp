# runsssp

(0) Generate large dataset, through https://github.com/farkhor/PaRMAT

E.g: 

./PaRMAT -nVertices 73171703 -nEdges 1307313017


Then, add this line to the 1st line of output file, out.

73171704 1307313017

mv this dataset, out, into datafoler/

Then run bfs_sssp on this dataset.

git clone git@github.com:codingjin/runsssp.git

unzip bfs.zip

(1) CSR tiling:

  cd BENCHMARK/tools/csr_tiling
  
  make
  
  ./page_rank DATAFOLDER/out
  
  (Now we get 64 files named out.xxx_untiled-xx)
  
(2) COO tiling:

  cd BENCHMARK/tools/coo_tiling
  
  make
  
  ./page_rank DATAFOLDER/out 1024 1024
  
  (It needs two 1024, then we got 64 files named out.xxx_coo-tiled-1024-xx; 1024 is the tile width, we may change it accordingly. If it reports Error: the tile width 1024, we just adjust them to 8192: ./page_rank DATAFOLDER/out 8192 8192)
  
(3) Column-major modifying
  
  cd BENCHMARK/tools/column_major_tile
  
  make
  
  ./kcore DATAFOLDER/out 1024 16 16
  
  (It needs two 16, then we got 64 files named out.xxx_col-16-coo-tiled-1024-xx; 16 is the stripe length, we may change it accordingly. For large dataset:  ./kcore DATAFOLDER/out 8192 128 128)

(4) execute sssp

  cd ../../apps/sssp_simd
  
  make
  
  ./sssp ~/datafolder/soc-pokec-relationships.txt 1024 16 16
  
  (For large dataset: ./sssp ~/datafolder/soc-pokec-relationships.txt 8192 128 16)
  
  (The last parameter is the number of threads)
  



