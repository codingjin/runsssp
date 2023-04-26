# runsssp
This specific data I am using for 74GB max memory consumption as a whole VM.

The workload running on Host OS will consume ~36GB.

(0) Generate large dataset, through https://github.com/farkhor/PaRMAT

E.g: 

./PaRMAT -nVertices 128171703 -nEdges 2027313017


Then, add this line to the 1st line of output file, out.

128171704 2027313017

mv this dataset, out, into GDATA/

Then run bfs_sssp on this dataset.

git clone git@github.com:codingjin/runsssp.git

unzip bfs.zip

(1) CSR tiling:

  cd BENCHMARK/tools/csr_tiling
  
  make
  
  ./page_rank GDATA/out
  
  (Now we get 64 files named out.xxx_untiled-xx)
  
(2) COO tiling:

  cd BENCHMARK/tools/coo_tiling
  
  make
  
  ./page_rank GDATA/out 8192 8192
  
  (It needs two 1024, then we got 64 files named out.xxx_coo-tiled-2048-xx; 2048 is the tile width, we may change it accordingly. If it reports Error: the tile width 2048, we just adjust them to 8192: ./page_rank DATAFOLDER/out 8192 8192)
  
(3) Column-major modifying
  
  cd BENCHMARK/tools/column_major_tile
  
  make
  
  ./kcore GDATA/out 8192 128 128
  
  (It needs two 16, then we got 64 files named out.xxx_col-16-coo-tiled-1024-xx; 16 is the stripe length, we may change it accordingly. For large dataset:  ./kcore DATAFOLDER/out 2048/8192 32/128 32/128)

(4) execute sssp

  cd ../../apps/sssp_simd
  
  make
  
  ./sssp GDATA/out 8192 128 4
  
  (For large dataset: ./sssp ~/datafolder/out 2048/8192 32/128 16)
  
  (The last parameter is the number of threads, like 1, 16, 32, 64, ...)
  



