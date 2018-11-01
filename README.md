# Hirschman-sensing-structure
The compressive sensing (CS), enables us to recovery a sparse signal from its few linear measurements.<br>It is knowns that systems sensitive at the sampling node take the most advantage of CS.<br>Examples:<br>    Using CS, the MRI system reduces the irradiation time on the location of lesions<br>    Using CS, the large-area morniting system increases lifetime since each scan costs less energy
However, the CS requires much computations on its decoding.
<br>On the platform of Python 3.7.0 (win-10@x64, i7-6700@3.4GHz), it takes 22.7 secs to recover a 256-point gray scaled image from its 128 times random linear measurements using single process Orthognal Matching Pursuit agrithom.
<br>We generate the Hirschman Transform Matrix (HTM) by interpolating and circular shifting of a DFT matrix. It has been proved that all the HTMs satisfy the Mutual Incoherent Property (MIP) and can be used as the sensing structures in CS.
<br>Examples:<br>    HTM(2x128) means we use 127 zeros to interpolate the 2-point DFT and then circularly shift it and finally get the 256-point HTM<br>    HTM(128x2) means we use 1 zero to interpolate the 128-point DFT and then circularly shift it and finally get the 256-point HTM
<br>For a 256-point HTM, we have HTM(2X128),HTM(4X64),HTM(8X32),HTM(16X16),HTM(32X8),HTM(64X4),HTM(128X2) and HTM(1x256) eight choices in total. Among them, HTM(2X128) is sparse, HTM(128X2) is dense, and HTM(1x256) is equal to the 256-point DFT.
On the same platform, compare to the DFT sensing structure:<br>    HTM(128x2) takes 18.1 secs to do the recovery (speed increases by 20.3%)<br>    HTM(32x8) takes 15.986 secs to do the recovery (speed increases by 29.6%)<br>    HTM(2x128) takes 5.3 secs to do the recovery (speed increases by 76.7%)<br>
The sparse HTM is super fast, but its recovery performance heavily depends on how the target image looks like or more specifically, depends on how it looks like after sparsification.<br> 
We run this project to collect the recovery performances of different HTMs based on multiple types of input images. We would like to use statistical ways and machine learning to identify how can we pick up the appropriate HTMs for different target images to maximize the computational speed.

    
